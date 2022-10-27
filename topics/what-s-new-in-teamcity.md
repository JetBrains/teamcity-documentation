[//]: # (title: What's New in TeamCity 2022.10)
[//]: # (auxiliary-id: What's New in TeamCity 2022.10;What's New in TeamCity)


## Easy HTTPS Access Setup on TeamCity Server

Before this version, setting up HTTPS on a TeamCity server has been one of the challenging tasks for a server administrator. 
It required the knowledge of the TeamCity server configuration and experience in configuring proxy servers. 
Now enabling HTTPS access to TeamCity is easy: after you launch your TeamCity server, 
all you need to do is upload an HTTPS certificate, or a certificate chain in the PEM format to the server, 
and TeamCity will do the rest.

These settings will affect the built-in Tomcat server configuration. 
If your TeamCity server is [behind a proxy](configuring-proxy-server.md#Set+Up+TeamCity+Server+Behind+Proxy), 
configure HTTPS on the proxy side.

[Read this article](https-server-settings.md) for details.
{product="tc"}

## Refresh tokens for VCS Roots

If you have a connection to a Bitbucket Cloud, GitLab, and Azure DevOps VCS Root configured in TeamCity, you no longer need to enter your password when creating new entities 
(projects, build configurations, or VCS Roots) via this connection.
Refresh tokens are now enabled by default for these VCS Roots. Such tokens are short-lived providing more security than passwords or personal access tokens: 
the TeamCity server refreshes them automatically without sharing any related data with agents.

## Restricted access token

You can now use [access tokens with limited permissions](configuring-your-user-profile.md#token-scope) 
not only for REST API requests, but also for basic authentication and for logging in via the UI.

## Support for artifacts over 4GB

Now TeamCity supports large artifacts (over 4 GB) out of the box. No additional configuration is needed to publish and download zip archives of large artifacts.

## Support for Amazon Web Services (AWS)

This TeamCity version supports [Amazon Web Services (AWS) connection](configuring-connections.md#AmazonWebServices).
It allows defining AWS credentials once and using them in builds via the [AWS Credentials build feature](aws-credentials.md). 
You can use different AWS credential types: access keys, IAM Role, and the Default credential provider chain.

## Maintenance mode for cloud agents

Before this version, investigating issues on cloud agents was difficult, as the agent could become unavailable in the middle of the investigation process 
when its termination condition was met. 
Now you can [disable a cloud agent for maintenance](build-agents-configuration-and-maintenance.md#disable-for-maintenance). In maintenance mode, you can log in to the agent, view its log, and perform other operations. 
The cloud agent will not be stopped according to the termination conditions and will be unavailable for builds unless assigned to a build explicitly.

## Connecting to an agent's EC2 instance via AWS SSM 

You can launch an interactive browser-based shell directly from the TeamCity UI. 
The shell helps you investigate agent-related issues and works for EC2 agents 
with preinstalled [AWS Systems Manager Agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/prereqs-ssm-agent.html) (SSM Agent).

[Read this article](viewing-build-agent-details.md#debug-and-maintenance) for details.
{product="tc"}

## New UI for the list of build runners

The Sakura UI is making its way into the Administration area. 
Creating new build steps is more user-friendly now with the flat list of available build runners.

<img src="flat-list-build-runners.png" />


## Permissions to Change VCS Username in a Project

Project Administrators now have a new permission allowing them to change a user's VCS username in the project without adding the permission to modify user profile and roles. 
The permission will be present for this role in the new TeamCity installations; for existing installations it has to be added manually.

## Promoting personal build

TeamCity provides an option to [promote](running-custom-build.md#Promoting+Build) a personal build. 
After promotion, TeamCity will try to run the promoted build and all its dependencies as [personal builds](personal-build.md#Triggering+Personal+Build+Chain) unless the check out settings for any of the dependencies differ.







## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.