[//]: # (title: Perforce Workspace Handling in TeamCity)
[//]: # (auxiliary-id: Perforce Workspace Handling in TeamCity)

## Overview

To perform Perforce\-related operations, TeamCity usually operates in a "no\-workspace" mode, i.e. it executes Perforce commands without workspace context. For instance, checking for changes operations and creating server\-side patches do not require Perforce workspaces creation.

The cases when a workspace is created are:
* [Agent-side checkout](vcs-checkout-mode.md#agent-checkout), the default mode. In this case TeamCity creates a Perforce workspace to checkout the sources
* Using [versioned settings](storing-project-settings-in-version-control.md) with Perforce VCS.

## Perforce Workspace Name

The names of the created workspaces start with the `TC_p4_` prefix. It is possible to provide an additional prefix for the workspace name using the `teamcity.perforce.workspace.prefix` [configuration parameter](configuring-build-parameters.md).

The name of the workspace also includes the build agent name and a hash value built from the checkout directory and (optionally) checkout rules.

## Perforce Workspace Parameters

With [agent-side checkout](vcs-checkout-mode.md#agent-checkout), TeamCity provides environment variables describing the Perforce workspace created during the checkout process.   
If several Perforce VCS Roots are used for the checkout, the variables are created for the __first__ VCS root in the list of the build's VCS Roots.   
The variables are:
* `P4USER` \- same as `vcsroot.<VCS root ID>.user` [parameter](predefined-build-parameters.md#VCS+Properties)
* `P4PORT `\- same as `vcsroot.<VCS root ID>.port` [parameter](predefined-build-parameters.md#VCS+Properties)
* `P4CLIENT` \- the name of the generated P4 workspace on the agent

These variables can be used to perform custom p4 commands after the checkout.

## Workspace Deletion

TeamCity deletes the Perforce workspaces it created in different situations:
* Immediately after a versioned settings commit (a workspace is created for each commit)
* For the agent\-side checkout \- when a [clean checkout](clean-checkout.md) is performed (TeamCity will also run `p4 sync -f` in this case, see details [below](#Perforce+sync+-f+and+workspace+reuse))
* In the background of the agent process (between builds), when it detects a non\-existing workspace directory for a workspace associated with the current agent. A TeamCity agent performs a clean-up of unused [checkout directories](build-checkout-directory.md) (the default timeout is 8 days, can be changed with the `system.teamcity.build.checkoutDir.expireHours` [system property](configuring-build-parameters.md#Defining+Build+Parameters+in+Build+Configuration)). When a checkout directory is deleted, and this directory is associated with a Perforce workspace, this workspace is deleted as well. Cleaning Perforce workspaces can be disabled via the `teamcity.perforce.workspace.cleanup=false `setting, either in the [`buildAgent.properties`](build-agent-configuration.md) file or globally at the server level as a Root project [configuration parameter](configuring-build-parameters.md).

When deleting a Perforce workspace which contains pending changes or opened files, TeamCity tries to revert the changes and remove pending changelists, and after that repeats the operation. If the second attempt fails as well, TeamCity tries to run the `p4 client -d -f`operation (forced). All those actions are logged to `teamcity-vcs.log`.

Also, TeamCity does not force workspace deletion when a Perforce edge/replica server is used.

## Perforce sync -f and workspace reuse

When [agent-side checkout](vcs-checkout-mode.md#agent-checkout) is used, the TeamCity Perforce plugin creates a workspace which is bound to the checkout directory on an agent. The checkout is performed with an incremental __p4 sync__ command (both for personal and non\-personal builds).

When a VCS Root is configured to use `p4 sync -p`, the Perforce plugin always runs this command to checkout the sources.

Usually, each [clean checkout](clean-checkout.md) build results in `p4 sync -f` command with cleaning the sources. But for the Perforce agent checkout there are exceptions described below.

### Errors during checkout

When an error occurs during the checkout, or a build is interrupted/stopped during the checkout, or a timeout occurs, no [clean checkout](clean-checkout.md) will occur for the subsequent builds on the same build agent. Instead, TeamCity will rely on the Perforce ability to recover from the state. 

### VCS Root client mapping modification

Usually, when a project administrator modifies a VCS Root client mapping specified in the VCS Root, this is considered as a change in VCS Root settings and results in a [clean checkout](clean-checkout.md). This clean checkout behaviour can be disabled using the `teamcity.perforce.enable-no-clean-checkout=true` [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties).

<note>

Changing `teamcity.perforce.enable-no-clean-checkout `[internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties) results in one\-time clean checkout for all affected build configurations.
</note>

When a VCS Root is configured to use Client Name, or Stream, no clean checkout will occur when the client mapping of the corresponding client/stream is edited in Perforce. The corresponding TeamCity issue is [TW-25344](https://youtrack.jetbrains.com/issue/TW-25344).

### Forced protection against clean checkout

There is a [configuration parameter](configuring-build-parameters.md) which protects a build configuration from the TeamCity\-initiated clean checkout, `teamcity.agent.failBuildOnCleanCheckout`. When this parameter is set to `true`, TeamCity will fail a build instead of running a clean checkout. It will never clean the workspace, unless it is explicitly requested via the [Enforce clean checkout](clean-checkout.md#Enforcing+Clean+Checkout) action or if the "Clean all files in the checkout directory before the build" option is enabled in the checkout options of the version control settings for a build configuration.

When using a custom checkout path, TeamCity will not clean the checkout directory when VCS settings are changed, and it will fail a build instead. To ignore clean checkout and proceed with incremental checkout, use the `teamcity.agent.failBuildOnCleanCheckout=ignoreAndContinue` [parameter](configuring-build-parameters.md) for a project or build configuration. Do this only if you're absolutely sure that the sources in the checkout directory are in the correct state.

The same applies to the broken personal builds. When the sources become dirty and the option is set, TeamCity will fail the build instead of running a clean checkout. You can clean the working copy via `p4 clean`, for instance, and try to continue with the `ignoreAndContinue` value after this (you can run a custom build with the specified [configuration parameter](configuring-build-parameters.md)).

The corresponding TeamCity issue is [TW-33168](https://youtrack.jetbrains.com/issue/TW-33168).

__ __