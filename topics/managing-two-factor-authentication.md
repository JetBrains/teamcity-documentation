[//]: # (title: Managing Two-Factor Authentication)
[//]: # (auxiliary-id: Managing Two-Factor Authentication;Enabling Two-Factor Authentication)

Enabling two-factor user authentication (2FA) on your TeamCity server grants it an extra level of security. Users will have to verify their identity in two steps: by providing their regular credentials _plus_ by submitting disposable keys, generated on their personal mobile devices.

>You can combine TeamCity 2FA with authentication via an external service. See [supported providers](authentication-modules.md#auth-modules).

To select the required 2FA authentication mode, navigate to the **Administration | Authentication** page and scroll down to the **General settings** section. Note that only [system administrators](managing-roles-and-permissions.md) can modify authentication settings.

<img src="dk-2FASettings.png" width="708" alt="Available 2FA modes"/>

| 2FA Mode  | Behavior                                                                                                                                                                                        |
|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Optional  | Lets users decide whether they want to enable 2FA for their accounts. This is the default setting.                                                                                              |
| Mandatory | Requires all users to set up 2FA within one week. The grace period starts from the moment you enable the "Mandatory" mode (for existing users), or the moment a user registers (for new users). |
| Disabled  | Users cannot set up 2FA.                                                                                                                                                                        |


## Critical Settings Protection
{product="tc"}

If the two-factor authentication is enabled, users who pass the 2FA checkup have one hour to modify critical user settings. Once this period expires, users must pass a new 2FA verification before they can proceed with these edits.

Edits that are blocked until a user passes another verification include:

* Disabling 2FA in user profile settings
* Changing user password and email
* Generating access tokens

> These edits are blocked only if an attacker makes them in the TeamCity web UI. TeamCity server still accepts modification requests sent via [REST API](teamcity-rest-api.md).
> 
{type="note"}

This behavior adds an extra layer of protection that prevents attackers who gain access to a user's account from modifying user settings and inflicting more damage.

You can modify the duration of this interval via the `teamcity.2fa.sensitive.settings.access.duration` [internal property](server-startup-properties.md#TeamCity+Internal+Properties):

```Plain Text
teamcity.2fa.sensitive.settings.access.duration.seconds=45
# or
teamcity.2fa.sensitive.settings.access.duration.minutes=5
# or
teamcity.2fa.sensitive.settings.access.duration.hours=3
```


## Force 2FA for Individual User Groups
{product="tc"}

If the global two-factor authentication mode is "Optional", you can force individual [user groups](creating-and-managing-user-groups.md) to use 2FA. To do so, add the `teamcity.2fa.mandatoryUserGroupKey` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) and set its value to the required group key.

```Plain Text
teamcity.2fa.mandatoryUserGroupKey=SYSTEM_ADMINISTRATORS_GROUP
```

User groups with mandatory 2FA mode share this requirement with their child user groups. You can use this behavior to force 2FA for multiple groups at once. To do this, create a new user group, assign its key to the `teamcity.2fa.mandatoryUserGroupKey` internal property, and set this group as a parent for all existing groups whose users are required to use 2FA. 

<img src="dk-2FAGroups.png" width="708" alt="Parent user group with enforced 2FA"/>

## See Also

* [How to configure 2FA for your user account](configuring-your-user-profile.md#Configuring+Two-Factor+Authentication)
* [How to manage 2FA via TeamCity REST API](https://www.jetbrains.com/help/teamcity/rest/manage-2fa.html)
