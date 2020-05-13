[//]: # (title: Jira Cloud Integration)
[//]: # (auxiliary-id: Jira Cloud Integration)

The _Jira Cloud Integration_ build feature allows reporting build statuses directly to Jira Cloud in real time.

It uses the [Jira Software Cloud REST API](https://developer.atlassian.com/cloud/jira/software/rest/) and requires additional authentication parameters comparing to the [regular integration with Jira](integrating-teamcity-with-issue-tracker.md#Dedicated+Support+for+Issue+Trackers) that affects only TeamCity interface. Before adding this feature to your build configuration, you need to create an [issue tracker connection to Jira](jira.md) in the parent project's settings.

## Jira Cloud Integration Settings

To set up this feature:

1. Select the required [connection to Jira Cloud](jira.md) from those available among all parent projects.
2. Select an environment type (for example, _testing_ or _production_).
3. Enter an environment name to display in Jira.

The environment options are required to show the deployment information in Jira Cloud.

## Viewing Build Statuses in Jira Cloud

If you mention a Jira task ID in the commit message in VCS, TeamCity will detect this message during the build containing this commit and send information about the build to Jira Cloud.

The build status will be displayed in the Jira task details:

<img src="jira-cloud-integration.png" alt="TeamCity build status in Jira Cloud"/>

You can click the status to see more information on __Builds__ (for regular and [composite](composite-build-configuration.md) builds) or __Deployments__ (for [deployment](deployment-build-configuration.md) builds) tabs.