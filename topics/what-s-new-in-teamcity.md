[//]: # (title: What's New in TeamCity 2023.05)
[//]: # (auxiliary-id: What's New in TeamCity 2023.05;What's New in TeamCity)

## Interactive Agent Terminals

With this update you can open remote terminals to agent machines directly from the TeamCity UI. These terminals allow system administrators to maintain local and cloud agents and troubleshoot issues.

<img src="dk-agentTerminal-cat.png" width="706" alt="Agent Terminal Window"/>

Learn more: [Install and Start TeamCity Agents](install-and-start-teamcity-agents.md#Debug+Agents+Remotely).

## Assign the VCS Polling Responsibility to Multiple Nodes

The [multinode setup](multinode-setup.md) allows you to choose which duties each node has. To do so, check the required responsibilities on the **Administration | Nodes Configuration** page. 

Prior to version 2023.05, the "VCS repositories polling" responsibility (allows nodes to poll repositories for new commits and detect changes) was available for a single node in the entire cluster. Starting with this version, you can assign this responsibility to multiple nodes. This enhancement allows you to evenly distribute the load across nodes and reduce the delay before triggering new builds.

Learn more: [VCS Repositories Polling](multinode-setup.md#VCS+Repositories+Polling+on+Secondary+Node).

## Reissuing Refreshable Tokens for VCS Roots

If a VCS root is configured to access Git repositories hosted in Bitbucket Cloud, Bitbucket Server, GitLab or JetBrains Space, the "Authentication Settings" section of this root's settings now displays the **Acquire new** button. This button allows you to instantly replace the refreshable token used by the VCS Root with a new token issued for the current user.

<img src="dk-refreshTokenButton.png" width="706" alt="Reissue Token" />

Short-lived refreshable tokens provide more security compared to passwords or personal access tokens since the TeamCity server refreshes them automatically without sharing any related data with agents.

Learn more: [Refreshable tokens](git.md#refresh-token).

## Send Slack Messages and Emails via Service Messages

TeamCity [](service-messages.md) allow you to report various information about the build by adding special messages to your build scripts. The list of available service messages now includes the `##teamcity[notification ...]` message that sends emails, Slack direct messages, and posts updates to Slack channels.

<img src="dk-serviceMessages-whatsNew.png" width="706" alt="Custom TeamCity messages in Slack and Email boxes"/>

Built-in security features ensure messages cannot be sent to wrong recipients and cannot include links to external web resources that are not configured as trusted.

Learn more: [Slack Messages](service-messages.md#Sending+Custom+Slack+Messages) | [Emails](service-messages.md#Sending+Custom+Email+Messages).

## Manage SSH Keys via REST API

Starting with version 2023.05, you can perform the full range of operations on projects' SSH keys via [REST API](teamcity-rest-api.md): upload new keys, modify VCS authentication settings, set passphrases for encrypted keys, and browse and remove uploaded keys. To do this, send required requests to the `/app/rest/projects/<project_locator>/sshKeys` endpoint.

Learn more: [SSH Keys Management](ssh-keys-management.md#REST+API).

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



## Specify the Required Encryption Protocol for HTTPS Connection

If your TeamCity server allows access via HTTPS, the server's default protocol for communicating with clients is currently TLS Version 1.2. Starting with version 2023.05, you can specify the list of available encryption protocols or force TeamCity to use one specific protocol.

<img src="dk-tls-protocols.png" width="706" alt="TeamCity using the TLS 1.3 Protocol"/>

Learn more: [](https-server-settings.md#Specify+Available+Encryption+Protocols).





## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.

## We'd Love to Hear From You


We place a high value on your feedback and encourage you to share your thoughts and suggestions. See this link for more information: [Feedback](feedback.md).