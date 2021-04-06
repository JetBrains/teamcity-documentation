[//]: # (title: JIRA)
[//]: # (auxiliary-id: JIRA)

You can integrate TeamCity with [Jira](https://www.atlassian.com/software/jira) to provide links to Jira issues from the TeamCity UI. General information about the TeamCity integration with issue trackers is provided [here](integrating-teamcity-with-issue-tracker.md).

## Authentication

When configuring a connection to Jira, use the following authentication parameters:
* for self-managed Jira (Server or Data Center): username (specified in the Jira user profile) and password
* for Jira Cloud: user email and [API token](https://developer.atlassian.com/cloud/jira/platform/jira-rest-api-basic-authentication/)

Since version 2020.1, TeamCity can report build statuses directly to Jira Cloud in real time. To configure this extra option, you need to provide the Jira _Client ID_ and _Server secret_ when configuring a connection and add a respective [build feature](jira-cloud-integration.md).

 <seealso>
        <category ref="get_started">
            <a href="supported-platforms-and-environments.md#Issue+Tracker+Integration">Supported Issue Trackers</a>
        </category>
        <category ref="admin-guide">
            <a href="integrating-teamcity-with-issue-tracker.md">Integrating TeamCity with Issue Tracker</a>
            <a href="jira-cloud-integration.md">Jira Cloud Integration</a>
        </category>
</seealso>