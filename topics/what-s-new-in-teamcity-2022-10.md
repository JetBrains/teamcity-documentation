[//]: # (title: What's New in TeamCity 2022.10)
[//]: # (auxiliary-id: What's New in TeamCity 2022.10)

## Easy HTTPS access setup on TeamCity server
{instance="tc"}

Before this version, setting up HTTPS on a TeamCity server has been one of the challenging tasks for a server administrator. 
It required the knowledge of the TeamCity server configuration and experience in configuring proxy servers. 
Now enabling HTTPS access to TeamCity is easy: after you launch your TeamCity server, 
all you need to do is upload an HTTPS certificate, or a certificate chain in the PEM format to the server, 
and TeamCity will do the rest.

These settings will affect the built-in Tomcat server configuration. 
If your TeamCity server is [behind a proxy](configuring-proxy-server.md), 
configure HTTPS on the proxy side.

[Read this article](https-server-settings.md) for details.
{instance="tc"}

## The Sakura UI is now default

The Sakura UI is now enabled by default for all new TeamCity users.
This fresh, modern interface created with web accessibility in mind is constantly evolving:
we have reduced visual complexity of the classic UI, improved the UI performance, and provided easier access to essential features.

The Sakura UI boasts of feature parity with the classic TeamCity UI and offers unique features,
such as a convenient sidebar, the trends view for projects, and builds comparison page.

### The flat list of build runners

The Sakura UI is making its way into the Administration area.
Creating new build steps is more user-friendly now with the flat list of available build runners.

<img src="flat-list-build-runners.png" />

Refer to [this article](teamcity-sakura-ui.md) for details.

## Support for Amazon Web Services (AWS)

This TeamCity version supports [Amazon Web Services (AWS) connection](configuring-connections.md#AmazonWebServices).
It allows defining AWS credentials once and using them in builds via the [AWS Credentials build feature](aws-credentials.md).
You can use different AWS credential types: access keys, IAM Role, and the Default credential provider chain.

## Connecting to an agent's EC2 instance via AWS SSM

You can launch an interactive browser-based shell directly from the TeamCity UI.
The shell helps you investigate agent-related issues and works for EC2 agents
with preinstalled [AWS Systems Manager Agent](https://docs.aws.amazon.com/systems-manager/latest/userguide/prereqs-ssm-agent.html) (SSM Agent).

[Read this article](setting-up-teamcity-for-amazon-ec2.md) for details.
{instance="tc"}

## Maintenance mode for cloud agents

Before this version, investigating issues on cloud agents was difficult, as the agent could become unavailable in the middle of the investigation process
when its termination condition was met.
Now you can [disable a cloud agent for maintenance](build-agents-configuration-and-maintenance.md#disable-for-maintenance). In maintenance mode, you can log in to the agent, view its log, and perform other operations.
The cloud agent will not be stopped according to the termination conditions and will be unavailable for builds unless assigned to a build explicitly.


## Updated Kotlin DSL documentation

We have made changes to the [Kotlin DSL](kotlin-dsl.md) documentation: altered the design and adjusted the layout for better readability.
Most importantly, we provided meaningful examples to improve the experience for developers who want to create projects and build configurations 
in TeamCity programmatically.

<img src="new-kotlin-dsl-doc.png" alt="New Kotlin DSL Documentation"/>


## Google Account

You can sign in to TeamCity with a [Google account](authentication-modules.md). Before enabling this module, 
you need to configure a [Google connection](configuring-connections.md#Google) in the Root project's settings.

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

## Permissions to change VCS username in a project

Project Administrators now have a new permission allowing them to change a user's VCS username in the project without adding the permission to modify user profile and roles. 
The permission will be present for this role in the new TeamCity installations; for existing installations it has to be added manually.

## Promoting personal build

You can now [promote](running-custom-build.md#Promoting+Build) a personal build. 
After promotion, TeamCity will try to run the promoted build and all its dependencies as [personal builds](personal-build.md#Triggering+Personal+Build+Chain) unless the check out settings for any of the dependencies differ.

## New REST API Requests to monitor and manage server nodes
{instance="tc"}

In TeamCity 2022.10, you can use new REST API requests to check the status of your nodes in the high availability setup 
and reassign node responsibilities. See [this section for details](multinode-setup.md#Monitoring+and+Managing+Nodes+via+REST+API).

## Improvements in Perforce support

### Support for non-default streams/feature branches in Perforce Shelve Trigger

If stream support is enabled in a Perforce VCS Root, the [Perforce Shelve Trigger](perforce-shelve-trigger.md) will now automatically detect the target stream from the changed files and trigger a personal build in this stream.

- Autodetection of the branch works in the run custom build dialog even if the default branch is specified.
- The same applies to the [REST API endpoint](https://www.jetbrains.com/help/teamcity/rest/edit-build-configuration-settings.html#Manage+Build+Triggers). You do not have to specify the stream explicitly there, but can be specified via the desiredStream HTTP parameter.
- Autodetection also works in the REST API when the `desiredBranch parameter` is not set in an HTTP request.

### Check TeamCity build status in Swarm

After you run a build with [Commit Status Publisher](commit-status-publisher.md#Perforce+Helix+Swarm)
on a changelist that has a review in Helix Swarm,
TeamCity shows the _Swarm Reviews_ section on the build overview page. From each change,
you can navigate to the change page on the Helix Swarm using `Open in Helix Swarm`.


## Upgrade notes
{instance="tc"}

Before upgrading, we highly recommend reading about [important changes in version 2022.10 compared to 2022.04.x](upgrade-notes.md#Changes+from+2022.04+to+2022.10).


## Previous releases
{instance="tc"}

* [What's New in TeamCity 2022.04](https://www.jetbrains.com/help/teamcity/2022.04/what-s-new-in-teamcity.html)
* [What's New in TeamCity 2021.2](https://www.jetbrains.com/help/teamcity/2021.2/what-s-new-in-teamcity.html)
* [What's New in TeamCity 2021.1](https://www.jetbrains.com/help/teamcity/2021.1/what-s-new-in-teamcity.html)
* [What's New in TeamCity 2020.2](https://www.jetbrains.com/help/teamcity/2020.2/what-s-new-in-teamcity.html)
* [What's New in TeamCity 2020.1](https://www.jetbrains.com/help/teamcity/2020.1/what-s-new-in-teamcity.html)
* [What's New in TeamCity 2019.2](https://www.jetbrains.com/help/teamcity/2019.2/what-s-new-in-teamcity-2019-2.html)
* [What's New in TeamCity 2019.1](https://www.jetbrains.com/help/teamcity/2019.1/what-s-new-in-teamcity-2019-1.html)

## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.