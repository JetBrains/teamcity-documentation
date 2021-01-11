[//]: # (title: YouTrack)
[//]: # (auxiliary-id: YouTrack)

## Converting Strings into Links to Issues

When [enabling issue tracker integration](integrating-teamcity-with-issue-tracker.md#Enabling+Issue+Tracker+Integration), in addition to general settings, you need to specify which patterns are to be recognized as references to issues in your tracker.

For YouTrack, you need to provide a [permanent token](https://www.jetbrains.com/help/youtrack/incloud/authentication-with-permanent-token.html) for authentication and a space-separated list of __Project IDs__. You can also load all project IDs automatically: check _Use all YouTrack IDs automatically_ and test the connection to your YouTrack server. If the connection is successful, the __Project IDs__ field will be automatically populated. Newly created projects in YouTrack will be detected by TeamCity, and the project ID list will be automatically synchronized.

For example, if a project ID is __TW__, an issue ID like __TW-18802__ mentioned in a VCS comment will be resolved to a link to the corresponding issue.

## Enhancing Integration with YouTrack

YouTrack provides native TeamCity integration, which enhances the set of available features. For example:
* YouTrack is able to fill the "_Fixed in build_" field with a specific build number.
* YouTrack allows you to apply commands to issues by specifying them in a comment to a VCS change commit.
To use these features, [configure YouTrack](https://www.jetbrains.com/help/youtrack/standalone/Integration-with-TeamCity.html).

 <seealso>
        <category ref="concepts">
            <a href="supported-platforms-and-environments.md">Supported Issue Trackers</a>
        </category>
        <category ref="admin-guide">
            <a href="integrating-teamcity-with-issue-tracker.md">Integrating TeamCity with Issue Tracker</a>
        </category>
</seealso>