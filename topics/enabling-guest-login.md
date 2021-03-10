[//]: # (title: Enabling Guest Login)
[//]: # (auxiliary-id: Enabling Guest Login)

Logging in as a [guest user](guest-user.md) is disabled by default.

To enable the guest login in TeamCity:
1. Navigate to the __Administration | Authentication__ page.	
2. Select the _Allow login as guest user_ option.
3. Save your changes.

The __Log in as guest__ link will appear on the __Log in to TeamCity__ page.

By default, the guest user can view all the projects. To customize which projects a guest user has access to, click __Configure guest user roles__ under the _Allow login as guest user_ option and configure the respective [roles and permissions](role-and-permission.md). This link appears only if the [per-project authorization mode](role-and-permission.md#Changing+Authorization+Mode) is enabled.

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
