[//]: # (title: Integrating TeamCity with Jira)
[//]: # (auxiliary-id: Integrating TeamCity with Jira)

TeamCity integration with [Jira](https://www.atlassian.com/software/jira) allows:
* Displaying links to Jira issues in TeamCity UI — applicable to self-managed Jira (Server or Data Center) and Jira Cloud
* Reporting TeamCity build statuses to Jira Cloud


The [Supported Platforms and Environments](supported-platforms-and-environments.md#Issue+Trackers) page lists supported versions.


## Displaying Links to Jira Issues in TeamCity UI

When integration with Jira is enabled, TeamCity automatically detects Jira issue keys mentioned in VCS commit comments and transforms these keys into links to the corresponding issues in Jira. An issue's basic details are displayed in the TeamCity web UI when hovering over the icon next to the issue key (for example, on the __[Changes](working-with-build-results.md#Changes)__ tab of the build results).

<img src="issue-tracker-integration.png" width="661" alt="Issue tracker integration"/>

Issues fixed in the build can also be viewed on the __[Issues](working-with-build-results.md#Related+Issues)__ tab of the build results. You can filter the list to a particular range of builds and view issues mentioned in comments with their states.

<img src="issue-log.png" width="750" alt="Issue log"/>

To get maximum benefit from the Jira integration, follow these recommendations:
* When committing changes to your version control, __always mention the issue key__ related to the fix in the comment to the commit.
* Resolve issues when they are fixed (the time of resolve does not really matter).
* Use __Issue Log__ of a build configuration to get issues related to builds. Enable the _Show only resolved issues_ option to only display the issues fixed in the builds.


### Configuring Connection to Jira

>TeamCity integration with Jira requires Project Administrator permissions as it is configured at the project level. Note that enabling integration for the project also enables it for all its subprojects. If the configuration settings are different in a subproject, the subproject's settings have priority over the parent project's settings.

To enable integration, you need to configure a connection to Jira. To do this, create a connection on the __Project Settings | Issue Trackers__ page and specify the following settings:

<table>

<tr>

<td></td>
<td></td>

</tr>

<tr>

<td>

__Connection Type__

</td>

<td>

Select __Jira__ from the list.

</td></tr><tr>

<td>

__Display Name__

</td>

<td>

Specify the connection name.

</td></tr><tr>

<td>

__Server URL__

</td>

<td>

Enter Jira's base URL. For Jira Cloud, it is a URL like `https://XXX.atlassian.net`.

</td></tr><tr>

<td>

__Login__

</td>

<td>

A username for self-managed Jira (specified in the Jira user profile) or email for Jira Cloud.

</td></tr><tr>

<td>

__Password / API Token__

</td>

<td>

A password for self-managed Jira or [API token](https://developer.atlassian.com/cloud/jira/platform/jira-rest-api-basic-authentication/) for Jira Cloud.

</td></tr><tr>

<td>

__Project Keys__

</td>

<td>

Enter a space-separated list of [project keys](http://confluence.atlassian.com/display/JIRA044/What+is+a+Project) to specify which strings should be recognized as references to issues in Jira. For example, if a project key is `WEB`, an issue key like `WEB-101`, mentioned in a VCS comment, will be turned into a link to the corresponding issue.  

You can also load all project keys automatically: check the corresponding box and test the connection to your Jira server. If the connection is successful, the __Project Keys__ field will be automatically populated. Newly created projects in Jira will be detected by TeamCity, and the list of project keys will be automatically synchronized.   


</td></tr></table>

Nota that a user configured in the connection to Jira should have sufficient permissions to view issues that can be mentioned in TeamCity. Also, TeamCity users will be able to view details in TeamCity for all issues to which the configured user has access.

   
## Reporting TeamCity Build Statuses to Jira Cloud

Since version 2020.1, TeamCity can report build statuses to Jira Cloud in real time. If you mention a Jira issue key in a VCS commit message, TeamCity will detect this message when running the build that contains this commit and send information about the build to this Jira Cloud issue.

<note>

Currently, Jira Cloud API for [Builds](https://developer.atlassian.com/cloud/jira/software/rest/#api-group-Builds) limits the number of issues per build ID to 100. If your build references more than 100 Jira issues, TeamCity will only report the first 100.

</note>

The build status will be displayed in the Jira task details:

<img src="jira-cloud-integration.png" alt="TeamCity build status in Jira Cloud" width="800"/>

You can click the status to see more information on the __Builds__ (for regular and [composite](composite-build-configuration.md) builds) or __Deployments__ (for [deployment](deployment-build-configuration.md) builds) tab.

### Enabling Feature

To make TeamCity send build/deploy information to Jira Cloud:

1. When configuring a [connection to Jira Cloud](#Configure+Connection+to+Jira), specify also the _Jira Cloud Client ID_ and _Server secret_ settings.  

2. Add the _Jira Cloud Integration_ [build feature](adding-build-features.md) to a build configuration, and select the preconfigured connection to Jira Cloud.

   For a [deployment build configuration](deployment-build-configuration.md), specify also an environment type (for example, _testing_ or _production_) and an environment name. These options are required to show the deployment information in Jira Cloud.
