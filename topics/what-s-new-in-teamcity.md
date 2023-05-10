[//]: # (title: What's New in TeamCity 2023.05)
[//]: # (auxiliary-id: What's New in TeamCity 2023.05;What's New in TeamCity)

## Dark Theme

In this release cycle we implemented one of the most upvoted requests: TeamCity Dark Theme.

<img src="dk-TCDark.png" width="706" alt="TeamCity Dark Theme"/>

Along with "Light" and "Dark" options, the theme selector allows you to choose "System theme" to automatically apply a skin that fits your current OS settings.

Since TeamCity is a true powerhouse with hundreds of UI pages and dialogs, the new theme may contain deficiencies. We encourage you to report all issues and visual artifacts via the following channels: [Feedback](feedback.md).


## .NET 8 Support

TeamCity 2023.05 now supports [.NET 8.0](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) framework by Microsoft. This means TeamCity agents correctly recognize the corresponding SDK installed on agent machines and the [.NET build runner](net.md) successfully builds projects that target .NET 8.0.


## VCS Integrations Enhancements

### Connect to GitHub via GitHub Apps

Starting with this release, TeamCity can work with GitHub and GitHub Enterprise instances via connections that utilize [GitHub Apps](https://docs.github.com/en/apps/creating-github-apps/setting-up-a-github-app/about-creating-github-apps). GitHub Apps is the superior way to provide access to your personal and organization repositories. It boasts fine-grained permissions, grants you more control over which repositories the app can access, and does not require keeping a dedicated "service" user to produce OAuth access tokens.

<img src="dk-add-GHApp-connection.png" alt="Create a GitHub App Connection" width="706"/>

GitHub App connections allow you to check out GitHub.com and GitHub Enterprise repositories, share access tokens with [Commit Status Publisher](commit-status-publisher.md#GitHub) and [](pull-requests.md) build features, set up webhooks that GitHub uses to notify TeamCity about a repository change, and enable the related [authentication module](configuring-authentication-settings.md#GitHub).

Learn more: [Configuring Connections](configuring-connections.md#GitHub).

### Reissue Refreshable Tokens for VCS Roots

If a VCS root is configured via TeamCity [connections](configuring-connections.md) to access Git repositories hosted in Bitbucket Server, Bitbucket Cloud, GitHub (via [GitHub Apps](#Connect+to+GitHub+via+GitHub+Apps) connection) or GitLab, the "Authentication Settings" section of this root's settings now displays the **Acquire new** button. This button allows you to instantly replace the refreshable token used by the VCS Root with a new token issued for the current user.

<img src="dk-refreshableGitToken.png" width="706" alt="Reissue Token" />

Short-lived refreshable tokens provide more security compared to passwords or personal access tokens since the TeamCity server refreshes them automatically without sharing any related data with agents.

Learn more: [Refreshable tokens](git.md#refresh-token).


## AWS-Related Updates

### Share AWS Connections with Child Projects

The [](aws-credentials.md) build feature can now utilize AWS connections owned by a parent TeamCity project. Previously, you had to configure a connection and a build feature within the same project.

<!--### Utilize Amazon Spot Placement Scores

[Spot instances](setting-up-teamcity-for-amazon-ec2.md#Amazon+EC2+Spot+Instances+support) enable you to request unused AWS EC2 instances at steep discounts (compared to On-Demand prices). As a result, you can significantly lower costs for Amazon-hosted TeamCity agents.

Starting with version 2023.05, you can allow TeamCity to request [spot placement scores](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-placement-score.html) and automatically choose AWS Regions or Availability zones in which (according to these scores) your spot requests are most likely to succeed. As a result, you can expect more stable and frequently available spot instance agents.

Learn more: [](setting-up-teamcity-for-amazon-ec2.md#Required+IAM+permissions).-->

### IMDSv2 Support

IMDSv2 is the new version of the Instance Metadata Service by Amazon that [addresses a number of IMDSv1 vulnerabilities](https://aws.amazon.com/blogs/security/defense-in-depth-open-firewalls-reverse-proxies-ssrf-vulnerabilities-ec2-instance-metadata-service/). AWS Image settings allow you to choose whether using the IMDSv2 is optional or required.

<img src="dk-IMDSv2.png" width="706" alt="Configure Metadata Service Type"/>

TeamCity 2023.05 supports EC2 instances and Amazon Machine Images (AMIs) with both "Optional" and "Required" IMDSv2 settings. If the selected IMDSv2 mode is "Optional", TeamCity attempts to use v2 first.


## Multinode Setup Enhancements

### Round-Robin

Version 2023.05 introduces a new requests distribution logic that spreads the load more effectively between TeamCity nodes and minimizes the number of negatively affected users when a node is down due to a planned maintenance or an unexpected failover.

This new logic is based on sending new requests to a random node that has the [Processing user requests to modify data](multinode-setup.md#Processing+User+Requests+to+Modify+Data+on+Secondary+Node) responsibility. After this initial draw, TeamCity memorizes which node was selected and relegates subsequent requests to the same node.

Learn more: [](multinode-setup.md#Round-Robin).

### Assign the VCS Polling Responsibility to Multiple Nodes

The [multinode setup](multinode-setup.md) allows you to assign different sets of duties to TeamCity nodes. To do so, check the required responsibilities on the **Administration | Nodes Configuration** page.

Prior to version 2023.05, the "VCS repositories polling" responsibility (allows nodes to poll repositories for new commits and detect changes) was available for a single node in the entire cluster. Starting with this version, you can assign this responsibility to multiple nodes. This enhancement allows you to evenly distribute the load across nodes and reduce the delay before triggering new builds.

Learn more: [VCS Repositories Polling](multinode-setup.md#VCS+Repositories+Polling+on+Secondary+Node).


## HTTPS Access Enhancements

### Fetch HTTPS Certificates via Let's Encrypt
{id="fetch-certificates-via-lets-encrypt"}

[Let's Encrypt](https://letsencrypt.org) is a non-profit Certificate Authority (CA) that provides TLS certificates trusted by all modern browsers. Starting with version 2020.05, TeamCity can contact this CA to automatically issue and set up a valid certificate.

<img src="dk-https-letsencrypt.png" width="706" alt="Obtain certificate via Lets Encrypt"/>

Certificates issued by Let's Encrypt are automatically renewed 30 days before expiration, and cover both your TeamCity server domain and if configured, the [artifacts isolation domain](teamcity-configuration-and-maintenance.md#artifacts-domain-isolation).

Learn more: [HTTPS Server Settings](https-server-settings.md).

### Specify the Required Encryption Protocol for HTTPS Connection

If your TeamCity server allows access via HTTPS, the server's default protocol for communicating with clients is currently TLS Version 1.2. Starting with version 2023.05, you can specify the list of available encryption protocols or force TeamCity to use one specific protocol.

<img src="dk-tls-protocols.png" width="706" alt="TeamCity using the TLS 1.3 Protocol"/>

Learn more: [](https-server-settings.md#Specify+Available+Encryption+Protocols).


## Interactive Agent Terminals

With this update you can open remote terminals to agent machines directly from the TeamCity UI. These terminals allow system administrators to maintain local and cloud agents and troubleshoot issues.

<img src="dk-agentTerminal-cat.png" width="706" alt="Agent Terminal Window"/>

Learn more: [Install and Start TeamCity Agents](install-and-start-teamcity-agents.md#Debug+Agents+Remotely).


## New Service Messages

### Send Slack Messages and Emails via Service Messages

TeamCity [](service-messages.md) allow you to report various information about the build by adding special messages to your build scripts. The list of available service messages now includes the `##teamcity[notification ...]` message that sends emails, Slack direct messages, and posts updates to Slack channels.

<img src="dk-serviceMessages-whatsNew.png" width="706" alt="Custom TeamCity messages in Slack and Email boxes"/>

Built-in security features ensure messages cannot be sent to wrong recipients and cannot include links to external web resources that are not configured as trusted.

Learn more: [Slack Messages](service-messages.md#Sending+Custom+Slack+Messages) | [Emails](service-messages.md#Sending+Custom+Email+Messages).

### Add and Remove Build Tags via Service Messages

You can now send TeamCity [service messages](service-messages.md) to add and remove [build tags](build-actions.md#Add+Tags+to+Build).

<img src="dk-servicemessage-tags.png" width="706" alt="Tagging builds with OS names"/>

To add and remove tags, send the following messages:

```Plain Text
##teamcity[addBuildTag 'your-custom-tag']
##teamcity[removeBuildTag 'tag-to-remove']
```

Learn more: [Service Messages](service-messages.md#Adding+and+Removing+Build+Tags).


## REST API Updates

### Manage SSH Keys

Starting with version 2023.05, you can perform the full range of operations on projects' SSH keys via [REST API](teamcity-rest-api.md): upload and generate new keys, modify VCS authentication settings, set passphrases for encrypted keys, and browse and remove uploaded keys. To do this, send required requests to the `/app/rest/projects/<project_locator>/sshKeys` endpoint.

Learn more: [SSH Keys Management](ssh-keys-management.md#REST+API).

### Manage Versioned Settings

Our REST API now allows you to manage settings related to [storing project settings in VCS](storing-project-settings-in-version-control.md). You can use this new API to modify these settings, check for changes, and load/commit changes from/to the related VCS. Explore the `/app/rest/projects/{locator}/versionedSettings/` endpoint to view available requests.

### Manage User Roles

The new `/app/rest/roles` endpoint allows you to obtain, modify, and remove existing [roles](managing-roles-and-permissions.md#Managing+Roles), as well as create new ones.


## Two-Factor Authentication Enhancements

### Additional Verification for Critical Settings

Starting with version 2023.05, users who pass the two-factor authentication have one hour to perform security-related actions: disable 2FA, change user password and/or email, and generate access tokens. Once this period expires, users must re-confirm their identities and pass a new 2FA verification before proceeding with these actions.

This new behavior adds an extra layer of protection for your TeamCity server.

Learn more: [](managing-two-factor-authentication.md#Critical+Settings+Protection).

### Force 2FA for Specific User Groups

If the global two-factor authentication mode is "Optional", you can now force individual [user groups](creating-and-managing-user-groups.md) to use 2FA. To do so, add the `teamcity.2fa.mandatoryUserGroupKey` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) and set its value to the required group key.

```Plain Text
teamcity.2fa.mandatoryUserGroupKey=SYSTEM_ADMINISTRATORS_GROUP
```

Learn more: [](managing-two-factor-authentication.md#Force+2FA+for+Individual+User+Groups).


## Miscellaneous

* The [Notifications build feature](notifications.md) now allows you to enter multiple recipient addresses.
* Added `env.BUILD_URL` to the list of [predefined environment variables](predefined-build-parameters.md#Predefined+Server+Build+Parameters). This variable returns a link to the current build.
* The [SSH Keys](ssh-keys-management.md) page now displays the button that allows you to generate a new key. Generating keys on TeamCity server is faster and more secure (compared to running `ssh-keygen` locally and manually uploading the keys).
* Added two new endpoints that you can check by sending GET requests to obtain the current server status:
  * the `<server_URL>/healthCheck/healthy` endpoint returns "200" if a server is running, even if it is still initializing or in [maintenance mode](teamcity-maintenance-mode.md).
  * the `<server_URL>/healthCheck/ready` endpoint returns "200" if a server is fully initialized and ready to accept user requests. If the server is still initializing or awaits for a data upgrade, the endpoint returns "503".


## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.

## We'd Love to Hear From You


We place a high value on your feedback and encourage you to share your thoughts and suggestions. See this link for more information: [Feedback](feedback.md).