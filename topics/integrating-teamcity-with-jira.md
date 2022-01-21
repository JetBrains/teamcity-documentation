[//]: # (title: Integrating TeamCity with Jira)
[//]: # (auxiliary-id: Integrating TeamCity with Jira)

You can integrate TeamCity with [Jira](https://www.atlassian.com/software/jira) to display links to Jira issues in TeamCity UI. The [Supported Platforms and Environments](supported-platforms-and-environments.md#Issue+Trackers) page lists supported versions.


## Display Links to Jira Issues in TeamCity UI

When integration with Jira is enabled, TeamCity automatically detects Jira issues mentioned in the comments to version control changes and transforms issue keys into links to the corresponding issues in Jira. The basic issue details are displayed in the TeamCity web UI when hovering over the icon next to the issue key (for example, on the __[Changes](working-with-build-results.md#Changes)__ tab of the build results).

<img src="issue-tracker-integration.png" width="661" alt="Issue tracker integration"/>

Issues fixed in the build can also be viewed on the __[Issues](working-with-build-results.md#Related+Issues)__ tab of the build results. You can filter the list to a particular range of builds and view issues mentioned in comments with their states.

<img src="issue-log.png" width="750" alt="Issue log"/>


### Enabling Jira Integration

The integration is configured at the project level: the Project Administrator permissions are required. Note that enabling integration for the project also enables it for all its subprojects; if the configuration settings are different in a subproject, its settings have priority over the project's settings.

To enable integration, configure a connection to Jira on the __Project Settings | Issue Trackers__ page:

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

Select __JIRA__ from the list.

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

Your Jira site base URL. For Jira Cloud, it is an URL like `https://XXX.atlassian.net`.

</td></tr><tr>

<td>

__Login__

</td>

<td>

A username for self-hosted Jira (specified in the Jira user profile) or email for Jira Cloud.

</td></tr><tr>

<td>

__Password / API Token__

</td>

<td>

A assword for self-hosted Jira or [API token](https://developer.atlassian.com/cloud/jira/platform/jira-rest-api-basic-authentication/) for Jira Cloud.

</td></tr><tr>

<td>

__Project Keys__

</td>

<td>

A space-separated list of [project keys](http://confluence.atlassian.com/display/JIRA044/What+is+a+Project). You can also load all project keys automatically: check the corresponding box and test the connection to your Jira server. If the connection is successful, the __Project keys__ field will be automatically populated. Newly created projects in Jira will be detected by TeamCity, and the project keys list will be automatically synchronized.   
For example, if a project key is `WEB`, an issue key like `WEB-101`, mentioned in a VCS comment, will be resolved to a link to the corresponding issue.

</td></tr></table>


## Report TeamCity Build Statuses to Jira Cloud

Since version 2020.1, TeamCity can report build statuses directly to Jira Cloud in real time. 

If you mention a Jira issue key in a commit message in VCS, TeamCity will detect this message during the build containing this commit and send information about the build to this Jira Cloud issue.

<note>

Currently, Jira Cloud API for [Builds](https://developer.atlassian.com/cloud/jira/software/rest/#api-group-Builds) limits the number of issues per build ID to 100. If your build references more than 100 Jira issues, TeamCity will only report the first 100.

</note>

The build status will be displayed in the Jira task details:

<img src="jira-cloud-integration.png" alt="TeamCity build status in Jira Cloud" width="800"/>

You can click the status to see more information on __Builds__ (for regular and [composite](composite-build-configuration.md) builds) or __Deployments__ (for [deployment](deployment-build-configuration.md) builds) tabs.

### Enabling Jira Cloud Integration

To enable this option:

1. When configuring a connection to Jira Cloud, specify also the _Jira Cloud Client ID_ and _Server secret_.  

2. Add the _Jira Cloud Integration_ [build feature](adding-build-features.md) to a build configuration, and select a preconfigured connection to Jira Cloud.

   For a [deployment build configuration](deployment-build-configuration.md), specify also an environment type (for example, _testing_ or _production_) and an environment name. These options are required to show the deployment information in Jira Cloud.
