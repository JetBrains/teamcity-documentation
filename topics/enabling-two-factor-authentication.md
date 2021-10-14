[//]: # (title: Enabling Two-Factor Authentication)
[//]: # (auxiliary-id: Enabling Two-Factor Authentication)

Enabling two-factor user authentication (2FA) on your TeamCity server will grant it an extra level of security. Users will have to verify their identity in two steps: by providing their regular credentials _plus_ by submitting disposable keys, generated on their personal mobile devices.

2FA is disabled by default, but [system administrators](role-and-permission.md) can make it _Optional_ or _Mandatory_ on your server — in __Administration | Authentication__. The optional mode allows users to decide whether they want to enable 2FA for their accounts or not. The mandatory mode prevents users from accessing TeamCity without the 2FA verification.

We suggest that you only enable the mandatory mode after introducing the optional mode first and giving users enough time to set up 2FA for their accounts. After the mandatory mode is enabled, existing users with no 2FA will not be able to sign in to your TeamCity server. New users will get a grace period of 1 week during which they need to enable 2FA as well.

Read how to:
* [configure 2FA for your user account](managing-your-user-account.md#Configuring+Two-Factor+Authentication)
* [manage 2FA via TeamCity REST API](https://www.jetbrains.com/help/teamcity/rest/manage-2fa.html)
