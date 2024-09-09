[//]: # (title: Integrating TeamCity with Perforce)
[//]: # (auxiliary-id: Integrating TeamCity with Perforce;Perforce Streams as feature branches)

This article describes how to integrate TeamCity with [Perforce Helix Core](https://www.perforce.com/products/helix-core) to:
* Build sources of projects stored in a Helix Core repository.
* Use Perforce streams as feature branches and build their sources independently of each other.
* Pre-test and pre-build files in shelved changelists.
* Apply automatic labels to sources.
* Report build statuses to code reviews in Perforce Helix Swarm.

## Prerequisites

* TeamCity supports Perforce Helix Core servers/clients starting from Helix Core 2017.1
* A Perforce Helix Core client must be installed on the TeamCity server machine.
{product="tc"}
* The path to the Perforce client must be added to the `PATH` environment variable.  
  Alternatively, a full path to `p4` could be set via the [internal property](server-startup-properties.md#TeamCity+Internal+Properties) `teamcity.perforce.customP4Path`. The property value must include the `p4` filename.
{product="tc"}
* If you plan to use the [agent-side checkout mode](vcs-checkout-mode.md#agent-checkout), you need to install a Perforce Helix Core client and add it to `PATH` on build agent machines as well.
{product="tcc"}

## Running Builds on Perforce Helix Core Sources

To be able to run builds on project sources stored in Perforce Helix Core and to use all the features described in this article, you need to perform two procedures:
1. Create a dedicated project in TeamCity:
   1. Go to __Administration | Projects__ and click __Create project__.  
      Note that this will add the project right under the _Root project_. Alternatively, you can add it under any other existing project.
   2. Enter the _Name_ and _ID_ of the project.
   3. Click __Create__.
2. Create a Perforce VCS root:
   1. In the __Project Settings__, go to __VCS Roots__.
   2. Click __Create VCS root__.
   3. Choose _Perforce Helix Core_ as a VCS type.
   4. Configure the root's settings as described in [this article](perforce.md).

After the project and Perforce root are configured, you can proceed with [adding build configurations](creating-and-editing-build-configurations.md) and running builds.

## Running Builds on Perforce Streams

TeamCity can monitor commits in Perforce [streams](https://www.perforce.com/video-tutorials/vcs/what-perforce-streams) and work with them as with regular [feature branches](working-with-feature-branches.md).

If a [Perforce root](perforce.md) is configured to use the _Stream_ mode, you can enable the feature branches support in the root settings. After it is enabled, all streams which have the specified main stream as a parent will be included into the set of [feature branches](working-with-feature-branches.md) processed by TeamCity. To include only specific streams to this set, edit the branch specification to filter these streams. Each filter rule should start with a new line. The syntax is `+|-:stream_name`. For example, use `+://stream-depot/*` to monitor only streams located in the `stream-depot` depot, where `*` (for example, `master`) is a logical branch name. Note that streams used in the branch specification should be descendants of the main stream.

<include from="branch-filter.md" element-id="OR-syntax-tip"/>

TeamCity can process task streams as well, but it only detects new task streams if there is a non-merge commit made to them.

### Running Builds on Streams from IntelliJ IDE

You can launch a [remote build run](remote-run.md) from IntelliJ-platform IDEs, but only if a stream has been already detected by TeamCity. The TeamCity Remote Run plugin tries to deduce the correct stream according to the depot paths of the files in the IDE's working copy.  
For instance, if a file path in the working copy starts with `//depot/stream1/some/path`, TeamCity will try finding the `//depot/stream1` stream and starting the remote run there. If you have modified a file from another stream (imported into the working copy) and want to enforce a build in a particular stream, you need to specify a [configuration parameter](configuring-build-parameters.md) `teamcity.build.branch` when triggering the remote run.

### Cleaning Stream Workspaces

To properly process task streams, TeamCity needs to create dedicated workspaces on the Perforce server. To save the server resources, you can [clean inactive workspaces](perforce-workspace-handling-in-teamcity.md#Cleaning+Workspaces+on+Perforce+Server) created by TeamCity directly from the TeamCity UI.

## Running Personal Builds from IntelliJ IDE

If you write code in an IntelliJ-based IDE, you can pretest and prebuild local changes before committing them to a main Perforce repository: see common instructions on [remote run](remote-run.md), [remote debug](remote-debug.md), and [pre-test delayed commits](pre-tested-delayed-commit.md).

The remote run/debug functionality is common for all types of VCS, but there are extra conveniences specific to Perforce:
* [Remotely running builds on Perforce streams](#Running+Builds+on+Perforce+Streams).
* [Testing changes on shelved files without committing them to a depot](#Running+Builds+on+Perforce+Shelved+Files).

## Running Builds on Perforce Shelved Files

TeamCity allows you to run personal builds on Perforce [shelved files](https://www.perforce.com/manuals/v17.1/p4v/files.shelve.html). This way, you can try building changed source files before checking them into a common depot.

To **manually run** a custom build on Perforce shelved files:
1. Open the [custom run](running-custom-build.md) dialog by clicking the context menu next to the __Run__ button.
2. Enable _run as a personal build_ option.
3. Enter the ID of the changelist that contains the shelved files.
   <img src="p4-custom-run-shelved.png" alt="Run custom build on P4 shelved files" width="460"/>
4. Choose the target Perforce root.

To **configure automatic triggering** for a Perforce shelved changelist:
1. Go to __Build Configuration Settings | Triggers__.
2. Add a new trigger of the _Perforce Shelve Trigger_ type.
3. Configure its settings as described in [this article](perforce-shelve-trigger.md).

To build the target shelved changelist with [TeamCity REST API](teamcity-rest-api.md), send a request to the following endpoint:

```Shell
/app/perforce/runBuildForShelve?buildTypeId=<BUILD_TYPE_ID>&vcsRootId=<VCS_ROOT_ID>&shelvedChangelist=<SHELVED_CHANGELIST_ID>
```
{prompt="POST"}

* `BUILD_TYPE_ID` — the ID of your build configuration.
* `VCS_ROOT_ID` — the external ID of a related [](vcs-root.md).
* `SHELVED_CHANGELIST_ID` — the ID of the required changelist.

### Publishing Build Statuses to Perforce Helix Swarm

If you use [Perforce Helix Swarm](https://www.perforce.com/products/helix-swarm) for code review of shelved files, you can configure TeamCity to posts build statuses as comments to your reviews.

See this help article for more information: [](integrating-with-helix-swarm.md).

## TeamCity Workspaces in Perforce

To perform Perforce-related operations, a TeamCity server usually executes Perforce commands without the workspace context. For instance, workspaces are not required for tracking changes and for most server-side operations. However, certain cases require creating a dedicated workspace:
* By default, TeamCity uses [agent-side checkout mode](vcs-checkout-mode.md#agent-checkout) for checking out sources of a build. In this case, it creates a dedicated Perforce workspace and executes a corresponding `p4 sync` command to get the sources.
* Using a Perforce VCS root to store [versioned project settings](storing-project-settings-in-version-control.md).
* Using [Perforce streams as feature branches](integrating-teamcity-with-perforce.md#Running+Builds+on+Perforce+Streams). In this case, TeamCity creates workspaces on the Perforce server to correctly process task streams.

Learn how TeamCity creates workspaces in Perforce and works with them in [this article](perforce-workspace-handling-in-teamcity.md).

## Configuring Post-Commit Hooks on Perforce Server

By default, TeamCity uses a polling approach to detect changes in a VCS repository. It periodically sends requests to the Perforce server to detect new revisions. For large installations with hundreds of VCS roots, this may create a noticeable load on both Perforce server and TeamCity. To avoid background polling, you can set up a post-commit hook on your Perforce server. This hook will notify TeamCity to start checking for changes only when they are available.

TeamCity provides a dedicated hook script that should be saved on your Perforce server. You can find the detailed instruction [here](configuring-vcs-post-commit-hooks-for-teamcity.md#Perforce+Server).

## Label Perforce Sources

TeamCity can assign custom labels to Perforce project sources. The list of applied labels and their status is displayed on the __[Changes](build-results-page.md#Changes+Tab)__ tab of __Build Results__.

To configure automatic labeling for a build configuration:
1. Go to __Build Configuration Settings | Build Features__.
2. Add a new feature of the _VCS labeling_ type.
3. Choose a Perforce VCS root to label.
4. Specify the labeling pattern as described [here](vcs-labeling.md#Labeling+in+Perforce).

### View Perforce Jobs

If a build contains a changelist that is associated with one or more [jobs](https://www.perforce.com/manuals/p4guide/Content/P4Guide/chapter.jobs.html), TeamCity will show a wrench icon ![wrench.png](wrench.png) next to this change in the build results. Click or hover it to view the details of the relevant jobs.

### View Perforce Logs
{product="tc"}

All operations of the Perforce plugin are logged in to the `teamcity-vcs.log` files with the category `jetbrains.buildServer.VCS.P4` (on an agent or on a server, depending on the operation mode). The detailed logging can be enabled with [TeamCity Server Logs](teamcity-server-logs.md).