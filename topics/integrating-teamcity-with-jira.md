[//]: # (title: Integrating TeamCity with Jira)
[//]: # (auxiliary-id: Integrating TeamCity with Jira)

TeamCity integration with [Jira](https://www.atlassian.com/software/jira) allows:
* Displaying links to Jira issues in the TeamCity UI — applicable to self-managed Jira (Data Center and Server 4.4 or later) and Jira Cloud.
* Reporting TeamCity build statuses to Jira Cloud.

This article describes how TeamCity behaves when it is integrated with Jira and contains instructions on how to enable and configure the integration.

## Displaying Links to Jira Issues in TeamCity UI

When integration with Jira is enabled, TeamCity automatically detects Jira [issue keys](https://support.atlassian.com/jira-software-cloud/docs/what-is-an-issue/#Workingwithissues-Issuekeys) mentioned in VCS commit comments. It transforms these keys into links to the corresponding issues in Jira and displays them to TeamCity users in the UI. 

To see the basic details of an issue in the TeamCity UI, open the __[Changes](working-with-build-results.md#Changes)__ tab of the related build’s results and hover over the icon next to the issue key:

<img src="issue-tracker-integration.png" width="661" alt="Issue tracker integration"/>

Issues fixed in the build can be viewed on the __[Issues](working-with-build-results.md#Related+Issues)__ tab of the build results: 

<img src="issue-log.png" width="750" alt="Issue log"/>

To view issues related to a whole build configuration (not only to individual builds), use the __Issue Log__ tab of the Build Configuration Home page. You can filter the list to a particular range of builds and/or enable the _Show only resolved issues_ option to display only issues fixed in the builds.

<img src="build-configuration-issue-log.png" width="750" alt="Issue log"/>

Follow these recommendations to get the maximum benefit from the Jira integration:
* When committing changes to your version control, __always mention the issue key__ related to the fix in the comment to the commit.
* Resolve issues when they are fixed (the time of resolve does not really matter) in order that builds can display actual statuses of issues. 

### Configuring Connection to Jira

>TeamCity integration with Jira requires Project Administrator permissions as it is configured at a project level. Note that enabling integration for a project enables it for all its subprojects as well. If the settings are different in a subproject, they have priority over the parent project's settings.

To enable the integration, create a connection to Jira on the __Project Settings | Issue Trackers__ page and specify the following settings:

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

Specify the connection name (for example, `MyCompany Jira`).

</td></tr><tr>

<td>

__Server URL__

</td>

<td>

Enter the base URL of your Jira instance or server. For Jira Cloud, it is a URL like `https://XXX.atlassian.net`.

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

Enter a space-separated list of _project keys_ to specify which strings should be recognized as references to issues in Jira. For example, if a project key is `WEB`, an issue key like `WEB-101`, mentioned in a VCS comment, will be turned into a link to the corresponding issue.  

You can also load all project keys automatically: check the corresponding box and test the connection to your Jira server. If the connection is successful, the __Project Keys__ field will be automatically populated. TeamCity will detect newly created projects in Jira, and automatically synchronize the list of project keys.


</td></tr></table>

Note that a user specified in the connection to Jira should have sufficient permissions to view Jira issues, so that TeamCity can retrieve information about issues and then display it in the UI.
   
## Reporting TeamCity Build Statuses to Jira Cloud

TeamCity can report build statuses to Jira Cloud in real time. If you mention a Jira issue key in a VCS commit message, TeamCity will detect this message when running the build that contains this commit. The build status will be sent to Jira Cloud and displayed in the Jira task details.

<img src="jira-cloud-integration.png" alt="TeamCity build status in Jira Cloud" width="800"/>

You can click the status to see more information on the __Builds__ (for regular and [composite](composite-build-configuration.md) builds) or __Deployments__ (for [deployment](deployment-build-configuration.md) builds) tab.

<note>

Currently, Jira Cloud API for [Builds](https://developer.atlassian.com/cloud/jira/software/rest/#api-group-Builds) limits the number of issues per build ID to 100. If your build references more than 100 Jira issues, TeamCity will only report the first 100.

</note>

### Enabling Feature

To make TeamCity send build/deploy information to Jira Cloud:

1. When configuring a [connection to Jira Cloud](#Configure+Connection+to+Jira), specify the _Jira Cloud Client ID_ and _Server secret_ settings.  

2. Add the _Jira Cloud Integration_ [build feature](adding-build-features.md) to a build configuration, and select the preconfigured connection to Jira Cloud.

   For a [deployment build configuration](deployment-build-configuration.md), specify an environment type (for example, _testing_ or _production_) and an environment name. These options are required to show the deployment information in Jira Cloud.
