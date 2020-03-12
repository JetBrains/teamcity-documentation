[//]: # (title: Getting Started with NUnit)
[//]: # (auxiliary-id: Getting Started with NUnit)

This tutorial aims at describing the basic practices of using [NUnit 3](http://www.nunit.org/) in TeamCity. The test project and script samples can be found [here](https://github.com/JetBrains/teamcity-nunit-samples). The order of use cases is based on the number of the TeamCity features involved: the first case is the most basic, more complex cases that follow utilize a larger number of features. We recommend you familiarizing yourself with all features, finding their advantages and disadvantages, and then decide in favor of one or another.

## Installing NUnit

<include src="nunit.md" include-id="installing-nunit"/>

## Case 1. Command Line

Starting from version 3, NUnit supports TeamCity out-of-the-box, which allows running tests from the command line, preserving the basic features TeamCity-NUnit integration. NUnit automatically detects if it is run by TeamCity and, if so, switches to the integration mode.

In addition to auto-detection, the NUnit 3 console handles the special `--teamcity` argument, which also includes the integration with TeamCity. This argument can be used for the purposes of debugging, when you need to see what information NUnit sends to TeamCity: detailed test information, the order of tests, and so on.

Using the NUnit console from the command line is the simplest way to run tests. TeamCity provides the following basic features of integration for the command line:
* reporting the test run information in Build Log while the tests are executing
* reporting the test results upon the tests finish
* all the [TeamCity investigation features](investigating-and-muting-build-failures.md)

This is what the TeamCity build step to run tests from the command line looks like:

<img src="nunit-cmd.png" alt="Build step: Command Line" width="750"/>

To sum up, with this simple case you can use the basic features of the TeamCity-NUnit 3 integration. 

## Case 2. MSBuild

This use case is very similar to the previous one, but it aims at MSBuild which is the most widely used build platform for .NET projects.

TeamCity supports collecting code coverage statistics out of the box; additional plugins can be [installed](installing-additional-plugins.md) to use the [JetBrains dotTrace](https://github.com/JetBrains/teamcity-dottrace) and [JetBrains dotMemory Unit](https://github.com/JetBrains/teamcity-dotmemory) in TeamCity.

The [project file](https://github.com/JetBrains/teamcity-nunit-samples/blob/master/sample2.proj) is trivial: the [Exec](https://msdn.microsoft.com/en-us/library/x8zx72cd.aspx) task, which launches tests, is run:

<chunk include-id="msbuild-examples-nunit">


```Shell

<?xml version="1.0" encoding="utf-8"?>
<Project ToolsVersion="14.0" DefaultTargets="RunTests" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
 <Target Name="RunTests">
  <Exec IgnoreExitCode="True" Command="nunit3-console.exe Tests.dll">
 <Output TaskParameter="ExitCode" ItemName="exitCode" />
  </Exec>
 <Error Text="Error while running tests" Condition="@(exitCode) &lt; 0" />
  </Target>
</Project>

```

The NUnit console returns the number of failed tests as the positive exit code and, in case of the NUnit test infrastructure failure, as the negative exit code.

TeamCity controls the test execution progress, but the NUnit infrastructure exceptions may not allow TeamCity to collect the required information. That is why the `IgnoreExitCode="True"` attribute needs to be set, which will ignore the positive exit codes and will not interrupt the build due to several failed tests. The [Error](https://msdn.microsoft.com/en-us/library/8b08t3s4.aspx) task will stop the build in case of the test infrastructure errors for the negative exit codes.

<img src="nunit-msbuild.png" alt="Build step: MSBuild" width="750"/>

Besides the project file, you can define the MSBuild version and target platform, use profiles and other settings.

</chunk>
   
To make the build more stable, you can introduce a few changes to the project file. For example, you can define the path to the NUnit console dynamically. This path may change with the updates of the NUnit console NuGet package, but our build configuration will not require any changes. The code below gets the path to the NUnit console to the `pathToNUnitConsole` variable:

```Shell
<GetNUnitConsolePath
BaseDir="$(MSBuildProjectDirectory)\packages">
 <Output TaskParameter="PathToNUnitConsole" ItemName="pathToNUnitConsole"/>
</GetNUnitConsolePath>
```

You can dynamically list the assemblies to be tested. For example, search for them in specific directories and filter them to match a regular expression pattern:

```Shell
<ItemGroup>
 <Assemblies Include="Tests\bin\**\*.dll"/>
</ItemGroup>
 
<CreateNUnitListOfAssemblies
Assemblies="@(Assemblies)" RegexpAssemblyFilter=".*Tests.dll$">
  <Output TaskParameter="ListOfAssemblies" ItemName="listOfAssemblies"/>
</CreateNUnitListOfAssemblies>
```

The following project files contain some examples: [`sample2adv.proj`](https://github.com/JetBrains/teamcity-nunit-samples/blob/master/sample2adv.proj), [`unit-utils.targets`](https://github.com/JetBrains/teamcity-nunit-samples/blob/master/nunit-utils.targets).

## Case 3. NUnit Build Step

Note that the NUnit runner supports only [.NET Framework](https://docs.microsoft.com/en-us/dotnet/framework/get-started/overview). To run tests for [.NET Core](https://docs.microsoft.com/en-us/dotnet/framework/get-started/net-core-and-open-source) projects (and .NET Framework projects version 4.0 or later), use the .NET CLI (dotnet) build runner with the [`test`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-test) command instead. Refer to the [NUnit Support](nunit-support.md#Framework+Compatibility) page for details.

The [NUnit](nunit.md) build step is probably the simplest and yet most powerful way to launch NUnit tests in TeamCity.   
In most cases its sufficient to set only the 2 parameters: the path to the NUnit console runner and the list of assemblies to be tested.

<img src="nunit-step.png" alt="Build step: NUnit" width="750"/>

The _NUnit runner_ field defines the NUnit version used to run tests. When configuring your build step for NUnit 3, the _Path to NUnit console runner_ field is required to contain the path to the NUnit console: prior to TeamCity 9.1.4 specify the _directory_ containing the console executable file, in the later TeamCity versions specify _the path to the console executable file including the file name_.

In all the examples the NuGet package manager provides the NUnit infrastructure. Using NuGet enables the user to conveniently manage the test environment, update NUnit, and run test locally as they will be run by TeamCity.

The NUnit build step is a straightforward and user-friendly way to run NUnit tests in TeamCity. It provides the maximum range of options simultaneously concealing the specifics of running tests on different OSs by using [Mono](http://www.mono-project.com/).

## Case 4. NUnit Build Step, options

When configuring the NUnit build step for NUnit 3, it requires specifying the NUnit Console runner (either selecting a preinstalled tool or entering a custom path).

The other fields provide a lot of useful options, and this section discusses some of them.

<img src="nunit-step-advanced.png" alt="Build step: NUnit, advanced options" width="750"/>

 
One of the options is defining the __application configuration file__. Sometimes tests obtain data from a configuration file, and to facilitate this, you need to define the path to the application configuration file to be used when running tests in the _Path to application configuration file_ field. The path can be absolute or relative to the [Build Checkout Directory](build-checkout-directory.md). Unfortunately, NUnit is limited by allowing only one configuration file per build step. Due to this limitation, if you need to test several assemblies with different configurations in one build step, you have to aggregate several application configuration files into a common configuration file. If it is not possible, split the test launch into several steps and define a configuration file in each of the steps.

The NUnit 3 console has a great number of settings defined by command line arguments. The _[Additional command line parameters](nunit.md#NUnit+Test+Settings)_ field allows setting many of the command line parameters for the NUnit console, but there are some limitations:
* The `--where` command line argument is used as the priority test filtering setting. When it is used simultaneously with the _NUnit categories include_ or/and _NUnit categories exclude_ fields to filter tests by category, TeamCity will ignore these options: a warning will be displayed and only the `--where` argument will be used.
* Using the parameters below in the _Additional command line parameters_ field may cause your build to fail:
  * the list of assemblies to be tested, as the NUnit build step determines the list of assemblies from the NUnit project file due to the command line size limitations
  * NUnit project file, as TeamCity creates its own temporary NUnit project file (see [below](#Debugging+NUnit+tests))
  * `--work`: the NUnit build step uses the [Build Checkout Directory](build-checkout-directory.md) as the base directory
  * `--noheader` is used by default
  * `--x86` if the _.NET Runtime | Platform_ field is set to `x86`
  * `--framework` if the _.NET Runtime | Version_ is set to something other than `auto`
  * `--explore` if the algorithm below is used
 
The NUnit build step can be set to first run the tests which failed in the previous builds. The idea is that it saves time to conclude about a probable state of the finished build. If the _Reduce test failure feedback time_ flag is set, the tests are run in three steps provided that the previous build has failed tests:

1\. As the first step, TeamCity obtains all the list of tests for assemblies taking into account filters by category (the `--explore` console argument is used), the statistics of the previous step run is analysed and 2 test lists are created: the priority list and the rest. The priority list includes the tests which failed in the previous build. 

<note>

Note that when the _Reduce test failure feedback time_ option is enabled, the `Explicit` attribute __will not work__.

</note>

2\. During this step the priority tests are run.    
3\. The remaining tests follow.

Another great feature of the NUnit build step is the fact that it executes tests on different OSs the same way, without changing the configuration. This is true if the agents, where the full-blown .NET is unavailable, have [Mono](http://www.mono-project.com/). It allows the build configurations to be OS-independent.

## Debugging NUnit tests

All the actions performed by TeamCity to run tests a user can reproduce in the command line. This allows for quick and effective resolution of the potential issues with configuring a build.

TeamCity records all the data related to running tests into the [Build Log](build-log.md). Thus the commands that TeamCity uses to run tests can be copied from the build log and can be run from the command line locally or on agents.

When running tests, besides commands, TeamCity creates temporary files:
* NUnit project files
* JetBrains dotCover
* JetBrains dotTrace or JetBrains dotMemory Unit configuration files

These are contained in [hidden build artifacts](build-artifact.md#Hidden+Artifacts). For example, the command line launching tests for [case 4](#Case+4.+NUnit+Build+Step%2C+options) will look as follows in the build log:

```Shell
...\nunit3\-console.exe ...\5Oqkbf9J2qJNkUK4KEtKvxs8TFFnlrno.nunit
```

In this case `5Oqkbf9J2qJNkUK4KEtKvxs8TFFnlrno.nunit` is the NUnit project file, which can be downloaded from [hidden build artifacts](build-artifact.md#Hidden+Artifacts):

<img src="nunit-hidden-artifacts.png" alt="NUnit hidden artifacts" width="692"/>

__  __

__See also:__

__Administrator's Guide__: [NUnit Support](nunit-support.md)

__ __