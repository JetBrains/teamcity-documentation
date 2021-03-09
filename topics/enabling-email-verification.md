[//]: # (title: Enabling Email Verification)
[//]: # (auxiliary-id: Enabling Email Verification)

TeamCity administrators can enable / disable email verification (off by default).

If email verification is enabled on the TeamCity server, the _Email address_ field in the user account registration form becomes mandatory. When an address is added/modified, the users will be asked to verify their address. If the email address is not verified, TeamCity will display a notification on the __General__ tab of the [user account](managing-users-and-user-groups.md#Editing+User+Account) settings.

Verified email addresses will be marked with a green checkmark on the __Administration | Users__ page.

To enable email verification in TeamCity:
1. Navigate to the __Administration | Authentication__ page.
2. Select the __Enable email verification__ option __(off by default)__.
3. Save your changes.

>During the [project import](projects-import.md) TeamCity will take verified emails into account. If there are two users with different usernames, but the same verified email, TeamCity will provide an ability to merge these users.
> 
{type="note"}

<seealso>
        <category ref="concepts">
            <a href="user-account.md">User Account</a>
            <a href="role-and-permission.md">Role and Permission</a>
        </category>
        <category ref="admin-guide">
            <a href="configuring-authentication-settings.md">Configuring Authentication Settings</a>
            <a href="managing-users-and-user-groups.md">Managing Users and User Groups</a>
        </category>
</seealso>