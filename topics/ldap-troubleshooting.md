[//]: # (title: LDAP Troubleshooting)
[//]: # (auxiliary-id: LDAP Troubleshooting)
__General advice__: if you experience problems with LDAP configuration, turn on the debug logging (see [Reporting Issues](reporting-issues.md)).



On this page:

<tag-list of="chapter" mode="tree" depth="4"/>



## Cannot authenticate using LDAP



Check the `teamcity-ldap.log` file. For each unsuccessful login attempt there should be a reason specified. Most commonly these are:

	
* The login filter doesn't match the entered login ("_User-entered login does not match teamcity.auth.loginFilter=..., aborting_")
	
* The LDAP server rejected login with the "Invalid credentials" message ("_Failed to login user '...' due to authentication error. Cause: Invalid credentials (\[LDAP: error code 49 - 80090308: LdapErr: DSID\-0C090334, comment: AcceptSecurityContext error, data 525, vece\^\@\])_")


The first reason means that the login can't be used for signing in because it doesn't match a certain filter. For example, by default you can't login with `DOMAIN\username` \- the filter forbids `/`, `\ ` and `@` symbols. See the `teamcity.auth.loginFilter` property.

The second error can be caused by various things, for example:
	
* You are trying to login with your username, but LDAP server accepts only full DNsIf all users are stored in one LDAP branch,  you should use  the `teamcity.auth.formatDN` property. Otherwise see the section below.
	
* Check your DN and the actual principal from the logs, probably there is a typo or an unescaped sequence. Try to log in with this principal using another LDAP tool.
	
* Try changing the security level (`java.naming.security.authentication`): it can be "simple", "strong" or "none".



## Users in LDAP are stored in different branches, so the teamcity.auth.formatDN property can't be applied. How can the users login with their usernames?



This feature is available from version 5.0. You should specify how you want to find the user (`teamcity.users.login.filter`), for example, by the username or email. On each login TeamCity finds the user in LDAP _before_ logging in, fetches the user DN and then performs the bind. Thus you should also define the credentials for TeamCity to perform search operations (`java.naming.security.principal` and `java.naming.security.credentials`). 