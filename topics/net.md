[//]: # (title: .NET)
[//]: # (auxiliary-id: .NET;viewpage.actionpageId113084145;.NET CLI (dotnet))

TeamCity comes with the built-in support of the .NET toolchain providing the .NET build step, .NET detection on the build agents, and autodiscovery of build steps in your repository.

This page provides details on configuring the .NET runner.

<note>

Since TeamCity 2019.2.2, the .NET CLI (dotnet) build step has been reworked and renamed to __.NET__ thus defining that it now supports all .NET-related operations previously implemented in TeamCity as multiple different build steps.

</note>

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## Requirements

[.NET Core SDK](https://dotnet.microsoft.com/download) must be installed on your build agent machines.

TeamCity searches for the .NET executable files in the following order:
1. In the directory defined in the environment variable `DOTNET_HOME` for a TeamCity agent. For example, `DOTNET_HOME=D:\SDK\dotnet\`.
2. In the default directory for the .NET executable file:
   * Windows: `C:\Program Files\dotnet` or `C:\Program Files (x86)\dotnet`, or other default program files directory (depending on the environment variable `ProgramW6432`)
   * Unix: `/usr/share/dotnet`
   * Mac: `/usr/local/share/dotnet`
3. In paths specified in the `PATH` environment variable.

TeamCity will use the first .NET version it finds. If you have several .NET versions installed, we recommend that you specify the most recent version in the `DOTNET_HOME` variable.

## Build Runner Options

The set of available .NET build runner's options depends on the selected .NET command. Currently, the runner supports the following commands:

* CLI commands (read more in the [.NET Core guide](https://docs.microsoft.com/en-us/dotnet/core/tools/)):
  * Basic commands:
      * [`build`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-build)
      * [`clean`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-clean)
      * [`restore`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-restore)   
      (requires .NET CLI 2.1.400+ for authentication in private feeds)
      * [`pack`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-pack)
      * [`publish`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-publish)
      * [`run`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-run)
      * [`test`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-test)
   * Advanced commands:
     * msbuild
     * vstest
     * nuget delete
     * nuget push
* Visual Studio command-line mode (read more in the [Visual Studio reference](https://docs.microsoft.com/en-us/visualstudio/ide/reference/devenv-command-line-switches)):
  * devenv

### Basic CLI Commands

The basic .NET CLI commands have the following options:

<table><tr>

<td>

Option

</td>

<td>

Description

</td>

<td>

Commands

</td>

</tr>

<tr>

<td>

Projects

</td>

<td>

Paths to projects and solutions. Wildcards are supported. Parameter references are supported. If you have a finished build, you can use the file/directory selector here.

</td>

<td>

* All

</td>

</tr><tr>

<td>

Working directory

</td>

<td>

Optional, set if differs from the checkout directory. Parameter references are supported. If you have a finished build, you can use the file/directory selector here.

</td>

<td>

* All

</td>

</tr><tr>

<td>

Framework

</td>

<td>

Target framework, for example, `netcoreapp` or `netstandard`. Parameter references are supported.

</td>

<td>

* `build`
* `clean`
* `publish`
* `run`
* `test`

</td>
</tr>

<tr>

<td>

Configuration

</td>

<td>

Target configuration, for example, `Release` or `Debug`. Parameter references are supported.

</td>

<td>

* `build`
* `clean`
* `pack`
* `publish`
* `run`
* `test`

</td>

</tr><tr>

<td>

Runtime

</td>

<td>

Target runtime. Parameter references are supported.

</td>

<td>

* `build`
* `clean`
* `pack`
* `publish`
* `restore`
* `run`

</td>

</tr>

<tr>

<td>

Options

</td>

<td>

Declares not to build the projects before packing or testing.

</td>

<td>

* `pack`
* `test`

</td>

</tr>

<tr>

<td>

NuGet package sources

</td>

<td>

NuGet package sources to use during the restoration.

</td>

<td>

* `restore`

</td>

</tr>

<tr>

<td>

Output directory

</td>

<td>

The directory where to place outputs. Parameter references are supported. If you have a finished build, you can use the file/directory selector here.

</td>

<td>

* `build`
* `clean`
* `pack`
* `publish`
* `test`

</td>

</tr><tr>

<td>

Version suffix

</td>

<td>

Defines the value of the `$(VersionSuffix)` property in the project. Parameter references are supported.

</td>

<td>

* `build`
* `pack`
* `publish`

</td>

</tr><tr>

<td>

Command line parameters

</td>

<td>

[Additional command line parameters](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-build?tabs=netcore2x) for `dotnet`.

</td>

<td>

* All

</td></tr><tr>

<td>

Logging verbosity

</td>

<td>

Available logging modes: `<Default>`, `Minimal`, `Normal`, `Detailed`, or `Diagnostic`.

</td>

<td>

* All

</td>
</tr></table>

### Advanced CLI Commands

#### msbuild

#### vstest

#### nuget delete

#### nudget push

### Visual Studio command-line mode

## Docker Settings

The .NET CLI build step can be run in a specified [Docker container](docker-wrapper.md).

## Code Coverage

[JetBrains dotCover](jetbrains-dotcover.md) is supported as a coverage tool for the `msbuild`, `test`, and `vstest` commands.

## Authentication in private NuGet Feeds

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

The .NET Core SDK version.

</td></tr></table>

__ __
