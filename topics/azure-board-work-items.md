[//]: # (title: Azure Board Work Items)
[//]: # (auxiliary-id: Azure Board Work Items;Integrating TeamCity with Team Foundation Work Items;Team Foundation Work Items)

You can integrate TeamCity with Azure Board Work Items (or Team Foundation Work Items) to provide links to [work items](https://docs.microsoft.com/en-us/azure/devops/boards/work-items/about-work-items?view=azure-devops&tabs=agile-process) from the TeamCity UI. TeamCity supports Azure DevOps Server (previously named Team Foundation Server — versions 2012 or later) and Azure DevOps Services.

## Displaying Links to Work Items in TeamCity UI

When integration with Azure Board Work Items is enabled, TeamCity automatically detects work item IDs mentioned in the comments of VCS commits. It transforms these IDs into links to the corresponding work items and displays them to TeamCity users in the UI:
* To see the basic details of a work item in the TeamCity UI, open the __[Changes](build-results-page.md#Changes+Tab)__ tab of the related build’s results and hover over the icon next to the work item ID.
* Work items fixed in the build can be viewed on the __[Issues](build-results-page.md#Issues+Tab)__ tab of the build results.
* To view work items related to a whole build configuration (not only to individual builds), use the __Issue Log__ tab of the __Build Configuration Home__ page. You can filter the list to a particular range of builds and/or enable the _Show only resolved issues_ option to display only issues fixed in the builds.

Additionally, if your changeset has related work items, TeamCity can retrieve information about them and display information in the UI even if no comment is added to the changeset.

To get the maximum benefit from the integration with Azure Board Work Items, follow these recommendations:
* When committing changes to your version control, __always mention the work item ID__ related to the fix in the comment to the commit.
* Mark fixed work items as _Resolved_ in the issue tracker to display them with the _Fixed_ status in TeamCity logs (the time of resolve does not really matter).

>If a project has a [TFVC](team-foundation-version-control.md) root configured, TeamCity will suggest configuring integration with Azure Board Work Items as well.

### Configuring Connection to Azure Board Work Items

Enabling TeamCity integration with Azure Board Work Items requires Project Administrator permissions as it is configured at a project level. Note that enabling integration for a project enables it for all its subprojects as well. If the settings are different in a subproject, they have priority over the parent project's settings.

To enable the integration, create a connection to Azure Board Work Items on the __Project Settings | Issue Trackers__ page and specify the following settings:

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

Enter Azure DevOps Server URL in the following format:
* __Azure DevOps Services__: `https://dev.azure.com/<organization>/<project>`
* __Azure DevOps Server__: `http[s]://<host>:<port>/tfs/<collection>/<project>`

</td></tr><tr>

<td>

Username

</td>

<td>

Specify a user to access the Azure DevOps Server. This can be a username or `DOMAIN\UserName` string.   
Leave empty to let Azure DevOps select a user account that is used to run the TeamCity Server.

</td></tr><tr>

<td>

Password

</td>

<td>

Enter the password for the user entered above.

To authenticate via access token instead of password, leave the _Username_ field empty and enter your access token as _Password_. You can create a [personal access token](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) in your Azure DevOps account. Set the _Code_ access scope to _Work Items (read, write, and manage)_ in the repositories you are about to access from TeamCity.

</td></tr><tr>

<td>

Pattern

</td>

<td>

Specify a [Java Regular Expression](https://java.sun.com/j2se/1.5.0/docs/api/java/util/regex/Pattern.html) pattern to recognize a work item ID in the comment text. The matched text (or the first group if there are groups defined) is used as the work item number. The most common case is `#(\d+)` — this will extract `1234` as the work item ID from the text `Fix for #1234`.

</td></tr></table>

## Custom Resolved States
{instance="tc"}

TeamCity supports custom states for work items. For example, to customize the _resolved_ states, set the `teamcity.tfs.workItems.resolvedStates` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) to `Closed?|Done|Fixed|Resolved?|Removed?`.

<seealso>
        <category ref="admin-guide">
            <a href="team-foundation-version-control.md">Azure DevOps Integration</a>
        </category>
</seealso>