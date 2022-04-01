[//]: # (title: Jira Cloud Integration)
[//]: # (auxiliary-id: Jira Cloud Integration)

The _Jira Cloud Integration_ [build feature](adding-build-features.md) allows reporting build statuses directly to Jira Cloud in real time.

> This feature uses the [Jira Software Cloud REST API](https://developer.atlassian.com/cloud/jira/software/rest/) and requires additional authentication parameters comparing to the [regular integration with Jira](jira.md#Displaying+Links+to+Jira+Issues+in+TeamCity+UI). Before adding this feature to your build configuration, you need to create an [issue tracker connection to Jira](jira.md#Configuring+Connection+to+Jira) in the parent project's settings and provide the OAuth client ID/secret there. 

## Jira Cloud Integration Settings

To set up this feature, select the required [connection to Jira Cloud](jira.md#Configuring+Connection+to+Jira) from those available among all parent projects.   
For a deployment build configuration, you need to also specify an environment type (for example, _testing_ or _production_) and an environment name. These options are required to show the deployment information in Jira Cloud.

## Viewing Build Statuses in Jira Cloud

If you mention a Jira task ID in a commit message in VCS, TeamCity will detect this message during the build containing this commit and send information about the build to Jira Cloud.

<note>

Currently, Jira Cloud API for [Builds](https://developer.atlassian.com/cloud/jira/software/rest/#api-group-Builds) limits the number of issues per build ID to 100. If your build references more than 100 Jira issues, TeamCity will only report the first 100.

</note>

The build status will be displayed in the Jira task details:

<img src="jira-cloud-integration.png" alt="TeamCity build status in Jira Cloud" width="800"/>

You can click the status to see more information on __Builds__ (for regular and [composite](composite-build-configuration.md) builds) or __Deployments__ (for [deployment](deployment-build-configuration.md) builds) tabs.
