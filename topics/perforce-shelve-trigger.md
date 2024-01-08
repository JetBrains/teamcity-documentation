[//]: # (title: Perforce Shelve Trigger)
[//]: # (auxiliary-id: Perforce Shelve Trigger)

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

## Parametrized Shelved Changelist ID

TeamCity provides a [configuration parameter](predefined-build-parameters.md) `vcsRoot.rootExternalId.shelvedChangelist` with the ID of the changelist whose changes triggered this build. 

If the VCS Root ID is unavailable/unnecessary, use the `vcsRoot.1.shelvedChangelist` [configuration parameter](predefined-build-parameters.md).

## Logging Events

This trigger logs events to the `teamcity-triggers.log` file with the `perforceShelveTrigger` logging key.
