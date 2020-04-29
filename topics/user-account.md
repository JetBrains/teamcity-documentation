[//]: # (title: User Account)
[//]: # (auxiliary-id: User Account)

User account is a combination of username and password that allows TeamCity users to log in to the server and use its features. User accounts can be created manually, or automatically upon log in depending on used authentication scheme (refer to [Authentication Modules](authentication-modules.md) and [LDAP Integration](ldap-integration.md) sections for more details).

Each user account:
	
* Has an associated role that ensures access to all or specific TeamCity features through corresponding permissions. Learn more about [Role and Permission](role-and-permission.md).
* Belongs to at least one user group. Learn more about [user group](user-group.md).


In addition to logged in users, there is a special user account for non\-registered users called __Guest User__, that allows monitoring TeamCity projects without authorization. By default, guest login is disabled. Learn more on the [Guest User](guest-user.md) page.



<seealso>
        <category ref="concepts">
            <a href="user-group.md">User Group</a>
            <a href="role-and-permission.md">Role and Permission</a>
            <a href="authentication-modules.md">Authentication Modules</a>
        </category>
        <category ref="admin-guide">
            <a href="enabling-guest-login.md">Enabling Guest Login</a>
            <a href="ldap-integration.md">LDAP Integration</a>
            <a href="managing-user-accounts-groups-and-permissions.md">Managing User Accounts, Groups and Permissions</a>
        </category>
</seealso>