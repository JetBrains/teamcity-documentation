[//]: # (title: Configuring Build Parameters)
[//]: # (help-id: Configuring Build Parameters)
<!--[//]: # (Internal note. Do not delete. "Configuring Build Parametersd72e3.txt")-->

<show-structure for="chapter,procedure" depth="2"/>

Parameters are `name=value` pairs that you reference via the `%\parameterName%` syntax in TeamCity settings and build scripts.

The parameter `value` part can be a raw value (`release.number=2026.1`) or include a reference to another parameter (`system.tomcat.libs=%\env.CATALINA_HOME%/lib/*.jar`).

> TeamCity interprets any text enclosed in percentage characters as a parameter reference (and potentially adds an [implicit agent requirement](configuring-agent-requirements.md#Implicit+Requirements) if no such parameter exists). To avoid this behavior, double percentage characters. For example, if you want to pass `%\Y%m%\d%H%\M%S` into the build, change it to `%\%Y%\%m%\%d%\%H%\%M%\%S`.

## Parameter Types

TeamCity supports paramters of three types:

* **Configuration Parameters** — parameters whose primary objective is to share settings within a build configuration. You can also use these parameters to customize a configuration that was created from a [template](build-configuration-template.md) or uses a [recipe](working-with-meta-runner.md). TeamCity does not pass parameters of this type to a build process (that is, these parameters are not accessible by a build script engine).


* **Environment Variables** — parameters that start with the `env.` prefix. These parameters are passed to the process of a build runner similarly to the default env variables of a system.


* **System Properties** — parameters that start with the `system.` prefix. TeamCity can pass parameters of this type to configuration files of certain runners as variables specific to a build tool.

## Main Use Cases

<procedure title="Parameterize a build script" collapsible="true" id="parameter-use-cases-parameterize-scripts" help-id="parameter-use-cases-parameterize-scripts">

If occasionally you need to run custom script variations, you can replace raw values with parameters. For example, the following [](kotlin-dsl.md) sample illustrates a [](gradle.md) step that runs the `clean build` command by default.

```Kotlin
object GradleStepParameters : BuildType({
    params { param("gradle.task", "clean build") }
    steps {
        gradle {
            id = "gradle_runner"
            // runs "clean build" by default
            tasks = "%gradle.task%"
        }
    }
})
```

Users can trigger a [custom build](running-custom-build.md) to override this parameter and run a different Gradle task.

<img src="dk-override-build-parameter.png" width="706" alt="Override build parameter"/>

You can also pre-fill this parameter with supported values. Then, instead of typing, users will be able to select an option via a combo-box...

<img src="dk-build-param-select.png" width="706" alt="Select build parameter"/>

```Kotlin
params {
    select("gradle.task", "clean build",
            options = listOf("clean build", "test build", "build assemble"))
}
```

...or checkboxes.

<img src="dk-build-param-checkboxes.png" width="706" alt="Check build parameter values"/>

```Kotlin
params {
    select("gradle.task", "clean build",
            allowMultiple = true, valueSeparator = "%space.separator%",
            options = listOf("clean", "test", "build", "assemble", "package"))
    param("space.separator", " ")
}
```

See [](typed-parameters.md) for more information on parameter customization.

</procedure>


<procedure title="Share common settings" collapsible="true" id="parameter-use-cases-share-settings" help-id="parameter-use-cases-share-settings">

Project-owned parameters can store settings common for multiple build configurations or pipelines. For example, if your organization follows strict branch-naming guidelines for all repositories, you can avoid entering identical [branch specifications](working-with-feature-branches.md) and other settings for each VCS root.

```Kotlin
// Project level

params {
    param("default.branch.spec", """
            refs/heads/dev-*
            -:refs/heads/sandbox
            -:refs/heads/testing-*
        """.trimIndent())
    param("default.branch", "refs/heads/dev-2024.1")
}

// VCS Root

object GitHubRepoRoot : GitVcsRoot({
    name = "My Root"
    url = "..."
    branch = "%default.branch%"
    branchSpec = "%default.branch.spec%"
    authMethod = password {
        userName = "..."
        password = "..."
    }
})
```

</procedure>


<procedure title="Avoid raw values" collapsible="true" id="parameter-use-cases-replace-values" help-id="parameter-use-cases-replace-values">

TeamCity agents report a number of parameters that store tool installation paths. You can use these parameters in build scripts and TeamCity settings. Doing so allows you to create agent-agnostic conditions and minimize potential errors.

```Kotlin
steps {
    gradle {
        name = "Gradle step"
        tasks = "build-dist"
        jdkHome = "%\env.JDK_19_0_ARM64%"
    }
}
```

In other cases, you might want avoid raw values because this data is sensitive (for example, authentication credentials used inside a build script). To hide these sensitive values from both TeamCity UI and build logs, create a [password parameter](typed-parameters.md#Create+a+Secret).

</procedure>


<procedure title="Customize templates and recipes" collapsible="true" id="parameter-use-cases-templates-and-recipes" help-id="parameter-use-cases-templates-and-recipes">

[Templates](build-configuration-template.md) allow you to quickly create similar build configurations and pipelines. You can parameterize certain template settings to implement unique behavior for each object that derives from this template.

For example, the following build configuration has two steps and the Boolean `skip.optional.step` parameter. Step #2 will or will not be executed depending on this parameter value.

```Kotlin
import jetbrains.buildServer.configs.kotlin.*
import jetbrains.buildServer.configs.kotlin.buildSteps.script

object SourceConfig : BuildType({
    name = "SourceConfig"

    params {
        param("skip.optional.step", "false")
    }
    steps {
        script {
            name = "Mandatory Step"
            scriptContent = """echo "Mandatory step #1 is running...""""
        }
        script {
            name = "Optional Step"
            scriptContent = """echo "Optional step #2 is running...""""
            conditions {
                equals("skip.optional.step", "false")
            }
        }
    }})
```

If you extract a [template](build-configuration-template.md) from this configuration, you can create multiple copies of the same configuration. In those copies that do not need to run the optional step #2, override the `skip.optional.step` parameter and set it to `true`.


```Kotlin
import jetbrains.buildServer.configs.kotlin.*

object ConfigFromTemplate : BuildType({
    templates(SourceConfigTemplate)
    name = "Build Config Based on Template"

    params {
        param("skip.optional.step", "true")
    }
})
```

[Recipes](working-with-meta-runner.md) are generalized steps that encapsulate frequently used actions. These objects also use parameters to customize their behavior.

```XML
<meta-runner name="cURL: File Download">
    <description>A two-step recipe that utilizes the "curl -o %URL% %fileName%" command to download a file, and calls "ls" command to print the contents of a working directory afterwards</description>
    <settings>
        <parameters>
            <param name="URL" value="" spec="text description='The URL of a file to be downloaded' display='normal' label='Download URL:'"/>
            <param name="fileName" value="" spec="text description='Enter the saved file name or leave blank to keep the origin name' label='File name:'" />
            <!--other parameters-->
        </parameters>
        <build-runners>
            <runner name="" type="simpleRunner">
                <parameters>
                    <param name="script.content" value="curl -o %URL% %fileName%" />
                    <param name="teamcity.step.mode" value="default" />
                    <param name="use.custom.script" value="true" />
                </parameters>
            </runner>
            <runner name="" type="simpleRunner">
                <parameters>
                    <param name="script.content" value="ls" />
                    <param name="teamcity.step.mode" value="default" />
                    <param name="use.custom.script" value="true" />
                </parameters>
            </runner>
        </build-runners>
        <requirements />
    </settings>
</meta-runner>
```

</procedure>


<procedure title="Specify step execution conditions" collapsible="true" id="parameter-use-cases-step-conditions" help-id="parameter-use-cases-step-conditions">


You can define [step execution conditions](build-step-execution-conditions.md) to specify whether individual steps should run. You can craft these conditions using [custom](typed-parameters.md) and [predefined](predefined-build-parameters.md) configuration parameters and environment variables.

For example, you can run different shell scripts depending on the build agent's operating system.

```Kotlin
import jetbrains.buildServer.configs.kotlin.*
import jetbrains.buildServer.configs.kotlin.buildSteps.powerShell
import jetbrains.buildServer.configs.kotlin.buildSteps.script

object StepExecutionConditions : BuildType({
    params {
        param("win.destination.path", "C:/Sources")
        param("unix.destination.path", "/Users/Admin/Sources")
    }

    steps {
        // PowerShell script runs only on Windows agents
        powerShell {
            name = "Copy File (Windows)"
            conditions {
                startsWith("teamcity.agent.jvm.os.name", "Windows")
            }
            scriptMode = script {
                content = """
                    Copy-Item "%system.teamcity.build.workingDir%/result.xml" 
                    -Destination %win.destination.path%"""
            }
        }
        // Command Line runner for non-Windows agents
        script {
            name = "Copy File (Unix)"
            executionMode = BuildStep.ExecutionMode.RUN_ON_FAILURE

            conditions {
                doesNotContain("teamcity.agent.jvm.os.name", "Windows")
            }
            scriptContent = """
                cp "%system.teamcity.build.workingDir%/result.xml" 
                %unix.destination.path%"""
        }
    }
})
```


</procedure>




<procedure title="Specify agent requirements" collapsible="true" id="parameter-use-cases-agent-requirements" help-id="parameter-use-cases-agent-requirements">


[Agent requirements](configuring-agent-requirements.md) allow you to specify `parameter-operator-value` conditions. Only those agents that meet these conditions are allowed to build this build configuration.

You can define agent requirements using only those parameters whose values agents can report before the build starts. These parameters are:

* Predefined configuration parameters available for all agents (for example, `teamcity.agent.name`).

* Environment variables reported by agents (for example, `env.DOTNET_SDK_VERSION`).

* Custom configuration parameters that are present in agents' [buildAgent.properties](configure-agent-installation.md) files (for example, create a `custom.agent.parameter` in TeamCity UI and add the `custom.agent.parameter=MyValue` line to agents' properties files).

> TeamCity automatically adds agent requirements depending on the configured build steps. For example, if a build step should be executed inside a Linux container, TeamCity adds requirements that specify an agent must have [either Docker or Podman](integrating-teamcity-with-container-managers.md) running on a Linux machine.


In [](kotlin-dsl.md), use the [`requirements`](https://www.jetbrains.com/help/teamcity/kotlin-dsl-documentation/root/requirements/index.html) collection to define new requirements.

```Kotlin
object MyBuildConfig : BuildType({
    requirements {
        // Only agents with .NET SDK 5.0
        exists("DotNetCoreSDK5.0_Path")
        // Only Windows agents
        startsWith("teamcity.agent.jvm.os.name", "Windows")
        // Only agents with "Android" workload for .NET 7 SDK
        contains("DotNetWorkloads_7.0", "android")
    }
})
```

</procedure>


<procedure title="Parameterize builder configurations" collapsible="true" id="parameter-use-cases-builder-configs" help-id="parameter-use-cases-builder-configs">

[.NET](net.md), [Maven](maven.md), [Gradle](gradle.md), [Ant](ant.md) and [NAnt](nant.md) runners allow you to reference TeamCity parameters in build configuration files. This technique allows you to pass the required values to build processes.

> Parameters used in this scenario should start with either `env.` or `system.` prefixes but referenced without these prefixes. For example, use `${build.number}` in Maven configuration files to reference the predefined `system.build.number` parameter.
>
{style="warning"}

<tabs>


<tab title=".NET">

In .NET, pass parameter values using the `$(<parameter_name>)` syntax.

> * MSBuild does not support names with dots (.), so you need to replace dots with underscores ("_") when using the parameter inside a build script.
> * The `nuget push` and `nuget delete` commands do not support parameters.
>
{style="note"}

The following sample `.csproj` file defines two custom MSBuild [targets](https://learn.microsoft.com/en-us/visualstudio/msbuild/target-element-msbuild?view=vs-2022):

```XML
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <PropertyGroup>
        <OutputZipFile>project.zip</OutputZipFile>
        <OutputUnzipDir>unzipped</OutputUnzipDir>
    </PropertyGroup>

    <Target Name="Zip">
        <ItemGroup>
            <FilesToZip Include="project.proj*" />
        </ItemGroup>
        <Exec Command="dir" />
        <Microsoft.Build.Tasks.Message Text="##teamcity[progressMessage 'Archiving files to $(OutputZipFile) file...']"/>
        <Exec Command="PowerShell -command Compress-Archive @(FilesToZip, ',') $(OutputZipFile) -Force" />
    </Target>
    <Target Name="Unzip">
        <Microsoft.Build.Tasks.Message Text="##teamcity[progressMessage 'Unzipping files to $(OutputUnzipDir) folder...']"/>
        <Exec Command="PowerShell -command Expand-Archive $(OutputZipFile) -DestinationPath $(OutputUnzipDir) -Force" />
    </Target>
</Project>
```

</tab>

<tab title="Maven">

To reference a parameter value in Maven and Ant, use the `${parameterName}` syntax.

```XML
<configuration>
    <tasks>
        <property environment="env"/>
        <echo message="TEMP = ${env.TEMP}"/>
        <echo message="TMP = ${env.TMP}"/>
        <echo message="java.io.tmpdir = ${java.io.tmpdir}"/>
        <echo message="build.number = ${build.number}"/>
    </tasks>
</configuration>
```

</tab>


<tab title="Ant">

To reference a parameter value in Maven and Ant, use the `${parameterName}` syntax.

```XML
<target name="buildmain">
    <ant dir="${teamcity.build.checkoutDir}" antfile="${teamcity.build.checkoutDir}/build-test.xml" target="masterbuild_main"/>
</target>
```

</tab>


<tab title="Gradle">

For [Gradle](gradle.md) runner, TeamCity system properties can be accessed as native Gradle properties (those defined in the `gradle.properties` file). If the property name is allowed as a Groovy identifier (does not contain dots), use the following syntax:

```Shell
println "Custom user property value is ${customUserProperty}"
```

Otherwise, if the property has dots in its name (for example, `build.vcs.number.1`), use the `project.ext["build.vcs.number.1"]` syntax instead.

</tab>

</tabs>


</procedure>


<!--
### Pass Values to Simple Script Runners

You can insert references to parameters as `%\parameter_name%` when writing scripts for [](command-line.md), [](c-script.md), and [](python.md) runners.

> Note that these runners resolve parameter values only if scripts are written directly in runner settings pages in TeamCity UI. If you include parameter references in external script files, TeamCity will not replace these references with parameter values.
> 
{style="note"}


<table><tr><td>

<tabs>

<tab title="CLI Runner">

The script below prints the [checkout directory](build-checkout-directory.md) path (configuration parameter) and TeamCity server version (environment variable) to the build log.<br/><br/>

```Shell
echo "Checkout directory: %teamcity.build.checkoutDir%"
echo "Server version: '$TEAMCITY_VERSION'"
```
{ignore-vars="true"}

The script below uses a reference to the build branch to obtain a file that should be copied to the target directory.<br/><br/>

```Shell
set -e -x

FILE=%teamcity.build.branch%/fileToCopy.xml
if test -f "$FILE"; then
    cp "$FILE" folderA/folderB
fi
```

This sample script sends the REST API request to download the "libraries.tar.gz" archive from the server (whose URL is stored as the `serverURL` config parameter), add a build number to its name, and save it to the checkout directory. For example, the name of the archive from build #54 will be "libraries_54.tar.gz".<br/><br/>

```Shell
curl -o libraries_%build.number%.tar.gz %serverUrlBase%libraries.tar.gz
```

</tab>


<tab title="Python Runner">


The following script prints the [checkout directory](build-checkout-directory.md) path (configuration parameter) and TeamCity server version (environment variable) to the build log.<br/><br/>

```Python
print(f'Current checkout directory is: %teamcity.build.checkoutDir%')
print(f'TeamCity version is: %env.TEAMCITY_VERSION%')
# or
print(f"TeamCity version is: {os.environ['TEAMCITY_VERSION']}")
```
{ignore-vars="true"}

</tab>

<tab title="C# Script Runner">

The following script obtains the [checkout directory](build-checkout-directory.md) path and appends an additional path to it:<br/><br/>

```C#
string fullPath = Path.Combine("%teamcity.build.checkoutDir%", "myFolder/bin");
Console.WriteLine(fullPath);
```
{ignore-vars="true"}

To get values of environment variables, use the [Environment.GetEnvironmentVariable](https://learn.microsoft.com/en-us/dotnet/api/system.environment.getenvironmentvariable?view=net-7.0) method:<br/><br/>

```C#
Console.WriteLine("Predefined variable value = " + System.Environment.GetEnvironmentVariable("TEAMCITY_VERSION"));
Console.WriteLine("Custom variable value = " + System.Environment.GetEnvironmentVariable("My.Custom.Env.Variable"));
```

You can also add parameter references in the `%\parameter_name%` format to the **Script parameters** field of the runner. These parameters will then be available from the global `Args` array.

See this blog post for an example of using parameters in C# scripts and .NET runner: [How to automate CI/CD tasks with C# Scripting in TeamCity](https://blog.jetbrains.com/teamcity/2021/11/how-to-automate-ci-cd-tasks-with-c-scripting-in-teamcity/).

</tab>

</tabs>

</td></tr></table>

-->

<!--

#### Share Values Between Steps

You can use parameters to pass simple data from one step/script to another. To do this, send the `setParameter` [service message](service-messages.md) from a script that calculates new parameter values.

```Shell
echo "##teamcity[setParameter name='myParam1' value='TeamCity Agent %teamcity.agent.name%']"
```

<snippet id="change-parameter-from-build">

In the following configuration, a C# script checks the current day of the week and writes it to the `day.of.week` parameter. A subsequent Python runner then uses the updated parameter value.

```Kotlin
object MyBuildConf : BuildType({
    params {
        param("day.of.week", "Monday")
    }
    steps {
        csharpScript {
            name = "Check the current day"
            content = """
            if ("%day.of.week%" != DateTime.Today.DayOfWeek.ToString()) {
              string today = DateTime.Today.DayOfWeek.ToString();
              string TCServiceMessage = "##teamcity[setParameter name='day.of.week' value='" + today + "']";
              Console.WriteLine(TCServiceMessage);
            }
        """.trimIndent()
        }
        python {
            name = "Welcome message"
            command = script {
                content = "print('Hello %teamcity.build.triggeredBy.username%, today is %day.of.week%!')"
            }
        }
    }
})
```

</snippet>

-->


<anchor id="Using+Build+Parameters+in+Build+Scripts"/>

<!--

### Pass Values to Builders' Configuration Files

[.NET](net.md), [Maven](maven.md), [Gradle](gradle.md), [Ant](ant.md) and [NAnt](nant.md) runners allow you to reference TeamCity parameters in build configuration files. This technique allows you to pass the required values to build processes.

> Parameters used in this scenario should start with either `env.` or `system.` prefixes but referenced without these prefixes. For example, use `${build.number}` in Maven configuration files to reference the predefined `system.build.number` parameter.
> 
{style="warning"}

<tabs>


<tab title=".NET">

In .NET, pass parameter values using the `$(<parameter_name>)` syntax.

> * MSBuild does not support names with dots (.), so you need to replace dots with underscores ("_") when using the parameter inside a build script.
> * The `nuget push` and `nuget delete` commands do not support parameters.
>
{style="note"}

The following sample `.csproj` file defines two custom MSBuild [targets](https://learn.microsoft.com/en-us/visualstudio/msbuild/target-element-msbuild?view=vs-2022):

```XML
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <PropertyGroup>
        <OutputZipFile>project.zip</OutputZipFile>
        <OutputUnzipDir>unzipped</OutputUnzipDir>
    </PropertyGroup>

    <Target Name="Zip">
        <ItemGroup>
            <FilesToZip Include="project.proj*" />
        </ItemGroup>
        <Exec Command="dir" />
        <Microsoft.Build.Tasks.Message Text="##teamcity[progressMessage 'Archiving files to $(OutputZipFile) file...']"/>
        <Exec Command="PowerShell -command Compress-Archive @(FilesToZip, ',') $(OutputZipFile) -Force" />
    </Target>
    <Target Name="Unzip">
        <Microsoft.Build.Tasks.Message Text="##teamcity[progressMessage 'Unzipping files to $(OutputUnzipDir) folder...']"/>
        <Exec Command="PowerShell -command Expand-Archive $(OutputZipFile) -DestinationPath $(OutputUnzipDir) -Force" />
    </Target>
</Project>
```

</tab>

<tab title="Maven">

To reference a parameter value in Maven and Ant, use the `${parameterName}` syntax.

```XML
<configuration>
    <tasks>
        <property environment="env"/>
        <echo message="TEMP = ${env.TEMP}"/>
        <echo message="TMP = ${env.TMP}"/>
        <echo message="java.io.tmpdir = ${java.io.tmpdir}"/>
        <echo message="build.number = ${build.number}"/>
    </tasks>
</configuration>
```

</tab>


<tab title="Ant">

To reference a parameter value in Maven and Ant, use the `${parameterName}` syntax.

```XML
<target name="buildmain">
    <ant dir="${teamcity.build.checkoutDir}" antfile="${teamcity.build.checkoutDir}/build-test.xml" target="masterbuild_main"/>
</target>
```

</tab>


<tab title="Gradle">

For [Gradle](gradle.md) runner, TeamCity system properties can be accessed as native Gradle properties (those defined in the `gradle.properties` file). If the property name is allowed as a Groovy identifier (does not contain dots), use the following syntax:

```Shell
println "Custom user property value is ${customUserProperty}"
```

Otherwise, if the property has dots in its name (for example, `build.vcs.number.1`), use the `project.ext["build.vcs.number.1"]` syntax instead.

</tab>

</tabs>

-->


## Parameter Sources

All TeamCity parameters can be categorized in two main groups: predefined and custom (created by TeamCity users). Custom parameters can be declared on multiple levels, including individual projects, build configurations, and agent machines.


<deflist type="full">

<def title="Predefined parameters">

TeamCity exposes multiple predefined parameters that you can reference in your build workflows. For example, the `teamcity.agent.work.dir.freeSpaceMb` parameter reports the total free space on this particular build agent, and `DotNetCLI_Path` parameter returns the .NET CLI installation path.

See this article for more information: [](predefined-build-parameters.md).

</def>


<def title="Custom template, project, configuration, and pipeline parameters">

These parameters are created by users inside project, configuration, and pipeline settings. In certain cases, TeamCity creates them automatically. For example, if your [CLI step](command-line.md) runs the `echo %\MyParam%` command but `MyParam` does not exist, every build will fail. TeamCity recognizes this as a misconfiguration and does not run new builds unless you provide a value for this missing parameter. In other words, the presence of a missing parameter becomes an **implicit requirement** for new builds.

See [](typed-parameters.md) and [](configuring-agent-requirements.md#Implicit+Requirements) for more information.

</def>


<def title="Custom agent parameters">

You can manually declare parameters inside [agent configuration files](configure-agent-installation.md) (`<AGENT_HOME>/conf/buildAgent.properties`). For example, the following sample demonstrates how to implement a custom build agents' ranking system:

```
# An agent's "buildAgent.properties" files

######################################
#   Default Build Properties         #
######################################
# ...
agent.tier=Platinum
# ...
```

This custom agent rank can then be employed in [agent requirements](configuring-agent-requirements.md):


```Kotlin
object Build : BuildType({
    name = "My build config"
    requirements {
        equals("agent.tier", "Platinum")
    }
})
```

</def>


<def title="Custom build parameters">

Users who trigger [custom builds](running-custom-build.md) can override existing parameter values and add new parameters on a corresponding dialog tab.

<img src="dk-custom-run-new-parameter.png" width="706" alt="Add new parameters in custom run dialog"/>

For example, if you do not [specify the Java version](maven.md#Java+Parameters) for a [Maven build step](maven.md), it uses the agent’s default Java defined by the `JAVA_HOME` environment variable. To override it, add the `env.JAVA_HOME` parameter in the Custom Build Run dialog and set it to an existing agent parameter such as `%\env.JDK_21_0_ARM64%`.

> This scenario should be relatively rare. If you need to switch Java versions (or other agent tools) frequently, consider creating a build configuration parameter and [pre-filling it with supported values](#parameter-use-cases-parameterize-scripts).
> 
{style="tip"}

</def>


<def title="Dynamically created custom parameters">

Print the `##teamcity[setParameter name='foo' value='bar']` [service message](service-messages.md#set-parameter) to the build log to update an existing or add a new parameter.

</def>

</deflist>


## Parameter Values

TeamCity parameters can obtain their values from one or multiple sources listed below.

* Values from a template selected as the [enforced settings template](build-configuration-template.md#Enforcing+settings+inherited+from+template). These values cannot be disabled or overridden by users.

* The **Parameters** tab of the [Run Custom Build](running-custom-build.md) dialog.

* Custom values assigned to a parameter in build configuration or pipeline settings.

* Custom values assigned to a parameter parent project settings. Parameters defined within a project are inherited by all its child entities.

* Values specified in a regular [build configuration template](build-configuration-template.md).

* Values specified in a build agent's [configuration file](configure-agent-installation.md) (the `<AGENT_HOME>/conf/buildAgent.properties` file).

* Values reported by an agent when it connects to the TeamCity server. These values are passed to parameters that describe the agent environment. For example, the `DotNetCoreSDK7.0_Path` parameter that stores the path to .NET 7 SDK on this specific agent.

* Values of predefined build parameters. These parameters can collect their values on a server side in the scope of a specific build (for example, the `build.number` parameter), or on the agent side right before a build starts (for example, the `teamcity.agent.work.dir.freeSpaceMb` parameter).


The list above also ranges parameter value sources by priority, from highest to lowest. That is, if the same parameter retrieves different values from different sources, a value from the topmost source in this list is applied. For example, if the `my.parameter` is defined inside an agent configuration file and inside a build configuration, the value from the configuration settings page wins.

### Override Parameter Values During a Build

Initial parameter values can be overridden by sending the `##teamcity[setParameter name='foo' value='bar']` [service message](service-messages.md#set-parameter). Note that parameters modified in such manner update their values only in the scope of the current build or build chain. To **permanently** override a parameter value, send the REST API request from your build step like shown below:

```Kotlin
import jetbrains.buildServer.configs.kotlin.*
import jetbrains.buildServer.configs.kotlin.buildSteps.script

object UpdateBuildVersion : BuildType({
name = "Update Build Version"

    steps {
        script {
            id = "simpleRunner"
            scriptContent = """
                version=%\build.version%
                ((version=version+1))
                
                curl --location --request PUT 'http://<server_URL>/app/rest/projects/<project_name>/parameters/build.version' \
                --header 'Accept: */*' \
                --header 'Content-Type: text/plain' \
                --header 'Authorization: Bearer your_token' \
                --data ${'$'}version
            """.trimIndent()
        }
    }
})
```


### Get Parameter Value From a Remote Source

To hide sensitive data from both TeamCity UI and build logs (for example, login credentials and access tokens), use [password parameters](typed-parameters.md#Create+a+Secret) that mask their values. To secure critical values even further, store them in a third-party vault and create a TeamCity parameter of the **remote secret** type. These parameters have no explicit "value" part. Instead, they store a query that TeamCity runs whenever it needs to resolve the parameter reference.

Currently, only HashiCorp Vault is supported as a remote secret storage. See this article for more information: [](hashicorp-vault.md).


### Track Parameter Values

When a build finishes, you can check out all parameters that were present during this build on the **Parameters** tab of the [Build Results page](build-results-page.md). TeamCity highlights new parameters and those whose values changed during the build.

<img src="dk-params-newAndUpdated.png" width="706" alt="Build parameters report"/>

To check initial and actual parameter values of the specific build via [REST API](teamcity-rest-api.md), send GET requests to the `/app/rest/builds/[{buildLocator}](https://www.jetbrains.com/help/teamcity/rest/buildlocator.html)` endpoint and specify required payload fields according to the [Build schema](https://www.jetbrains.com/help/teamcity/rest/build.html).


* `/app/rest/builds/{buildLocator}?fields=originalProperties(*)` — returns user-defined parameters from the build configuration and their default values.
* `/app/rest/builds/{buildLocator}?fields=startProperties(*)` — returns all parameters reported by an agent and their values at the time the build started.
* `/app/rest/builds/{buildLocator}?fields=resultingProperties(*)` — returns all parameters reported by an agent and their values by the time the build finished.

You can also check initial and final values of the specific parameter. To do this, specify the name of the target parameter:

```Shell
curl -L \
  https:<SERVER_URL>/app/rest/builds/<BUILD_LOCATOR>?fields=\
    originalProperties($locator(name:(value:(myParam),matchType:matches)),property),\
    startProperties($locator(name:(value:(myParam),matchType:matches)),property),\
    resultingProperties($locator(name:(value:(myParam),matchType:matches)),property)
```






<seealso>
        <category ref="admin-guide">
            <a href="predefined-build-parameters.md">Predefined Build Parameters</a>
            <a href="typed-parameters.md">Typed Parameters</a>
            <a href="configuring-agent-requirements.md">Configuring Agent Requirements</a>
        </category>
</seealso>