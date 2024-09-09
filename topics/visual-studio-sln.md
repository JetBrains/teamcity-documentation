[//]: # (title: Visual Studio \(sln\))
[//]: # (auxiliary-id: viewpage.actionpageId113084096;Visual Studio \(sln\))

<note>

Since TeamCity 2019.2.3, we have stopped providing active support for the Visual Studio (sln) runner.

Since the Visual Studio (sln) runner uses MSBuild for building projects under its hood, you can softly migrate to using the .NET build runner with the `msbuild` command. Your configured Visual Studio (sln) settings (such as command line parameters) will work similarly with `msbuild`.  
Note the .NET runner also supports the VS-native `devenv` command which allows launching Visual Studio in a command-line mode and using its own [switches](https://docs.microsoft.com/en-us/visualstudio/ide/reference/devenv-command-line-switches) for building, debugging, and deploying projects.

For more details on migration, refer to the [.NET runner description](net.md#migrating-to-net-from-sln).

For compatibility, the Visual Studio (sln) runner will be bundled with the nearest future versions of TeamCity. You can continue using it if migration to the .NET runner is too time-consuming for your setup.  
However, we will unbundle this runner after a sufficient transition period. In this case, you will still be able to install it as an external plugin. Remember to check our [upgrade notes](upgrade-notes.md) before upgrading to each following version.
{instance="tc"}

</note>

<note>

The Visual Studio (sln) build runner requires the proper version of Microsoft Visual Studio installed on the build agent.
</note>

This page contains reference information for the _Visual Studio (sln)_ build runner that builds Microsoft Visual Studio 2005-2017, and, since TeamCity 2019.1, Microsoft Visual Studio 2019 solution files. To build Microsoft Visual Studio 2003 solution files, use the [Visual Studio 2003](visual-studio-2003.md) runner.

<note>

The Visual Studio (sln) build runner requires the proper version of Microsoft Visual Studio installed on the build agent.
</note>

### General Build Runner Options

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

Specify the path to the solution to be built relative to the [Build Checkout Directory](build-checkout-directory.md). For example:

```Shell
vs-addin\addin\addin.sln

```

</td></tr><tr>

<td>

Working directory

</td>

<td>

Specify the [Build Working Directory](build-working-directory.md). (optional)

</td></tr><tr>

<td>

Visual Studio

</td>

<td>

Select the Visual Studio version.

</td></tr><tr>

<td>

Targets

</td>

<td>

Specify the Microsoft Visual Studio targets specific for the previously selected Visual Studio version. The possible options are __Build__, __Rebuild__, __Clean__, __Publish__ or a combination of these targets based on your needs. Multiple targets are space-separated.

</td></tr><tr>

<td>

Configuration

</td>

<td>

Specify the name of Microsoft Visual Studio solution configuration to build (optional).

</td></tr><tr>

<td>

Platform

</td>

<td>

Specify the platform for the solution. You can leave this field blank, and TeamCity will obtain this information from the solution settings (optional).

</td></tr><tr>

<td>

Command line parameters

</td>

<td>

Specify additional command line parameters to be passed to the build runner. Instead of explicitly specifying these parameters, it is recommended to define them on the [Configuring Build Parameters](configuring-build-parameters.md) page.

</td></tr></table>

<seealso>
        <category ref="troubleshooting">
            <a href="reporting-issues.md">Visual Studio logging</a>
        </category>
</seealso>