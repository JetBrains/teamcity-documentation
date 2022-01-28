[//]: # (title: Integrating TeamCity with Perforce)
[//]: # (auxiliary-id: Integrating TeamCity with Perforce)

This article describes how to integrate TeamCity with [Perforce Helix Core](https://www.perforce.com/products/helix-core) to:

## Prerequisites

* A Perforce Helix Core client must be installed on the TeamCity server machine.
* The path to the Perforce client must be added to the `PATH` environment variable.  
  Alternatively, a full path to `p4` could be set via the [internal property](server-startup-properties.md#TeamCity+Internal+Properties) `teamcity.perforce.customP4Path`. The property value must include the `p4` filename.

If you plan to use the agent-side [checkout mode](vcs-checkout-mode.md#agent-checkout), you need to install a Perforce Helix Core client and add it to `PATH` on the build agent machines as well.

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

## Run Builds on Perforce Sources

To be able to run builds on project sources stored in Perforce Helix Core, you need to perform two steps:
1. Create a dedicated project:
   1. Go to __Administration | Projects__ and click __Create project__.
   2. Enter the _Name_ and _ID_ of the project.
   3. Click __Create__.
  Note that this will add the project right under the _Root project_. Alternatively, you can add it under any other existing project.
2. Create a Perforce VCS root:
   1. In the __Project Settings__, go to __VCS Roots__.
   2. Click __Create VCS root__.
   3. Choose _Perforce Helix Core_ as a VCS type.
   4. Configure the settings as described in [this article](perforce.md).

After the project and Perforce root are configured, you can proceed with [adding build configurations](creating-and-editing-build-configurations.md) and running builds.

## Perforce Jobs Support

For a changelist which was checked in with one or several associated jobs, TeamCity shows a wrench icon ![wrench.png](wrench.png) which allows you to view details of the jobs when clicked or hovered over.

## Logging

All Perforce plugin operations are logged into `teamcity-vcs.log` files with category `jetbrains.buildServer.VCS.P4` (on an agent or on a server, depending on the operation context). The detailed logging can be enabled with [TeamCity Server Logs](teamcity-server-logs.md).
{product="tc"}

All Perforce plugin operations are logged into `teamcity-vcs.log` files with category `jetbrains.buildServer.VCS.P4` (on an agent or on a server, depending on the operation context).
{product="tcc"}

## Perforce Workspace Handling in TeamCity

Refer to a [separate page](perforce-workspace-handling-in-teamcity.md).


## Perforce Streams as feature branches

Refer to a [separate page](perforce-streams-as-feature-branches.md).

## Running Builds on Perforce Shelved Files

Since version 2021.2, you can [manually run](running-custom-build.md#P4-shelved-files-custom-run) or [automatically trigger](perforce-shelve-trigger.md) builds on Perforce shelved files.

If you use [Perforce Helix Swarm](https://www.perforce.com/products/helix-swarm) for code reviews, you can also [configure TeamCity to posts build statuses](commit-status-publisher.md#Perforce+Helix+Swarm) as comments to your reviews.

