[//]: # (title: TeamCity NUnit Test Launcher)
[//]: # (auxiliary-id: TeamCity NUnit Test Launcher)

TeamCity provides its own NUnit tests launcher that can be used from the command line. The tests are run according to the passed parameters and, if the process is run inside the TeamCity build agent environment, the results are reported to the TeamCity agent.

<include src="nunit.md" include-id="supported-versions"/>

<note>

* If you need to access the path to the TeamCity NUnit launcher from some process, you can add the `%system.teamcity.dotnet.nunitlauncher%` [environment variable](configuring-build-parameters.md).
* Values surrounded with `%` in custom scripts in the [Command Line runner](command-line.md) are treated as TeamCity references.

</note>

You can pass the following command line options to the TeamCity NUnit Test Launcher:

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

\<.NET Framework\>

</td>

<td>

Version of .NET Framework to run tests. Acceptable values are __v1.1__, __v2.0__, __v4.0__, and __ANY__.

</td></tr><tr>

<td>

\<platform\>


</td>

<td>

Platform to run tests. Acceptable values are __x86__, __x64__, and __MSIL__.

<note>

For .NET Framework 1.1 only __MSIL__ option is available.
</note>


</td></tr><tr>

<td>

\<NUnit vers.\>


</td>

<td>

Test framework to use. The value has to be specified in the following format: __NUnit-\<version\>__.

</td></tr><tr>

<td>

/category-include:\<list\>


</td>

<td>

The list of categories separated by "`,`" (optional). Category expression are supported since [NUnit v2.4.6](http://www.nunit.org/index.php?p=consoleCommandLine&amp;r=2.4.6) to NUnit v3.0 not inclusive.

</td></tr><tr>

<td>

/category-exclude:\<list\>


</td>

<td>

The list of categories separated by "`,`" (optional). Category expression are supported since [NUnit v2.4.6](http://www.nunit.org/index.php?p=consoleCommandLine&amp;r=2.4.6) to NUnit v3.0 not inclusive.


</td></tr><tr>

<td>

/addin:\<list\>


</td>

<td>

List of third-party NUnit addins to use (optional).


</td></tr><tr>

<td>

\<assemblies to test\>


</td>

<td>

List of assemblies paths separated by "`;`" or space.


</td></tr><tr>

<td>

/runAssemblies:processPerAssembly


</td>

<td>

Specify, if you want to run each assembly in a new process.


</td></tr></table>

### Category Expression

Beginning with NUnit 2.4.6 and up to but not including NUnit v3.0, a __Category Expression__ can be used. The table shows some examples:

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

Selects only tests having all three of the categories assigned.

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

__Note:__ As shown by the last two examples, the comma operator (`,`) is equivalent to the pipe (`|`) but has a higher precendence. The order of evaluation is as follows:

1. Unary exclusion operator (`-`)
2. High-precendence union operator (`,`)
3. Intersection and set subtraction operators (`+` and binary `-`)
4. Low-precedence union operator (`|`)

__Note:__ Since the operator characters have special meaning, avoid creating a category that uses any of them in its name. For example, the category `db-tests` must not be used in the command line, as it appears to mean "run category db, except for category tests". The same limitation applies to the characters having a special meaning for the shell you are using.

__Examples__

The following examples assume that the `teamcity.dotnet.nunitlauncher` property is set as a system property on the [Parameters](configuring-build-parameters.md) page of the Build Configuration.

Run tests from an assembly:

```Shell
%teamcity.dotnet.nunitlauncher% v2.0 x64 NUnit-2.2.10 Assembly.dll

```

Run tests from an assembly with NUnit categories filter

```Shell
%teamcity.dotnet.nunitlauncher% v2.0 x64 NUnit-2.2.10 /category-include:C1 /category-exclude:C2 Assembly.dll

```

Run tests from assemblies:

```Shell
%teamcity.dotnet.nunitlauncher% v2.0 x64 NUnit-2.5.0 /addin:Addin1.dll;Addin2.dll Assembly.dll Assebly2.dll

```

[//]: # (Internal note. Do not delete. "TeamCity NUnit Test Launcherd319e337.txt")