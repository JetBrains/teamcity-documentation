[//]: # (title: What's New in TeamCity 2023.05)
[//]: # (auxiliary-id: What's New in TeamCity 2023.05;What's New in TeamCity)

## Two-Factor Authentication Enhancements

### Additional Verification for Critical Edits

Starting with version 2023.05, users who pass the two-factor authentication have one hour to perform security-related actions (disable 2FA, change user passwords, set up external OAuth accounts, generate access tokens, and so on). Once this period expires, users must re-confirm their identities and pass a new 2FA checkup before they can proceed with these edits.

This new behavior adds an extra layer of protection for your TeamCity server, and is another reason why you should always opt for enabling 2FA for users with elevated access permissions.

Learn more: [](managing-two-factor-authentication.md#Critical+Settings+Protection).

### Force 2FA for Specific User Groups

If the global two-factor authentication mode is "Optional", you can force individual [user groups](creating-and-managing-user-groups.md) to use 2FA. To do so, add the `teamcity.2fa.mandatoryUserGroupKey` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) and set its value to the required group key.

```Plain Text
teamcity.2fa.mandatoryUserGroupKey=SYSTEM_ADMINISTRATORS_GROUP
```

Learn more: [](managing-two-factor-authentication.md#Force+2FA+for+Individual+User+Groups).

## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.

## We'd Love to Hear From You


We place a high value on your feedback and encourage you to share your thoughts and suggestions. See this link for more information: [Feedback](feedback.md).