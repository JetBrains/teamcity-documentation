[//]: # (title: What's New in TeamCity 2023.05)
[//]: # (auxiliary-id: What's New in TeamCity 2023.05;What's New in TeamCity)

## Manage SSH Keys via REST API

Starting with version 2023.05, you can perform the full range of operations on projects' SSH keys via [REST API](teamcity-rest-api.md): upload new keys, modify VCS authentication settings, set passphrases for encrypted keys, and browse and remove uploaded keys. To do this, send required requests to the `/app/rest/projects/<project_locator>/sshKeys` endpoint.

Learn more: [SSH Keys Management](ssh-keys-management.md#REST+API).

## Two-Factor Authentication Enhancements

### Additional Verification for Critical Edits

Starting with version 2023.05, users who pass the two-factor authentication have one hour to perform security-related actions: disable 2FA, change user password and/or email, and generate access tokens. Once this period expires, users must re-confirm their identities and pass a new 2FA verification before proceeding with these edits.

This new behavior adds an extra layer of protection for your TeamCity server.

Learn more: [](managing-two-factor-authentication.md#Critical+Settings+Protection).

### Force 2FA for Specific User Groups

If the global two-factor authentication mode is "Optional", you can force individual [user groups](creating-and-managing-user-groups.md) to use 2FA. To do so, add the `teamcity.2fa.mandatoryUserGroupKey` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) and set its value to the required group key.

```Plain Text
teamcity.2fa.mandatoryUserGroupKey=SYSTEM_ADMINISTRATORS_GROUP
```

Learn more: [](managing-two-factor-authentication.md#Force+2FA+for+Individual+User+Groups).



## Specify the Required Encryption Protocol for HTTPS Connection

If your TeamCity server allows access via HTTPS, the server's default protocol for communicating with clients is TLS Version 1.2. Starting with version 2023.05, you can specify the list of available encryption protocols or force TeamCity to use one specific protocol.

<img src="dk-tls-protocols.png" width="706" alt="TeamCity using the TLS 1.3 Protocol"/>

Learn more: [](https-server-settings.md#Specify+Available+Encryption+Protocols).





## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.

## We'd Love to Hear From You


We place a high value on your feedback and encourage you to share your thoughts and suggestions. See this link for more information: [Feedback](feedback.md).