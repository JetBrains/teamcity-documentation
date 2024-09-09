[//]: # (title: Pre-Tested \(Delayed\) Commit)
[//]: # (auxiliary-id: Pre-Tested \(Delayed\) Commit)

A pre-tested commit is an approach that prevents committing defective code into a build, so the entire team's process is not affected. [These diagrams](https://www.jetbrains.com/teamcity/features/delayed_commit.html) illustrate the TeamCity approach to pre-tested commits described below. 

The submitted code changes go through testing first. If the code passes all the tests, TeamCity can automatically submit the changes to the version control. From there, the changes will automatically be integrated into the next build. If any test fails, the code is not committed, and the submitting developer is notified.

Developers test their changes by performing a [Remote Run](remote-run.md). A pre-tested commit is enabled when the __commit changes if successful__ option is selected.

The pre-tested commit is initiated via a plugin to one of [the supported IDEs](supported-platforms-and-environments.md#IDE+Integration). 
For a remote run a command-line tool is also [available](https://plugins.jetbrains.com/plugin/9101-command-line-remote-run-tool).

For Git and Mercurial the recommended way to use [Branch Remote Run Trigger](branch-remote-run-trigger.md) approach to run personal builds off branches.

## Matching Changes and Build Configurations

<!--[//]: # (Internal note. Do not delete. "Pre-Tested \(Delayed\) Commitd256e43.txt")-->

To submit changed files to pre-tested commit or remote run, VCS integration should work in the IDE and TeamCity should be able to check that the files, when committed, will affect the build configurations on the server.

If TeamCity cannot match the changes with the builds, a message "Submitted changes cannot be applied to the build configuration" is displayed.

<!--[//]: # (Internal note. Do not delete. "Pre-Tested \(Delayed\) Commitd256e52.txt")-->

The VCS integration should be correctly configured in the IDE, and TeamCity should be able to match the files on the developer workstation to the build configurations present on the TeamCity server. In order to be able to do that, VCS should be configured in the same way on the developer's workstation and on the server.

This includes:
* For CVS, TFS, and Perforce version control systems — use exactly the same URLs to the version control server.
* For VSS — use exactly the same path to the VSS database (machine and path).
* For Subversion — use the same server (TeamCity matches [server UUID](http://svnbook.red-bean.com/en/1.7/svn.reposadmin.maint.html#svn.reposadmin.maint.uuids)).
* For Git — the current checked out branch should have "remote" set to the server/branch monitored by the TeamCity server and should have common commits in the history with the server-monitored branch.

If upon changed files choosing TeamCity is unable to find build configurations that the files can be sent to, the option to initiate the personal build will not be available.

## General Flow of Pre-tested Commit

* A developer uses the Remote Run dialog of a [TeamCity IDE plugin](installing-tools.md) to select the files to be sent to TeamCity.
* Based on the selected files, a list of applicable build configurations is displayed. The developer selects the build configurations to test the change against and sets options for a pre-tested commit.
* The TeamCity IDE plugin builds a "patch" - full content of all the files selected and sends it to the TeamCity server. The patch is also preserved locally on the developer's machine. When sent, the change appears on the developer's [changes](viewing-user-changes-in-builds.md) page. Developer can continue working with the code and can modify the files sent to the pre-tested commit.
* The personal build is queued and processed like other queued builds.
* When the build starts, it checks out the latest sources just like a normal build and then applies the developer's personal changes sent from the IDE over (full file content is used)
* The build runs as usual
* At the end of the build the personal changes are reverted from the build's checkout directory to make sure they do not affect following builds
* The TeamCity IDE plugin pings the TeamCity server to check if all the selected build configurations have personal builds ready. If a build fails, a notification is displayed in the IDE and the process ends.
* If all the personal builds finish successfully, the IDE plugin displays a progress, backs up the current version of the files participating in the personal change (as they might already be modified since the pre-tested commit was initiated), then restores the file contents from the saved "patch", performs the version control commit (reports an error if there was an error like a VCS conflict) and restores the just backed up files to bring the working copy in the last seen state. The pre-tested commit in the TeamCity plugin window gets an error or success mark.

<seealso>
        <category ref="inst_tools">
            <a href="intellij-platform-plugin.md">IntelliJ Platform Plugin</a>
            <a href="visual-studio-addin.md">Visual Studio Addin</a>
        </category>
        <category ref="concepts">
            <a href="remote-run.md">Remote Run</a>
        </category>
        <category ref="admin-guide">
            <a href="branch-remote-run-trigger.md">Remote Run on Branch Build Trigger for Git and Mercurial</a>
        </category>
</seealso>