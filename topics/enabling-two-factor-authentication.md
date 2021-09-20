[//]: # (title: Enabling Two-Factor Authentication)
[//]: # (auxiliary-id: Enabling Two-Factor Authentication)

>This functionality is provided in terms of TeamCity 2021.2 Early Access Program.

Enabling two-factor user authentication (2FA) on your TeamCity server will grant it an extra level of security. Users will have to verify their identity in two steps: by providing their regular credentials _plus_ by submitting disposable keys, generated on their personal mobile devices.

## 2FA Modes

2FA is disabled by default, but you can make it _Optional_ or _Mandatory_ on your server — in __Administration | Authentication__. The optional mode allows users to decide whether they want to enable 2FA for their accounts or not. The mandatory mode prevents users from accessing TeamCity without the 2FA verification.

We suggest that you only enable the mandatory mode after introducing the optional mode first and giving users enough time to set up 2FA for their accounts. After the mandatory mode is enabled, existing users with no 2FA will not be able to sign in to your TeamCity server. New users will get a grace period of 1 week during which they need to enable 2FA as well.

## 2FA REST API Endpoints

To manage 2FA through the [TeamCity REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html), use the following endpoints:

* `/2FA/setup` — returns a new secret key, the set pf recovery keys, and the UUID for the unconfirmed lookup of secret keys.
* `/2FA/{userLocator}/disable` — disable 2FA for a given user, can be used by a user themselves of by the administrator.
* `/2FA/confirm` — takes the UUID and password as parameters, looks up the unconfirmed secret key by the provided UUID. After that, the TOTP 6-digit password for this key is generated. If the password parameter is correct, the secret key is confirmed and 2FA is enabled.
* `/2FA/newRecoveryKeys` — generates, sets, and returns new recovery keys for a user with active 2FA. Format of recovery keys: `[0-9a-f]{6}-[0-9a-f]{6}`.
* `/2FA/refreshGracePeriod` — refreshes the period when a user can sign in without enabled 2FA. The period is managed by the `teamcity.auth.2fa.grace.period` property; the default value is 1 week.

Note that these endpoints accept only authentication via [access tokens](configuring-authentication-settings.md#Token-Based+Authentication).

The [User](https://www.jetbrains.com/help/teamcity/rest/user.html) object now has a respective boolean field `enabled2FA`.