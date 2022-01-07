[//]: # (title: Managing Two-Factor Authentication)
[//]: # (auxiliary-id: Managing Two-Factor Authentication;Enabling Two-Factor Authentication)

Enabling two-factor user authentication (2FA) on your TeamCity server grants it an extra level of security. Users will have to verify their identity in two steps: by providing their regular credentials _plus_ by submitting disposable keys, generated on their personal mobile devices.

>You can combine TeamCity 2FA with authentication via an external service. See [supported providers](authentication-modules.md#auth-modules).

2FA is set to _Optional_ by default, but [system administrators](managing-roles-and-permissions.md) can make it _Disabled_ or _Mandatory_ on your server — in __Administration | Authentication__. The optional mode allows users to decide whether they want to enable 2FA for their accounts or not. The mandatory mode prevents users from accessing TeamCity without the 2FA verification.

After you enable the mandatory mode, users will have a grace period of 1 week during which they are supposed to set up 2FA for their accounts. New users will also get 1 week of no-2FA admission since the moment of their registration.

Read how to:
* [configure 2FA for your user account](configuring-your-user-profile.md#Configuring+Two-Factor+Authentication)
* [manage 2FA via TeamCity REST API](https://www.jetbrains.com/help/teamcity/rest/manage-2fa.html)
