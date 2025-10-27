[//]: # (title: Command Line (Script))
[//]: # (help-id: Command Line)

<primary-label ref="primary-step-pipeline"/>

<show-structure for="chapter" depth="2"/>

**Command Line** (in [build configurations](creating-and-editing-build-configurations.md)) or **Script** (in [pipelines](create-and-edit-pipelines.md)) is the most flexible build step in TeamCity. It runs commands directly on the agent machine, allowing interaction with any installed tool (for example, cURL, Homebrew, Python, or Unreal Engine).

You can also use it as an alternative to tool-specific TeamCity steps. For instance, run `mvn package` script instead of using the [Maven](maven.md) step with the `package` goal.

## Step Settings

The list of Script step settings and their corresponding UI labels slightly differ depending on whether you configure a build configuration or a pipeline.

### Main Settings

<deflist type="medium">

<def title="Run">

Allows you to choose between running a custom script entered to the corresponding step settings field or (currently available only in build configurations) launching any executable with required parameters.

Scripts are executed as executable scripts in Unix-like environments and as `*.cmd` batch files on Windows. Under Unix-like OS the script is saved with the executable bit set and is then executed by OS. This defaults to `/bin/sh` interpreter on the most systems. If you need a specific interpreter to be used, specify shebang (for example, `#!/bin/bash`) as the first line of the script.

>TeamCity treats a string surrounded by percentage signs (`%`) in the script as a [parameter reference](predefined-build-parameters.md). To prevent TeamCity from treating the text in the percentage signs as a property reference, use double percentage signs to escape them: for example, if you want to pass `%\Y%m%\d%H%\M%S` into the build, change it to `%\%Y%\%m%\%d%\%H%\%M%\%S`.
>
{style="note"}

> When running an executable, space characters in parameters can be enclosed in double quotes. For non-trivial parameters it is recommended to use "Custom script" option instead.
> 
{style="tip"}

</def>

<def title="Working directory">

<include from="common-templates.md" element-id="step-settings-working-dir"/>

</def>


</deflist>

### Advanced Settings

<deflist type="medium">

<def title="Format stderr output as">

Allows you to choose how the error output should be handled by the step. Available options:

* __error__ — Any output to `stderr` is handled as an error.
* __warning__ (default) — Any output to `stderr` is handled as a warning.

</def>

</deflist>


### Container Settings

<include from="common-templates.md" element-id="build-step-run-in-docker"/>

### Configuration as Code

<include from="common-templates.md" element-id="step-settings-config-as-code"/>

<tabs>

<tab title="Kotlin DSL">

```Kotlin
import jetbrains.buildServer.configs.kotlin.*
import jetbrains.buildServer.configs.kotlin.buildSteps.script

object YTApi : BuildType({
    name = "Sample Build Configuration"
    steps {
        script {
            name = "Extract the list of issues from YT"
            id = "simpleRunner"
            scriptContent = """
            curl -X GET "https://youtrack.mycompany.com/api/issues?fields=idReadable,summary,customFields(value(name),name)&query=Project:%20MyProject" \
              -H "Authorization: Bearer ${'$'}YT_TOKEN" \
              -H "Accept: application/json" \
              -o response.txt
            """.trimIndent()
        }
    }
})
```


See also: [ScriptBuildStep Kotlin DSL documentation](https://teamcity.jetbrains.com/app/dsl-documentation/buildSteps/script-build-step/index.html?query=ScriptBuildStep)

</tab>

<tab title="YAML">

```yaml
jobs:
  Job1:
    name: Sample Job
    steps:
      - type: script
        script-content: >-
          curl -X GET
          "https://youtrack.mycompany.com/api/issues?fields=idReadable,summary,customFields(value(name),name)&query=Project:%20MyProject" \
              -H "Authorization: Bearer $YT_TOKEN" \
              -H "Accept: application/json" \
              -o response.txt
```

</tab>

</tabs>



<seealso>
        <category ref="concepts">
            <a href="configuring-build-steps.md">Build Step</a>
            <a href="build-checkout-directory.md">Build Checkout Directory</a>
            <a href="build-working-directory.md">Build Working Directory</a>
        </category>
        <category ref="admin-guide">
            <a href="configuring-build-steps.md">Configuring Build Steps</a>
            <a href="build-failure-conditions.md">Build Failure Conditions</a>
        </category>
</seealso>

