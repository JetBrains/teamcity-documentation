[//]: # (title: NUnit Support)
[//]: # (auxiliary-id: NUnit Support;TeamCity NUnit Test Launcher;NUnit for NAnt Build Runner)

There are two most common methods to set up NUnit tests reporting in TeamCity:
* Use the [.NET](net.md) build runner.
* Use the [NUnit](nunit.md) build runner.

Besides that, you can try [alternative approaches](#Alternative+Approaches) or run tests in any other runner (like [PowerShell](powershell.md) or [Command Line](command-line.md)) with the [TeamCity VSTest Adapter](https://github.com/JetBrains/TeamCity.VSTest.TestAdapter).

## Supported NUnit Versions

The following NUnit versions are supported: 2.2.10, 2.4.1, 2.4.6, 2.4.7, 2.4.8, 2.5.0, 2.5.2, 2.5.3, 2.5.4, 2.5.5, 2.5.6, 2.5.7, 2.5.8, 2.5.9, 2.5.10, 2.6.0, 2.6.1, 2.6.2, 2.6.3, 3.0.

It is possible to have several versions of NUnit installed on an agent machine and use any of them in a build.

<warning>

NUnit version __3.4.0__ is __not__ supported by the NUnit build runner due to a problem in [NUnit](https://github.com/nunit/docs/wiki/Release-Notes#issues-resolved-1). Only version 3.4.0 was affected, other NUnit 3.x versions work fine with TeamCity.
</warning>

Note that this runner supports only [.NET Framework](https://docs.microsoft.com/en-us/dotnet/framework/get-started/overview). To run tests for [.NET Core](https://docs.microsoft.com/en-us/dotnet/framework/get-started/net-core-and-open-source) projects (and .NET Framework projects version 4.0 or later), use the [.NET](net.md) build runner with the [`test`](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-test) command instead.

## NUnit Framework Compatibility

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

__.NET runner__
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
* The standard [NUnit for NAnt Build Runner](nunit-for-nant-build-runner.md).
* [NUnit for MSBuild](nunit-for-msbuild.md).
* The NUnit Test Launcher that can be configured in the [MSBuild build script](nunit-for-msbuild.md) or launched from the [command line](nunit-support.md#NUnit+Test+Launcher).
* [TeamCity Add-in for NUnit](teamcity-addin-for-nunit.md) is available to turn on reporting on the NUnit level without build procedure modifications. 
* The bundled [XML Test Reporting plugin](xml-report-processing.md) allows importing any XML report to TeamCity. In this case, it is not always possible to track results on the fly.   
    You can add the __XML Report Processing__ build feature to your build configuration, or use the following service message: `##teamcity[importData type='sometype' path='<path to the xml file>']`. Learn more: [XML Report Processing](xml-report-processing.md), [Importing XML Reports](service-messages.md#Importing+XML+Reports).
* Configuring test reports manually via [service messages](build-script-interaction-with-teamcity.md).

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

TeamCity Add-in for NUnit

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

### NUnit Test Launcher
{initial-collapse-state="collapsed"}

TeamCity provides its own NUnit tests launcher that can be used from the command line. The tests are run according to the passed parameters and, if the process is run inside the TeamCity build agent environment, the results are reported to the TeamCity agent.

>__Tips__:
>* If you need to access the path to the TeamCity NUnit launcher from some process, you can add the `%\system.teamcity.dotnet.nunitlauncher%` [environment variable](configuring-build-parameters.md).
>* Values surrounded with `%` in custom scripts in the [Command Line runner](command-line.md) are treated as TeamCity references.
> 

You can pass the following command-line options to the TeamCity NUnit Test Launcher:

```Shell
${teamcity.dotnet.nunitlauncher} <.NET Framework> <platform> <NUnit vers.> [/category-include:<list>] [/category-exclude:<list>] [/addin:<list>] <assemblies to test>
```

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

`<.NET Framework>`

</td>

<td>

Version of .NET Framework to run tests. Acceptable values are __v1.1__, __v2.0__, __v4.0__, and __ANY__.

</td></tr><tr>

<td>

`<platform>`

</td>

<td>

Platform to run tests. Acceptable values are __x86__, __x64__, and __MSIL__.

For .NET Framework 1.1, only __MSIL__ option is available.

</td></tr><tr>

<td>

`<NUnit vers.>`


</td>

<td>

The test framework to use. The value has to be specified in the following format: `NUnit-<version>`.

</td></tr><tr>

<td>

`/category-include:<list>`

</td>

<td>

The list of categories separated by `,` (optional).

</td></tr><tr>

<td>

`/category-exclude:<list>`

</td>

<td>

The list of categories separated by `,` (optional).

</td></tr><tr>

<td>

`/addin:<list>`

</td>

<td>

List of third-party NUnit add-ins to use (optional).

</td></tr><tr>

<td>

`<assemblies to test>`

</td>

<td>

List of assembly paths separated by `;` or whitespace.

</td></tr><tr>

<td>

`/runAssemblies:processPerAssembly`

</td>

<td>

Specify to run each assembly in a new process.

</td></tr></table>

#### Category Expression

Beginning with NUnit 2.4.6 and up to but not including NUnit v3.0, a _category expression_ can be used. This table shows some examples:

<table><tr>

<td>

Expression

</td>

<td>

Action

</td></tr><tr>

<td>

A|B|C

</td>

<td>

Selects tests having any of the categories A, B or C.

</td></tr><tr>

<td>

A,B,C

</td>

<td>

Selects tests having any of the categories A, B or C.

</td></tr><tr>

<td>

A+B+C

</td>

<td>

Selects only tests having all three categories assigned.

</td></tr><tr>

<td>

A+B|C

</td>

<td>

Selects tests with both A and B OR with category C.

</td></tr><tr>

<td>

A+B\-C

</td>

<td>

Selects tests with both A and B but not C.

</td></tr><tr>

<td>

-A

</td>

<td>

Selects tests not having category A assigned.

</td></tr><tr>

<td>

A+(B|C)

</td>

<td>

Selects tests having both category A and either of B or C.

</td></tr><tr>

<td>

A+B,C

</td>

<td>

Selects tests having both category A and either of B or C.

</td></tr></table>

__Note:__ As shown by the last two examples, the comma operator (`,`) is equivalent to the pipe (`|`) but has a higher precedence. The order of evaluation is as follows:
1. Unary exclusion operator (`-`).
2. High-precedence union operator (`,`).
3. Intersection and set subtraction operators (`+` and binary `-`).
4. Low-precedence union operator (`|`).

Since the operator characters have special meaning, avoid creating a category that uses any of them in its name. For example, the category `db-tests` must not be used in the command line, as it appears to mean "run category db, except for category tests". The same limitation applies to the characters having a special meaning for the shell you are using.

The following examples assume that the `teamcity.dotnet.nunitlauncher` property is set as a system property on the __[Parameters](configuring-build-parameters.md)__ page of __Build Configuration Settings__.

Run tests from an assembly:

```Shell
%\teamcity.dotnet.nunitlauncher% v2.0 x64 NUnit-2.2.10 Assembly.dll

```

Run tests from an assembly with the NUnit categories filter:

```Shell
%\teamcity.dotnet.nunitlauncher% v2.0 x64 NUnit-2.2.10 /category-include:C1 /category-exclude:C2 Assembly.dll

```

Run tests from assemblies:

```Shell
%\teamcity.dotnet.nunitlauncher% v2.0 x64 NUnit-2.5.0 /addin:Addin1.dll;Addin2.dll Assembly.dll Assebly2.dll

```

[//]: # (Internal note. Do not delete. "TeamCity NUnit Test Launcherd319e337.txt")

### NUnit for NAnt Build Runner
{initial-collapse-state="collapsed"}

This section assumes, that you already have a NAnt build script with the configured `nunit2` task in it, and want TeamCity to track test reports without making any changes to the existing build script. Otherwise, consider adding the [NUnit build runner](nunit.md) as one of the steps for your build configuration.

To track tests defined in a NAnt build via the standard `nunt2` task, TeamCity provides a custom [task](http://nant.sourceforge.net/nightly/latest/help/tasks/nunit2.html) implementation and automatically replaces the original `<nunit2>` task with its own task. When the build is triggered, TeamCity starts TeamCity NUnit Test Launcher using own implementation of `<nunit2>`. This allows you to leave your the build script without changes and receive on-the-fly test reports in TeamCity.

If you don't want TeamCity to replace the original `nunit2` task, consider the following options:
* Use the NUnit console with TeamCity Add-in for NUnit.
* Import XML test results via the XML Test Report plugin.
* Use the command-line [NUnit Test Launcher](#NUnit+Test+Launcher).
* Configure reporting tests manually via [service messages](service-messages.md).
* To disable `nunit2` task replacement, set the `teamcity.dotnet.nant.replaceTasks` [system property](configuring-build-parameters.md) to `false`.

The `nunt2` task implementation in TeamCity supports additional options that can be specified either as NAnt `<property>` tasks in the build script, or as <emphasis tooltip="system-property">system properties</emphasis> under __Build Configuration | Build Parameters__.

The following options are supported for the TeamCity `<nunit2>` task implementation:

<table><tr>

<td>

Property

</td>

<td>

Description

</td></tr><tr>

<td>

`teamcity.dotnet.nant.nunit2.failonfailureatend`

</td>

<td>

Runs __all tests__ regardless of the number of failed ones. Fails if at least one test has failed.

</td></tr><tr>

<td>

`teamcity.dotnet.nant.nunit2.platform`

</td>

<td>

Sets desired runtime execution mode for .NET 2.0 on x64 machines. Supported values are __x86__, __x64__, and __ANY__(default).

</td></tr><tr>

<td>

`teamcity.dotnet.nant.nunit2.platformVersion`


</td>

<td>

Sets the required .NET Framework version. Supported values are __v1.1__, __v2.0__, __v4.0__. The default value is equal to the NAnt target framework.

</td></tr><tr>

<td>

`teamcity.dotnet.nant.nunit2.version`

</td>

<td>

Specifies which version of the NUnit runner to use. The value has to be specified in the following format: `NUnit-<version>`.

It is possible to have several versions of NUnit installed on an agent machine and use any of them in a build.

</td></tr><tr>

<td>

`teamcity.dotnet.nant.nunit2.addins`

</td>

<td>

Specifies the list of third-party NUnit add-ins used for NAnt build runner.

</td></tr><tr>

<td>

`teamcity.dotnet.nant.nunit2.runProcessPerAssembly`

</td>

<td>

Set `true` if you want to run each assembly in a new process.

</td></tr></table>

TeamCity NUnit test launcher will run tests in the .NET Framework, which is specified by the NAnt target framework: that is, on .NET Framework 1.1, 2.0 or 4.0 runtime. TeamCity also supports test categories for the `<nunit2>` task.

Adding the listed properties to the NAnt build script makes it TeamCity-dependent. To avoid this, specify properties as system properties under __Build Configuration__, or consider adding the `<if>` task.

>If you need a TeamCity test runner to support third-party NUnit add-ins, refer to the [NUnit Addins Support](nunit-addins-support.md) section for the details.

#### Examples

Start tests form a single assembly files under x64 mode on .NET 2.0.

```XML
<property name="teamcity.dotnet.nant.nunit2.platform" value="x64" />
<nunit2>
    <formatter type="Plain" />
    <test assemblyname="MyProject.Tests.dll" />
</nunit2>

```

Run all tests from category C1, but not C2.

```XML
<nunit2 verbose="true" haltonfailure="false" failonerror="true">
         <formatter type="Plain" />
          <test>
            <assemblies>
              <include name="dll.dll" />
            </assemblies>
            <categories>
               <include name="C1" />
               <exclude name="C2"/>
            </categories>
          </test>
        </nunit2>

```

Explicitly specify the version on NUnit to run tests with.   
Note, that in this case, the following property should be added __before__ the `nunit2` task call.

```XML
<property name="teamcity.dotnet.nant.nunit2.version" value="NUnit-2.4.10" />
<nunit2> <!--....--> </nunit2>

```



<seealso>
        <category ref="admin-guide">
            <a href="nunit.md">NUnit build runner</a>
            <a href="getting-started-with-nunit.md">Getting Started with NUnit</a>
            <a href="mstest-support.md">MSTest Support</a>
            <a href="running-risk-group-tests-first.md">Running Risk Group Tests First</a>
            <a href="xml-report-processing.md">XML Report Processing</a>
        </category>
</seealso>