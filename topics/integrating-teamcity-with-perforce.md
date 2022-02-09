[//]: # (title: Integrating TeamCity with Perforce)
[//]: # (auxiliary-id: Integrating TeamCity with Perforce;Perforce Streams as feature branches)

This article describes how to integrate TeamCity with [Perforce Helix Core](https://www.perforce.com/products/helix-core) to:

* Build sources of projects stores in a Helix Core repository.
* 

## Prerequisites

* A Perforce Helix Core client must be installed on the TeamCity server machine.
{product="tc"}
* The path to the Perforce client must be added to the `PATH` environment variable.  
  Alternatively, a full path to `p4` could be set via the [internal property](server-startup-properties.md#TeamCity+Internal+Properties) `teamcity.perforce.customP4Path`. The property value must include the `p4` filename.
{product="tc"}
* If you plan to use the [agent-side checkout mode](vcs-checkout-mode.md#agent-checkout), you need to install a Perforce Helix Core client and add it to `PATH` on build agent machines as well.
{product="tcc"}

### Perforce Helix Core Compatibility
{initial-collapse-state="collapsed"}

<table><tr>

<td>

Perforce server version

</td>

<td>

TeamCity version

</td>

<td>

Comment

</td></tr>

<tr>

<td>

2017.1\+

</td>

<td>

2021.2

</td>

<td>

[TW-73075](https://youtrack.jetbrains.com/issue/TW-73075)

</td></tr>

<tr>

<td>

2014.2\+

</td>

<td>

2017.1 — 2021.1

</td>

<td>


</td></tr></table>

To see the compatibility matrix for earlier versions, refer to [this page](https://www.jetbrains.com/help/teamcity/2021.1/perforce-helix-core-compatibility.html).

## Running Builds on Perforce Helix Core Sources

To be able to run builds on project sources stored in Perforce Helix Core, you need to perform two procedures:
1. Create a dedicated project in TeamCity:
   1. Go to __Administration | Projects__ and click __Create project__.  
      Note that this will add the project right under the _Root project_. Alternatively, you can add it under any other existing project.
   2. Enter the _Name_ and _ID_ of the project.
   3. Click __Create__.
2. Create a Perforce VCS root:
   1. In the __Project Settings__, go to __VCS Roots__.
   2. Click __Create VCS root__.
   3. Choose _Perforce Helix Core_ as a VCS type.
   4. Configure the settings as described in [this article](perforce.md).

After the project and Perforce root are configured, you can proceed with [adding build configurations](creating-and-editing-build-configurations.md) and running builds.

## Running Builds on Perforce Streams

TeamCity can monitor commits in Perforce [streams](https://www.perforce.com/video-tutorials/vcs/what-perforce-streams) and work with them as with regular [feature branches](working-with-feature-branches.md).

If a Perforce root is configured to use the _Stream_ mode, you can enable the feature branches support in the root settings. After it is enabled, all streams which have the specified main stream as a parent will be included into the set of [feature branches](working-with-feature-branches.md) processed by TeamCity. To include only specific streams to this set, edit the branch specification to filter these streams. Each filter rule should start with a new line. The syntax is `+|-:stream_name` (with the optional `*` placeholder). For example, use `+://stream-depot/*` to monitor only streams located in the `stream-depot` depot.

<include src="branch-filter.md" include-id="OR-syntax-tip"/>

TeamCity can process task streams as well, but it only detects new task streams if there is a non-merge commit made to them.

### Running Builds on Streams Remotely

You can launch a [remote build run](remote-run.md) from IntelliJ IDEA only if a stream has been already detected by TeamCity. The TeamCity Remote Run plugin tries to deduce the correct stream according to the depot paths of the files in the IDE's working copy.  
For instance, if a file path in the working copy starts with `//depot/stream1/some/path`, TeamCity will try finding the `//depot/stream1` stream and starting the remote run there. If you have modified a file from another stream (imported into the working copy) and want to enforce a build in a particular stream, you need to specify a [configuration parameter](configuring-build-parameters.md) `teamcity.build.branch` when triggering the remote run.

### Cleaning Stream Workspaces

To properly process task streams, TeamCity needs to create dedicated workspaces on the Perforce server. To save the server resources, you can [clean inactive workspaces](perforce-workspace-handling-in-teamcity.md#Cleaning+Workspaces+on+Perforce+Server) created by TeamCity directly from the TeamCity UI.

## Using Perforce Workspaces

## Using Perforce Shelved Changelists

##

## View Build Details

### View Perforce Jobs

If a build contains a changelist that is associated with one or more [jobs](https://www.perforce.com/manuals/p4guide/Content/P4Guide/chapter.jobs.html), TeamCity will show a wrench icon ![wrench.png](wrench.png) next to this change in the build results. Click or hover it to view the details of the relevant jobs.

## Perforce Logs
{product="tc"}

All operations of the Perforce plugin are logged in to the `teamcity-vcs.log` files with the category `jetbrains.buildServer.VCS.P4` (on an agent or on a server, depending on the operation mode). The detailed logging can be enabled with [TeamCity Server Logs](teamcity-server-logs.md).

## Perforce Workspace Handling in TeamCity

Refer to a [separate page](perforce-workspace-handling-in-teamcity.md).


## Running Builds on Perforce Shelved Files

Since version 2021.2, you can [manually run](running-custom-build.md#P4-shelved-files-custom-run) or [automatically trigger](perforce-shelve-trigger.md) builds on Perforce shelved files.

If you use [Perforce Helix Swarm](https://www.perforce.com/products/helix-swarm) for code reviews, you can also [configure TeamCity to posts build statuses](commit-status-publisher.md#Perforce+Helix+Swarm) as comments to your reviews.

