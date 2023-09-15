[//]: # (title: Configuring Build Parameters)
[//]: # (auxiliary-id: Configuring Build Parameters)
[//]: # (Internal note. Do not delete. "Configuring Build Parametersd72e3.txt")

In TeamCity, you can utilize predefined and custom build parameters. Parameters are `name=value` pairs that can be used instead of plain values.

There are three major parameter types in TeamCity:

* **Configuration Parameters** — parameters whose primary objective is to share settings within a build configuration. You can also use these parameters to customize a configuration that is based on a [template](build-configuration-template.md) or uses a [meta-runner](working-with-meta-runner.md). Parameters of this type are not passed into a build process (that is, not accessible by a build script engine).

* **Environment Variables** — parameters that start with the `env.` prefix. These parameters are passed to the process of a build runner similarly to default env variables of a system.

* **System Properties** — parameters that start with the `system.` prefix. These parameters can be passed to configuration files of [certain runners](#Pass+Values+to+Builders%27+Configuration+Files) as variables specific to a build tool.

<anchor name="Parameter+References"/>

## Common Information

Parameters allow you to avoid using plain values in TeamCity UI and build scripts. Instead, you can reference a parameter by its name using the `%\<parameter_name>%` syntax. In addition, parameters can reference other parameters in their values (for example, `system.tomcat.libs=%\env.CATALINA_HOME%/lib/*.jar`).

Storing values in parameters allows you to:

* quickly reuse frequently used values;

* create reusable [templates](build-configuration-template.md) and [meta-runners](working-with-meta-runner.md) whose parameter values are overridden in target configurations;

* add flexibility to your build configurations: parameter values can be quickly altered in TeamCity UI, via the [service message](service-messages.md) sent during a build, or in the [Run Custom Build dialog](running-custom-build.md);

* hide sensitive information that should not be visible to regular TeamCity developers;

* improve readability of your configurations by using shorter parameter references instead of full values, and so on.

The following examples illustrate different cases where you might opt for parameters instead of plain values. For other scenarios, see the [](#Main+Use+Cases) section.

### Store a Docker Registry Name
{initial-collapse-state="collapsed"}

If you have various configurations that utilize the same image registry, you can create a [custom parameter](custom-parameters.md) for the **&lt;Root&gt;** project to store this registry's name. Then you can reference this parameter in any [Docker step](docker.md) that pulls or pushes your images.

> Parameters declared inside the **&lt;Root&gt;** project are available in all TeamCity build configurations.
> 
{style="tip"}

<img src="dk-params-reuseParameter.png" width="706" alt="Use a custom parameter in TeamCity"/>

### Specify the JDK Version
{initial-collapse-state="collapsed"}

The following [](gradle.md) step always uses JDK 19 instead of the default version referenced by the `JDK_HOME` environment variable. The runner retrieves a path for this required JDK from the corrensponding `env.` parameter.

```Kotlin
steps {
    gradle {
        name = "Gradle step"
        tasks = "build-dist"
        jdkHome = "%\env.JDK_19_0_ARM64%"
    }
}
```

### Set Additional .NET Parameters
{initial-collapse-state="collapsed"}

In the following sample, the "Command line parameters" field of the [](net.md) runner references the `dotnet.output.type` parameter.

<img src="dk-params-additionalSettings.png" width="706" alt="Additional parameters"/>

<br/>

```Kotlin
object MyBuildConfig : BuildType({
    params {
        param("dotnet.output.type", "WinExe")
    }

    steps {
        dotnetBuild {
            args = "-p:OutputType=%\dotnet.output.type%"
        }
    }
})
```


> You can achieve the same result even faster by creating the `system.dotnet.output.type` parameter with the `OutputType=WinExe` value. This value will be automatically written to the response (.rsp) file with .NET settings, so you do not need to set the **Command line parameters** field in TeamCity UI.
> 
> This approach is based on th`system.` prefix to your parameter. Parameters that start with "system." are automatically passed to a build engine. This means you can create the `system.dotnet.output.type` parameter with the `OutputType=WinExe` value and it will be written to the response (.rsp) file with .NET settings. As a result, you can skip setting the **Command line parameters** field in TeamCity.
> 
{type="tip"}

### Specify Artifact Paths
{initial-collapse-state="collapsed"}

When setting artifacts paths on the **Build Configuration Settings | General Settings | Artifact paths** page, you can utilize custom configuration parameters to substitute plain values.

```Kotlin
object GoalInBuildScripts : BuildType({
    artifactRules = "testfile1.txt => %\master.artifact.name%"
    params {
        param("master.artifact.name", "master_artifact.tar.gz")
    }
})
```


### Modify the Build Numbering Pattern
{initial-collapse-state="collapsed"}


The **Build Configuration Settings | General Settings | Build Number Format** field allows you to customize the numbering pattern for builds of this configuration.



The default zero-based integer index of a build can be retrieved via the `build.counter` parameter. The sample below adds the name of a TeamCity user who triggered this build to this default index.


```Kotlin
object MyBuildConf : BuildType({
    buildNumberPattern = "%\build.counter%-%\teamcity.build.triggeredBy.username%"
})
```

### Label Builds
{initial-collapse-state="collapsed"}

Navigate to **Build Configuration Settings | Build Features** page and click **Add | VCS Labeling** to [tag build sources](vcs-labeling.md) in your VCS. 


<img src="dk-params-vcs-labeling.png" width="706" alt="VCS Labeling with Parameters"/>

<br/>

```Kotlin
object MyBuildConf : BuildType({
    params {
        param("release.status", "EAP")
    }
    features {
        vcsLabeling {
            vcsRootId = "${DslContext.settingsRoot.id}"
            labelingPattern = "%release.status%"
        }
    }
})
```


### Specify Checkout Rules
{initial-collapse-state="collapsed"}

[](vcs-checkout-rules.md) can be configured on the **Build Configuration Settings | Version Control Settings** page. If your organization has a certain convention for branch names, you can store these default names as parameters and specify checkout rules as shown below.

Kotlin:

```Kotlin
object GoalInBuildScripts : BuildType({
    params {
        param("branch.ignored", "refs/heads/sandbox")
        param("branch.default", "refs/heads/master")
    }

    vcs {
        root(YourVcsRootName, "+:%\branch.default%", "-:%\branch.ignored%")
    }
})
```


## Main Use Cases

### Customize Template-Based Configurations

Configuration parameters allow you to create a base build configuration with parameters, extract it to the template, and override these parameters in configurations based on this template.

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

If now you extract a [template](build-configuration-template.md) from this configuration, you can duplicate this configuration and override the `skip.optional.step` parameter for those instances that do not need this optional step #2.

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

See the following section to learn more about step execution conditions: [](#Specify+Step+Execution+Conditions).


### Pass Values to Meta-Runners

A [meta-runner](working-with-meta-runner.md) allows you to extract build steps, requirements, and parameters from a build configuration and create a custom build runner out of them.

See this article for the example of an Ant-based meta-runner that utilizes the custom `system.artifact.paths` parameter to publish artifacts via a corresponding [service message](service-messages.md): [](working-with-meta-runner.md#Preparing+Build+Configuration).


### Specify Step Execution Conditions

You can define [step execution conditions](build-step-execution-conditions.md) to specify whether individual steps should be run. You can use any parameters for these conditions (configuration parameters and environment variables, both custom and predefined).

To set up step execution conditions in TeamCity UI, go to step settings and click **Add condition | Other condition...**

<img src="dk-params-StepExecutionCondition.png" width="706" alt="Step execution condition"/>

For example, you can run different shell scripts depending on the type of build agent's operating system.

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
        powerShell {
            name = "Copy File (Windows)"
            conditions {
                startsWith("teamcity.agent.jvm.os.name", "Windows")
            }
            scriptMode = script {
                content = """Copy-Item "%\system.teamcity.build.workingDir%/result.xml" -Destination %\win.destination.path%"""
            }
        }
        script {
            name = "Copy File (Unix)"
            executionMode = BuildStep.ExecutionMode.RUN_ON_FAILURE

            conditions {
                doesNotContain("teamcity.agent.jvm.os.name", "Windows")
            }
            scriptContent = """cp "%\system.teamcity.build.workingDir%/result.xml" 
              %\unix.destination.path%""".trimIndent()
        }
    }
})
```

### Specify Agent Requirements

[](agent-requirements.md) allow you to specify `parameter-operator-value` conditions. Only those agents that meet these conditions are allowed to build this build configuration.

You can define agent requirements using only those parameters whose values can be reported by agents before the build starts. These parameters are:

* Predefined configuration parameters available for all agents (for example, `teamcity.agent.name`).
* Environment variables reported by agents (for example, `env.DOTNET_SDK_VERSION`).
* Custom configuration parameters that are present in agents' [buildAgent.properties](configure-agent-installation.md) files (for example, create a `custom.agent.parameter` in TeamCity UI and add the `custom.agent.parameter=SomeValue` line to agents' properties files).

TeamCity automatically adds agent requirements depending on the configured build steps. For example, if a build step should be executed inside a Linux container, TeamCity adds requirements that specify an agent must have [either Docker or Podman](integrating-teamcity-with-container-managers.md) running on Linux machine.

To define custom agent requirements, navigate to the **Administration | &lt;Build Configuration&gt; | Agent Requirements** tab.

<img src="dk-params-AgentRequirements.png" width="706" alt="Set agent requirements in TeamCity UI"/>

To define agent requirements in [](kotlin-dsl.md), use the `requirements` block of you build configuration.

```Kotlin
import jetbrains.buildServer.configs.kotlin.*

object BuildConfig : BuildType({
    name = "Build Config"
    steps { 
        // Build steps
    }

    requirements {
        // Requirements in the following format:
        // Operator("ParameterName", "ParameterValue")
    }
})
```

For example, the following condition allows only Mac agents to run builds for the parent build configuration.

```Kotlin
matches("teamcity.agent.jvm.os.family", "Mac OS")
```
{product="tcc"}

```Kotlin
startsWith("teamcity.agent.jvm.os.name", "Mac")
```
{product="tc"}

The sample condition below allows builds to run only on machines with "Android" workload installed for .NET 7 SDK:

```Kotlin
contains("DotNetWorkloads_7.0", "android")
```

The following condition requires that an agent running builds has either Docker or Podman:

```Kotlin
exists("container.engine")
```





### Pass Values to Simple Script Runners

You can insert references to parameters as `%\parameter_name%` when writing scripts for [](command-line.md), [](c-script.md), and [](python.md) runners.

> Note that this technique works when you write scripts directly on the TeamCity runner settings page. If you include parameter references in external script files executed by these runners, these references will not be replaced with parameter values.
> 
{type="note"}


<table><tr><td>

<tabs>

<tab title="CLI Runner">

The following script prints the [checkout directory](build-checkout-directory.md) path (configuration parameter) and TeamCity server version (environment variable) to the build log.<br/><br/>

```Shell
echo "Checkout directory: %\teamcity.build.checkoutDir%"
echo "Server version: '$TEAMCITY_VERSION'"
```

The script below uses a reference to the build branch to obtain a file that should be copied to the target directory.<br/><br/>

```Shell
set -e -x

FILE=%\teamcity.build.branch%/fileToCopy.xml
if test -f "$FILE"; then
    cp "$FILE" folderA/folderB
fi
```

This sample script sends the REST API request to download the "libraries.tar.gz" archive from the server (whose URL is stored as the `serverURL` config parameter), add a build number to its name, and save it to the checkout directory. For example, the name of the archive from build #54 will be "libraries_54.tar.gz".<br/><br/>

```Shell
curl -o libraries_%\build.number%.tar.gz %\serverUrlBase%libraries.tar.gz
```

</tab>


<tab title="Python Runner">


The following script prints the [checkout directory](build-checkout-directory.md) path (configuration parameter) and TeamCity server version (environment variable) to the build log.<br/><br/>

```Python
print(f'Current checkout directory is: %\teamcity.build.checkoutDir%')
print(f'TeamCity version is: %\env.TEAMCITY_VERSION%')
# or
print(f"TeamCity version is: {os.environ['TEAMCITY_VERSION']}")
```

</tab>

<tab title="C# Script Runner">

The following script obtains the [checkout directory](build-checkout-directory.md) path and appends an additional path to it:<br/><br/>

```C#
string fullPath = Path.Combine("%\teamcity.build.checkoutDir%", "myFolder/bin");
Console.WriteLine(fullPath);
```

To get values of environment variables, use the [Environment.GetEnvironmentVariable](https://learn.microsoft.com/en-us/dotnet/api/system.environment.getenvironmentvariable?view=net-7.0) method:<br/><br/>

```C#
Console.WriteLine("Predefined variable value = " + System.Environment.GetEnvironmentVariable("TEAMCITY_VERSION"));
Console.WriteLine("Custom variable value = " + System.Environment.GetEnvironmentVariable("My.Custom.Env.Variable"));
```

You can also add parameter references in the `%\parameter_name%` format to the **Script parameters** field of the runner. These parameters will then be available from the global `Args` array.


For example, the runner that executes the script below has the `%\nuget.package.name%` parameter added to its **Script parameters**. The C# script retrieves the NuGet package name and calculates the number of the next package version.<br/><br/>

```C#
using System.Linq
Props["version"] =
    GetService<INuGet>()
    .Restore(Args[0], "*", "net6.0"
    .Where(i => i.Name == Args[0])
    .Select(i => i.Version)
    .Select(i => new Version(i.Major,i.Minor,i.Build+1))
    .DefaultIfEmpty(new Version[1,0,0])
    .Max()
    .ToString();
Console.WriteLine($"Version number: {Props["version"]}", Success)
```
</tab>

</tabs>

</td></tr></table>

#### Share Values Between Steps

You can use parameters to pass simple data from one step/script to another. To do this, send the `setParameter` [service message](service-messages.md) from a script that calculates new parameter values.

```Shell
echo "##teamcity[setParameter name='myParam1' value='TeamCity Agent %\teamcity.agent.name%']"
```

<chunk id="change-parameter-from-build">

In the following configuration a C# script checks the current day of week and sets a corresponding value to the `day.of.week` parameter. The updated parameter value is then used by a subsequent Python runner.

```Kotlin
object MyBuildConf : BuildType({
    params {
        param("day.of.week", "Monday")
    }
    steps {
        csharpScript {
            name = "Check the current day"
            content = """
            if ("%\day.of.week%" != DateTime.Today.DayOfWeek.ToString()) {
              string today = DateTime.Today.DayOfWeek.ToString();
              string TCServiceMessage = "##teamcity[setParameter name='day.of.week' value='" + today + "']";
              Console.WriteLine(TCServiceMessage);
            }
        """.trimIndent()
        }
        python {
            name = "Welcome message"
            command = script {
                content = "print('Hello %\teamcity.build.triggeredBy.username%, today is %\day.of.week%!')"
            }
        }
    }
})
```

</chunk>

### Pass Values to Builders' Configuration Files

[Maven](maven.md), [](gradle.md), [](ant.md), [](nant.md), and [](net.md) runners allow you to reference TeamCity parameters in build configuration files. This allows you to pass required values to the build process.

> Parameters used in this scenario should start with either "env." or "system." prefix, but referenced without these prefixes. For example, the predefined `system.build.number` parameter should be referenced as `${build.number}` in Maven configuration files.
> 
{type="warning"}

> ???? Current docs say parameters are passed without "env." prefix, but I can actually find these in our space repo build files:
>
> ```XML
> <properties>
>     <test.property>${env.MY_TEST_VAR}</test.property>
>     <testProperty>${test.property}</testProperty>
> </properties>
> ```
> <br/>
> ```XML
> <echo message="${env.ENV_PARAM}" level="info"/>
> <echo message="${SYSTEM_PARAM}" level="info"/>
> ```
> <br/>
> ```XML
> <condition property="javadocPath" value="${env.JDK_11}/bin/javadoc" />
> ```
>
{type="warning"}


<tabs>

<tab title="Maven">

To reference a parameter value in Maven and Ant, use the `${parameterName}` syntax.

```XML
<!--pom.xml file-->
<build>
    <plugins>
        <plugin>
            <artifactId>maven-antrun-plugin</artifactId>
            <version>1.3</version>
            <executions>
                <execution>
                    <phase>generate-sources</phase>
                    <configuration>
                        <tasks>
                            <property environment="env"/>
                            <echo message="TEMP = ${env.TEMP}"/>
                            <echo message="TMP = ${env.TMP}"/>
                            <echo message="java.io.tmpdir = ${java.io.tmpdir}"/>
                            <echo message="build.number = ${build.number}"/>
                        </tasks>
                    </configuration>
                    <goals>
                        <goal>run</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
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

<tab title=".NET">

For .NET, use the `$(<parameter_name>)` syntax.

> * MSBuild does not support names with dots (.), so you need to replace dots with underscores ("_") when using the parameter inside a build script.
> * The `nuget push` and `nuget delete` commands do not support parameters.
>
{type="note"}

<br/>

```XML
<!--A .csproj file-->

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

<tab title="Gradle">

For [Gradle](gradle.md) runner, TeamCity properties can be accessed as Gradle properties (similar to the ones defined in the `gradle.properties` file). If the property name is allowed as a Groovy identifier (does not contain dots), use the following syntax:

```Shell
println "Custom user property value is $\{customUserProperty\}"
```

Otherwise, if the property has dots in its name (for example, `build.vcs.number.1`), use the `project.ext["build.vcs.number.1"]` syntax instead.

</tab>

</tabs>






### Share Values Between Chain Builds

TeamCity parameters allow you to exchange values between configurations of a [build chain](build-chain.md). See this help article for more information: [](use-parameters-in-build-chains.md).












<seealso>
        <category ref="admin-guide">
        <a href="using-build-parameters.md">Using Build Parameters</a>
            <a href="levels-and-priority-of-build-parameters.md">Project- and Agent-Level Build Parameters</a>
            <a href="predefined-build-parameters.md">Predefined Build Parameters</a>
            <a href="typed-parameters.md">Typed Parameters</a>
            <a href="configuring-agent-requirements.md">Configuring Agent Requirements</a>
        </category>
</seealso>