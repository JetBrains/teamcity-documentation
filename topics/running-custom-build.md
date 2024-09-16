[//]: # (title: Running Custom Build)
[//]: # (auxiliary-id: Running Custom Build;Triggering a Custom Build)

A build configuration typically uses [build triggers](configuring-build-triggers.md) to start new builds on a required schedule or whenever TeamCity detects a new change in a source code.

In addition to these automatically triggered builds, TeamCity allows you to run builds manually and, if needed, customize their settings: add new or modify existing properties, choose specific changes, schedule a build, choose which agent should run it, and so on.

TeamCity provides multiple options to run custom builds:

* Click the ellipsis button next to the __Run__ button and specify optional settings in the __Run Custom Build__  dialog (see [this section](#General+Options) for more information).
* To run a custom build with specific changes, open the build results page and switch to the __[Changes](build-results-page.md#Changes+Tab)__ tab. Expand the required change, click the __Run build with this change__ button, and specify required [options](#General+Options).
* Send an [HTTP](accessing-server-by-http.md) or [REST API](https://www.jetbrains.com/help/teamcity/rest/edit-build-configuration-settings.html#Manage+Build+Triggers) request to the TeamCity server.
* [Promote a build](#Promoting+Build).
* Set up [build triggers](configuring-build-triggers.md) to launch builds with custom parameters.

## General Options

The **General** tab displays the most basic and frequently used settings.

<img src="dk-customRun-general.png" width="706" alt="Run custom build dialog, General Settings tab"/>

### Agent

This setting allows you to choose an agent that should run your build. The following options are available:

* __&lt;the fastest idle agent&gt;__ (default option) — if selected, TeamCity will automatically choose an agent to run a build.

* Select a specific TeamCity agent from a list. TeamCity displays a designated agent's current state and (if it is already running a build) estimates when it will become idle.

* __&lt;the fastest idle agent in the N pool&gt;__ — TeamCity will run a build on an agent from a specified pool.

* if [cloud integration](teamcity-integration-with-cloud-solutions.md) is configured, you can run a build on an agent spawned from a __certain cloud image__. If no cloud agents of this type are available, TeamCity will attempt to start a new one.
  {product="tc"}

* __&lt;All enabled compatible agents&gt;__ — run a build simultaneously on all agents that are enabled and compatible with the build configuration. Use this option to:
  * Run a build for agent maintenance purposes (for example, you can create a configuration to check whether agents function correctly after an environment upgrade/update).
  * Run a build on different platforms (for example, you can set up a configuration and specify a number of compatible build agents with different environments installed.


### Date &amp; Time

Leave the **As soon as possible** option to place a new build to a regular queue immediately after you click **Run Build**.

To schedule a build to the specific date &amp; time, switch to the **At specific date and time** option. Scheduled builds remain at the end of a [build queue](working-with-build-queue.md) until their scheduled date and time.

<img src="dk-scheduledBuild.png" width="706" alt="Scheduled build and time"/>

### Build Options


* **run as a personal build** — allows you to run [personal builds](personal-build.md).

* **put the build to the queue top** — places this new build to the top of the current [build queue](working-with-build-queue.md). Since your newly started build can have no immediately ready compatible agents, it can move down the queue as it waits for one. If this happens, click the **Move to top** icon on the build configuration page, or navigate to the [](build-results-page.md) page and click **Actions | Move to top**.

  <img src="dk-moveBuildToTop.png" width="706" alt="Move queued build to top"/>

* **delete all files in the checkout directory before the build** — specifies whether TeamCity should clear the [build checkout directory](build-checkout-directory.md).
  * If snapshot dependencies are configured, this option can be applied to snapshot dependencies. In this case, all the builds of the build chain will use a clean checkout.


<anchor name="P4-shelved-files-custom-run"/>

### Perforce-Specific Settings

If the current build configuration uses a [Perforce](perforce.md) VCS root, you can also run a custom build on [shelved files](https://www.perforce.com/manuals/v17.1/p4guide/Content/CmdRef/p4_shelve.html). To do this:
1. Tick _Run as a personal build_ option.
2. Enter the ID of the changelist that contains the shelved files.
3. Choose the target Perforce root.

> If stream support is enabled in a Perforce VCS Root, TeamCity will automatically detect the target stream from the changed files even if the default stream is specified.
> 
{style="note"}


> Learn how to automate running builds on shelved files with [Perforce Shelve Trigger](perforce-shelve-trigger.md).
>
{style="tip"}

## Dependencies

_This tab is available only for builds that have dependencies on other builds_.

The **Dependencies** tab allows you to rebuild all dependencies and select a particular build whose artifacts this new build should use. By default, TeamCity shows the last 20 builds. To increase the number of available recent builds, add the `teamcity.runCustomBuild.buildsLimit=<your value>` [internal property](server-startup-properties.md#TeamCity+Internal+Properties).
{product="tc"}

The **Dependencies** tab allows you to rebuild all dependencies and select a particular build whose artifacts this new build should use.
{product="tcc"}

If you re-run a dependent build, TeamCity will try to rebuild all dependency builds, including those that previously failed.

Dependency builds in the list are initially grouped by their alphabetically sorted branches. Builds of the same branch are sorted by the build date. To discard branch-based sorting and sort all dependency builds only by their dates, click __Sort dependencies by date__. This allows you to view the most recent builds first. To restore the default sorting, click __Reset all__.

## Changes

_This tab is available only if your TeamCity user has permission to access VCS roots for the build configuration._

The **Changes** tab allows you to select a change to be included to the build. TeamCity will use the change's revision to check out the sources and include all changes up to the selected one in this new build.

Note that if a corresponding VCS root was detached from the build configuration, TeamCity is unable to retrieve newest commits and displays only a limited number of changes. To run a build with an older chage, locate the required commit in the Change Log and use the __Run build with this change__ action.

## Include Changes

The __Include changes__ drop-down menu allows you to choose which changes in the VCS roots attached to the configuration should be included in this new build.

* __Latest changes at the moment the build is started__: TeamCity will automatically include all latest changes available at the moment.
* __Last change to include__: Choose a required change to ignore all later commits. TeamCity marks builds that ignore newest changes as [history builds](history-build.md).

<note>

Build numbers and the build history list reflect the time these builds started. As such, a build with a greater build number (and positioned at the top of the build history) may reflect older sources changelist compared to a previous.

</note>

## Build Branch

The __Build branch__ drop-down menu is available if this build configuration (or its snapshot dependency configuration) has branches. Allows you to choose a branch for the custom build.

<anchor name="TriggeringCustomBuild-UsesettingsfromVCS"/>

## Use Settings

If a project [stores its settings in a VCS](storing-project-settings-in-version-control.md), this tab allows you to choose which settings should be used for this new build:


* Settings currently defined on the TeamCity server
* Settings loaded from the VCS revision calculated for this build.

The default behavior depends on the currently selected **Project Settings | Versioned Settings** page setting (see this section for more information: [](storing-project-settings-in-version-control.md#Defining+Settings+to+Apply+to+Builds)).

If you selected [specific changes revision](#Include+Changes), TeamCity will also load a corresponding revision of the project settings.

## Parameters

<note>
 
These settings are available only if your TeamCity user has permissions to change system properties and environment variables for this build configuration.

</note>

This tab allows you to adding, edit, and delete parameters/properties/variables, as well as override initial values of [predefined parameters](predefined-build-parameters.md).   

The following limitations apply:

* Predefined properties and variables do not allow you to edit their names (only values are editable).
* You can delete only newly added properties and variables. Predefined properties cannot be removed.
* Parameter values must not exceed 16,000 characters.

## Comment and Tags

This tab allows you to add optional comments and [tags](build-actions.md#Add+Tags+to+Build) to your custom builds. You can also add a custom build to [favorites](build-actions.md#Add+Build+to+Favorites) by checking the corresponding option in this section.



## Promoting Build

Promoted builds are custom builds with an overridden [artifact or snapshot dependencies](dependent-build.md). Such builds utilize different dependency builds compared to those they would use by default.

For example, a build configuration A retrieves artifacts from a build configuration B. Normally, running a new A build utilizes the last successful B build. If you want A to use an older B build, this earlier B build needs to be promoted.

To promote a build, open the build results page of the dependency build and click __Actions | Promote__. Promotion has a one-time effect: after the current run, build configurations revert back to their default dependency logic (last successful or last pinned build).

See the [following blog post](https://blog.jetbrains.com/teamcity/2012/04/teamcity-build-dependencies-2/) for more information.

 <seealso>
        <category ref="installation">
            <a href="upgrading-teamcity-server-and-agents.md" product="tc">Upgrade</a>
        </category>
        <category ref="concepts">
            <a href="working-with-build-queue.md">Working with Build Queue</a>
            <a href="dependent-build.md">Dependent Build</a>
            <a href="personal-build.md">Personal Build</a>
        </category>
        <category ref="admin-guide">
            <a href="configuring-build-triggers.md">Configuring Build Triggers</a>
        </category>
</seealso>
