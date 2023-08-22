[//]: # (title: Visual Studio Add-in)
[//]: # (auxiliary-id: Visual Studio Add-in;Visual Studio Addin)

## Visual Studio Add-in Features

The TeamCity add-in for Microsoft Visual Studio provides the following features:
* [Remote Run](remote-run.md) for TFS, Subversion and Perforce (for remote run for Mercurial and Git see [Branch Remote Run Trigger](branch-remote-run-trigger.md))
* [Pre-Tested (Delayed) Commit](pre-tested-delayed-commit.md) for TFS, Subversion and Perforce
* fetching [JetBrains dotCover](https://www.jetbrains.com/dotcover/index.html) coverage analysis data from the TeamCity server (see [more](jetbrains-dotcover.md)) to MS Visual Studio (requires dotCover of the [supported version](supported-platforms-and-environments.md#Code+Coverage) installed in Visual Studio)
* viewing recently committed changes and personal builds with their build status in the __My Changes__ tool window
* opening build failure details in MS Visual Studio from the TeamCity web UI
* viewing failed tests' details for a build
* rerunning tests failed in the TeamCity build locally via the [ReSharper](https://www.jetbrains.com/resharper/) test runner
* navigation from the IDE to the build results web page
* reapplying changes sent in Remote Run or Pre-tested commit to the working directory

For detailed instructions, refer to the [TeamCity Add-in Online Help](https://www.jetbrains.com/help/teamcity/vs-addin/TeamCity_Getting_Started.html).

>To enable navigation to the failed tests in MS Visual Studio by using _Open in IDE_ actions in the web UI, make sure that `.pdb` file generation for the assemblies involved in NUnit/MSTest unit tests is switched on in the current Visual Studio project.

## Installing Add-in

1. Close all running instances of Visual Studio before starting the Add-in installation (initial or upgrade).
2. Navigate to the download page of the Visual Studio Add-in:
   * Click the arrow next to your username in the top right corner of the TeamCity web UI and select __Profile__.
   * In the __TeamCity Tools__ section on the right, click the Visual Studio Add-in download link.

The TeamCity Visual Studio Add-in is shipped as a part of [ReSharper Ultimate](https://www.jetbrains.com/dotnet/) products bundle. After installation, the TeamCity add-in will be available under the RESHARPER menu in Visual Studio.

<note>

The installer will remove the prebundled products' versions: TeamCity and ReSharper versions prior to 9.0, dotCover prior to 3.0, dotTrace prior to 6.0. ReSharper Ultimate does not support the Visual Studio versions 2005 and 2008.
</note>

## Requirements

See the [Supported Platforms and Environment](supported-platforms-and-environments.md#IDE+Integration) page for the system requirements to configure integration with different version control systems or coverage tools.

<seealso>
        <category ref="blog">
            <a href="https://blog.jetbrains.com/teamcity/2013/03/teamcity-plugin-for-visual-studio/">TeamCity plugin for Visual Studio</a>
        </category>
        <category ref="troubleshooting">
            <a href="reporting-issues.md">TeamCity Visual Studio Add-in issues</a>
        </category>
</seealso>