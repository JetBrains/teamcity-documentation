[//]: # (title: Use Parameters in Build Chains)

This topic illustrates how you can use TeamCity [build parameters](configuring-build-parameters.md) to exchange simple data between configurations and pipelines of a [build chain](build-chain.md), as well as separate jobs of single pipeline.

<show-structure for="chapter" depth="2"/>

## Input and output parameters

Build configurations and pipelines allow you to create parameters of two types: input and output.

<img src="input-output-config.png" width="706" alt="Inputs and outputs in configurations"/>

<img src="input-output-pipeline.png" width="706" alt="Inputs and outputs in pipelines"/>

Both of them are manually created name/value pairs. The key difference lies in intended use cases and accessibility settings.

> The input/output parameters selection is available only in build configuration and pipeline settings. For projects and jobs, you can create only input parameters.
> 
{style="note"}

<deflist>
<def title="Input parameters">
Input parameters are designed to be consumed by the same configuration/pipeline that defines them. For example, an input parameter that stores a default branch name and referenced in <a href="configuring-vcs-roots.md">VCS root settings</a>. This value is of no use for other configurations and thus, should not be visible to them.
</def>

<def title="Output parameters">
Output parameters are configured in one build configuration/pipeline, but can be accessed by another via a <a href="snapshot-dependencies.md">snapshot</a> or <a href="artifact-dependencies.md">artifact</a> dependency. For example, a build configuration that builds a Docker image can write this image name to its parameter. The parameter is later used by a downstream configuration that deploys this image to a registry.

Output parameters can share existing parameters as is, modified parameters, and constants.

```Kotlin
outputParams {
    // Expose predefined parameter as is
    param("originConfName", "%system.teamcity.buildConfName%")
    
    // Expose modified parameter value
    param("buildNumber", "Build %\system.build.number%")
    
    // Expose input parameter as is
    param("name", "%customInputParam%")
    
    // Expose static value
    param("number", "54")
}
```
{ignore-vars="true"}

> For security reasons, input parameters of the ["Password" type](typed-parameters.md) cannot be exposed via output parameters.
>
{style="note"}
</def>
</deflist>


## Read parameters of upstream objects

Jobs can share their parameters with other jobs of the same pipeline only if they are linked in a sequence. Similarly, pipelines and configurations can access each other's parameters only when linked in a build chain.

### Access job parameters

Pipeline jobs can [retrieve values of preceding job parameters](job-settings.md#Parameters) via the `job.<job_ID>.<param-name>` syntax. 

```yaml
jobs:
  Job1:
    name: Job 1
    steps:
      - type: script
        script-content: |-
          echo "Print Job1 parameter: %env.ParamJobA%"    # prints 'foo'
    parameters:
      env.ParamJobA: foo
  Job2:
    name: Job 2
    dependencies:
      - Job1
    parameters:
      env.ParamJobB: '%job.Job1.env.ParamJobA% bar'
    steps:
      - type: script
        script-content: |-
          echo "Print parameter from upstream Job: %job.Job1.env.ParamJobA%"     # prints 'foo'
          echo "Print modified parameter: %env.ParamJobB%"    # prints 'foo bar'
```

Other pipelines and configurations from the same chain have no access to job parameters of upstream pipelines. If needed, you can assign a job parameter to a pipeline output parameter.

```yaml
jobs:
  Job1:
    name: Job 1
    parameters:
      jobParam: foo
output-parameters:
  PipelineOutputParam: %job.Job1.jobParam%
```

### Access configuration and pipeline parameters

To share a configuration/pipeline parameter with a downstream configuration or pipeline, this parameter must be an output one. In this case, any downstream configuration or pipeline can read its value via the `dep.<config-or-pipeline-ID>.<parameter-name>` syntax.

```Kotlin
object Upstream : BuildType({
    name = "Upstream config"

    params {
        param("inputParam", "foo")    // Input parameter, cannot be shared
    }

    outputParams {
        // Output parameter, accessible via the 'dep.' prefix
        // Modified value of an input parameter
        param("outputParam", "%inputParam% bar")
    }
})

object Downstream : BuildType({
    name = "Downstream config"

    steps {
        script {
            id = "simpleRunner"
            // Prints 'foo bar'
            scriptContent = "echo ${Upstream.depParamRefs["outputParam"]}"
        }
    }

    dependencies { snapshot(Upstream) { }}
})
```

Output parameters are visible to downstream objects but cannot be used in their own parent configurations or pipelines.

<include from="pipeline-settings.md" element-id="output-param-in-self"/>


<!--

For example, the following configuration uses an [input](typed-parameters.md) "DateFormat" parameter to calculate a value of the "Date" parameter created via the [setParameter service message](service-messages.md#set-parameter):


```Kotlin
import jetbrains.buildServer.configs.kotlin.*
import jetbrains.buildServer.configs.kotlin.buildSteps.csharpScript

object CalculateDate : BuildType({
    name = "Calculate Date"
    
    params {
        param("DateFormat", "dd/MM/yyyy") // Input parameter
    }
    outputParams {
        // TODO
    }

    steps {
        csharpScript {
            id = "csharpScript"
            content = """
                var date = DateTime.Now;
                // Calculate and add a new 'Date' parameter
                // Its value is shared by the output parameter
                Console.WriteLine("##teamcity[setParameter name='Date' value='" + date.ToString("%DateFormat%") + "']");
            """.trimIndent()
            tool = "%teamcity.tool.TeamCity.csi.DEFAULT%"
        }
    }
})
```
{ignore-vars="true"}

To share the "Date" parameter value down the chain, wrap it in an output parameter.

```Kotlin
object CalculateDate : BuildType({
    name = "Calculate Date"
    
    params {
        param("DateFormat", "dd/MM/yyyy") // Input parameter
    }
    outputParams {
        exposeAllParameters = false
        param("OutputDate", "%Date%") // Output parameter that exposes "Date"
    }
    steps {
        // ...
    }
})
```
{ignore-vars="true"}

> In TeamCity UI, the **Add new output parameter** dialog has two options: enter the value manually, or choose an existing input parameter. Since our "Date" parameter does not exist until the build step executes, we need to enter the `%\Date%` value by hand.
> 
> Otherwise, if "Date" was a regular input parameter, you could choose it from the list:
>
> <img src="dk-add-output-param.png" width="706" alt="Add output parameter"/>
>
{style="tip"}

The output parameter is now accessible by another configuration that [depends on](snapshot-dependencies.md) the origin configuration. To access a parameter, use the `dep.<origin configuration ID>.<output parameter name>` syntax.

<img src="dk-access-output-param.png" width="706" alt="Access output param"/>

```Kotlin
object PrintDate : BuildType({
    name = "Print Date"

    // Retrieve the date from the 'OutputDate' configuration and print it
    steps {
        script {
            id = "simpleRunner"
            scriptContent = """echo "The date is ${CalculateDate.depParamRefs["OutputDate"]}""""
        }
    }

    // An artifact or snapshot dependency is required
    // to access another configuration's parameter
    dependencies { snapshot(CalculateDate) {} }
})
```

-->



### Expose all input parameters of a configuration

In build configurations, the **Add new output parameter** dialog has two options: enter the value manually or share an existing input parameter as is. Select a required input parameter, and TeamCity will create an output parameter with the `%\inputParameter%` value.

<img src="dk-add-output-param.png" width="706" alt="Add output parameter"/>

If the **All parameters are available to other build configurations** setting is checked, you do not need to share each input parameter individually. All of them are automatically exposed and can be referenced via the `dep.<config-ID>.<parameterName>` syntax.

<img src="dk-expose-all-input-params.png" width="706" alt="Expose all output parameters"/>

This setting is enabled by default for backward compatibility. However, we strongly recommend you to do the following:

1. Check all cases of input parameters being used by other configurations.
2. Expose input parameters you wish to keep sharing by referencing them in new output parameters.
3. Disable the **All parameters are available to other build configurations** setting.

Doing so ensures configurations remain easily maintainable: you can edit and remove input parameters as needed, without accidentally breaking downstream configurations that used these parameters via the `dep...` syntax. In addition, it enhances security by keeping hidden parameters that were never designed to be shared.



## Override parameters of upstream objects

A downstream configuration/pipeline can read parameters from an upstream configuration/pipeline via `dep.<source-object-ID>.<parameter-name>`. Add the `override.dep.` prefix to do the opposite: override a value of an upstream parameter from a directly or indirectly dependent entity.

The following example illustrates two objects exchanging parameter values:

* The build chain contains two objects: an upstream pipeline and a downstream build configuration.

* The pipeline declares an input parameter with the default value `foo`. When the pipeline runs on its own, this value is used in the build script.

* When the pipeline runs as part of a build chain (triggered by the configuration's [snapshot dependency](snapshot-dependencies.md)), the downstream configuration overrides this parameter and sets it to `bar`.

* The pipeline output parameter references the input parameter, so it reports the updated value as well.

In other words, the downstream configuration writes `bar` to the pipeline input via `override.dep.`, the pipeline exposes it through its output parameter, and the downstream configuration can then read it back via `dep.`.


```yaml
# Upstream pipeline
jobs:
  Job1:
    name: Job 1
    steps:
      - type: script
        # Prints "foo" if the pipeline runs alone
        ## Or "bar" if runs in a chain
        script-content: 'echo "Input param: %PipelineInputParam%"'
parameters:
  PipelineInputParam: foo
output-parameters:
  # Output parameter shares input parameter as is
  PipelineOutputParam: '%PipelineInputParam%'

```

```Kotlin
// Downstream configuration
object DownstreamConfig : BuildType({
    name = "Downstream build configuration"
    params {
        // Overrides the pipeline input parameter
        param("override.dep.MyProject_UpstreamPipeline.PipelineInputParam", "bar")
    }

    steps {
        script {
            id = "simpleRunner"
            // Prints "bar"
            scriptContent = """echo "${UpstreamPipeline.depParamRefs["PipelineOutputParam"]}""""
        }
    }
    dependencies { snapshot(UpstreamPipeline) { }}
})
```

The types of the sender and receiver do not matter. In this example, a build configuration overrides a pipeline parameter, but the same syntax and behavior apply to any combination: pipeline to build configuration, pipeline to pipeline, or build configuration to build configuration.

### Wildcards

Unlike `dep.` parameters, which require the exact source object ID, `override.dep.` parameters can replace part or all of the ID with an asterisk (`*`). This allows you to override input parameters in multiple matching objects at once.

For example, consider a build chain with three configurations that build a .NET project, topped by a [composite](composite-build-configuration.md) configuration named "Build All". Each build configuration has a `build.mode` parameter that [sets the compiler mode](https://www.kenmuse.com/blog/understanding-dotnet-debug-vs-release/) to either `Debug` or `Release`.

When you run the full chain from "Build All", you can keep the current mode or change it for all three configurations at once. To do this, use a single `override.dep.*.build.mode` parameter instead of three separate `override.dep.<config-ID>.build.mode` parameters.

> See [](typed-parameters.md#Single-Select+Parameter) for more information on parameter appearance settings.
> 
{style="tip"}


```Kotlin
// "Build All" composite configuration
object OverrideWildcard_BuildAll : BuildType({
    id("BuildAll")
    name = "Build All"
    type = BuildTypeSettings.Type.COMPOSITE

    params {
        // The "select" parameter with an extra "Default" value
        // Writes the same value to all upstream "build.mode" parameters
        select("override.dep.*.build.mode", "Default",
            options = listOf("<current setting>" to "Default", "Release", "Debug"))
    }

    dependencies {
        snapshot(BuildDmg) {}
        snapshot(BuildExe) {}
        snapshot(UnitTests) {}
    }
})

// Regular build/test configurations
object BuildDmg : BuildType({
    id = AbsoluteId("BuildDmg")
    name = "Build dmg"

    params {
        // The default mode is "Release"
        // Other configurations can have this set to "Debug"
        // Note there's no "Default" option
        select("build.mode", "Release",
            options = listOf("Debug", "Release"))
    }
    
    steps {
        csharpScript {
            name = "Set default debug mode"
            id = "Set_default_debug_mode"
            // If "Build All" set the parameter to "Default"...
            conditions {
                equals("build.mode", "Default")
            }
            // ...then send a service message to revert it back to "Release"
            content = """
            Console.WriteLine("The build.mode parameter was set to 'Default'.");
            Console.WriteLine("Setting the build mode to 'Release'...");
            Console.WriteLine("##teamcity[setParameter name='build.mode' value='Release']");
        """.trimIndent()
            tool = "%teamcity.tool.TeamCity.csi.DEFAULT%"
        }

        dotnetBuild {
            // TODO
        }
    }
})
```

### Conflict resolution

If a parameter is edited by multiple `override.dep.` parameters owned by different configurations or pipelines, TeamCity applies the most recent edit made by the entity that runs last.


```Text
ConfigD (runs last, triggers the chain)
+------------------------------------------+
| override.dep.ConfigA.Fruit = "pear"      |
+------------------------------------------+
                 │
                 ▼
ConfigC (runs third)
+------------------------------------------+
| override.dep.ConfigA.Fruit = "orange"    |
+------------------------------------------+
                 │
                 ▼
ConfigB (runs second)
+------------------------------------------+
| override.dep.ConfigA.Fruit = "banana"    |
+------------------------------------------+
                 │
                 ▼
ConfigA (runs first)
+------------------------------------------+
| Fruit = "apple"                          |
+------------------------------------------+

Final value in ConfigA: "pear"
```

Otherwise, if the edits are made by multiple same-level entities, the target parameter retains its original value.



### Special notes

* Output parameters cannot be edited via `override.dep.` parameters.
* The `override.dep.` parameters only update existing parameters in target configurations or pipelines; they do not create missing parameters. In addition, upstream parameters that already have the same value are not forcibly updated, which allows TeamCity to [reuse previous builds](snapshot-dependencies.md#Suitable+Builds).

    <!--To force propagation and create a parameter in upstream entities that do not already have it, use the `*!` wildcard instead of a source object ID. For example, use this syntax to propagate a composite configuration's build number to all upstream configurations.

    ```Kotlin
    object BuildAll : BuildType({
        name = "Build All"
        type = BuildTypeSettings.Type.COMPOSITE
        params {
            param("override.dep.*!.build.counter", "%build.counter%")
        }
        dependencies {
            snapshot(BuildDmg) {}
            snapshot(BuildExe) {}
            snapshot(UnitTests) {}
        }
    })
    ```
    -->

* In pipelines, only pipeline-level parameters can have the `override.dep.` prefix. Job parameters cannot modify remote parameters.

    ```yaml
    jobs:
      Job1:
        name: Job 1
        parameters:
            # Ignored
          override.dep.TargetID.myParam: foo
    dependencies:
      - TargetID
    parameters:
        # Applied
      override.dep.TargetID.myParam: bar
    ```
  
* When overriding pipeline parameters, `override.dep.*.paramName` updates any `paramName` regardless of its scope: both pipeline inputs and job parameters are affected.

* If you pass a parameter reference, it will be resolved inside the entity that owns this `override.dep.`, rather than the entity whose parameter value is edited. If it cannot be resolved, the reference will be passed as plain text and the "Parameter is not fully resolved" warning will be shown in a build log.

    ```Kotlin
    object UpstreamConfig : BuildType({
        name = "Upstream config"
        params {
            param("bar", "default")
            param("foo", "default")
        }
    
        steps {
            script {
                id = "simpleRunner"
                scriptContent = """
                    echo "%foo%"    // Prints "Downstream config"
                    echo "%bar%"    // Prints "%invalid%"
                """.trimIndent()
            }
        }
    })
    
    object DownstreamConfig : BuildType({
        name = "Downstream config"
        params {
            param("override.dep.UpstreamConfig.foo", "%system.teamcity.buildConfName%") // The name of THIS configuration
            param("override.dep.UpstreamConfig.bar", "%invalid%") // Unresolved
        }
    
        dependencies { snapshot(UpstreamConfig) { } }
    })
    ```
    Note that parameter references are resolved before an upstream build actually starts. By that time, all parameters must have their values assigned in pipeline/configuration settings or passed via the [Run custom build dialog](running-custom-build.md). If you reference a parameter calculated during a build, no value will be passed.




## 'reverse.dep.' parameters

The `override.dep.<target-ID>.<parameter-name>` syntax was introduced in TeamCity 2026.1. In earlier versions, upstream parameters were modified with the similar `reverse.dep.<target-ID>.<parameter-name>` syntax.

`reverse.dep.` is still supported, but we recommend `override.dep.` because its behavior is simpler and more predictable.

* Unlike `override.dep.`, `reverse.dep.` does not resolve parameter references and passes them as is, which can make upstream configurations or pipelines incompatible with some build agents.

* `reverse.dep.` is also more invasive: if a target configuration or pipeline has no matching parameter, TeamCity creates one. By contrast, `override.dep.` only updates existing parameters and ignores entities without a matching parameter. This is also worth noting since adding new parameters nullifies the "_[Do not run new build if there is a suitable one](snapshot-dependencies.md#Suitable+Builds)_" snapshot dependency policy, so upstream builds will never be reused.

* `reverse.dep.` uses more complex conflict resolution. When multiple entities modify the same parameter, TeamCity first gives priority to the configuration or pipeline that runs last, just as it does for `override.dep.`. If the conflicting editors are on the same level, TeamCity compares target ID specificity: the parameter with the most specific ID wins.

    ```Text
    Config A
    +-----------------------------------------------------------+
    | person = "Mary"                                           |
    +-----------------------------------------------------------+
           ┌────────────────────├───────────────────────┐
           ▼                    ▼                       ▼
        Config B             Config C               Config D
    +----------------+   +----------------+    +----------------+
    | reverse.dep.   |   | reverse.dep.   |    | reverse.dep.   |
    | ConfigA.person |   | Conf*.person   |    | *.person       |
    | = John         |   | = Mike         |    | = Jane         |
    +----------------+   +----------------+    +----------------+
           └─────────────────────├───────────────────────┘
    Run all                      ▼
    +-----------------------------------------------------------+
    | composite configuration                                   |
    | depends on: ConfigB, ConfigC, ConfigD                     |
    +-----------------------------------------------------------+
    
    Config B: fully clarified target ID
    Config C: ID partially replaced with a wildcard
    Config D: wildcard instead of target ID
    
    Final value in ConfigA: "John" (Config B)
    ```

    Finally, if the conflicting edits come from same-level entities with equally specific IDs, TeamCity leaves the target parameter unchanged and adds a `conflict.<sender_config_ID>.paramName` parameter for each unique conflicting value.

    <img src="dk-params-overrideConflict.png" width="706" alt="Conflicting Overrides"/>



