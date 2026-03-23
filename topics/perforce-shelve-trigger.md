[//]: # (title: Perforce Shelve Trigger)
[//]: # (help-id: Perforce Shelve Trigger)

The _Perforce shelve trigger_ automatically runs a build on detecting a change in [shelved files](https://www.perforce.com/manuals/v17.1/p4guide/Content/CmdRef/p4_shelve.html) of your Perforce changelists.

>You can also run such a build manually, with the [custom run](running-custom-build.md).

## Prerequisites

The trigger supports Perforce 2018.2 or later.

## Trigger Settings

The trigger monitors all [Perforce VCS roots](perforce.md) associated with the current [build configuration](managing-builds.md). You can filter monitored changelists by their description. To do this, specify the required keyword to search.

>See this article to learn how to invoke this trigger via TeamCity REST API: [Manage Build Triggers](https://www.jetbrains.com/help/teamcity/rest/manage-build-configuration-details.html#Manage+Build+Triggers).

## Trigger Behavior

On any change made in shelved files of a matching changelist, TeamCity will start a new [personal build](personal-build.md) with the contents of these files.

If the current build is [composite](composite-build-configuration.md), the whole build chain will be triggered on a change in shelved files.

If [stream support](integrating-teamcity-with-perforce.md#Running+Builds+on+Perforce+Streams) is enabled in the Perforce VCS root settings, this trigger will detect the target stream from the changed files and run the personal build in this stream even if the default stream is specified.

<snippet id="p4-shelve-build-steps">

TeamCity processes shelved files as follows:

1. Remote sources are checked out (on the agent side, see the note below).
2. The `p4 unshelve -s <specified-changelist>` command is called.
3. `p4 sync` runs again to restore the latest revisions of any files affected by the `unshelve` step.
4. `p4 resolve -am` automatically merges changes and resolves conflicts. If any conflicts remain unresolved, the build fails.
5. The [personal build](personal-build.md) starts.
6. After the build completes, the `p4 revert` and `p4 clean` restore the workspace to its original state and remove files introduced from the shelf.

> If the [](vcs-checkout-mode.md) is set to **Always checkout files on server** (or if agent-side checkout fails), TeamCity cannot unshelve files or perform conflict resolution with p4. In this case, the build fails.
> 
> To bypass this limitation, you can set the `teamcity.internal.perforce.useUnshelve=false` parameter to the parent configuration or project to skip these steps. However, this switches TeamCity to a simpler approach that replaces existing source files with shelved ones, ignoring potential conflicts.
> 
{style="note"}

</snippet>


## Parametrized Shelved Changelist ID

TeamCity provides a [configuration parameter](predefined-build-parameters.md) `vcsRoot.rootExternalId.shelvedChangelist` with the ID of the changelist whose changes triggered this build. 

If the VCS Root ID is unavailable/unnecessary, use the `vcsRoot.1.shelvedChangelist` [configuration parameter](predefined-build-parameters.md).

## Logging Events

This trigger logs events to the `teamcity-triggers.log` file with the `perforceShelveTrigger` logging key.
