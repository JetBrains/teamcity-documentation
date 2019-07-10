[//]: # (title: NUnit Support)
[//]: # (auxiliary-id: NUnit Support)

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## NUnit runner

The easiest way to set up NUnit tests reporting in TeamCity is to add [NUnit build runner](nunit.md) as one of the steps to your [build configuration](creating-and-editing-build-configurations.md) making sure the [requirements](nunit.md) are met and specify there all the required parameters.


<tip include-id="supported-versions">
    
Supported NUnit versions: __2.2.10__, __2.4.1__, __2.4.6__, __2.4.7__, __2.4.8__, __2.5.0__, __2.5.2__, __2.5.3__, __2.5.4__, __2.5.5__, __2.5.6__, __2.5.7__, __2.5.8__, __2.5.9__, __2.5.10__, __2.6.0__, __2.6.1__, __2.6.2__, __2.6.3__. 

__Since TeamCity 9.1__, NUnit __3.0 and above__ is also supported.

It is possible to have several versions of NUnit installed on an agent machine and use any of them in a build.

</tip>    

<warning include-id="supported-warning">
    
NUnit version __3.4.0__ is __not__ supported by the NUnit build runner due to a problem in [NUnit](https://github.com/nunit/docs/wiki/Release-Notes#issues-resolved-1). Only version 3.4.0 was affected, other NUnit 3.x versions work fine with TeamCity.
</warning>

   
For details refer to [NUnit build runner](nunit.md) page.

## Alternative approaches

If using NUnit build runner is inapplicable, TeamCity provides the following ways to configure NUnit tests reporting in TeamCity:
* TeamCity supports the standard [NUnit for NAnt Build Runner](nunit-for-nant-build-runner.md).
* TeamCity provides the [NUnit for MSBuild](nunit-for-msbuild.md) and supports the [NUnit for MSBuild](nunit-for-msbuild.md) from [MSBuild Community tasks](http://msbuildtasks.tigris.org/).
* TeamCity provides its own NUnit Test Launcher that can be configured [in the MSBuild build script](nunit-for-msbuild.md) or launched from the [command line](teamcity-nunit-test-launcher.md).
* [TeamCity Addin for NUnit](teamcity-addin-for-nunit.md) is available to turn on reporting on the NUnit level without build procedure modifications. 
* The bundled [XML Test Reporting plugin](xml-report-processing.md) allows importing any xml report to TeamCity. In this case it is not always possible to track results on the fly.   
    You can add the __XML Report Processing__ build feature to your build configuration, or use the following service message: `##teamcity[importData type='sometype' path='<path to the xml file>'`. Learn more: [XML Report Processing](xml-report-processing.md), [Build Script Interaction with TeamCity](build-script-interaction-with-teamcity.md).
* TeamCity allows configuring tests reporting manually via [service messages](build-script-interaction-with-teamcity.md).

## Comparison matrix:

<table><tr>

<td>

Approach


</td>

<td>

Real\-Time Reporting


</td>

<td>

Execution without TeamCity


</td>

<td>

Tests Reordering


</td>

<td>

Implicit TeamCity .NET Coverage


</td></tr><tr>

<td>

NUnit runner


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

`<nunit2>` NAnt task


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

`<NUnit>` MSBuild task


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

`<NUnitTeamCity>` MSBuild task


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

only xml


</td>

<td>

N/A


</td>

<td>

N/A


</td></tr></table>

__\*__ TeamCity\-provided tasks may have different syntax/behavior. Some workarounds may be required to run the script without TeamCity.

In addition to the common test reporting features, TeamCity relieves a headache of running your NUnit tests under x86 process on the x64 machine by introducing an explicit specification of the platform and runtime environment versions. You can define whether to use .NET Framework 1.1, 2.0 or 4.0 started under a MSIL, x64 or x86 platform.



 __  __

__See also:__



__Administrator's Guide__: [NUnit build runner](nunit.md) | [Getting Started with NUnit](getting-started-with-nunit.md) | [MSTest Support](mstest-support.md) | [Running Risk Group Tests First](running-risk-group-tests-first.md) | [XML Report Processing](xml-report-processing.md)
