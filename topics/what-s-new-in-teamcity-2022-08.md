[//]: # (title: What's New in TeamCity 2022.08)
[//]: # (auxiliary-id: What's New in TeamCity 2022.08)

## Refresh tokens for VCS Roots

If you have a connection to a Bitbucket Cloud or GitLab VCS Root configured in TeamCity, you no longer need to enter your password when creating new entities
(projects, build configurations, or VCS Roots) via this connection.
Refresh tokens are now enabled by default for these VCS Roots. Such tokens are short-lived providing more security than passwords or personal access tokens:
the TeamCity server refreshes them automatically without sharing any related data with agents.

## Restricted access token

Starting from TeamCity 2022.08, you can use [access tokens with limited permissions](configuring-your-user-profile.md#token-scope) not only for REST API requests, but also for basic authentication and for logging in via the UI.

## Support for artifacts over 4GB

Now TeamCity supports large artifacts (over 4 GB) out of the box. No additional configuration is needed to publish and download zip archives of large artifacts.

##  Maintenance mode for self-hosted cloud agents

Interaction with cloud build agents can be complicated, as they can be stopped or terminated due to a number of reasons,
even if you just logged in to a build agent to investigate an issue.

Starting from this version, you can disable [a self-hosted cloud agent](teamcity-cloud-subscription-and-licensing.md#cloud-self-hosted-agents) for maintenance, which means
that the agents will not process any new builds, and you can, for example, log in to it and view its log.
{instance="tcc"}

## New UI for the list of build runners

The New UI is making its way into the Administration area. Creating new build steps is more user-friendly now with the flat list of available build runners.

<img src="flat-list-build-runners.png" />


## Permissions to Change VCS Username in a Project

Starting from TeamCity 2022.08, Project Administrators get a new permission allowing them to change a user's VCS username in the project without adding the permission to modify user profile and roles.
The permission will be present for this role in the new TeamCity installations; for existing installations it has to be added manually.

See [TeamCity Build 115573 release notes](teamcity-release-notes-build-115573.md).
{instance="tcc"}

## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.