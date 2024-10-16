[//]: # (title: Inspections \(ReSharper\))
[//]: # (auxiliary-id: viewpage.actionpageId113084113;Inspections \(ReSharper\))

The _Inspections (ReSharper)_ build runner allows you to use the benefits of the [JetBrains ReSharper code quality analysis](https://www.jetbrains.com/resharper/webhelp/Code_Analysis__Index.html) feature right in TeamCity, with the help of the bundled JetBrains ReSharper Command Line Tools. You can use the tools within TeamCity without any additional ReSharper license.   
[ReSharper](https://www.jetbrains.com/resharper) analyzes your C#, VB.NET, XAML, XML, ASP.NET, ASP.NET MVC, JavaScript, HTML, CSS code, and allows you to:
* Find probable bugs
* Eliminate errors and code smells
* Detect performance issues
* Improve the code structure and maintainability
* Ensure the code conforms to guidelines, standards and specifications

ReSharper command line tools 2018.2 or newer require .NET Framework 4.6.1 or newer.

If you want to run ReSharper inspections using a specific ReSharper version (for example, to ensure it matches the version you have installed in Visual Studio), you can install another version of the tools and change the default version to be used on the __[Administration | Tools](installing-agent-tools.md)__ page. This page contains reference information about the _Inspections (.NET)_ build runner's fields.
{instance="tc"}

You can also refer to the [ReSharper documentation](https://www.jetbrains.com/help/resharper/Detect_code_issues_in_a_build_using_ReSharper_and_TeamCity.html) for more details.

<note>

To run inspections for your project, you must have a ReSharper inspection profile for .NET projects.
</note>

Refer to [Configuring Build Steps](configuring-build-steps.md) for a description of common build steps' settings. Refer to [](container-wrapper.md) to learn how you can run this step inside a Docker container.

## Sources to Analyze

<table><tr>

<td>

Option


</td>

<td>

Description

</td></tr><tr>

<td>

Solution file path

</td>

<td>

The path to the `.sln` file created by Microsoft Visual Studio __2005 or later__.   
The specified path should be relative to the checkout directory.

</td></tr><tr>

<td>

Projects filter

</td>

<td>

Specify project name wildcards to analyze only a part of the solution. Leave _blank_ to analyze the _whole_ solution.  Separate wildcards with new lines.   
Example:

```Shell
JetBrains.CommandLine.*
*.Common
*.Tests.*

```

</td></tr></table>

## Environment Requirements

<note>

To launch inspection analysis, you should have __.NET Framework 4.6.1__ (or higher) installed on an agent where builds will run.
</note>

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td id="targetFramework">

Target Frameworks

</td>

<td>

This option allows you to handle the [Visual Studio Multi-Targeting](https://msdn.microsoft.com/en-us/library/bb398197.aspx) feature.   
Agent requirement will be created for every checked item.

.NET Framework versions 2.0–4.8.1 are supported.

<note>

__.NET Frameworks client profiles__ are not supported as target frameworks.
</note>

</td></tr></table>

## JetBrains ReSharper Command Line Tools Settings

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

R# CLT Home Directory

</td>

<td>

Select the ReSharper Command Line Tools version.

You can check the installed JetBrains ReSharper Command Line Tools versions on the __[Administration | Tools](installing-agent-tools.md)__ page.
{instance="tc"}

<img src="dk-tools.png" width="706" alt="Check the version of installed Resharper tools"/>
{instance="tc"}

If you want to run ReSharper duplicates using a specific ReSharper version (for example, to ensure it matches the version you have installed in Visual Studio), you can use this page to install another version of the tools and change the default version to be used.
{instance="tc"}


</td></tr>


<tr>

<td>R# CLT Plugins</td>

<td>

The list of additional [ReSharper plugins](https://plugins.jetbrains.com/resharper) to install. This field accepts values in the `pluginID/version` format, for example: `StyleCop.StyleCop/2023.2.0`.

* `pluginID`s can be found on main plugin pages in [JetBrains Marketplace](https://plugins.jetbrains.com/resharper) (the "Additional Information" section of the "Overview" tab).
* `version` should be in the same format as on the "Versions" tab of the plugin Marketplace page. Omitting the version allows ReSharper to automatically fetch the latest.

If you want to install plugins from local storage rather than download it from a JetBrains Marketplace, set the **Additional InspectCode parameters** field to the `--source=<Directory_Path>` value.

<img src="dk-resharperPlugins-source.png" width="706" alt="Install plugins from local storage"/>

Note that the `--source` parameter affects all plugins, so you cannot mix Marketplace downloads and local storage installs.

> * Some of ReSharper plugins may not work for R# CLT.
> * We recommend using the "R# CLT Plugins" option for ReSharper CLT 2021.3.0 and newer. Older versions may incorrectly interpret values from this field.
>
{style="note"}


</td>



</tr>



<tr>

<td>

InspectCode Platform

</td>

<td id="ReSharperInspectionsPlatform" auxiliary-id="ReSharperInspectionsPlatform">

Select the platform bitness of the InspectCode tool. To find code issues in C++ projects, use the x86 platform. The cross-platform inspections are also supported in ReSharper 2020.2.1 or later.

</td></tr></table>

## InspectCode Options

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td id="settings">

Custom settings profile path

</td>

<td>

The path to the file containing __ReSharper settings__ created with JetBrains ReSharper __6.1 or later__.   
The specified path should be __relative__ to the checkout directory.   
If specified, this settings layer has the top priority, so it overrides ReSharper build-in settings. __By default__, __build-in__ ReSharper settings layers are applied.
 
For additional information about the ReSharper settings system, see [ReSharper Web Help](https://www.jetbrains.com/resharper/webhelp/Configuring_ReSharper__Sharing_Configuration_Options.html) and [JetBrains .NET Tools Blog](https://blog.jetbrains.com/dotnet/)


</td></tr><tr>

<td id="debug">

Enable debug output

</td>

<td>

Check this option to include debug messages in the build log and publish the file with additional logs (`dotnet-tools-inspectcode.log`) as a hidden artifact.


</td></tr><tr>

<td id="cmdArgs">

Additional inspectCode.exe arguments

</td>

<td>

Specify newline-separated command line parameters to add to calling `inspectCode.exe`.

<note>

Only XML reports are supported by the runner.   
To get the output XML report, specify the path to the output file here via the `-o` or `–output` additional command line arguments. The paths relative to the [build checkout directory](build-checkout-directory.md) as well as absolute paths are supported. 
</note>


</td></tr></table>

## Build Failure Conditions

If a build has too many inspection errors or warnings, you can configure it to fail by setting a [build failure condition](build-failure-conditions.md).

## Build before analyze

In order to have adequate inspections' execution results, you may need to __build your solution before running analysis__. This pre-step is especially actual when you use (implicitly or explicitly) __code generation__ in your project.

<note>
 
Starting from version 2021.2, R# InspectCode builds the target solution by default before starting the analysis.

</note>

### Bundled ReSharper Versions

<table><tr>

<td>

TeamCity Version


</td>

<td>

ReSharper Version


</td></tr><tr>

<td>

2018.1

</td>

<td>

2018.1.2

</td></tr><tr>

<td>

2018.2

</td>

<td>

2018.1.4

</td></tr><tr>

<td>

2019.1

</td>

<td>

2019.1.1

</td></tr><tr>

<td>

2019.2

</td>

<td>

2019.2.3

</td></tr>

<tr>

<td>

2020.1

</td>

<td>

2019.2.3

</td></tr>

<tr>

<td>

2020.2

</td>

<td>

2020.2.4

</td></tr>

<tr>

<td>

2021.1

</td>

<td>

2021.1.2

</td></tr>

<tr>

<td>

2021.2

</td>

<td>

2021.1.2

</td></tr>

<tr>

<td>

2022.04

</td>

<td>

2021.1.2

</td></tr>

<tr>

<td>

2022.10

</td>

<td>

2021.1.2

</td></tr>

<tr>

<td>

2023.05

</td>

<td>

2021.1.2

</td></tr>

<tr>

<td>

2023.11

</td>

<td>

2023.1.1

</td></tr>

</table>

You can view the installed versions of ReSharper on the __Server Administration | Tools__ page. The bundled version is set as default; you can install other versions and change the default settings.

[//]: # (Internal note. Do not delete. "Inspections ReSharper d165e293.txt")    
