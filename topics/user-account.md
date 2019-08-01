[//]: # (title: User Account)
[//]: # (auxiliary-id: User Account)

User account is a combination of username and password that allows TeamCity users to log in to the server and use its features. User accounts can be created manually, or automatically upon log in depending on used authentication scheme (refer to [Authentication Modules](authentication-modules.md) and [LDAP Integration](ldap-integration.md) sections for more details).



Each user account:


	
* Has an associated role that ensures access to all or specific TeamCity features through corresponding permissions. Learn more about [Role and Permission](role-and-permission.md).
	
* Belongs to at least one user group. Learn more about [user group](user-group.md).




In addition to logged in users, there is a special user account for non\-registered users called __Guest User__, that allows monitoring TeamCity projects without authorization. By default, guest login is disabled. Learn more on the [Guest User](guest-user.md) page.



__  __

__See also:__


__Concepts__: [User Group](user-group.md) | [Role and Permission](role-and-permission.md) | [Authentication Modules](authentication-modules.md)

__Administrator's Guide__: [Enabling Guest Login](enabling-guest-login.md) | [LDAP Integration](ldap-integration.md) | [Managing User Accounts, Groups and Permissions](managing-user-accounts-groups-and-permissions.md)
