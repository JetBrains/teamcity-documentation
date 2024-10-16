[//]: # (title: Enabling Guest Login)
[//]: # (auxiliary-id: Enabling Guest Login)

>If you are using TeamCity Cloud, please be extremely cautious when enabling this functionality. Unlike the TeamCity On-Premises instances, most often installed in private environments, each TeamCity Cloud instance is globally available via its URL. If guest login is disabled on the instance (default behavior), only authorized users will be able to access it. If you enable it, anyone who knows your server address will be able to view its UI in the read-only mode. We suggest that you enable it for a Cloud server only if absolutely necessary and carefully manage the restrictions of the guest user roles.
>
{type="warning" instance="tcc"}

Logging in as a [Guest User](guest-user.md) is disabled by default.

To enable the guest login in TeamCity:
1. Navigate to the __Administration | Authentication__ page.	
2. Select the _Allow login as guest user_ option.
3. Save your changes.

The __Log in as guest__ link will appear on the __Log in to TeamCity__ page.

By default, the guest user can view all the projects. To customize which projects a guest user has access to, click __Configure guest user roles__ under the _Allow login as guest user_ option and configure the respective [roles and permissions](managing-roles-and-permissions.md). This link appears only if the [per-project authorization mode](managing-roles-and-permissions.md#Changing+Authorization+Mode) is enabled.

 <seealso>
        <category ref="admin-guide">
            <a href="configuring-authentication-settings.md">Configuring Authentication Settings</a>
            <a href="creating-and-managing-users.md">Creating and Managing Users</a>
            <a href="creating-and-managing-user-groups.md">Creating and Managing User Groups</a>
            <a href="creating-and-managing-users.md#User+Account">User Account</a>
            <a href="managing-roles-and-permissions.md">Roles and Permissions</a>
        </category>
</seealso>
