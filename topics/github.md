[//]: # (title: GitHub)
[//]: # (auxiliary-id: GitHub)

TeamCity integration with GitHub issue tracker can be set up separately or as a part of TeamCity integration with [GitHub source code hosting service](integrating-teamcity-with-vcs-hosting-services.md).

<tip>

If you were using the TeamCity-GitHub [third-party plugin](https://github.com/milgner/TeamCityGithub) prior to TeamCity 10.0, you can safely [remove](installing-additional-plugins.md#Uninstalling+a+plugin+via+Web+UI) it: the built-in TeamCity integration will detect the existing connection to GitHub issue tracker and pick up your settings automatically.
</tip>


When setting up [integration](integrating-teamcity-with-issue-tracker.md#Enabling+Issue+Tracker+Integration) with GitHub, in addition to the repository URL and other general settings, you need to configure authentication and specify the issue ID pattern.

## Authentication

TeamCity allows you to select whether you want to connect to GitHub anonymously or to be authenticated via a personal access token (PAT).

<note>

Authentication via login/password is still supported but will be deprecated by GitHub on November 13, 2020. We recommend that you switch to authentication via access tokens.

[Read more](https://developer.github.com/changes/2020-02-14-deprecating-password-auth/) about the GitHub password authentication deprecation.

</note>

## Converting Strings into Links to Issues

You also need to specify which strings should be recognized as references to issues in your tracker. For GitHub, you need to use the regex syntax, for example, `#(\d+)`.

TeamCity will resolve the issue number mentioned in a VCS comment and will display a link to this issue in the web UI (for example, on the [Changes](working-with-build-results.md#Changes) page, [Issues](working-with-build-results.md#Related+Issues) tab of the [Build Results](working-with-build-results.md) page).
