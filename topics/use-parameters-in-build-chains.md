[//]: # (title: Use Parameters in Build Chains)

This topic illustrates how you can use TeamCity [build parameters](configuring-build-parameters.md) to exchange simple data between configurations of a [build chain](build-chain.md).


## Input and Output Parameters

The **Parameters** page of [build configuration settings](project-administrator-guide.md#Edit+and+View+Modes) lets you switch between input and output parameters, both of which are name/value pairs functioning similarly. Their key difference lies in intended use cases and visibility settings.

> The input/output parameters toggle is available only in build configuration settings. For projects, you can create only input parameters.
> 
{style="note"}

<deflist>
<def title="Input parameters">
Input parameters are designed to be consumed by the same configuration that defines them. For example, an input parameter that stores a default branch name and referenced in <a href="configuring-vcs-roots.md">VCS root settings</a>. This value is of no use for other configurations and thus, should not be visible to them.
</def>

<def title="Output parameters">
Output parameters are configured in one build configuration, but can be accessed by another configuration via a <a href="snapshot-dependencies.md">snapshot</a> or <a href="artifact-dependencies.md">artifact</a> dependency. For example, a build configuration that builds a Docker image can write this image name to its parameter. The parameter is later used by a downstream configuration that deploys this image to a registry.

See the sections below for more information on how to create and use output parameters.
</def>
</deflist>


## Create an Output Parameter

Since an output parameter is calculated in one configuration and passed into another, it often makes sense to create the input-output parameters pair:

* the input parameter is used inside the origin configuration;
* the output parameter references the input parameter to expose its value.

For example, [create an input parameter](typed-parameters.md) called "Date" and calculate its value:

```Kotlin
object CalculateDate : BuildType({
    name = "Calculate Date"
    
    // Create a parameter
    params { param("Date", "") }

    // Calculate paramter value
    // and write it using the 'setParameter' TeamCity service message
    steps {
        csharpScript {
            id = "csharpScript"
            content = """
                var date = DateTime.Now;
                Console.WriteLine("##teamcity[setParameter name='Date' value='" + date.ToString("dd/MM/yyyy") + "']");
            """.trimIndent()
            tool = "%teamcity.tool.TeamCity.csi.DEFAULT%"
        }
    }
})
```

Now you can create an output parameter that references this input parameter:

<img src="dk-add-output-param.png" width="706" alt="Add output parameter"/>

```Kotlin
object CalculateDate : BuildType({
    name = "Calculate Date"

    // Input parameter
    params { param("Date", "") }

    // Output parameter whose value is synced with the 'Date' input parameter
    outputParams {
        exposeAllParameters = false
        param("OutputDate", "%Date%")
    }

    // ...
})
```

The output parameter (and by proxy, the referenced input parameter) is now accessible by another configuration that [depends on](snapshot-dependencies.md) the origin configuration. To access a parameter, use the `dep.<origin configuration ID>.<output parameter name>` syntax.

<img src="dk-access-output-param.png" width="706" alt="Access output param"/>

```Kotlin
object PrintDate : BuildType({
    name = "Print Date"

    // Retrieve the 'OutputDate' value and print it
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

You can also expose predefined configuration parameters and constants.

```Kotlin
outputParams {
    param("originConfName", "%system.teamcity.buildConfName%") // Expose predefined parameter
    param("number", "54") // Expose static value
}
```

> For security reasons, input parameters of the _Password_ type cannot be exposed via output parameters.
>
{style="note"}


## Expose All Input Parameters

The **Output Parameters** tab displays the **All parameters are available to other build configurations** setting.

<img src="dk-expose-all-input-params.png" width="706" alt="Expose all output parameters"/>

If this setting is enabled, all input parameters are accessible by dependent configurations via the `dep.<config ID>.<parameter name>` syntax. This setting is enabled by default for backward compatibility. However, we strongly recommend you to do the following:

1. Check all cases of input parameters being used by other configurations.
2. Expose input parameters you wish to keep sharing by referencing them in new output parameters.
3. Disable the **All parameters are available to other build configurations** setting.

This enhances security by keeping hidden parameters that were never designed to be shared.


## Access Output Parameters From Dependent Configurations

Dependent builds can access output parameters of the previous chain builds as `dep.<bcID>.<parameter_name>`, where `bcID` is the ID of a source build configuration whose parameter value you need to access.

<img src="dk-params-in-chains.png" width="706" alt="Parameters in dependent builds"/>

You can use `dep...` parameters to access parameters from a configuration even if the current configration has only indirect dependencies. For example, in the A &rarr; B &rarr; C chain where C depends on B and B depends on A, configuration C can access A's parameters.

The following build configuration builds and pushes a Docker image. The name of this image is written to the `DockerImageName` parameter.

```Shell
TAG=v1
docker build -f Dockerfile --tag your.registry/MyApp:${TAG}
docker push your.registry/MyApp:v1
echo "##teamcity[setParameter name='DockerImageName' value='MyApp:${TAG}']"
```

If this configuration's ID is "ConfigA", builds executed further down the build chain can access the image name as `dep.ConfigA.DockerImageName`:

```Shell
docker run -d your.registry/%\dep.ConfigA.DockerImageName%
```

## Override Input Parameters of Preceding Configurations

Add a parameter with the `reverse.dep.<build_conf_ID>.<parameter_name>` name syntax to override the input `<parameter_name>` parameter defined in the target configuration that precedes the current configuration.

For example, the following [Kotlin](kotlin-dsl.md) code defines a project with three build configurations united in a single build chain (ConfigA &rarr; ConfigB &rarr; ConfigC). Each build configuration has a `chain.ConfigX.param` parameter with its custom value. The last configuration has the additional `reverse.dep.ChainConfigA.chain.ConfigA.param` parameter.

```Kotlin
import jetbrains.buildServer.configs.kotlin.*

project {
    buildType(ChainConfigA)
    buildType(ChainConfigB)
    buildType(ChainConfigC)
}

object ChainConfigA : BuildType({
    name = "ChainConfigA"

    params {
        param("chain.ConfigA.param", "Config A")
    }

    steps {
        script {
            scriptContent = """echo "Parameter value is: %\chain.ConfigA.param%""""
        }
    }
})

object ChainConfigB : BuildType({
    name = "ChainConfigB"

    params {
        param("chain.ConfigB.param", "Config B")
    }

    steps {
        script {
            scriptContent = """echo "Parameter value is: %\chain.ConfigB.param%""""
        }
    }

    dependencies {
        snapshot(ChainConfigA) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})

object ChainConfigC : BuildType({
    name = "ChainConfigC"

    params {
        param("chain.ConfigC.param", "Config C")
        param("reverse.dep.ChainConfigA.chain.ConfigA.param", "Value Overridden in ConfigC")
    }

    steps {
        script {
            scriptContent = """echo "Parameter value is: %\chain.ConfigC.param%""""
        }
    }

    dependencies {
        snapshot(ChainConfigB) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})
```

If you run the ConfigA or the ConfigA &rarr; ConfigB sub-chain, the first configuration will report its original parameter value.

```
# ConfigA build log
Parameter value is: Config A
```

However, if you run a full build chain that ends with ConfigC, this last configuration will feed ConfigA a custom parameter value.

```
# ConfigA build log
Parameter value is: Value Overridden in ConfigC
```

You can use `*` wildcards in parameter names to the same parameters in multiple preceding configurations. For example, the ConfigC in the following sample has the `reverse.dep.ChainConfig*.MyParam` parameter, which overrides `MyParam` in both ConfigA and ConfigB.

```Kotlin
object ChainConfigA : BuildType({
    params {
        param("MyParam", "OriginalValue_A")
    }
})

object ChainConfigB : BuildType({
    params {
        param("MyParam", "OriginalValue_B")
    }

    dependencies {
        snapshot(ChainConfigA) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})

object ChainConfigC : BuildType({
    params {
        param("reverse.dep.ChainConfig*.MyParam", "CustomValue_C")
    }

    dependencies {
        snapshot(ChainConfigB) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})
```

### Conflicting Parameter Overrides

If in the ConfigA &rarr; ... &rarr; ConfigB &rarr; ... &rarr; ConfigC chain both ConfigB and ConfigC configurations try to override ConfigA's parameter ConfigC has a higher priority since it depends on ConfigB (either directly or through intermediate configurations).

```Kotlin
object ChainConfigA : BuildType({
    params {
        param("MyParam", "OriginalValue_A")
    }
})

object ChainConfigB : BuildType({
    params {
        // Lower priority
        param("reverse.dep.ChainConfigA.MyParam", "CustomValue_B")
    }

    // Depends on config A
    dependencies {
        snapshot(ChainConfigA) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})

object ChainConfigC : BuildType({
    params {
        // Higher priority
        param("reverse.dep.ChainConfigA.MyParam", "CustomValue_C")
    }

    // Depends on config B
    dependencies {
        snapshot(ChainConfigB) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})
```

However, if ConfigB and ConfigC do not depend on each other, an ambiguity regarding which configuration should have a priority emerges. TeamCity tries to resolve this ambiguity by comparing parameter names and prioritizing a parameter with the most specific build configuration ID.

* Highest priority: parameters with no wildcards in build configuration IDs (for example, `reverse.dep.ChainConfigA.MyParam`).
* Medium priority: parameters with partial configuration IDs (for example, `reverse.dep.Chain*A.MyParam`). The more specific the target configuration ID is, the higher the priority of this parameter. For instance, the `ChainConf*A` ID has a priority over the `Chain*A` ID since it is considered more specific.
* Lowest priority: parameters with the `*` wildcard instead of configuration IDs (for example, `reverse.dep.*.MyParam`).

If all conflicting configurations have similar parameter names and neither of them is a clear winner, TeamCity reports a conflict and creates additional `conflict.<build_config_ID>.<parameter_name>=<value>` parameters (one for each conflicting configuration).

```Kotlin
object ChainConfigA : BuildType({
    params {
        param("MyParam", "OriginalValue_A")
    }
})

object ChainConfigB : BuildType({
    params {
        // Equal priority
        param("reverse.dep.ChainConfigA.MyParam", "CustomValue_B")
    }

    // Depends on config A
    dependencies {
        snapshot(ChainConfigA) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})

object ChainConfigC : BuildType({
    params {
        // Equal priority
        param("reverse.dep.ChainConfigA.MyParam", "CustomValue_C")
    }

    // Depends on config A
    dependencies {
        snapshot(ChainConfigA) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})

// Composite build configuration that runs the entire chain
object ChainABC : BuildType({
    type = BuildTypeSettings.Type.COMPOSITE
    dependencies {
        snapshot(ChainConfigB) {}
        snapshot(ChainConfigC) {}
    }
})
```

<img src="dk-params-overrideConflict.png" width="706" alt="Conflicting Overrides"/>

### Other Considerations


* The `reverse.dep.*` parameters are processed on queuing a build where these parameters are defined. Since parameter values should be already known at this stage, these values must be assigned either in the build configuration or in the [custom build dialog](running-custom-build.md). Setting the parameter to a value calculated during a build has no effect.

* Pushing a new parameter into a build overrides the "_[Do not run new build if there is a suitable one](snapshot-dependencies.md#Suitable+Builds)_" snapshot dependency option and may trigger a new build if the parameter is set to a non-default value.

* Values of the `reverse.dep.` parameters are pushed to the dependency builds "as is", without [reference resolution](configuring-build-parameters.md#Parameter+References). `%`-references, if any, will be resolved in the destination (target) build's scope.
