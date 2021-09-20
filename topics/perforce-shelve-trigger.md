[//]: # (title: Perforce Shelve Trigger)
[//]: # (auxiliary-id: Perforce Shelve Trigger)

>This functionality is provided in terms of TeamCity 2021.2 Early Access Program.

The _Perforce shelve trigger_ automatically runs a build on detecting a change in [shelved Perforce files](https://www.perforce.com/manuals/v17.1/p4guide/Content/CmdRef/p4_shelve.html).

>You can also run such a build manually, with the [custom run](running-custom-build.md).

The trigger supports Perforce 2018.2 or later.

## Settings

The trigger monitors all [Perforce VCS roots](perforce.md) associated with the current [build configuration](build-configuration.md).

You can filter monitored changelists by their description. To do this, specify the required keyword to search.

On any change made in shelved files of a matching changelist, TeamCity starts a new [personal build](personal-build.md) with the contents of these files.

## API

To invoke this trigger from a script, use the following [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html) method:

```Shell
POST /app/perforce/runBuildForShelve?buildTypeId=<BuildConfigurationID>&vcsRootId=<VCSRootID>&shelvedChangelist=<ChangelistID>
```
