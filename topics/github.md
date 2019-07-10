[//]: # (title: GitHub)
[//]: # (auxiliary-id: GitHub)
__Since TeamCity 10.0__, TeamCity integrates with GitHub, and the integration with GitHub issue tracker can be set up separately, or as a part of TeamCity integration with [Integrating TeamCity with VCS Hosting Services](integrating-teamcity-with-vcs-hosting-services.md).

<tip>

If you were using the TeamCity\-GitHub [third-party plugin](https://github.com/milgner/TeamCityGithub) prior to TeamCity 10.0, you can safely [remove](installing-additional-plugins.md) it: the built\-in TeamCity integration will detect the existing connection to GitHub issue tracker and pick up your settings automatically.
</tip>

  

When setting up [integration](integrating-teamcity-with-issue-tracker.md) with GitHub, in addition to the repository URL and other general settings, you need to configure authentication and specify the issue ID pattern.

## Authentication

TeamCity allows you to select whether you want to connect to GitHub anonymously or to be authenticated via username/password or an Access token.

## Converting Strings into Links to Issues

You also need to specify which strings should be recognized as references to issues in your tracker. For GitHub, you need to use the regex syntax, e.g. `#(\d+)`.  

TeamCity will resolve the issue number mentioned in a VCS comment  and will display a link to this issue in the Web UI (e.g. on the [Changes](working-with-build-results.md) Page, [Issues](working-with-build-results.md) tab of the [build results](working-with-build-results.md) page).
