[//]: # (title: NUnit)
[//]: # (auxiliary-id: NUnit)

The _NUnit_ build runner is intended to run NUnit tests right on the TeamCity server. However, there are other ways to report NUnit tests results to TeamCity, refer to the [NUnit Support](nunit-support.md) page for the details and supported versions.

<anchor name="NUnit3Extensions"/>

Note that this runner supports only [.NET Framework](https://docs.microsoft.com/en-us/dotnet/framework/get-started/overview). To run tests for [.NET Core](https://docs.microsoft.com/en-us/dotnet/framework/get-started/net-core-and-open-source) projects (and .NET Framework projects version 4.0 or later), use the [.NET](net.md) build runner with the [`test`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-test) command instead. See the [NUnit Support in TeamCity](nunit-support.md) article for details.

## Installing NUnit

<snippet include-id="installing-nunit">

To use the NUnit build runner, you need to install the [NUnit NuGet package](https://www.nuget.org/packages/NUnit/) on TeamCity agents via one of the following options:
* Instruct the first build step to install NUnit from a NuGet package.  
  For example, you can add a [Command Line](command-line.md) build step which will install the `NUnit.Console` NuGet package as follows:

    ```Shell
    %\teamcity.tool.NuGet.CommandLine.DEFAULT%\tools\nuget.exe install NUnit.Console -version 3.6.0 -o packages
   
    ```
    
    Note that `%\teamcity.tool.NuGet.CommandLine.DEFAULT%` is a reference to the NuGet installed under the TeamCity agent.   
    You can install NuGet on agents on the __Administration | Tools__ page, where you can also mark any of the installed NuGet versions as default.
    
    After that, the `%\teamcity.tool.NuGet.CommandLine.DEFAULT%` parameter reference should properly resolve to the NuGet installation path on the agent.   
    Then nunit3-console should appear under the package directory.

* Install NUnit manually to a standard location on all the build agents and configure the path to `nunit-console.exe` in your NUnit build step.

### Installing Extensions
[//]: # (AltHead: NUnit3Extensions)

Starting from version 3.2.0, NUnit requires the `NUnit.Extension.NUnitProjectLoader` extension to be installed on the TeamCity agent. Starting from version 3.4.1, NUnit requires the `NUnit.Extension.TeamCityEventListener` extension to be installed on the TeamCity agent.  
If the extensions are not found in versions 3.2.0 and 3.2.1, the build will fail without a warning. Since version 3.4.1, a message will be displayed suggesting you install them.   

The extensions can be installed as separate packages or in bulk using the [NUnit Console Version 3](https://www.nuget.org/packages/NUnit.Console) NuGet package.

</snippet>

<anchor name="NUnit-settings"/>

## NUnit Test Settings

<table><tr>
       
<td>

Setting

</td>
       
<td>

Description

</td></tr><tr>

<td id="runner">

<anchor name="NUnit-runner"/>

NUnit runner

</td>

<td>

Select the NUnit version to be used to run the tests. The set of available settings varies depending on the selected version.

</td></tr><tr>

<td id="pathToNUnitConsoleTool">

<anchor name="NUnit-pathToNUnitConsoleTool"/>

NUnit Console

</td>

<td>

_Available for NUnit 3.0_

Select a preinstalled console tool or specify a custom path to the `nunit3-console.exe` console executable, including the filename.

</td></tr><tr>

<td id="workingDirectory">

<anchor name="NUnit-workingDirectory"/>

Working directory

</td>

<td>

_Available for NUnit 3.0_

Specify the path to the [build working directory](build-working-directory.md) if it differs from the directory of the testing assembly.

</td></tr><tr>

<td id="appConfigFile">

<anchor name="NUnit-appConfigFile"/>

Path to application configuration file

</td>

<td>

_Available for NUnit 3.0_

Specifу the path to the application configuration file to be used when running tests. The path should be either absolute or relative to the [checkout directory](build-checkout-directory.md).

</td></tr><tr>

<td id="NUnit-cmdParameters">

Additional command line parameters

</td>

<td>

_Available for NUnit 3.0_

Enter [additional command line parameters](https://github.com/nunit/docs/wiki/Console-Command-Line) to pass to `nunit-console.exe`.

</td></tr><tr>

<td>

.NET Runtime

</td>

<td>

From the __Platform__ drop-down menu, select the desired execution mode on an x64 machine. Supported values are: `Auto (MSIL)`, `x86`, and `x64`.

From the __Version__ drop-down menu, select the required .NET Framework version. Supported values are: `v2.0`, `v4.0`; and `auto`, _available if NUnit 3.0 is selected_.   
_For NUnit 3.0_, if `auto` is selected, tests will run under the framework they are compiled with.

</td></tr><tr>

<td>

Run tests from

</td>

<td>

Specify the .NET assemblies where the NUnit tests stored. Multiple entries should be comma-separated. The MSBuild wildcards are supported. The paths to assembly files should be absolute or relative to the [checkout directory](build-checkout-directory.md).

In the example, TeamCity will search for the tests assemblies in all the project directories and run these tests:

```Shell
**\*.dll

```

Wildcards are specified relatively to the checkout directory.

</td></tr><tr>

<td>

Do not run tests from

</td>

<td>

Specify .NET assemblies that should be excluded from the list of assemblies to test. Multiple entries should be comma-separated. The MSBuild wildcards are supported. The paths to assembly files should be absolute or relative to the [checkout directory](build-checkout-directory.md).

In the example, TeamCity will omit tests specified in this directory:


```Shell
**\obj\**\*.dll

```


Wildcards are specified relatively to the checkout directory.

</td></tr><tr>

<td>

Include categories

</td>

<td>

Specify NUnit categories of tests to be run. Multiple entries should be comma-separated.    
[Category expressions](nunit-support.md#Category+Expression) are supported as well. Commas, semicolons, and new lines are treated as global __or__ operations (prior to the expression parsing).

Since NUnit 3.0, category expressions are not supported and must be set via the command line as described in the [NUNit documentation](https://github.com/nunit/docs/wiki/Test-Selection-Language).

</td></tr><tr>

<td>

Exclude categories

</td>

<td>

Specify NUnit categories to be excluded from the tests to be run. Multiple entries should be comma-separated.   
[Category expressions](nunit-support.md#Category+Expression) are supported here as well. Commas, semicolons, and new lines are treated as global __or__ operations (prior to the expression parsing).

Since NUnit 3.0, category expressions are not supported_and must be set via the command line as described in the [NUNit documentation](https://github.com/nunit/docs/wiki/Test-Selection-Language).

</td></tr><tr>

<td>

Run process per assembly

</td>

<td>

_Available for versions prior to NUnit 3.0 only_

Select this option if you want to run each assembly in a new process.

</td></tr><tr>

<td>

Reduce test failure feedback time

</td>

<td>

Use this option to instruct TeamCity to run some tests before others.

</td></tr></table>

## NUnit Code Coverage

To learn about configuring code coverage options, refer to the [this article](configuring-.net-code-coverage.md).

For NUnit 3.x, only [JetBrains dotCover](jetbrains-dotcover.md) is supported as a coverage tool.
 
<seealso>
        <category ref="admin-guide">
            <a href="configuring-test-reports-and-code-coverage.md">Configuring Unit Testing and Code Coverage</a>
            <a href="nunit-support.md">NUnit Support</a>
            <a href="getting-started-with-nunit.md">Getting Started with NUnit</a>
        </category>
</seealso>