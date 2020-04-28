[//]: # (title: IntelliJ Platform Plugin)
[//]: # (auxiliary-id: IntelliJ Platform Plugin)
TeamCity plugin provides TeamCity integration for IntelliJ Platform-based IDEs, including JetBrains IntelliJ IDEA, RubyMine, PyCharm, PhpStorm/WebStorm, AppCode, and Rider. Remote run / pretested commit functionality is only supported with the VCS integrations bundled with the IDEs by JetBrains. See a [separate page](intellij-platform-plugin-compatibility.md) for the list of supported versions.

<tip>

A usage example is provided in the related [blog post](https://blog.jetbrains.com/teamcity/2017/10/teamcity-integration-with-intellij-based-ides/).
</tip>

## Features

TeamCity integration provides the following features:
* [Remote Run](remote-run.md) and [Pre-Tested (Delayed) Commit](pre-tested-delayed-commit.md)
* customizing parameters for personal builds
* [Remote Debug](remote-debug.md)
* possibility to review the code duplicates
* analyzing the results of remote code inspections
* monitoring the status of particular projects and build configurations and the status of changes committed to the project code base
* viewing failed tests and build logs with highlighted stacktraces and current project file names
* start investigation of a failed build
* assign investigation of a build configuration problem or failed test form the plugin to another team member
* viewing build failures, which you are supposed to investigate, and giving up investigation when the problem is fixed
* applying quick\-fixes to the results of remote code analysis: the problematic code can be highlighted in the editor and you can work with a complete report of the project inspection results in a tool window
* downloading and viewing only the new inspection results that appeared since the last build was created
* work with the results of server\-side code duplicates search in the dedicated tool window
* accessing the server\-side code coverage information and visualizing the portions of code covered by unit tests
* viewing build compilation errors in a separate tab of the build results pane with navigation to source code
* re\-rununing failed tests from IntelliJ IDEA plugin using JUnit or TestNG
* opening the patch from the change details web page (for this feature to work you need to have IDEA X installed)

## Installing TeamCity plugin

TeamCity IDE plugin version must correspond to the version of the TeamCity server it connects to. Connections to TeamCity servers with different versions are generally not supported.

### Installing the Plugin from the Plugin Repository

The [plugin repository](https://plugins.jetbrains.com/) has a [TeamCity plugin](https://plugins.jetbrains.com/plugin/1820) from one of the recently released versions. You can install the plugin from repository (for example, from IntelliJ IDEA Settings &gt; Plugins), then enter the address of your local TeamCity server and let the plugin update itself to the version corresponding to the server.

__To install the TeamCity plugin for IntelliJ platform IDE__:
1. In IDE, open the __Settings__ dialog. To do so either press __Ctrl\+Alt\+S__ or choose __File__ &gt; __Settings...__ (__Apple__ &gt; __Settings...__ on macOS) from the main menu.
2. Open __Plugins__ section.
3. In the __Plugins__ section, search for 'TeamCity' or click __Install JetBrains plugin...__ to view the list of available plugins.
4. Select the TeamCity Integration, click the __Install__ button.
5. Restart the IDE.
6. Use the TeamCity menu to log in to your TeamCity server from the plugin.
7. Invoke the __Update__ command in the __TeamCity__ menu to install the plugin version matching the server version and restart the IDE.

### Installing the Plugin Manually

The plugin for IntelliJ platform can be downloaded from the __TeamCity Tools__ area on the __My Settings &amp; Tools__ page of TeamCity web UI.

__To install the TeamCity plugin__:
1. In the top right corner of the TeamCity web UI, click the arrow next to your username, and select __My Settings &amp; Tools__.
2. On the right, locate the IntelliJ Platform Plugin section in the __TeamCity Tools__ area, click the __download__ link, and save the archive.
3. In IDE, open the __Settings__ dialog. To do so either press __Ctrl\+Alt\+S__ or choose __File__ &gt; __Settings...__ (__Apple__ &gt; __Settings...__ on macOS) from the main menu.
4. Open __Plugins__ section.
5. In the __Plugins__ section click __Install plugin from disk...__.
6. In __Choose Plugin File__ dialog select downloaded archive, click the __Ok__ button.
7. Ensure there's enabled checkbox next to __TeamCity Integration__ in plugins list.
8. Restart the IDE.
All additional information on how to work with the TeamCity plugin is available in __IDE Help System__.

## Configuring IntelliJ IDEA-platform based IDE to Check for Plugin Updates
1. In IntelliJ IDEA, open __Settings | Updates__.
2. Add [`http://<your_teamcity_server_URL>/update/idea-plugins.xml`](http://<your_teamcity_server_URL>/update/idea-plugins.xml) to the list.
3. Set "Check for updates" to "Daily".
4. Press __Apply__, then __Check Now__.

<seealso>
        <category ref="troubleshooting">
            <a href="reporting-issues.md">Logging in IntelliJ IDEA/Platform-based IDEs</a>
        </category>
</seealso>
