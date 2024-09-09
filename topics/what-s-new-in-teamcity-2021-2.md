[//]: # (title: What's New in TeamCity 2021.2)
[//]: # (auxiliary-id: What's New in TeamCity 2021.2)

## Two-factor authentication

TeamCity now provides built-in two-factor user authentication (2FA). Enabling it on your TeamCity server grants it an extra level of security. Users will have to verify their identity in two steps: by providing their regular credentials _plus_ by submitting disposable keys, generated on their personal devices.

2FA is set to _Optional_ by default, but you can make it _Disabled_ or _Mandatory_ on your server — in **Administration | Authentication**. The optional mode allows users to decide whether they want to enable 2FA for their accounts or not. The mandatory mode prevents users from accessing TeamCity without the 2FA verification. After you enable it, users will get a grace period of 1 week during which they are supposed to set up 2FA for their accounts.

<img src="2fa-user-page.png" alt="Configuring two-factor authentication" width="706"/>

Read how to [enable 2FA on your server](managing-two-factor-authentication.md) and [set it up for your user account](configuring-your-user-profile.md#Configuring+Two-Factor+Authentication).

## C# Script Runner

The new C# Script runner offers a handy way to automate your service tasks in C#: prepare a build environment, create OS users, report to messengers, and so on. This runner is a good alternative to [PowerShell](powershell.md) and [Kotlin](kotlin-script.md) for users who feel more confident with C#.

The runner can launch C# scripts across platforms: on Windows, Linux, and macOS. It needs .NET 6.0, so the easiest way is to launch it inside a Docker container with preinstalled .NET. It also requires installing our [custom C# Interactive shell](https://github.com/JetBrains/teamcity-csharp-interactive#readme) as an [agent tool](installing-agent-tools.md).
{instance="tc"}

Another advantage of this runner is that it’s capable of automatically restoring NuGet packages referenced in your scripts. By default, TeamCity searches for packages on NuGet.org, but you can specify other target feeds, including private and [TeamCity-internal](using-teamcity-as-nuget-feed.md) ones.
{instance="tc"}

The runner can launch C# scripts across platforms: on Windows, Linux, and macOS. It needs .NET 6.0, so the easiest way is to launch it inside a Docker container with preinstalled .NET.
{instance="tcc"}

Another advantage of this runner is that it’s capable of automatically restoring NuGet packages referenced in your scripts. By default, TeamCity searches for packages on NuGet.org, but you can specify other target feeds, including private ones.
{instance="tcc"}

To configure a C# Script build step, you just need to enter the script code (or a path to the `.csx` file) and its arguments, if necessary. Read about other settings in [this article](c-script.md).

<img src="C-script.png" alt="Configuring C# Script step" width="706"/>

>We are about to explore the capabilities of this runner in the upcoming tutorial in [our blog](https://blog.jetbrains.com/teamcity/) — follow it to get updates.

## Perforce integration: run builds on shelved files, report statuses to Helix Swarm, and more

TeamCity 2021.2 brings plenty of enhancements for projects that rely on the [Perforce Helix Core](https://www.perforce.com/) VCS.

### Run builds on shelved files

Perforce allows users to temporarily share changed files with each other without checking them into depots. This is called _[shelving](https://www.perforce.com/manuals/v17.1/p4guide/Content/CmdRef/p4_shelve.html)_. And now TeamCity can run builds on such shelved files, which lets you pretest the local code in a real build environment.

You can run a build on a shelved changelist:
* Manually: via a [custom run](running-custom-build.md), or
* Automatically: by configuring a [Perforce shelve trigger](perforce-shelve-trigger.md).

In both cases, TeamCity will run a _[personal build](personal-build.md)_ which is a build type intended for processing changes that don’t affect the project version history.

These options are available for all build configurations that use [Perforce VCS roots](perforce.md).

<img src="p4-custom-run-shelved.png" alt="Run custom build on P4 shelved files" width="460"/>

[Learn more](perforce-shelve-trigger.md).

### Reporting build statuses to Perforce Helix Swarm

If a build is run on a shelved changelist, TeamCity can automatically report its status to [Perforce Helix Swarm](https://www.perforce.com/products/helix-swarm). If you use this tool for code reviews, there is no more need to go back and forth to TeamCity just to check if the build succeeded or not.

To set up this integration, enable the [Commit Status Publisher](commit-status-publisher.md) feature in a build configuration that uses a [Perforce VCS root](perforce.md):

<img src="csp-helix-swarm.png" alt="Configure Commit Status Publisher for P4 Helix Swarm" width="460"/>

After this feature is configured, TeamCity will be publishing the build statuses’ updates as comments to the respective code reviews in Helix Swarm:

<img src="helixswarm.png" alt="Displaying TeamCity build statuses in Helix Swarm" width="706"/>

[Learn more](commit-status-publisher.md#Perforce+Helix+Swarm).

### Automatic labels support

TeamCity can assign custom labels to your project sources. In case with Perforce, the [VCS labeling](vcs-labeling.md) build feature was previously creating [static labels](https://www.perforce.com/manuals/p4guide/Content/P4Guide/labels.archive.html), which are archives of local workspaces. However, it seems that [automatic Perforce labels](https://www.perforce.com/manuals/p4guide/Content/P4Guide/labels.alias.html) are way better in terms of performance, as they work as mere aliases for changelists. And since version 2021.2, TeamCity publishes automatic labels by default.

If you prefer using static labels, please read our [upgrade notes](upgrade-notes.md#Perforce+automatic+labels+become+default) with an instruction on how to revert this change on your server.
{instance="tc"}

[Learn more](vcs-labeling.md#Labeling+in+Perforce).

### Parameterized connection variables

If a build agent needs to connect to several Perforce VCS roots during one build run, it can now get connection parameters of each of these roots.

Previously, if you wanted to use `[P4PORT](https://www.perforce.com/manuals/cmdref/Content/CmdRef/P4PORT.html)`, `[P4USER](https://www.perforce.com/perforce/r12.1/manuals/cmdref/env.P4USER.html)`, or `[P4CLIENT](https://www.perforce.com/manuals/v18.1/cmdref/Content/CmdRef/P4CLIENT.html)` in your build scripts, you could only refer to variables of the first Perforce root. Now, TeamCity stores these variables as parameters, so you can reference them in the scripts separately for each root: `P4USER`, `P4PORT`, `P4CLIENT`. For example, `env.P4PORT=%vcsRoot.<rootID>.port%`.

where `rootID` is the VCS root’s external ID, specified in its settings.

[Learn more](perforce-workspace-handling-in-teamcity.md#Perforce+Workspace+Parameters).

## JetBrains Space integration: authenticating users and creating projects from repositories

[Space](https://www.jetbrains.com/space/) is the new JetBrains collaboration solution for software teams. If your company is already on board with it, you can now:
* Natively create projects, build configurations, and VCS roots from JetBrains Space repositories.
* Allow users to sign in to TeamCity with their JetBrains Space accounts.

This integration requires preconfiguring a connection to your Space instance: see [how to do this](configuring-connections.md#jetbrains-space-connection).

### Create projects, build configurations, and VCS roots from JetBrains Space repositories

After the connection is configured, the JetBrains Space button will become available in the project and build configuration creation wizards in TeamCity. When clicking it for the first time, you will need to sign in to Space and grant TeamCity access to view your project data.

TeamCity will display the list of Space repositories available to you:

<img src="space-auth.png" alt="Creating a project from JetBrains Space" width="706"/>

Choose a repository to create a project or build configuration from — and TeamCity will scan it and suggest the settings, as described [here](creating-and-editing-projects.md#Creating+project+pointing+to+JetBrains+Space).

>You can similarly add additional JetBrains Space [roots](vcs-root.md) to an existing project.

### Authenticate in TeamCity with JetBrains Space account

To activate the Space authentication on your server, you need to:
1. Configure the [connection to Space](configuring-connections.md#jetbrains-space-connection) on the Root project level.
2. Enable the JetBrains Space authentication module in **Administration | Authentication**.

As a result, users will see the JetBrains Space icon on the TeamCity login page and can click it to sign in with their Space account.

<img src="spaceauth.png" width="296" alt="Signing in to TeamCity with JetBrains Space"/>

>If you want to associate an existing TeamCity user with a JetBrains Space user, you can do this in the authentication settings of their TeamCity profile.

## Authenticate in TeamCity with Azure DevOps Services account

If your team uses [Azure DevOps Services](https://azure.microsoft.com/en-us/services/devops/), its members can now sign in to TeamCity with their Azure accounts.

To enable this functionality on your TeamCity server:
1. Configure an OAuth 2.0 connection to your Azure DevOps Services instance as described [here](configuring-connections.md#Connecting+to+Azure+DevOps).
2. Enable the _Azure DevOps OAuth 2.0_ module in **Administration | Authentication**. To secure the access further, you can restrict the scope of Azure organizations whose members can access TeamCity:

<img src="azureprofileauth.png" width="460" alt="Configuring authentication in Azure"/>

Now, users will be able to sign in to TeamCity by clicking the Azure icon on the login page and confirming the authentication.

>If you want to associate an existing TeamCity user with an Azure DevOps Service user, you can do this in the authentication settings of their TeamCity profile.

## User Interface improvements

Version 2021.2 brings the following TeamCity UI improvements:
* **Displaying user avatars**.
* **Displaying pending changes and change details** in experimental UI.
* **Ability to group builds by projects on the build chain graph** in experimental UI.

>As in the multiple preceding releases, our focus stays on improving the [experimental UI](teamcity-sakura-ui.md) and polishing it to the point when it can serve as a better alternative to the classic UI for all of our users. Your feedback helps determine which features are the most expected, so we can shape our roadmap to suit these needs better.  
>If you tried the first versions of the new UI and switched back to the classic mode, we encourage you to give the new UI another go, as its current implementation differs a lot from the first one.  
>You are welcome to share your feedback about the new improvements via [this form](https://teamcity-support.jetbrains.com/hc/en-us/requests/new?ticket_form_id=360001686659)!

### Displaying user avatars

User avatars are now displayed next to the usernames, both in the classic and experimental UIs. Visual icons can help quickly identify the commits’ authors.

<img src="avatars.png" alt="Showing user avatars in TeamCity" width="706"/>

You can upload the avatar in your user profile settings.

### Experimental UI for displaying changes

The **Pending Changes** tab and single **Change** page have been fully reproduced in the new UI. They have a familiar layout of the classic pages, but offer a better look&feel.

The **Pending Changes** page displays the list of changes committed to the remote repository since the last successful build.

<img src="new-pending-changes.png" alt="New Pending Changes tab" width="706"/>

The individual **Change** page allows you to view detailed information about each change and displays the list of failed tests and build problems that emerged when this change was introduced.

<img src="changes-page.png" alt="New Change page" width="706"/>

### Group builds by projects in experimental build chain graph

It is now possible to group builds by projects on the experimental build chain graph. This can give a convenient overview of a large pipeline.

To try this option, open the results of any build that is a part of a [chain](build-chain.md), go to its **Dependencies** tab, and switch to the **Chain** mode.

<img src="group-projects-chain.png" alt="Group builds by projects on a chain" width="706"/>

## Other improvements

### OpenSSH keys support

TeamCity now supports keys in the OpenSSH format, including ECDSA and ED25519. You can upload such a key to TeamCity and reuse it when configuring [VCS roots](vcs-root.md) or running an [SSH agent](ssh-agent.md) during a build.

The support for this format has also been introduced in a bugfix build TeamCity 2021.1.4.

### Kotlin DSL updates

The [typed Kotlin DSL](kotlin-dsl.md) is now supported for numerous settings of projects and build configurations. See the full list of improvements [here](https://youtrack.jetbrains.com/issues?q=%23TW%20tag:%20missing-dsl%20%23Fixed%20-%7Btrunk%20issue%7D%20visible%20to:%20%7BAll%20Users%7D%20Fix%20versions:%20%7BMorena%202021.2%20RC%20(99472)%7D,%20%7BMorena%202021.2%20EAP3%20(99319)%7D,%20%7BMorena%202021.2%20EAP2%20(99125)%7D,%20%7BMorena%202021.2%20EAP1%20(98941)%7D,%20%7BMorena%202021.2%20(99542)%7D%20).

### Pause and resume build queue on secondary nodes
{instance="tc"}

In a [multinode setup](multinode-setup.md), a secondary node has limited functionality compared to the main one, but we strive to eventually make it equally functional. This release makes it possible to pause and resume the build queue on secondary nodes.

### Pass parameters to selected dependency builds

TeamCity allows passing build parameters up the chain: that is, a later build in a chain can override properties of a preceding build. This lets you compose flexible and adaptable pipelines. To improve the flexibility even further, we’ve extended the reversed parameters’ syntax to support build ID masks.

Previously, it was possible to redefine a property only in a specific build or in all preceding builds at once. Now, you can specify a set of target builds if their IDs have a common prefix or suffix:

`reverse.dep.[prefix]*[suffix].<property_name>`

where `prefix` corresponds to the beginning of the target builds’ IDs, and `suffix` corresponds to their ending.

[Read more about this syntax](use-parameters-in-build-chains.md#Override+Parameters+of+Preceding+Configurations).

### Autodetecting PowerShell on ARM64

TeamCity now can automatically detect PowerShell in ARM64 systems by checking the recommended locations: `~/powershell` and `/opt/microsoft/powershell/<version>/`. To add a custom location to this scope, you can set a special agent property, as described here.

### Easier API access with cookies

Disabling cookies is not anymore necessary for accessing TeamCity from external HTTP clients.  
Previously, if you needed to send an API request with enabled cookies, it was required to pass both an access token and [CSRF token](csrf-protection.md). Now, sending an access token for authentication will be enough.

### Easier CSP header customization

To customize a [Content Security Policy (CSP)](content-security-policy-in-teamcity.md) header, which prohibits downloading unknown external resources from TeamCity, it is now enough to provide only the custom part, with no need to specify the full header value. If the full value is provided, only the different parts will be applied.

## Fixed issues

See [TeamCity 2021.2 release notes](teamcity-2021-2-release-notes.md).

## Upgrade notes
{instance="tc"}

Before upgrading, we highly recommend reading about [important changes in version 2021.2 compared to 2021.1.x](upgrade-notes.md#Changes+from+2021.1+to+2021.2).

## Previous releases
{instance="tc"}

* [What's New in TeamCity 2021.1](https://www.jetbrains.com/help/teamcity/2021.1/what-s-new-in-teamcity.html)
* [What's New in TeamCity 2020.2](https://www.jetbrains.com/help/teamcity/2020.2/what-s-new-in-teamcity.html)
* [What's New in TeamCity 2020.1](https://www.jetbrains.com/help/teamcity/2020.1/what-s-new-in-teamcity.html)
* [What's New in TeamCity 2019.2](https://www.jetbrains.com/help/teamcity/2019.2/what-s-new-in-teamcity-2019-2.html)
* [What's New in TeamCity 2019.1](https://www.jetbrains.com/help/teamcity/2019.1/what-s-new-in-teamcity-2019-1.html)

## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.