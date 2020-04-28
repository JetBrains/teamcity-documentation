[//]: # (title: Enabling Guest Login)
[//]: # (auxiliary-id: Enabling Guest Login)
Logging in as [Guest User](guest-user.md) is turned off by default.

__To enable the guest login to TeamCity:__
	
1. Navigate to the __Administration | Authentication__ page.	
2. Select the __Allow login as guest user__ option.
3. Save your changes.


The __Log in as guest__ link appears on the __Log in to TeamCity__ page.

By default, the guest user can view all the projects. To customize which projects guest user has access to:

Click the __Configure guest user roles__ link to configure the [Role and Permission](role-and-permission.md). The link appears when the [per-project authorization mode](role-and-permission.md#Changing+Authorization+Mode) is enabled.


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
