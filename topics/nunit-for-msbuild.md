[//]: # (title: NUnit for MSBuild)
[//]: # (auxiliary-id: NUnit for MSBuild)

This page describes how to use NUnit from MS build.
* for __NUnit prior to 3.0__ see [Working with TeamCity-provided NUnit task](#Working+with+NUnit+Task+in+MSBuild+Build)
* for __NUnit 3.0 and above__ see [Working with NUnit 3.0](#Working+with+NUnit+3.0)

## Working with NUnit Task in MSBuild Build

The information in this section is applicable if you are using __NUnit prior to version 3.0__. For later versions, refer to the [section below](#Working+with+NUnit+3.0).

This section assumes that you already have an MSBuild build script with a configured `NUnit` task in it, and want TeamCity to track test reports without making any changes to the existing build script. Otherwise, consider adding [NUnit build runner](nunit.md) as one of the steps for your build configuration.

### Using NUnitTeamCity task in MSBuild Build Script

TeamCity provides a custom `NUnitTeamCity` task compatible with the `NUnit` task from [MSBuild Community tasks](http://msbuildtasks.tigris.org/) project. If you provide the `NUnitTeamCity` task in your build script, TeamCity will launch its own test runner based on the options specified within the task. Thus, you do not need to have any NUnit runner, because TeamCity will run the tests.

In order to correctly use the `NUnitTeamCity` task, perform the following steps:
1. Make sure the `teamcity_dotnet_nunitlauncher` system property is accessible on build agents. Build agents running Windows should automatically detect these properties as environment variables. If you need to set them manually, see defining [agent-specific](project-and-agent-level-build-parameters.md#Agent+Level+Build+Parameters) properties for more information.
2. Configure your MSBuild build script with the `NUnitTeamCity` task using the following syntax:   
   ```XML
   <UsingTask TaskName="NUnitTeamCity" AssemblyFile="$(teamcity_dotnet_nunitlauncher_msbuild_task)" />

   <NUnitTeamCity Assemblies="@(assemblies_to_test)" />

   ```

The `NUnitTeamCity` task supports the following attributes:

<table><tr>

<td width="200">

Property

</td>

<td>

Description

</td></tr><tr>

<td>

`Platform`

</td>

<td>

Execution mode on a x64 machine. Supported values are: __x86__, __x64__, and __ANY__.

</td></tr><tr>

<td>

`RuntimeVersion`

</td>

<td>

.NET Framework to use: __v1.1__, __v2.0__, __v4.0__, __ANY__. By default, the MSBuild runtime is used. Default is __v2.0__ for MSBuild 2.0 and 3.5. For MSBuild 4.0 the default value is __v4.0__.

</td></tr><tr>

<td>

`IncludeCategory`

</td>

<td>

As used in the `NUnit` task from [MSBuild Community tasks](http://msbuildtasks.tigris.org/) project.

</td></tr><tr>

<td>

`ExcludeCategory`

</td>

<td>

As used in the `NUnit` task from [MSBuild Community tasks](http://msbuildtasks.tigris.org/) project.

</td></tr><tr>

<td>

`NUnitVersion`

</td>

<td>

Version of NUnit to be used to run the tests. Supported NUnit versions: __2.2.10__, __2.4.1__, __2.4.6__, __2.4.7__, __2.4.8__, __2.5.0__, __2.5.2__, __2.5.3__, __2.5.4__, __2.5.5__, __2.5.6__, __2.5.7__, __2.5.8__, __2.5.9__, __2.5.10__, __2.6.0__, __2.6.1__,__2.6.2__, __2.6.3__. For example, `NUnit-2.2.10`.

To use NUnit 3.0 and above, see the [section below](#Working+with+NUnit+3.0).

</td></tr><tr>

<td>

`Addins`

</td>

<td>

List of third-party NUnit addins to be used. For more information on using NUnit addins, refer to the [NUnit Addins Support](nunit-addins-support.md) page.

</td></tr><tr>

<td>

`HaltIfTestFailed`

</td>

<td>

__True__ to fail task, if any test fails.

</td></tr><tr>

<td>

`Assemblies`

</td>

<td>

List of assemblies to run tests with.

</td></tr><tr>

<td>

`RunProcessPerAssembly`

</td>

<td>

Set __true__, if you want to run each assembly in a new process.


</td></tr></table>


The custom TeamCity `NUnit` task also supports additional attributes. For the list of available attributes refer to [this section](#Using+NUnitTeamCity+task+in+MSBuild+Build+Script).

If you need the TeamCity test runner to support third-party NUnit addins, refer to the [NUnit Addins Support](nunit-addins-support.md) section for the details.

Example (part of the MSBuild build script):

```XML
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <UsingTask TaskName="NUnitTeamCity" AssemblyFile="$(teamcity_dotnet_nunitlauncher_msbuild_task)"/>
 
  <Target Name="SayHello">
     <NUnitTeamCity Assemblies="!!!*put here item group of assemblies to run tests on*!!!"/>
  </Target>
</Project>

```

__Important notes__
* Be sure to replace "`.`" with "`_`" when [using System Properties](configuring-build-parameters.md#Using+Build+Parameters+in+the+Build+Scripts) in MSBuild scripts. For example, use `teamcity_dotnet_nunitlauncher_msbuild_task` instead of `teamcity.dotnet.nunitlauncher.msbuild.task`.
* TeamCity also provides [Visual Studio Solution Runner](visual-studio-sln.md) for solution files of Microsoft Visual Studio 2005 and above. It allows you to use MSBuild-style wildcards for the assemblies to run unit tests on.

### Examples

Run NUnit tests using specific NUnit runner version:

```XML
<Target Name="build_01">
     <!-- start tests for NUnit-2.2.10 -->
     <NUnitTeamCity Assemblies="@(TestAssembly)" NUnitVersion="NUnit-2.2.10"/>
 
     <!-- start tests for NUnit-2.4.6 -->
     <NUnitTeamCity Assemblies="@(TestAssembly)" NUnitVersion="NUnit-2.4.8"/>
  </Target>

```

Run NUnit tests with custom addins with NUnit 2.4.6:

```XML
<Target Name="build">
     <NUnitTeamCity Assemblies="@(TestAssembly)" Addins="NUnitExtension.RowTest.AddIn.dll" NUnitVersion="NUnit-2.4.6"/>
  </Target>

```

Run NUnit tests with custom addins with NUnit 2.4.6 __in per-assembly mode:__

```XML
<Target Name="build">
     <NUnitTeamCity Assemblies="@(TestAssembly)" Addins="NUnitExtension.RowTest.AddIn.dll" NUnitVersion="NUnit-2.4.6" RunProcessPerAssembly="True"/>
</Target>

```

To make a TeamCity independent build script, consider the following trick:

```XML
<NUnitTeamCity ... Condition=" '$(TEAMCITY_VERSION)' != '' "/>

```

The MSBuild property `TEAMCITY_VERSION` is added to msbuild when started from TeamCity.

## Working with NUnit 3.0

The information in this section is applicable if you are using NUnit 3.0 and above. For earlier versions of NUnit, refer to the [section above](#Working+with+NUnit+Task+in+MSBuild+Build).

Starting from version 3.0, NUnit supports TeamCity natively, so there is no need to use a special task for MSBuild as it was done for the [earlier NUnit versions](#Working+with+NUnit+Task+in+MSBuild+Build). The simplest way is to run the NUnit console via the standard [Exec task](https://msdn.microsoft.com/en-us/library/x8zx72cd.aspx). For example:

<include src="getting-started-with-nunit.md" include-id="msbuild-examples-nunit"/>

<note>

The MSBuild runner option [Reduce test failure feedback time](msbuild.md#General+Build+Runner+Options) __will not work__ out of the box in this case. To use this feature, configure the [NUnit](nunit.md) build step.
</note>

[Getting Started with NUnit](getting-started-with-nunit.md) contains details and more examples.
