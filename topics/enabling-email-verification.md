[//]: # (title: Enabling Email Verification)
[//]: # (auxiliary-id: Enabling Email Verification)
__Since TeamCity 10.0__, TeamCity administrators can enable / disable email verification (off by default).

If email verification is enabled on the TeamCity server, the Email address field in the user account registration form becomes mandatory. When an address is added / modified,  the users will be asked to verify their address. If the email address is not verified, TeamCity will display a notification on the __General__ tab of the [user account](managing-users-and-user-groups.md#Editing+User+Account) settings. 

Verified email addresses will be marked with a green check on the __Administration | Users__ page.

__To enable email verification in TeamCity:__
1. Navigate to the __Administration | Authentication__ page.
2. Select the __Enable email verification__ option __(off by default)__.
3. Save your changes.   
During the [Projects Import](projects-import.md) TeamCity will take verified emails into account. If there are two users with different usernames, but the same verified email, TeamCity will provide a possibility to merge these users.

__  __

__See also:__

__Concepts__: [User Account](user-account.md) | [Role and Permission](role-and-permission.md)    
__Administrator's Guide__: [Configuring Authentication Settings](configuring-authentication-settings.md) | [Managing Users and User Groups](managing-users-and-user-groups.md)

__ __