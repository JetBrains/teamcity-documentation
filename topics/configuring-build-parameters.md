[//]: # (title: Configuring Build Parameters)
[//]: # (auxiliary-id: Configuring Build Parameters)
[//]: # (Internal note. Do not delete. "Configuring Build Parametersd72e3.txt")

Rename to "TeamCity Parameters" and move to "Managing Projects"?

In TeamCity, you can utilize predefined and custom build parameters. Parameters are `name=value` pairs that can be referenced throughout TeamCity to avoid using plain values.

There are two major parameter types in TeamCity:

* **Configuration Parameters** — parameters whose primary objective is to share settings within a build configuration. You can also use these parameters to customize a configuration that is based on a [template](build-configuration-template.md) or uses a [meta-runner](working-with-meta-runner.md). Parameters of this type are not passed into a build process (that is, not accessible by a build script engine).

* **Environment Variables** — parameters that start with the `env.` prefix that are passed to the process of a build runner similarly to default env variables of a system.

See the following sections to better understand where you can use parameters.

## Main Use Cases

### Substitute Plain Values

Parameters allow you to avoid using plain values in TeamCity UI. Instead, you can reference a parameter by its name using the `%\NAME%` syntax. You can do this to:

* store frequently used values;
* store sensitive information that should not be visible to regular TeamCity developers;
* improve readability of your configurations by using shorter parameter names instead of full values, and so on.

<tabs>

<tab title="Example 1 — Docker">

Create a custom parameter for the **&lt;Root&gt;** project to save the path for your default Docker registry (parameters declared for the **&lt;Root&gt;** project are available inside all child projects):

<img src="dk-params-newParameter.png" alt="Create new parameter in TeamCity" width="706"/>

You can then reference this parameter in all required [Docker runners](docker.md) that pull or push your images.

<img src="dk-params-reuseParameter.png" width="706" alt="Use a custom parameter in TeamCity"/>

</tab>

<tab title="Example 2 — JDK version">

The code below forces the [](gradle.md) runner to use the specific version of agent JDK instead of the default one. The parameter allows you to avoid using the exact path (which may vary for different build agents).

```Kotlin
steps {
    gradle {
        name = "Gradle step"
        tasks = "build-dist"
        jdkHome = "%\env.JDK_19_0_ARM64%"
    }
}
```

</tab>

<tab title="Example 3 — .NET Parameter">

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

You can achieve the same result even faster by adding the "system." prefix to your parameter. Since parameters that start with "system." are automatically passed to a build engine, you can create the `system.dotnet.output.type` parameter with the "OutputType=WinExe" value. In this case, the response (.rsp) file with .NET settings will have this value, which means you can skip setting the "Command line parameters" field in TeamCity.

</tab>
</tabs>





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

To set up step execution conditions in TeamCity UI, go to step settings and click **Add condition | Other condition...**.

<img src="dk-params-StepExecutionCondition.png" width="706" alt="Step execution condition"/>

See this section for the example: [](#Customize+Template-Based+Configurations).

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
> <condition property="javadocPath" value="${env.JDK_11}/bin/javadoc" >
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



### Custom Build Numbers and Tags

Custom build number: Conf Settings | General Settings | Build Number format

Only predefined server build parameters

UI:

Screenshot here

Kotlin:

??? The if not env. variable, then it's system.teamcity.buildConfName, and we're hiding system — OK to use env here ???

```Kotlin
object MyBuildConf : BuildType({
    buildNumberPattern = "%\build.counter%-%\env.TEAMCITY_BUILDCONF_NAME%"
})
```

Labeling: Conf settings | Build Features | Add | VCS Labeling

UI:

Screenshot here

Kotlin:

```Kotlin
object MyBuildConf : BuildType({
    features {
        vcsLabeling {
            vcsRootId = "${UrlRefsHeadMaster.id}"
            labelingPattern = "TC-build-%\build.number%"
        }
    }
})
```


### VCS Root and Checkout Rule Settings

VCS root settings - tried creating the new "branch.default" parameter that equals "refs/heads/master" and referencing this parameter in the "Default branch". Fires the "Test connection failed in Parameters Test / Gradle
Cannot find revision of the default branch '%\branch.default%'" error.

Example: Custom Path to git in VCS Root settings

UI: Screenshot

`Path to git = %\CustomMyGitPath%`

Kotlin:

```Kotlin
object MyBuildConf : BuildType({
    params {
        param("env.CustomMyGitPath", "path/to/required/git/version")
    }

    // ??? No vcs root settings visible

})
```

Checkout rules:

UI: Build Conf Settings | Version Control Settings | Edit Checkout Rules

Kotlin:

```Kotlin
object GoalInBuildScripts : BuildType({
    params {
        param("branch.ignored", "refs/heads/ignored")
        param("branch.default", "refs/heads/master")
    }

    vcs {
        root(HttpsGithubComValrravnGradleSampleAppRefsHeadsMaster, "+:%\branch.default%", "-:%\branch.ignored%")
    }
})
```


### Artifact Rules

UI: Build Conf Settings | General Settings | Artifact paths

Kotlin:

```Kotlin
object GoalInBuildScripts : BuildType({
    artifactRules = "testfile1.txt => %\master.artifact.name%"
    params {
        param("master.artifact.name", "master_artifact.tar.gz")
    }
})
```





## OLD

_Build parameters_ are name-value pairs, defined by a user or provided by TeamCity, which can be used in a build. They help flexibly share settings and pass them to build steps.

This article explains how to configure build parameters. See how to [use them inside build settings and build scripts](using-build-parameters.md).

## Types of Build Parameters

There are three types of build parameters in TeamCity:
* [Environment variables](#Environment+Variables)
* [System properties](#System+Properties)
* [Configuration parameters](#Configuration+Parameters)

<anchor name="ConfiguringBuildParameters-BuildParameters"/>

### Environment Variables

Environment variables can be passed into a spawned build process as into an environment.

They are defined by the `env.` prefix.

### System Properties

System properties can be passed into build scripts of [certain runners](using-build-parameters.md#Using+Build+Parameters+in+Build+Scripts) as variables specific to a build tool.

They are defined by the `system.` prefix.

<anchor name="ConfiguringBuildParameters-ConfigurationParameters"/>

### Configuration Parameters

Configuration parameters are not passed into a build process and are only meant to share settings within a build configuration. They are the primary means for customizing a build configuration which is based on a [template](build-configuration-template.md) or uses a [meta-runner](working-with-meta-runner.md).

They come with no prefix.

<anchor name="parameter-reference"/>

## Parameter Name Restrictions

The names of configuration parameters must contain only the `[a-zA-Z0-9._-*]` characters and start with an ASCII letter.

## Predefined Build Parameters

TeamCity provides a set of _predefined parameters_ that can be used within a build configuration settings or directly inside build steps. For example, you can access the current build's number by calling the respective parameter generated by TeamCity. See the [list of predefined parameters](predefined-build-parameters.md) for details.

## Custom Build Parameters

In __Build Configuration Settings | Parameters__, project administrators can define build parameters for the current build configuration. As soon as a new build starts in this configuration, TeamCity passes these parameters to its build scripts and environment.

>It is possible to redefine the parameters' values in a single build run by launching a [custom build](running-custom-build.md).

Build parameters defined in a build configuration are used only within this configuration. See how to define them on a [project or agent level](levels-and-priority-of-build-parameters.md).

Any user-defined build parameter (<emphasis tooltip="system-property">system property</emphasis> or <emphasis tooltip="environment-variable">environment variable</emphasis>) can reference other parameters as follows: `%\system.parameter_name%` or `%\env.parameter_name%`. For example, `system.tomcat.libs=%\env.CATALINA_HOME%/lib/*.jar`.  
Read more about parameter references [below](#Parameter+References).

You can also configure a [parameter's type](typed-parameters.md), so the parameter is displayed as a UI field in the _Run Custom Build_ dialog. This way, users will be able to use the UI dialog to quickly change the parameter's value in the next build run.

## Parameter References

Most text-field settings in TeamCity support referencing a build parameter as a variable. If you enter a string in the `%\parameter.name%` format, TeamCity will substitute it with the actual value during the build.

If a build references a parameter which is not defined, TeamCity will consider it an [implicit agent requirement](agent-requirements.md#Implicit+Requirements): the build will only run on the agents where this parameter is defined.  
The references to parameters which names do not satisfy the [above restrictions](#Parameter+Name+Restrictions) do not create an [implicit requirement](agent-requirements.md#Implicit+Requirements) and are ignored.

See [where you can use parameter references](using-build-parameters.md#Where+References+Can+Be+Used).


## Check Parameter Values After a Build

When a build finishes, open the **Parameters** tab on the [](build-results-page.md) to view actual (at the time of this build) values for all parameters.

<chunk id="build-results-parameters-tab">

<img src="dk-buildParametersTab.png" width="706" alt="Build parameters tab"/>

This page has two tabs:

* **Parameters** — lists values for all configuration parameters, system properties, and environment variables. Highlights parameters that were added/modified in [custom builds](running-custom-build.md).
* **Statistic values** — lists all [statistics values](custom-chart.md#Default+Statistics+Values+Provided+by+TeamCity) reported for the build (for example, build success rate or time required to check out a remote repository). The *View Chart* button (<img src="dk-viewChart.png" width="12" alt="View Chart"/>) allows you to check how these values trend throughout build runs.

</chunk>

<seealso>
        <category ref="admin-guide">
        <a href="using-build-parameters.md">Using Build Parameters</a>
            <a href="levels-and-priority-of-build-parameters.md">Project- and Agent-Level Build Parameters</a>
            <a href="predefined-build-parameters.md">Predefined Build Parameters</a>
            <a href="typed-parameters.md">Typed Parameters</a>
            <a href="configuring-agent-requirements.md">Configuring Agent Requirements</a>
        </category>
</seealso>