[//]: # (title: GitHub)
[//]: # (auxiliary-id: GitHub)

TeamCity integration with GitHub issue tracker can be set up separately or as a part of TeamCity integration with the [GitHub source code hosting service](integrating-teamcity-with-vcs-hosting-services.md).

When setting up [integration](integrating-teamcity-with-issue-tracker.md#Enabling+Issue+Tracker+Integration) with GitHub, in addition to the repository URL and other general settings, you need to configure authentication and specify the issue ID pattern.

## Authentication

TeamCity allows you to select whether you want to connect to GitHub anonymously or to be authenticated via a personal access token (PAT).

>Authentication via login/password is [deprecated](https://developer.github.com/changes/2020-02-14-deprecating-password-auth/) by GitHub. Beginning August 13, 2021, GitHub [will no longer accept passwords](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/) when authenticating Git operations on GitHub.com.   
>We highly recommend that you authenticate with access tokens instead.
> 
{type="warning"}

## Converting Strings into Links to Issues

You need to specify which strings should be recognized as references to issues in your tracker. For GitHub, use the regex syntax: for example, `#(\d+)`.

TeamCity will resolve the issue number mentioned in a VCS comment and will display a link to this issue in the UI (for example, on the [__Changes__](working-with-build-results.md#Changes) page, or the [__Issues__](working-with-build-results.md#Related+Issues) tab of [__Build Results__](working-with-build-results.md)).
