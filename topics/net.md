[//]: # (title: .NET)
[//]: # (auxiliary-id: .NET;viewpage.actionpageId113084145;.NET CLI (dotnet))

TeamCity comes with the built-in support of the .NET toolchain providing the .NET build step, .NET detection on the build agents, and autodiscovery of build steps in your repository.

This page gives details on configuring the .NET runner.

<note>

Since TeamCity 2019.2.3, the .NET CLI (dotnet) build runner has been refactored and renamed to __.NET__ thus emphasizing that now it supports all .NET-related operations previously implemented in TeamCity as multiple build runners.

Note that we stop providing active support for the [MSBuild](msbuild.md), [Visual Studio (sln)](visual-studio-sln.md), [Visual Studio 2003](visual-studio-2003.md), and [Visual Studio Tests](visual-studio-tests.md) runners. These runners are left for compatibility of existing build configurations with new versions of TeamCity. To receive the following updates and use extra features of our .NET implementation, we suggest that you select the .NET runner instead of any of the listed runners in all corresponding build steps. This page describes the settings of all supported .NET commands and gives guidelines on migration from the obsolete build runners.

</note>

## Requirements

The .NET runner requires the following software to be installed on a build agent machine:

<table>
<tr><td>Command</td><td>Required software</td></tr>

<tr>
<td>

[.NET CLI commands](#Build+Runner+Options)   
(including cross-platform `msbuild` and `vstest`)

</td>
<td>

* [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core/) (versions 1 – 3.1)   
   _or_
* [.NET SDK](https://dotnet.microsoft.com/download/dotnet/5.0) (version 5.0)
</td>

</tr>

<tr>

<td>

`msbuild` command via `msbuild.exe`   
(if Windows-only MSBuild version is selected)

</td>

<td>

* Visual Studio (version 2013 or later)   
   _or_
* Visual Studio Build Tools (2013 or later)   
   _or_
* .NET Framework Developer Pack (version 4.5 or later) with .NET SDK

</td>

</tr>

<tr>

<td>

`vstest` command via `VSTests.Console.exe`   
(if Windows-only VSTest version is selected)

</td>

<td>

* Visual Studio (version 2013 or later)   

</td>

</tr>

<tr>

<td>

`devenv` command

</td>

<td>

* Visual Studio (version 2013 or later)   

</td>

</tr>

</table>

### .NET Version Detection Algorithm

TeamCity searches for the .NET executable files in the following order:
1. In the directory defined in the environment variable `DOTNET_HOME` for a TeamCity agent. For example, `DOTNET_HOME=D:\SDK\dotnet\`.
2. In the default directory for the .NET executable file:
   * Windows: `C:\Program Files\dotnet` or `C:\Program Files (x86)\dotnet`, or other default program files directory (depending on the environment variable `ProgramW6432`)
   * Unix: `/usr/share/dotnet`
   * Mac: `/usr/local/share/dotnet`
3. In paths specified in the `PATH` environment variable.

TeamCity will use the first .NET version it finds. If you have several .NET versions installed, we recommend that you specify the most recent version in the `DOTNET_HOME` variable.

## Build Runner Options

Currently, the .NET runner supports the following commands:

* Basic .NET CLI commands:
   * [`build`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-build)
   * [`clean`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-clean)
  * [`restore`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-restore)   
  (requires .NET CLI 2.1.400+ for authentication in [private feeds](#Authentication+in+Private+NuGet+Feeds))
  * [`pack`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-pack)
  * [`publish`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-publish)
  * [`run`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-run)
  * [`test`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-test)
* Advanced commands:
     * [`msbuild`](#msbuild)\*
     * [`vstest`](#vstest)\*
     * [`nuget delete`](#nuget+delete)   
     (requires .NET CLI 2.1.500+ for authentication in [private feeds](#Authentication+in+Private+NuGet+Feeds))
     * [`nuget push`](#nudget+push)   
     (requires .NET CLI 2.1.500+ for authentication in [private feeds](#Authentication+in+Private+NuGet+Feeds))
* Visual Studio command-line mode (read more in the [Visual Studio reference](https://docs.microsoft.com/en-us/visualstudio/ide/reference/devenv-command-line-switches)):
  * [`devenv`](#Visual+Studio+command-Line+mode)
  
\* _`msbuild` and `vstest` are executed as [CLI commands](https://docs.microsoft.com/en-us/dotnet/core/tools/) if cross-platform .NET SDK is used for building a project. Otherwise, they are run using the `msbuild` or `VSTest.Console` tool respectively._

### Basic Commands

The set of .NET runner's options depends on the selected command. Available options for basic .NET CLI commands are:

<table><tr>

<td>

Option

</td>

<td>

Description

</td>

</tr>

<tr>

<td>

Projects

</td>

<td>

Paths to projects and solutions. Wildcards are supported. Parameter references are supported. If you have a finished build, you can use the file/directory selector here.

</td>

</tr><tr>

<td>

Working directory

</td>

<td>

Optional, set if differs from the [checkout directory](build-checkout-directory.md). Parameter references are supported. If you have a finished build, you can use the file/directory selector here.

</td>

</tr><tr>

<td>

Framework

</td>

<td>

Target framework. For example, `netcoreapp` or `netstandard`. Parameter references are supported.

</td>

</tr>

<tr>

<td>

Configuration

</td>

<td>

Target configuration, for example, `Release` or `Debug`. Parameter references are supported.

</td>

</tr><tr>

<td>

Runtime

</td>

<td>

Target runtime. Parameter references are supported.

</td>

</tr>

<tr>

<td>

Options

</td>

<td>

The "_Do not build the projects_" checkbox declares whether to build the projects before packing or testing or not.

</td>

</tr>

<tr>

<td>

NuGet package sources

</td>

<td>

NuGet package sources to use during restoring.

</td>

</tr>

<tr>

<td>

Output directory

</td>

<td>

The directory where to place outputs. Parameter references are supported. If you have a finished build, you can use the file/directory selector here.

</td>

</tr><tr>

<td>

Version suffix

</td>

<td>

The value of the `$(VersionSuffix)` property in the project. Parameter references are supported.

</td>

</tr><tr>

<td>

Command line parameters

</td>

<td>

[Additional command line parameters](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-build?tabs=netcore2x) for the `dotnet` command.

</td>

</tr><tr>

<td>

Logging verbosity

</td>

<td>

Available logging modes: `<Default>`, `Minimal`, `Normal`, `Detailed`, or `Diagnostic`.

</td>

</tr></table>

### Advanced Commands

#### msbuild

The `msbuild` command is used for building a project and all its dependencies with the Microsoft Build Engine.   
Depending on the selected MSBuild version, `msbuild` can either be run as the [cross-platform .NET CLI command](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-msbuild) or as the [Windows-only `msbuild.exe` tool](https://docs.microsoft.com/en-us/visualstudio/msbuild/msbuild).

The `msbuild` command shares some of the common options with the basic CLI commands of the .NET runner (see the [corresponding section](#Basic+Commands) for more details).

MSBuild-specific settings are:

<table><tr>

<td>

Option

</td>

<td>

Description

</td>

</tr>

<tr>

<td>

Targets

</td>

<td>

List of targets separated by a space or semicolon. A target is an arbitrary script for your project purposes. Click the list icon next to the field to view available targets.

</td>

</tr>

<tr>

<td>

<anchor name="msbuild-version"/>

MSBuild version

</td>

<td>

Specify the version of the installed MSBuild engine. See the [Requirements](#Requirements) section for more details.

</td>

</tr>

</table>

##### Migrating from MSBuild Runner

Since TeamCity 2019.2.3, the .NET runner is the recommended method for building projects with the MSBuild engine. We have included the `msbuild` command to our refactored .NET runner to ensure a long-term support of the .NET platform development strategy.

You can safely switch [MSBuild steps](msbuild.md) in your existing build configurations to the .NET runner. Make sure to copy all additional command-line parameters and other important settings to the new runner. See the [`msbuild`](#msbuild) section for more details on the settings available in the .NET runner.

Additional features you will get in the .NET runner are:
* Support of cross-platform MSBuild for .NET projects
* Ability to build a project for a different platform specified in the _Runtime_ field
* Ability to run the project in a Docker container with our [Docker Wrapper](docker-wrapper.md) extension

Consider the following notes before migrating:
* The .NET runner provides code coverage only for [dotCover](jetbrains-dotcover.md).
* Mono is not supported with this runner.

If you are actively using either Mono or NCover/PartCover in your MSBuild steps, please let us know about it via any of the [feedback channels](https://confluence.jetbrains.com/display/TW/Feedback).

##### Migrating from Visual Studio (sln) Runner

The [Visual Studio (sln)](visual-studio-sln.md) build runner is using the MSBuild engine under its hood and provides a few tweaks for the VS users to ease their experience with building projects in TeamCity. Since TeamCity 2019.2.3, the .NET runner is the recommended method for building projects with the MSBuild engine which makes it a migration option for the users of the Visual Studio (sln) step as well.

In general, to softly switch each existing Visual Studio (sln) build step to the .NET runner you need to:
1. Remember/copy the values of your Visual Studio (sln) runner's settings and command-line parameters.
2. Switch the Visual Studio (sln) build step to the .NET runner and select the `msbuild` command.
3. Fill in the fields according to the [`msbuild`](#msbuild) section.   
Note that certain fields have different analogs in the .NET runner:
   * The MSBuild version should be specified instead of the version and platform of Visual Studio. See the [reference on versions](https://en.wikipedia.org/wiki/MSBuild#Versions).
   * Paths to solutions should be specified in the _Projects_ field.
   
Refer to the [respective section](#Migrating+from+MSBuild+Runner) for more information on migration to `msbuild`.

<note>

Note that TeamCity provides a new way to run Visual Studio projects. The .NET runner supports a full-featured implementation of the VS command-line mode with the `devenv` command ([read more](#Visual+Studio+Command-Line+Mode)).

</note>

#### vstest

The `vstest` command is used for testing a project with the VSTest engine and automatically importing the test results. Depending on the selected VSTest version, `vstest` can either be run as the [cross-platform .NET CLI command](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-vstest) or as the [VSTest console](https://plugins.jetbrains.com/plugin/9056-vstest-console-runner).

The `vstest` command shares some of the common options with the basic CLI commands of the .NET runner (see the [corresponding section](#Basic+Commands) for more details).

VSTest-specific fields are:

<table><tr>

<td>

Option

</td>

<td>

Description

</td>

</tr>

<tr>

<td>

<anchor name="vstest-assemblies"/>

Test assemblies

</td>

<td>

Specify the new-line separated list of paths to assemblies to run tests on. [Wildcards](wildcards.md) are supported.   
Paths to the assemblies must be relative to the [build checkout directory](build-checkout-directory.md).

</td>

</tr>

<tr>

<td>

<anchor name="vstest-version"/>

VSTest version

</td>

<td>

Specify the installed version of VSTest. See the [Requirements](#Requirements) section for more details.

</td>

</tr>

<tr>

<td>

<anchor name="vstest-platform"/>

Platform

</td>

<td>

If necessary, specify the target platform: x86, x64, or ARM. Leave _\<Auto\>_ to use the platform selected by VSTest.

</td>

</tr>

<tr>

<td>

<anchor name="vstest-isolation"/>

Run in isolation

</td>

<td>

Select to run the tests in an isolated process.

</td>

</tr>

<tr>

<td>

Test filtration

</td>

<td>

Select the test filtration mode:
* _Test names_: Of all tests discovered in the included assemblies, only the tests with the names matching the provided values will be run. For multiple values, separate them with a new line. If the field is empty, all tests will be run.   
See details in the [Microsoft documentation](https://msdn.microsoft.com/en-us/library/jj155800.aspx).
* _Test case filter_: Run tests that match the given expression.   
See details in the [Microsoft documentation](https://msdn.microsoft.com/en-us/library/jj155800.aspx).

</td>

</tr>

<tr>

<td>

Settings file

</td>

<td>

Set the path to the [`.runsettings`](https://docs.microsoft.com/en-us/visualstudio/test/configure-unit-tests-by-using-a-dot-runsettings-file) file.

</td>

</tr>

</table>

##### Migrating from Visual Studio Tests Runner

Since TeamCity 2019.2.3, the .NET runner is the recommended method for testing projects with VSTest instead of the [Visual Studio Tests](visual-studio-tests.md) runner. We have included the `vstest` command to our refactored .NET runner to ensure a long-term support of the .NET platform development strategy.

You can safely migrate existing Visual Studio Tests build steps to the .NET runner with the selected `vstest` command. Make sure to copy all additional command-line parameters and other important settings to the new runner. See the [`vstest`](#vstest) section for more details on the settings available in the .NET runner.

Additional features you will get in the .NET runner are:
* Support of cross-platform VSTest for .NET projects
* Real-time test reporting by default
* Support of ARM platform, along with x86 and x64
* Ability to run and test the project inside a Docker container with our [Docker Wrapper](docker-wrapper.md) extension

Consider the following notes before migrating:
* The .NET runner supports the new [`.runsettings`](https://docs.microsoft.com/en-us/visualstudio/test/configure-unit-tests-by-using-a-dot-runsettings-file) format of the VSTest settings file. However, it does not support the obsolete run configuration file format used in the Visual Studio Tests runner.
* Instead of the framework version, the .NET runner requests to specify the VSTest version.
* The .NET runner provides code coverage only for [dotCover](jetbrains-dotcover.md). If you are actively using NCover or PartCover in your MSBuild steps, please let us know about it via any of the [feedback channels](https://confluence.jetbrains.com/display/TW/Feedback).
* The .NET runner does not support the MSTest tool since all features of its framework are covered by VSTest. If you were using MSTest as the engine of the Visual Studio Tests runner, we suggest that you switch to VSTest when migrating to the .NET runner.

#### nuget delete

TeamCity provides a full support for the [`nuget delete`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-nuget-delete) command.

#### nuget push

TeamCity provides a full support for the [`nuget push`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-nuget-push) command.

### Visual Studio Command-Line Mode

Since TeamCity 2019.2.3, the .NET runner supports the Visual Studio command-line mode with the [`devenv`](https://docs.microsoft.com/en-us/visualstudio/ide/reference/devenv-command-line-switches) command.

Devenv allows configuring custom options for the IDE, build, debug, and deploy projects from the command line using different [switches](https://docs.microsoft.com/en-us/visualstudio/ide/reference/devenv-command-line-switches#devenv-switch-syntax).

`devenv` shares some of the common options with the basic CLI commands of the .NET runner (see the [corresponding section](#Basic+Commands) for more details).

Devenv-specific fields are:

<table><tr>

<td>

Option

</td>

<td>

Description

</td>

</tr>

<tr>

<td>

<anchor name="#devenv-build-action"/>

Build action

</td>

<td>

Select one of the supported switches: `clean`, `rebuild`, `build`, or `deploy`.

</td>

</tr>

<tr>

<td>

Visual Studio version

</td>

<td>

If necessary, specify the version of the installed Visual Studio. Leave _\<Any\>_ to use the latest installed version.

See the [Requirements](#Requirements) section for more details.

</td>

</tr>

</table>

<note>

Note that Devenv does not provide functionality for displaying a structured build log or test reports. Its main purpose is to allow using custom switches and add-ins for Visual Studio.

</note>

## Docker Settings

The .NET CLI build step can be run in a specified [Docker container](docker-wrapper.md).

## Code Coverage

[JetBrains dotCover](jetbrains-dotcover.md) is supported as a coverage tool for the `msbuild`, `test`, and `vstest` commands.

## Authentication in Private NuGet Feeds

TeamCity allows you to authenticate using private NuGet feeds. Read more in [NuGet](nuget.md#Authentication+in+private+NuGet+Feeds).

## Parameters Reported by Agent

When starting, the build agent reports the following parameters:

<table><tr>

<td>

Parameter

</td>

<td>

Description

</td></tr><tr>

<td>

`DotNetCLI`

</td>

<td>

The .NET CLI version.

</td></tr><tr>

<td>

`DotNetCLI_Path`

</td>

<td>

The path to .NET CLI executable.

</td></tr><tr>

<td>

`DotNetCoreSDKx.x_Path`

</td>

<td>

The .NET SDK version.

</td></tr></table>

__ __
