[//]: # (title: Bitbucket Cloud)
[//]: # (auxiliary-id: Bitbucket Cloud)


__Since TeamCity 10.0__, TeamCity integrates with Bitbucket Cloud, and the integration with its issue tracker can be set up separately, or as a part of TeamCity integration with [Bitbucket Cloud](integrating-teamcity-with-vcs-hosting-services.md) as a source code hosting service.

When setting up [integration](integrating-teamcity-with-issue-tracker.md#Enabling+Issue+Tracker+Integration) with the Bitbucket issue tracker, in addition to the repository URL and other general settings, you need to configure authentication and specify the issue ID pattern.

<tip>

For Bitbucket Cloud team accounts, it is possible to use the team name as the username, and the [API key](https://developer.atlassian.com/bitbucket/api/2/reference/meta/authentication#api-key) as the password.
</tip>

## Authentication

TeamCity allows you to select whether you want to connect to Bitbucket  issue tracker anonymously or to be authenticated via username/password.

## Converting Strings into Links to Issues

You also need to specify which strings should be recognized as references to issues in your tracker. For Bitbucket, you need to use the regex syntax, for example, `#(\d\+)`.  

TeamCity will resolve the issue number mentioned in a VCS comment  and will display a link to this issue in the Web UI ( for example, on the [Changes](working-with-build-results.md#Changes) Page, [Issues](working-with-build-results.md#Related+Issues) tab of the [build results](working-with-build-results.md) page).

