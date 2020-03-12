[//]: # (title: Integrating TeamCity with Issue Tracker)
[//]: # (auxiliary-id: Integrating TeamCity with Issue Tracker)
TeamCity can be integrated with your issue tracker to provide a comprehensive view of your development project. TeamCity detects issues mentioned in the comments to version control changes, turning them into links to your issue tracker in the TeamCity web UI.

The integration is configured at the project level: the Project Administrator permissions are required. You can configure integration if you have multiple projects on both the TeamCity and the issue tracker server or if you are using different issue trackers for different projects.

Enabling integration for the project also enables it for all its subprojects; if the configuration settings are different in a subproject, its settings have priority over the project's settings.

## Dedicated Support for Issue Trackers

TeamCity supports [JIRA](jira.md), [Bugzilla](bugzilla.md), [YouTrack](youtrack.md) and __since TeamCity 10.0__ [GitHub](https://confluence.jetbrains.com/display/TCD10/GitHub), [Bitbucket Cloud](bitbucket-cloud.md), and [TFS](team-foundation-work-items.md) trackers out of the box. The [Supported Platforms and Environments](supported-platforms-and-environments.md#Issue+Tracker+Integration) page lists supported versions.

When an integration is configured, TeamCity automatically transforms an issue ID (=issue key in JIRA, work item id in TFS) mentioned in the VCS commit comment into a link to the corresponding issue and the basic issue details are displayed in the TeamCity web UI when hovering over the icon next to the issue ID (for example, on the __[Changes](working-with-build-results.md#Changes)__ tab of the build results).

<img src="issue-tracker-integration.png" width="661" alt="Issue tracker integration"/>

Issues fixed in the build can also be viewed on the __[Issues](working-with-build-results.md#Related+Issues)__ tab of the build results. You can filter the list to a particular range of builds and view issues mentioned in comments with their states.

<img src="issue-log.png" width="750" alt="Issue log"/>

## Recommendations on Using Issue Tracker Integration

To get maximum benefit from the issue tracker integration, do the following:
* When committing changes to your version control, __always mention the issue id (issue key)__ related to the fix in the comment to the commit.
* Resolve issues when they are fixed (the time of resolve does not really matter).
* Use __Issue Log__ of a build configuration to get issues related to builds; turn on the "_Show only resolved issues_" option to only display the issues fixed in the builds.

## Enabling Issue Tracker Integration

<anchor name="requirements"/>

### Requirements

The information about the issues is retrieved by the TeamCity server using the credentials provided and then is displayed to TeamCity users.

This has several implications:
* The TeamCity server needs direct access to the issue tracker. (Also, TeamCity does not yet [support](http://youtrack.jetbrains.net/issue/TW-8876) proxy for connections to issue trackers).
* The user configured in the connection to the issue tracker has to have sufficient permissions to view the issues that can be mentioned in TeamCity. Also, TeamCity users will be able to view the issue details in TeamCity for all the issues that the configured user has access to.

### Configuring connection

To enable integration, you need to create a connection to your issue tracker on the __Project Settings__ | __Issue Trackers__ page.

The settings described below are common for all issue trackers:

<table>

<tr>

<td></td>
<td></td>

</tr>

<tr>

<td>

__Connection type__


</td>

<td>

Select the type of your issue tracker from the list.


</td></tr><tr>

<td>

__Display name__


</td>

<td>

A symbolic name that will be displayed in the TeamCity UI for the issue tracker.


</td></tr><tr>

<td>

__Server URL (Repository URL)__


</td>

<td>

The Issue tracker URL


</td></tr><tr>

<td>

__Username/Password (Authentication)__


</td>

<td>

Credentials to log in to the issue tracker, if it requires authorization.


</td></tr></table>

Additional authentication information or/and the details on how to specify strings to be recognized by TeamCity and converted to your tracker issue links is provided in the corresponding sections:
* [YouTrack](youtrack.md)
* [JIRA](jira.md)
* [Bugzilla](bugzilla.md)
* [GitHub](github.md)
* [Bitbucket Cloud](bitbucket-cloud.md)
* [TFS](team-foundation-work-items.md)

## Integrating TeamCity with Other Issue Trackers

To integrate TeamCity with other issue trackers, configure TeamCity to turn any issue tracker issue ID mentions in change comments into links. See [mapping external links in comments](mapping-external-links-in-comments.md) for details.

Dedicated support for an issue tracker can also be added via a custom [issue tracker integration plugin](https://plugins.jetbrains.com/docs/teamcity/issue-tracker-integration-plugin.html).

 __  __

__See also:__


__Concepts__: [Supported Issue Trackers](supported-platforms-and-environments.md)   
__Administrator's Guide__: [Mapping External Links in Comments](mapping-external-links-in-comments.md)   
__Developing TeamCity Plugins__: [Issue Tracker Integration Plugin](https://plugins.jetbrains.com/docs/teamcity/issue-tracker-integration-plugin.html)

__ __
