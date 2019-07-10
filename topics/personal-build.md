[//]: # (title: Personal Build)
[//]: # (auxiliary-id: Personal Build)
A personal build is a build\-out of the common builds sequence and which typically uses the changes not yet committed into the version control. Personal builds are usually initiated from one of the [supported IDEs](supported-platforms-and-environments.md) via the [Remote Run](remote-run.md) procedure.

The build uses the current VCS repository sources plus the changed files identified during the remote run initiation. The results of the Personal Build can be seen in the "My Changes" view of the corresponding IDE plugin and on the __[Changes](viewing-your-changes.md)__ page. Finished personal builds are listed in the builds history, but only for the users who initiated them.   
See more at [Pre-Tested (Delayed) Commit](pre-tested-delayed-commit.md).

By default, users only see their own personal builds in the builds lists, but this can be changed on the [user profile page](managing-your-user-account.md).

One can also mark a build as personal using the corresponding option of the [Run](triggering-a-custom-build.md) dialog.   
By default, only users with the [Project Developer role](role-and-permission.md) can initiate a personal build.

__Since TeamCity 9.1__, it is possible to [restrict running personal builds](configuring-general-settings.md).

 __  __

__See also:__



__Concepts__: [Pre-tested Commit](pre-tested-delayed-commit.md) | [Remote Run](remote-run.md)

__Installing Tools__: [IntelliJ Platform Plugin](intellij-platform-plugin.md) | [Eclipse Plugin](eclipse-plugin.md) | [Visual Studio Addin](visual-studio-addin.md)

__Troubleshooting__: [Remote Run Problems](reporting-issues.md)
