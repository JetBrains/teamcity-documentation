[//]: # (title: Getting Started with NUnit)
[//]: # (auxiliary-id: Getting Started with NUnit)

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

This tutorial aims at describing the basic practices of using [NUnit 3](http://www.nunit.org/) in TeamCity. The test project and script samples can be found [here](https://github.com/JetBrains/teamcity-nunit-samples). The order of use cases is based on the number of the TeamCity features involved: the first case is the most basic, more complex cases that follow utilize a larger number of features. We recommend that you familiarize yourself with all features, finding their advantages and disadvantages, and then decide in favor of one or another.

## Installing NUnit

To use the NUnit with TeamCity, you need to install [NUnit NuGet package](https://www.nuget.org/packages/NUnit/) on TeamCity agents first.

To do that, use one of the following options:

* you can add the NuGet install build step as the first step of your build configuration.For example, you can add a command line build step before the NUnit build step which will install the NUnit.Console NuGet package as follows:

    ```Shell
    %teamcity.tool.NuGet.CommandLine.DEFAULT%\tools\nuget.exe install NUnit.Console -version 3.6.0 -o packages
   
    ```
    
    Note that `%teamcity.tool.NuGet.CommandLine.DEFAULT%` is a reference to the NuGet installed under the TeamCity agent.   
    You can install NuGet on agents from the __Administration | Tools__ page, where you can also mark one of the installed NuGet versions as default. 
    
    After that the `%teamcity.tool.NuGet.CommandLine.DEFAULT%` parameter reference should properly resolve to the NuGet installation path on the agent.   
    Then nunit3\-console should appear under the packages directory.   
    
    To run tests, in the next NUnit build step, specify the NUnit path in the NUnit settings as `packages\NUnit.ConsoleRunner.3.6.0\tools\nunit3-console.exe`. 

* Another approach is to install NUnit manually on all of the agents in some standard place, and configure the path to `nunit-console.exe` in your NUnit build step. 



## Case 1. Command Line

What can be simpler than the command line? Starting from version 3, NUnit supports TeamCity out\-of\-the\-box, which allows running tests from the command line, preserving the basic features TeamCity\-NUnit integration. NUnit automatically detects if it is run by TeamCity, and if so, it switches to the integration mode.

In addition to auto-detection, the NUnit 3 console handles the special `--teamcity argument`, which also includes the integration with TeamCity. This argument can be used for the purposes of debugging, when you need to see which information NUnit sends to TeamCity: detailed test information, the order of tests, and so on.  

As mentioned earlier, using the NUnit console from the command line is the simplest way to run tests. And even in this case TeamCity will provide the following basic features of integration:
* reporting the test run information in Build Log while the tests are executing
* TeamCity will report the test results upon the tests finish
* all the [investigation](investigating-and-muting-build-problems.md) features will be available in TeamCity.

This is what the TeamCity build step to run tests from the command line looks like:

<img src="nunit-cmd.png" alt="Build step: Command Line" width="780"/>

To sum up, with this simple use case you can avail yourself to the basic features of the TeamCity\-NUnit 3 integration. 

## Case 2. MSBuild

This use case is very similar to the previous one, but it should be mentioned that MSBuild is the standard build platform in the .Net world.  

TeamCity supports collecting code coverage statistics out of the box; additional plugins can be [installed ](installing-additional-plugins.md)to use the [JetBrains dotTrace](https://github.com/JetBrains/teamcity-dottrace) and [JetBrains dotMemory Unit](https://github.com/JetBrains/teamcity-dotmemory) in TeamCity.

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

TeamCity controls the test execution progress, but the NUnit infrastructure exceptions may not allow TeamCity to collect the required information. That is why the IgnoreExitCode="True" attribute needs to be set, which will ignore the positive exit codes and will not interrupt the build due to several failed tests. The [Error](https://msdn.microsoft.com/en-us/library/8b08t3s4.aspx) task will stop the build in case of the test infrastructure errors for the negative exit codes.

<img src="nunit-msbuild.png" alt="Build step: MSBuild" width="830"/>

Besides the project file, you can define the MSBuild version and platform, the target, you can use profiles and other settings.

</chunk>
   
To make the build more stable, you can introduce a few changes to the project file. For example, you can define the path to the NUnit console dynamically. This path may change with the updates of the NUnit console NuGet package, but our build configuration will not require any changes. The code below gets the path to the NUnit console to the `pathToNUnitConsole` variable: 


```Shell
<GetNUnitConsolePath
BaseDir="$(MSBuildProjectDirectory)\packages">
 <Output TaskParameter="PathToNUnitConsole" ItemName="pathToNUnitConsole"/>
</GetNUnitConsolePath>
```

You can dynamically list the assemblies to be tested: for example, search for them in specific directories and filter them to match a regular expression pattern:


```Shell
<ItemGroup>
 <Assemblies Include="Tests\bin\**\*.dll"/>
</ItemGroup>
 
<CreateNUnitListOfAssemblies
Assemblies="@(Assemblies)" RegexpAssemblyFilter=".*Tests.dll$">
  <Output TaskParameter="ListOfAssemblies" ItemName="listOfAssemblies"/>
</CreateNUnitListOfAssemblies>
```


The following project files contain some examples: [sample2adv.proj](https://github.com/JetBrains/teamcity-nunit-samples/blob/master/sample2adv.proj), [unit-utils.targets](https://github.com/JetBrains/teamcity-nunit-samples/blob/master/nunit-utils.targets)

## Case 3. NUnit Build Step

The [NUnit](nunit.md) build step is probably the simplest and yet the most powerful way to launch NUnit tests in TeamCity.   
In most cases its sufficient to set only the 2 parameters: the path to the NUnit console runner and the list of assemblies to be tested:

<img src="nunit-step.png" alt="Build step: NUnit" width="1005"/>

 
Look at the __NUnit runner__ field which defines the NUnit version used to run tests. When configuring your build step for NUnit 3, the __Path to NUnit console runner__ field is required to contain the path to the NUnit console: prior to TeamCity 9.1.4 specify the _directory_ containing the console executable file, in the later TeamCity versions specify _the path to the console executable file including the file name_.

In all the examples the NuGet package manager provides the NUnit infrastructure. Using NuGet enables the user to conveniently manage the test environment, update NUnit, run test locally as they will be run by TeamCity.

The NUnit build step is a is straightforward and user\-friendly way to run NUnit tests in TeamCity. It provides the maximum range of options simultaneously concealing the specifics of running tests on different OSs by using [Mono](http://www.mono-project.com/).



## Case 4. NUnit Build Step, options

The NUnit build step requires the Path to NUnit console runner to be set when configuring the step for  NUnit 3: this is a mandatory field.

The other fields provide a lot of useful options, and this section discusses some of them.  

<img src="nunit-step-advanced.png" alt="Build step: NUnit, advanced options" width="1056"/>

 
One of the options is defining the __application configuration file__. Sometimes tests obtain data from a configuration file and to facilitate this, you need to define the path to the application configuration file to be used when running tests in the __Path to application configuration file__ field. The path can be absolute or relative to the [Build Checkout Directory](build-checkout-directory.md). Unfortunately, NUnit is limited by allowing only one configuration file per build step. Due to this limitation, if you need to test several assemblies with different configurations in one build step, youll have to aggregate several application configuration files into a common configuration file. If it is not possible, the test launch can be split into several steps and define a configuration file in each of the steps.

The NUnit 3 console has a great number of settings defined by command line arguments. The __[Additional command line parameters](nunit.md#NUnit+Test+Settings)__ field allows setting many of the command line parameters for the NUnit console, but there are some limitations:
* The `--where` command line argument is used as the priority test filtering setting. When it is used simultaneously with the __NUnit categories include__ or/ and __NUnit categories exclude__ fields to filter tests by category, TeamCity will ignore these options: a warning will be displayed and only the `--where` argument will be used.
* Using the parameters below in the __Additional command line parameters__ field may cause your build to fail: 
  * the list of assemblies to be tested, as the NUnit build step determines the list of assemblies from the NUnit project file due to the command line size limitations
  * NUnit project file, as TeamCity creates its own temporary NUnit project file (see [below](#Debugging+NUnit+tests))
  * `--work`: the NUnit build step uses the [Build Checkout Directory](build-checkout-directory.md) as the base directory
  * `--noheader` is used by default
  * `--x86` if the __.NET Runtime \- Platform__ field is set to  x86
  * `--framework` if the __.NET Runtime \- Version__ is set to something other than auto
  * `--explore` if the algorithm below is used
 
The NUnit build step can be set to first run the tests which failed in the previous builds. The idea is that it saves time to conclude about a probable state of the finished build. If the __Reduce test failure feedback time__ flag is set, the test are run in three steps provided that the previous build has failed tests:

1\. As the first step, TeamCity obtains all the list of tests for assemblies taking into account filters by category (the `--explore` console argument is used), the statistics of the previous step run is analysed and 2 test lists are created: the priority list and the rest. The priority list includes the tests which failed in the previous build. 

<note>

Note that when the "Reduce test failure feedback time option" is set, the `Explicit` attribute __will not work__.

</note>

2\. During this step the priority tests are run.    
3\. The remaining tests follow.

Another great feature of the NUnit build step is the fact that it executes tests on different OSs the same way, without changing the configuration. This is true if the agents, where the full\-blown .Net is unavailable, have [Mono](http://www.mono-project.com/). It allows the build configurations to be OS\-independent. 

## Debugging NUnit tests

One of the priority tasks for NUnit support in TeamCity was the possibility for the user to easily reproduce in the command line all the actions performed by TeamCity to run tests. This allows for quick and effective resolution of the potential issues with configuring a build.

TeamCity records all the data related to running tests into the [Build Log](build-log.md). Thus the commands which TeamCity uses to run tests can be copied from the build log and can be run from the command line locally or on agents.

When running tests, besides commands, TeamCity creates temporary files: 
* NUnit project files; 
* JetBrains dotCover, 
* JetBrains dotTrace or JetBrains dotMemory Unit configuration files. 

These are contained in [hidden build artifacts](build-artifact.md#Hidden+Artifacts). For example, the command line launching tests for case 4 will look as follows in the build log:

```Shell
...\nunit3\-console.exe ...\5Oqkbf9J2qJNkUK4KEtKvxs8TFFnlrno.nunit
```

In this case `5Oqkbf9J2qJNkUK4KEtKvxs8TFFnlrno.nunit` is the NUnit project file, which can be downloaded from [hidden build artifacts](build-artifact.md#Hidden+Artifacts):

<img src="nunit-hidden-artifacts.png" alt="NUnit hidden artifacts" width="692"/>


__  __

__See also:__

__Administrator's Guide__: [NUnit Support](nunit-support.md)
