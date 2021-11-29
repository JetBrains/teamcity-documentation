[//]: # (title: Team Foundation Work Items)
[//]: # (auxiliary-id: Team Foundation Work Items)

>In 2019, Team Foundation Server has been renamed to Azure DevOps Server. The content of this page is applicable to Azure DevOps Server.
>
{type="note"}

Team Foundation Work Items tracking is integrated with TeamCity. Supported versions are Microsoft Visual Studio Team Foundation Server 2012 or later, and Azure DevOps Services.

TFS work items support can be configured on the [Issue Trackers](integrating-teamcity-with-issue-tracker.md) page for a project. If a project has a [TFVC](team-foundation-server.md) root configured, TeamCity will suggest configuring the issue tracker as well.

## Integration

By default, the integration works the same way as the other issue tracker integrations: you need to mention the work item ID in the comment message, so the work items can be linked to builds and the links will be displayed in various places in the TeamCity web UI. 

Additionally, if your changeset has related work items, TeamCity can retrieve information about them and display information on the __Issue Log__ tab even if no comment is added to the changeset. Besides, custom states for resolved work items are supported by TeamCity.

## Settings

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

Display Name

</td>

<td>

Specify the tracker connection name

</td></tr><tr>

<td>

Server URL

</td>

<td>


Team Foundation Server URL in the following format:

__TFS 2010\+__: `http[s]://<host>:<port>/tfs/<collection>/<project>`

__Azure DevOps Services__: `https://<account>.visualstudio.com/<project>`

</td></tr><tr>

<td>

Username

</td>

<td>


Specify a user to access Team Foundation Server. This can be a username or `DOMAIN\UserName` string.   
Use blank to let TFS select a user account that is used to run the TeamCity Server. For VSTS use [personal access tokens](team-foundation-server.md#teamFoundationServerLive).

</td></tr><tr>

<td>

Password

</td>

<td>

Enter the password of the user entered above

</td></tr><tr>

<td>

Pattern

</td>

<td>

Specify the work item id format in changeset comments in the form of regexp.

</td></tr></table>

[Learn more](team-foundation-server.md#teamFoundationServerLive) about authentication in Azure DevOps Services.

## Custom Resolved States
{product="tc"}

In addition, resolved states in TeamCity can be customized by using the `teamcity.tfs.workItems.resolvedStates` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) set to `Closed?|Done|Fixed|Resolved?|Removed?` by default.