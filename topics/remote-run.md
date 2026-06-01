[//]: # (title: Remote Run)
[//]: # (help-id: Remote Run)

A _remote run_ is a [personal build](personal-build.md) initiated by a developer from one of the supported IDE plugins to test how the changes will integrate into the project's code base. For example, to initiate a remote run from the IntelliJ IDEA Platform, see [Running Builds Remotely](https://www.jetbrains.com/help/teamcity/ij-addin/tc-run-build-remotely.html).

Unlike [Pre-tested (delayed) commit](pre-tested-delayed-commit.md), no code is checked into the VCS regardless of the state of the personal build initiated via Remote Run.

For a list of version control systems supported by each IDE, see [Supported Platforms and Environments](supported-platforms-and-environments.md#IDE+Integration).

> You can also trigger remote runs from the terminal using [TeamCity CLI](teamcity-cli-managing-runs.md).
> 
> ```Shell
> teamcity run start MyProject_Build --local-changes
> ```
> 
{style="tip"}


<seealso>
        <category ref="inst_tools">
            <a href="intellij-platform-plugin.md">IntelliJ Platform Plugin</a>
            <a href="visual-studio-addin.md">Visual Studio Add-in</a>
        </category>
        <category ref="concepts">
            <a href="pre-tested-delayed-commit.md">Pre-tested (delayed) commit</a>
            <a href="personal-build.md">Personal Build</a>
        </category>
        <category ref="admin-guide">
            <a href="branch-remote-run-trigger.md">Branch Remote Run Trigger</a>
        </category>
        <category ref="troubleshooting">
            <a href="reporting-issues.md">Reporting Issues</a>
        </category>
        <category ref="external">
            <a href="https://youtu.be/icuhBgEFtVM">Video tutorial: TeamCity for developers</a>
        </category>
</seealso>