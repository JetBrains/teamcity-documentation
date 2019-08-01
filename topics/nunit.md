[//]: # (title: NUnit)
[//]: # (auxiliary-id: NUnit)

The _NUnit_ build runner is intended to run NUnit tests right on the TeamCity server. However, there are other ways to report NUnit tests results to TeamCity, refer to the [NUnit Support](nunit-support.md) page for the details.

<include src="nunit-support.md" include-id="supported-versions"/> 
<include src="nunit-support.md" include-id="supported-warning"/>


On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

<anchor name="NUnit3Extensions"/>

## NUnit 3 Requirements
[//]: # (AltHead: NUnit3Extensions)



### Installing NUnit

To use the TeamCity NUnit build runner, you need to install the [NUnit NuGet package](https://www.nuget.org/packages/NUnit/) on TeamCity agents first.


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


### Installing Extensions

Starting from version 3.2.0, NUnit requires the `NUnit.Extension.NUnitProjectLoader` extension to be installed on the TeamCity agent.    
Starting from version 3.4.1, NUnit requires the `NUnit.Extension.TeamCityEventListener` extension to be installed on the TeamCity agent.

The NUnit runner checks for the extensions, and if they are not found, in versions 3.2.0 and 3.2.1 the build will fail without a warning; since version 3.4.1 a message is displayed suggesting you install them.   
The extensions can be installed in bulk using the [NUnit Console Version 3](https://www.nuget.org/packages/NUnit.Console) NuGet package or as separate packages, `NUnit.Extension.TeamCityEventListener` and `NUnit.Extension.NUnitProjectLoader`.

## NUnit Test Settings

<table><tr>
       
<td>

Setting
</td>
       
<td>

Description

</td></tr><tr>

<td>

 NUnit runner


</td>

<td>

Select the NUnit version to be used to run the tests. The number of settings available will vary depending on the selected version.


</td></tr><tr>

<td>

Path to NUnit console tool:


</td>

<td>

_Available if NUnit 3.0 is selected_. Specify the path to nunit3\-console.exe:    
__prior to TeamCity 9.1.4__ specify the directory containing the console executable file,           
__since 9.1.4__ specify the path to the console executable file including the file name. 


</td></tr><tr>

<td>

Working directory

</td>

<td>

_Available if NUnit 3.0 is selected_. Optional. Specify the path to the [build working directory](build-working-directory.md) if it differs from the directory of the testing assembly.

</td></tr><tr>

<td>

 Path to application configuration file


</td>

<td>

_Available if NUnit 3.0 is selected_. Specifу the path to the application configuration file to be used when running tests. The path is absolute or relative to the [checkout directory](build-checkout-directory.md).

</td></tr><tr>

<td>

<anchor name="NUnit-cmdParameters"/>

Additional command line parameters


</td>

<td>

_Available if NUnit 3.0 is selected_. Enter [additional command line parameters](https://github.com/nunit/docs/wiki/Console-Command-Line) to pass to nunit\-console.exe


</td></tr><tr>

<td>

.NET Runtime


</td>

<td>

From the __Platform__ drop\-down, select the desired execution mode on a x64 machine. Supported values are: `Auto (MSIL)`, `x86`, and `x64`.   
From the __Version__ drop\-down, select the desired .NET Framework version. Supported values are: `v2.0`, `v4.0`; and `auto`, _available if NUnit 3.0 is selected_.      
_For NUnit 3.0_, if `auto` is selected, tests will run under the framework they are compiled with.


</td></tr><tr>

<td>

Run tests from


</td>

<td>

Specify the .NET assemblies where the NUnit tests to be run are stored. Multiple entries are comma\-separated; usage of MSBuild wildcards is enabled. The paths to assembly files are absolute or relative to the [checkout directory](build-checkout-directory.md). In the following example, TeamCity will search for the tests assemblies in all project directories and run these tests.


```Shell
**\*.dll

```



<note>

All these wildcards are specified relative to the checkout directory.
</note>


</td></tr><tr>

<td>

Do not run tests from


</td>

<td>

Specify .NET assemblies that should be excluded from the list of assemblies to test. Multiple entries are comma\-separated; usage of MSBuild wildcards is enabled. The paths to assembly files are absolute or relative to the [checkout directory](build-checkout-directory.md).   
In the following example, TeamCity will omit tests specified in this directory.


```Shell
**\obj\**\*.dll

```



<note>

All these wildcards are specified relative to the checkout directory.
</note>


</td></tr><tr>

<td>

Include categories


</td>

<td>

Specify NUnit categories of tests to be run. Multiple entries are comma\-separated.    
[Category expressions](teamcity-nunit-test-launcher.md#Category+Expression) are supported here as well; commas, semicolons, and new\-lines are treated as global __or__ operations (prior to the expression parsing). Since NUnit 3.0 __category expressions are not supported__ and must be set via the command line as [described in the NUNit documentation](https://github.com/nunit/docs/wiki/Test-Selection-Language).


</td></tr><tr>

<td>

Exclude categories


</td>

<td>

Specify NUnit categories to be excluded from the tests to be run. Multiple entries are comma\-separated.    
[Category expressions](teamcity-nunit-test-launcher.md#Category+Expression) are supported here as well; commas, semicolons, and new\-lines are treated as global __or__ operations (prior to the expression parsing). Since NUnit 3.0 __category expressions are not supported__ and must be set via the command line as [described in the NUNit documentation](https://github.com/nunit/docs/wiki/Test-Selection-Language).


</td></tr><tr>

<td>

Run process per assembly


</td>

<td>

_Available for versions  prior to _[NUnit 3.0](upgrade-notes.md#UpgradeNotes-Changesfrom9.1.5to9.1.6)only_. Select this option if you want to run each assembly in a new process.


</td></tr><tr>

<td>

Reduce test failure feedback time


</td>

<td>

Use this option to instruct TeamCity to run some tests before others.


</td></tr></table>

## Code Coverage

To learn about configuring code coverage options, refer to the [Configuring .NET Code Coverage](configuring-.net-code-coverage.md) page.

__For NUnit 3.x__, only [JetBrains dotCover](jetbrains-dotcover.md) is supported as a coverage tool.  
 
 
 
__  __

__See also:__

__Administrator's Guide__: [Configuring Unit Testing and Code Coverage](configuring-unit-testing-and-code-coverage.md) | [NUnit Support](nunit-support.md) | [Getting Started with NUnit](getting-started-with-nunit.md)
