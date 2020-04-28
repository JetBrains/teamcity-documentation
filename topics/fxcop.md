[//]: # (title: FxCop)
[//]: # (auxiliary-id: FxCop)

The [FxCop](https://docs.microsoft.com/en-us/previous-versions/dotnet/netframework-3.0/bb429476(v=vs.80)) _Build Runner_ is intended for inspecting .NET assemblies and reporting possible design, localization, performance, and security improvements. If you want TeamCity to display FxCop reports, you can either configure the corresponding build runner, or import XML reports by means of service messages if you prefer to run the FxCop tool directly from the script.

<note>

The FxCop build runner requires FxCop installed on the build agent.
</note>

On this page:
* The description of the [FxCop build runner settings](#FxCop+Build+Runner+Settings)
* Details on using [dedicated service messages](#Using+Service+Messages)

For the list of supported FxCop versions, see [Supported Platforms and Environments](supported-platforms-and-environments.md#Supported+.NET+platform+build+runners).

## FxCop Build Runner Settings

### FxCop Installation

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

FxCop detection mode


</td>

<td>

When a build agent is started, it detects automatically whether FxCop is installed. If FxCop is detected, TeamCity defines the `%system.FxCopRoot%` agent system property. You can also use a custom installation of FxCop or the use FxCop checked in your version control. Depending on the selection, the settings displayed below will vary.


</td></tr><tr>

<td>

Autodetect installation


</td>

<td>

Select to use the FxCop installation on an agent.


</td></tr><tr>

<td>

 FxCop version


</td>

<td>

_The option is available when  is selected._ Select one of the options from the dropdown. If you have several versions of FxCop installed on your build agents, it is recommended to select here a specific version of FxCop you want to use to run inspections in your build to avoid inconsistency. As a result, an agent requirement will be created. If you leave the default value of the field ('Any Detected'), TeamCity will use any available agent with FxCop installed. In this case the version of FxCop used in one build may not be the same as the one used in the previous build, thus the number of new problems found will be different from the actual state.


</td></tr><tr>

<td>

Specify installation root


</td>

<td>

Select to use a custom installation of FxCop (not the autodetected one), or if you do not have FxCop installed on the build agent (for example, you can place the FxCop tool in your source control, and check it out with the build sources).


</td></tr><tr>

<td>

Installation root


</td>

<td>

_The option is available when  is selected._ Enter the path to the FxCop installation root on the agent machine or the path to an FxCop executable relative to the [Build Checkout Directory](build-checkout-directory.md).

<note>

If you want to have the __line numbers information__ and __Open in IDE__ features, run an FxCop build on the same machine as your compilation build because FxCop requires the source code to be present to display links to it.
</note>


</td></tr></table>

### What to inspect

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Assemblies


</td>

<td>

Enter the paths to the assemblies to be inspected (use ant\-like wildcards to select files by a mask). FxCop will use default settings to inspect them. The paths should be relative to the [Build Checkout Directory](build-checkout-directory.md) and separated by spaces. Enter exclude wildcards to refine the included assemblies list.

Note that there is a limitation to the maximum number of assemblies that can be specified here due to [command-line string limitation](https://support.microsoft.com/en-us/kb/830473).

 


</td></tr><tr>

<td>

FxCop project file


</td>

<td>

Enter the path relative to the [Build Checkout Directory](build-checkout-directory.md) to an FxCop project.


</td></tr></table>

### FxCop Options

<table>
<tr>

<td>

Option

</td>

<td>

Description

</td>
</tr>

<tr>

<td>

Search referenced assemblies in GAC


</td>

<td>

Search the assemblies referenced by targets in Global Assembly Cache.


</td></tr><tr>

<td>

Search referenced assemblies in directories


</td>

<td>

Search the assemblies referenced by targets in the specified directories separated by spaces.


</td></tr><tr>

<td>

Ignore generated code


</td>

<td>

A new option introduced in FxCop 1.36. Speeds up inspection.


</td></tr><tr>

<td>

Report XSLT file


</td>

<td>

The path to the XSLT transformation file relative to the [Build Checkout Directory](build-checkout-directory.md) or absolute on the agent machine. You can use the path to the detected FxCop on the target machine (i.e. `%system.FxCopRoot%/Xml/FxCopReport.xsl`). When the __Report XSLT file__ option is set, the build runner will apply an XSLT transform to FxCop XML output and display the resulting HTML in a new __FxCop__ tab on the build results page.


</td></tr><tr>

<td>

Additional FxCopCmd options


</td>

<td>

Additional options for calling FxCopCmd executable. All options entered in this field will be added to the beginning of the command line parameters.


</td></tr></table>

### Build failure conditions

Check the box to fail a build on the specified analysis errors. Click [build failure condition](build-failure-conditions.md#Fail+build+on+metric+change) to define the number of the errors.

## Using Service Messages

If you prefer to call the FxCop tool directly from the script, not as a build runner, you can use the `importData` [service messages](build-script-interaction-with-teamcity.md#Service+Messages) to import an xml file generated by [the FxCopCmd tool](http://msdn.microsoft.com/en-us/library/bb429474(VS.80).aspx) into TeamCity. In this case the FxCop tool results will appear in the [Code Inspection tab](working-with-build-results.md#Code+Inspection+Results) of the build results page.

The [service message](build-script-interaction-with-teamcity.md#Service+Messages) format is described below:


```Shell
##teamcity[importData type='FxCop' path='<path_to_the_xml_file>']

```



<note>

The TeamCity agent will import the specified xml file in the background. Make sure that the xml file is not deleted right after the `importData` message is sent.
</note>

<seealso>
        <category ref="concepts">
            <a href="build-runner.md">Build Runner</a>
        </category>
</seealso>