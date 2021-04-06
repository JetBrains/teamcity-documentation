[//]: # (title: YouTrack)
[//]: # (auxiliary-id: YouTrack)

You can integrate TeamCity with [YouTrack](https://www.jetbrains.com/youtrack/) to provide links to YouTrack issues from the TeamCity UI. General information about the TeamCity integration with issue trackers is provided [here](integrating-teamcity-with-issue-tracker.md).

Note that TeamCity does not support the [legacy YouTrack REST API endpoints](https://blog.jetbrains.com/youtrack/2021/02/discontinuing-the-legacy-rest-api-action-required/). See [this issue](https://youtrack.jetbrains.com/issue/TW-69857) for details.

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