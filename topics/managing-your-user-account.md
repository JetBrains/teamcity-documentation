[//]: # (title: Managing your User Account)
[//]: # (auxiliary-id: Managing your User Account)

To manage your account settings, in the top right corner of the screen, click the arrow next to your username and select __My Settings &amp; Tools__ from the drop\-down list.

## Changing Your Password

If _Built-in Authentication_ is configured, TeamCity server maintains passwords for the user authentication. To change the password, on the __General__ tab of the user profile page, type your new password in the __Password__ and __Confirm password__ fields.

The password can only be changed for the built-in authentication. If you don't see these fields, this means that TeamCity is configured to use external authentication and the password should be changed in the corresponding external system. 

You can reset your built-in authentication password using the "Reset password" link on the login page.

If you change or reset your password, TeamCity will automatically sign you out of all sessions.

## Managing Access Tokens

If [Token-Based Authentication](configuring-authentication-settings.md#Token-Based+Authentication) is enabled on the TeamCity server, you can create access tokens and use them for authentication:

* instead of your password (for example, in scripts or IDE plugin login), _or_
* as the value of the `Authorization: Bearer <token-value>` HTTP header. For instance, in REST API requests:   
   ```Shell
   curl --header "Authorization: Bearer <token-value>" http://<host>:<port>/app/rest/builds
   ```

You can manage tokens in __My Settings &amp; Tools | Access Tokens__. Note that the token value is only available during token creation and is not possible for retrieval afterwards.

## Managing Version Control Username Settings

On the __General__ tab of the __My Settings &amp; Tools__ page, you can see the list of your version control usernames in the __Version Control Username Settings__ area.   
By default, TeamCity uses your login name as the VCS username. Click __Edit__ to provide actual usernames for version control systems you use. Make sure the user names are correct.   
These settings are not used for authentication for the particular VCS, and so on.

These settings enable you to:
* track your changes status on the [Changes](viewing-your-changes.md),
* highlight such builds on the Projects page if the appropriate [option is selected](#Customizing+UI),
* notify you on such builds when the __Builds affected by my changes__ option is selected in [notifications settings](subscribing-to-notifications.md#What+Will+Be+Watched).

## Customizing UI

On the __General__ tab of the __My Settings &amp; Tools__ page, you can customize the following UI settings:
* Highlight my changes and investigations: Select to highlight builds that include your changes (changes committed by a user with the VCS username provided in the [Version Control Username Settings](#Managing+Version+Control+Username+Settings) section) and problems you were assigned to investigate on the __Projects__ page, __Project Home__ page, __Build Configuration Home__ page.
* Show date/time in my timezone: Check the option, if you want TeamCity to automatically detect your time zone and show the date and time (for example, build start, vcs change time, and so on) according to it.
* Show all personal builds
* Add builds manually triggered by you to your [favorites](favorite-build.md).

## Viewing your Roles and Permissions
In the top right corner of the screen, click the arrow next to your username, and select __My Settings &amp; Tools__ from the drop-down list.
* To view the list of user groups you are in, go to the __Groups__ tab.
* To view your roles and permissions in different projects, go to the __Roles__ tab. Note, that roles are assigned to the user by the system administrator.

__ __

__See also:__

__Concepts__: [Role and Permission](role-and-permission.md)   
__User's Guide__: [Viewing Your Changes](viewing-your-changes.md) | [Subscribing to Notifications](subscribing-to-notifications.md) 

__ __