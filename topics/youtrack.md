[//]: # (title: YouTrack)
[//]: # (auxiliary-id: YouTrack)
## Converting Strings into Links to Issues

When [enabling issue tracker integration](integrating-teamcity-with-issue-tracker.md#Enabling+Issue+Tracker+Integration), in addition to general settings, you need to specify which patterns are to be recognized as references to issues in your tracker.

For YouTrack, you need to provide the parameters for authentication (a [permanent token](https://www.jetbrains.com/help/youtrack/incloud/authentication-with-permanent-token.html), or username/password) and a space\-separated list of __Project IDs__. You can also load all project IDs automatically: check _Use all YouTrack ids automatically_ and test connection to your YouTrack server. If the connection is successful, the __Project IDs__ field will be automatically populated. Newly created projects in YouTrack will be detected by TeamCity, and the project IDs list will be automatically synchronized.

For example, if a project ID is __TW__, an issue ID like __TW\-18802__ mentioned in a VCS comment will be resolved to a link to the corresponding issue.

## Enhancing Integration with YouTrack

YouTrack provides native TeamCity integration, which enhances the set of available features. For example:
* YouTrack is able to fill the "_Fixed in build_" field with a specific build number.
* YouTrack allows you to apply commands to issues by specifying them in a comment to a VCS change commit.
To use these features, [configure YouTrack](https://www.jetbrains.com/help/youtrack/standalone/Integration-with-TeamCity.html).

 __  __

__See also:__



__Concepts__: [Supported Issue Trackers](supported-platforms-and-environments.md)  

__Administrator's Guide__: [Integrating TeamCity with Issue Tracker](integrating-teamcity-with-issue-tracker.md)
