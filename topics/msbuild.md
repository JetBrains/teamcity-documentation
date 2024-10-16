[//]: # (title: MSBuild)
[//]: # (auxiliary-id: MSBuild)

<note>

Since TeamCity 2019.2.3, we have stopped providing active support for the MSBuild runner. We recommend using the .NET runner with the `msbuild` command instead as it provides more features and is actively updated. For more details on migration, refer to the [.NET runner description](net.md#Migrating+from+MSBuild+Runner).

For compatibility, the MSBuild runner will be bundled with the nearest future versions of TeamCity. You can continue using it if migration to the .NET runner is too time-consuming for your setup.   
However, we will unbundle this runner after a sufficient transition period. In this case, you will still be able to install it as an external plugin. Remember to check our [upgrade notes](upgrade-notes.md) before upgrading to each following version.
{instance="tc"}

</note>

This page contains reference information for the _MSBuild_ build runner fields.

The MSBuild runner requires .NET Framework or Mono installed on the build agent. [Microsoft Build Tools](https://devblogs.microsoft.com/visualstudio/msbuild-is-now-part-of-visual-studio/) 2013-2019 are supported.

Before setting up a build configuration to use MSBuild as the build runner, make sure you are using an XML build project file with the MSBuild runner.

To build a Microsoft Visual Studio solution file, you can use the [Visual Studio (sln)](visual-studio-sln.md) build runner.

## Settings

<table><tr>

<td>

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Build file path

</td>

<td>

Specify the path to the solution to be built relative to the [build checkout directory](build-checkout-directory.md). For example, `vs-addin\addin\addin.sln`.

</td></tr><tr>

<td>

Working directory

</td>

<td>

Optional. Specify the path to the [build working directory](build-working-directory.md) if it differs from the build checkout directory.

</td></tr><tr>

<td>

MSBuild version


</td>

<td>

Select the MSBuild version: .NET Framework, Mono xbuild or Microsoft Build Tools.


</td></tr><tr>

<td>

MSBuild ToolsVersion


</td>

<td>

Specify here the version of tools that will be used to compile (equivalent to the `/toolsversion:` commandline argument).

<note>

MSBuild supports compilation to older versions, thus you may set __MSBuild version__ as 4.6.1 with __MSBuild ToolsVersion__ set to 2.0 to produce .NET 2.0 assemblies while running MSBuild from .NET Framework 4.6.1. For more information, refer to [this article](https://msdn.microsoft.com/en-us/library/bb383796(VS.100).aspx)

</note>

</td></tr><tr>

<td>

Run platform


</td>

<td>

From the drop-down menu, select the desired execution mode on a x64 machine.


</td></tr><tr>

<td>

Targets


</td>

<td>

A target is an arbitrary script for your project purposes. Enter targets separated by spaces. The available targets can be viewed in the Web UI by clicking the icon next to the field and added by checking the appropriate boxes.


</td></tr><tr>

<td>

Command line parameters


</td>

<td>

Specify any additional parameters for `MSBuild.exe.`


</td></tr><tr>

<td>

Reduce test failure feedback time


</td>

<td>

Use this option to instruct TeamCity to run the tests which failed in the previous builds before others.


</td></tr></table>

<!--[//]: # (Internal note. Do not delete. "MSBuildd215e169.txt")-->

## Code Coverage

To learn about configuring code coverage options, refer to the [Configuring .NET Code Coverage](configuring-.net-code-coverage.md) page.

## Implementation Notes
{instance="tc"}

The MSBuild runner generates an MSBuild script that includes the user's script. This script is used to add TeamCity-provided MSBuild tasks. Your MSBuild script will be included with the &lt;Import&gt; task. If you specified a Visual Studio solution file, it will be called from the &lt;MSBuild&gt; task. To disable it, set the `teamcity.msbuild.generateWrappingScript` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) to `false`.

As this runner is deprecated, it no longer supports some legacy tools like MSBuildBootstrap. To perform custom tasks within a build, consider using [TeamCity service messages](service-messages.md). For example, use `<Message Text="##teamcity[buildNumber '1.2.3']" Importance="high" />` to print text in the standard output stream of build log.

<seealso>
        <category ref="concepts">
            <a href="build-runner.md">Build Runner</a>
            <a href="build-checkout-directory.md">Build Checkout Directory</a>
        </category>
        <category ref="admin-guide">
            <a href="nunit-support.md#Using+NUnit+for+MSBuild">NUnit for MSBuild</a>
            <a href="nunit-support.md#MSBuild+Service+Tasks">MSBuild Service Tasks</a>
        </category>
</seealso>