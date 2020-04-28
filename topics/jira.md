[//]: # (title: JIRA)
[//]: # (auxiliary-id: JIRA)
## Authentication

When configuring a connection to Jira, use the following authentication parameters:
* for self-managed Jira (Server or Data Center): username (specified in the Jira user profile) and password
* for Jira Cloud: user email and [API token](https://developer.atlassian.com/cloud/jira/platform/jira-rest-api-basic-authentication/)


## Converting Strings into Links to Issues

When [enabling issue tracker integration](integrating-teamcity-with-issue-tracker.md#Enabling+Issue+Tracker+Integration) in addition to general settings, you need to specify which strings should be recognized as references to issues in your tracker.   
For JIRA, you need to provide a space\-separated list of [Project keys](http://confluence.atlassian.com/display/JIRA044/What+is+a+Project). You can also load all project keys automatically: check the corresponding box and test the connection to your JIRA server. If the connection is successful, the __Project keys__ field will be automatically populated. Newly created projects in JIRA will be detected by TeamCity and the project keys list will be automatically synchronized.

For example, if a project key is __WEB__, an issue key like __WEB\-101__ mentioned in a VCS comment will be resolved to a link to the corresponding issue.

 <seealso>
        <category ref="concepts">
            <a href="supported-platforms-and-environments.md">Supported Issue Trackers</a>
        </category>
        <category ref="admin-guide">
            <a href="integrating-teamcity-with-issue-tracker.md">Integrating TeamCity with Issue Tracker</a>
        </category>
</seealso>