[//]: # (title: NUnit Support)
[//]: # (auxiliary-id: NUnit Support)

There are two most common methods to set up NUnit tests reporting in TeamCity:
* using the [.NET CLI](net-cli-dotnet.md) build runner
* using the [NUnit](nunit.md) build runner

Besides that, you can try [alternative approaches](#Alternative+Approaches) or run tests in any other runner (like [PowerShell](powershell.md) or [Command Line](command-line.md)) with the [TeamCity VSTest Adapter](https://github.com/JetBrains/TeamCity.VSTest.TestAdapter).

## Framework Compatibility

The [NUnit](nunit.md) build runner supports only [.NET Framework](https://docs.microsoft.com/en-us/dotnet/framework/get-started/overview). To run tests for [.NET Core](https://docs.microsoft.com/en-us/dotnet/framework/get-started/net-core-and-open-source) projects (and .NET Framework projects version 4.0 or later), use the [.NET CLI (dotnet)](net-cli-dotnet.md) build runner with the [`test`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-test) command instead.

The following table represents the compatibility of TeamCity runners with the .NET implementations:

<table width="550">

<tr>

<td width="250">

</td>

<td width="100">

.NET Framework 1, 1.1, 2, 3.5

</td>

<td width="100">

.NET Framework 4+

</td>

<td width="100">

.NET Core 1+

</td>

</tr>

<tr>

<td>

__NUnit runner__
* `nunit-console.exe`
* `nunit3-console.exe`

</td>

<td>

![check.png](check.png)

</td>

<td>

![check.png](check.png)

</td>

<td>

![error.png](error.png)

</td>

</tr>

<tr>

<td>

__.NET CLI runner__
* `dotnest test`
* `dotnet msbuild /t:VSTest`
* `dotnet vstest`

</td>

<td>

![error.png](error.png)

</td>

<td>

![check.png](check.png)

</td>

<td>

![check.png](check.png)

</td>

</tr>

<tr>

<td>

__VSTest adapter in other runners__

</td>

<td>

![error.png](error.png)

</td>

<td>

![check.png](check.png)

</td>

<td>

![check.png](check.png)

</td>

</tr>

</table>

## Alternative Approaches

If using NUnit build runner is inapplicable, TeamCity provides the following ways to configure NUnit tests reporting in TeamCity:
* TeamCity supports the standard [NUnit for NAnt Build Runner](nunit-for-nant-build-runner.md).
* TeamCity provides the [NUnit for MSBuild](nunit-for-msbuild.md) and supports the [NUnit for MSBuild](nunit-for-msbuild.md) from [MSBuild Community tasks](http://msbuildtasks.tigris.org/).
* TeamCity provides its own NUnit Test Launcher that can be configured in the [MSBuild build script](nunit-for-msbuild.md) or launched from the [command line](teamcity-nunit-test-launcher.md).
* [TeamCity Addin for NUnit](teamcity-addin-for-nunit.md) is available to turn on reporting on the NUnit level without build procedure modifications. 
* The bundled [XML Test Reporting plugin](xml-report-processing.md) allows importing any XML report to TeamCity. In this case it is not always possible to track results on the fly.   
    You can add the __XML Report Processing__ build feature to your build configuration, or use the following service message: `##teamcity[importData type='sometype' path='<path to the xml file>']`. Learn more: [XML Report Processing](xml-report-processing.md), [Build Script Interaction with TeamCity](build-script-interaction-with-teamcity.md#Importing+XML+Reports).
* TeamCity allows configuring tests reporting manually via [service messages](build-script-interaction-with-teamcity.md).

### Feature Comparison for Alternative Approaches

<table><tr>

<td>

Approach


</td>

<td>

Real-time Reporting


</td>

<td>

Execution in CLI-based runners


</td>

<td>

Tests Reordering


</td>

<td>

Implicit TeamCity .NET Coverage


</td></tr><tr>

<td>

\<nunit2\> NAnt task


</td>

<td>

![check.png](check.png)

</td>

<td>

![check.png](check.png)/![error.png](error.png)\*


</td>

<td>

![check.png](check.png)

</td>

<td>

![check.png](check.png)

</td></tr><tr>

<td>

\<NUnit\> MSBuild task


</td>

<td>

![check.png](check.png)

</td>

<td>

![check.png](check.png)/![error.png](error.png)\*


</td>

<td>

![check.png](check.png)

</td>

<td>

![check.png](check.png)

</td></tr><tr>

<td>

\<NUnitTeamCity\> MSBuild task


</td>

<td>

![check.png](check.png)

</td>

<td>

![check.png](check.png)/![error.png](error.png)\*


</td>

<td>

![check.png](check.png)

</td>

<td>

![check.png](check.png)

</td></tr><tr>

<td>

TeamCity Addin for NUnit

</td>

<td>

![check.png](check.png)

</td>

<td>

![error.png](error.png)

</td>

<td>

![error.png](error.png)

</td>

<td>

![error.png](error.png)

</td></tr><tr>

<td>

TeamCity NUnit Test Launcher

</td>

<td>

![check.png](check.png)

</td>

<td>

![error.png](error.png)

</td>

<td>

![check.png](check.png)

</td>

<td>

![check.png](check.png)

</td></tr><tr>

<td>

XML Reporting Plugin

</td>

<td>

![error.png](error.png)

</td>

<td>

![check.png](check.png)

</td>

<td>

N/A

</td>

<td>

N/A


</td></tr></table>

__\*__ TeamCity-provided tasks may have different syntax/behavior. Some workarounds may be required to run the script without TeamCity.

In addition to the common test reporting features, TeamCity allows running NUnit tests under the x86 process on the x64 machine by introducing an explicit specification of the platform and runtime environment versions. You can define whether to use .NET Framework 1.1, 2.0 or 4.0 started under an MSIL, x64 or x86 platform.

 __  __

__See also:__


__Administrator's Guide__: [NUnit build runner](nunit.md) | [Getting Started with NUnit](getting-started-with-nunit.md) | [MSTest Support](mstest-support.md) | [Running Risk Group Tests First](running-risk-group-tests-first.md) | [XML Report Processing](xml-report-processing.md)

__ __