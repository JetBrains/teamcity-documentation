[//]: # (title: Integrating TeamCity with Team Foundation Work Items)
[//]: # (auxiliary-id: Integrating TeamCity with Team Foundation Work Items;Team Foundation Work Items)

>In 2019, Team Foundation Server (TFS) has been renamed to [Azure DevOps Server](https://azure.microsoft.com/en-us/services/devops/server/). The content of this page is applicable to Azure DevOps Server.
>
{type="note"}

You can integrate TeamCity with Team Foundation Work Items to provide links to [work items](https://docs.microsoft.com/en-us/azure/devops/boards/work-items/about-work-items?view=azure-devops&tabs=agile-process) from the TeamCity UI. TeamCity supports Azure DevOps Server (previously named Team Foundation Server — versions 2012 or later), and Azure DevOps Services.

## Displaying Links to Work Items in TeamCity UI

When integration with Team Foundation Work Items is enabled, TeamCity automatically detects work item IDs mentioned in the comments of VCS commits. It transforms these IDs into links to the corresponding work items and displays them to TeamCity users in the UI: 

* To see the basic details of a work item in the TeamCity UI, open the __[Changes](working-with-build-results.md#Changes)__ tab of the related build’s results and hover over the icon next to the work item ID.
* Work items fixed in the build can be viewed on the __[Issues](working-with-build-results.md#Related+Issues)__ tab of the build results.
* To view work items related to a whole build configuration (not only to individual builds), use the __Issue Log__ tab of the __Build Configuration Home__ page. You can filter the list to a particular range of builds and/or enable the _Show only resolved issues_ option to display only issues fixed in the builds.

Additionally, if your changeset has related work items, TeamCity can retrieve information about them and display information in the UI even if no comment is added to the changeset.

To get the maximum benefit from the integration with Team Foundation Work Items, follow these recommendations:
* When committing changes to your version control, __always mention the work item ID__ related to the fix in the comment to the commit.
* Mark fixed work items as _Resolved_ in the issue tracker to display them with the _Fixed_ status in TeamCity logs (the time of resolve does not really matter).

>If a project has a [TFVC](team-foundation-server.md) root configured, TeamCity will suggest configuring integration with Team Foundation Work Items as well.

### Configuring Connection to Team Foundation Work Items

>Enabling TeamCity integration with Team Foundation Work Items requires Project Administrator permissions as it is configured at a project level. Note that enabling integration for a project enables it for all its subprojects as well. If the settings are different in a subproject, they have priority over the parent project's settings.

To enable the integration, create a connection to Team Foundation Work Items on the __Project Settings | Issue Trackers__ page and specify the following settings:

<table><tr>

<td>

Setting

</td>

<td>

Description

</td></tr><tr>

<td>

Connection Type

</td>

<td>

Select __Team Foundation Work Items__ from the list.

</td></tr><tr>

<td>

Display Name

</td>

<td>

Specify the connection name to distinguish it from the other connections.

</td></tr><tr>

<td>

Server URL

</td>

<td>


Enter Team Foundation Server URL in the following format:

__TFS__: `http[s]://<host>:<port>/tfs/<collection>/<project>`

__Azure DevOps__: `https://dev.azure.com/<organization>/<project>`

</td></tr><tr>

<td>

Username

</td>

<td>


Specify a user to access Team Foundation Server. This can be a username or `DOMAIN\UserName` string.   
Leave empty to let TFS select a user account that is used to run the TeamCity Server. For Azure DevOps, use [personal access tokens](team-foundation-server.md#teamFoundationServerLive).

</td></tr><tr>

<td>

Password

</td>

<td>

Enter the password for the user entered above.

</td></tr><tr>

<td>

Pattern

</td>

<td>

Specify a [Java Regular Expression](http://java.sun.com/j2se/1.5.0/docs/api/java/util/regex/Pattern.html) pattern to recognize a work item ID in the comment text. The matched text (or the first group if there are groups defined) is used as the work item number. The most common case is `#(\d+)` — this will extract `1234` as the work item ID from the text `Fix for #1234`.

</td></tr></table>

## Custom Resolved States
{product="tc"}

TeamCity supports custom states for work items. For example, to customize the _resolved_ states, set the `teamcity.tfs.workItems.resolvedStates` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) to `Closed?|Done|Fixed|Resolved?|Removed?`.
