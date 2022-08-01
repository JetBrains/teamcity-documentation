[//]: # (title: What's New in TeamCity 2022.08)
[//]: # (auxiliary-id: What's New in TeamCity 2022.08;What's New in TeamCity)

## Refresh tokens for VCS Roots 

If you have a connection to a Bitbucket Cloud or GitLab VCS Root configured in TeamCity, you no longer need to enter your password when creating new projects, build configurations, or VCS Roots via this connection.
Refreshable tokens are now enabled by default for these VCS Roots. Such tokens are refreshed more frequently than passwords or access tokens, therefore providing a more secure connection.

## Restricted access token

Starting from TeamCity 2022.08, you can use [access tokens with limited permissions](configuring-your-user-profile.md#token-scope) not only for REST API requests, but also for basic authentication and for logging in via the UI.

## Support for artifacts over 4GB

Now TeamCity supports large artifacts (over 4 GB) out of the box. No additional configuration is needed to publish and download zip archives of large artifacts.

##  Maintenance mode for cloud agents

Interaction with cloud build agents can be complicated, as they can be stopped or terminated due to a number of reasons,
even if you just logged in to a build agent to investigate an issue.

Starting from this version, you can disable an agent for maintenance, which means 
that the agent will not process any new builds, and you can, for example, log in to it and view its log.

## New UI for the list of build runners

The New UI is making its way into the Administration area. Creating new build steps is more user-friendly now with the flat list of available build runners.

<img src="flat-list-build-runners.png alt="New UI for Build Runners List"/>


## Permissions to Change VCS Username in a Project

Starting from TeamCity 2022.08, Project Administrators get a new permission allowing them to change a user's VCS username in the project without adding the permission to modify usr profile and roles. 
The permission will be present for this role in the new TeamCity installations; for existing installations it has to be added manually.

See [TeamCity Build ???? release notes](teamcity-release-notes-build-.md).

## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.