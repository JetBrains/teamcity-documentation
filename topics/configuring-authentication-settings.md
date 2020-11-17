[//]: # (title: Configuring Authentication Settings)
[//]: # (auxiliary-id: Configuring Authentication Settings)

TeamCity can authenticate users via an internal database, or can integrate into your system and use external authentication sources such as Windows Domain or LDAP.

## Configuring Authentication

Authentication is configured on the __Administration | Authentication__ page; the currently used authentication modules are also displayed here.

TeamCity provides several preconfigured authentication options (presets) to cover the most common [use case](#Simple+Mode). The presets are combinations of authentication modules supported by TeamCity. These are three credentials authentication modules and two HTTP authentication modules:

* [Credentials Authentication Modules](#Credentials+Authentication+Modules)
  * [Built-in](#Built-in+Authentication)
  * [Windows Domain Authentication](#Windows+Domain+Authentication)
  * [LDAP Integration](ldap-integration.md)
  * [Token-Based Authentication](#Token-Based+Authentication)
* [HTTP Authentication Modules](#HTTP+Authentication+Modules)
  * [Basic HTTP Authentication](#Basic+HTTP+Authentication)
  * [NTLM HTTP Authentication](ntlm-http-authentication.md)
  * [Bitbucket Cloud](#Bitbucket+Cloud)
  * [GitHub.com](#GitHub.com)
  * [GitHub Enterprise](#GitHub+Enterprise)
  * [GitLab.com](#GitLab.com)
  * [GitLab CE/EE](#GitLab+CE%2FEE)

<tip>

If you are using [JetBrains Hub](https://www.jetbrains.com/hub/), you can configure single sign-on (SSO) via JetBrains Hub from the TeamCity login form and IDE using a [separate plugin for TeamCity](https://plugins.jetbrains.com/plugin/9156-jetbrains-hub-integration).
</tip>

When you first sign in to TeamCity, the default authentication including the Built-in and Basic HTTP Authentication modules is enabled and editing authentication settings in the [simple mode](#Simple+Mode) is active.

* To modify the existing settings, click the __Edit__ link in the table next to the description of the enabled authentication module.
* To switch to a different preconfigured scheme, use the __Load preset__ button. For more options, switch to the [Advanced mode](#Advanced+Mode).

<tip>

Any changes made to authentication in the UI will be reflected in the `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/auth-config.xml` file, which can be used to configure authentication if editing via the Web UI is not suitable for some reason. The detailed description is available in the [earlier documentation version](https://confluence.jetbrains.com/display/TCD8/Configuring+Authentication+Settings).
</tip>

### Simple Mode

The Simple mode (default) allows you to select presets created for the most common use cases. To override the existing authentication settings, use the __Load preset__ button, select one of the options and __Save__ your changes. The following presets are available:
* Default ([built-in authentication](#Built-in+Authentication) – [Token-Based Authentication](#Token-Based+Authentication) and [Basic HTTP](accessing-server-by-http.md))
* [LDAP](ldap-integration.md)
* Active directory ([LDAP](ldap-integration.md) with [NTLM](ntlm-http-authentication.md) and [Token-Based Authentication](rest-api.md))
* Microsoft Windows Domain ([NTLM](ntlm-http-authentication.md), [Token-Based Authentication](rest-api.md) and [Basic HTTP](rest-api.md))

### Advanced Mode

TeamCity allows enabling several authentication modules simultaneously using the advanced mode in the TeamCity web UI.

When a user attempts to log in, all the modules will be tried one by one. If one of them authenticates the user, the login will be successful; if all of them fail, the user will not be able to log into TeamCity.

<note>

If the System Administrator creates users without a password with several authentication modes enabled on the server including the [Built-in](#Built-in+Authentication) one, and later changes authorization from mixed one to the build-in one, users with no password will be unable to sign in to TeamCity.
</note>

It is possible to use a combination of internal and external authentication. The recommended approach is to configure [LDAP Integration](ldap-integration.md) for your internal employees first and then to add [Built-in](#Built-in+Authentication) authentication for external users.
 
1. Switch to advanced mode with the corresponding link on the __Administration | Authentication__ page.
2. Click __Add Module__ and select a module from the drop-down menu.
3. Use the properties available for modules by selecting/deselecting checkboxes in the __Add Module__ dialog.
4. Click __Apply__ and __Save__ your changes.

Also, TeamCity plugins can provide [additional authentication modules](https://plugins.jetbrains.com/docs/teamcity/custom-authentication-module.html).

## User Authentication Settings

The very first time TeamCity server starts with no users (and no administrator) so you will be prompted for the administrator account. If you are not prompted for the administrator account, refer to [How To Retrieve Administrator Password](how-to.md#Retrieve+Administrator+Password) for a resolution.

The TeamCity administrator can modify the authentication settings for every user on their profile page.

The TeamCity list of users and authentication modules just map external credentials to the users. This means that a single TeamCity user can authenticate using different modules, provided the entered credentials are mapped to the same TeamCity user. Authentication modules have a configuration on how to map external user data to a TeamCity user, and some (Windows Domain, JetBrains Hub) allow editing the external user linking data on the TeamCity user profile.

Handling of the user mapping by the bundled authentication modules:

* Built-in authentication stores a TeamCity-maintained password for each user.
* Windows Domain authentication allows specifying the default domain and assumes the Domain account name is equal to the TeamCity user. The domain account can be edited on the user profile page.
* LDAP Integration allows setting LDAP property to get TeamCity username from user's LDAP entry.

Be cautious when modifying authentication settings: there can be a case when the administrator cannot login after changing authentication modules.      
Let's imagine that the administrator had the "jsmith" TeamCity username and used the default authentication. Then the authentication module was changed to Windows domain authentication (i.e. Windows domain authentication module was added and the default one was removed). If, for example, the Windows domain username of that administrator is "john.smith", they will not able to sign in anymore: they cannot login using the default authentication since it is disabled and cannot login using Windows domain authentication since their Windows domain username is not equal to the TeamCity username. The solution nevertheless is quite simple: the administrator can sign in using the superuser account and change their TeamCity username or specify their Windows domain username on their own profile page.

### Special User Accounts

By default, TeamCity has a [Super User](super-user.md) account with maximum permissions and a [Guest User](guest-user.md) with minimal permissions. These accounts have no personal settings such as the [Changes](viewing-your-changes.md) page and Profile information as they are not related to any particular person but rather intended for special use cases.

## Credentials Authentication Modules

### Built-in Authentication

By default, TeamCity uses the built-in authentication, meaning that users and their passwords are maintained by TeamCity.

When logging to TeamCity for the first time, the user will be prompted to create the TeamCity username and password which will be stored in TeamCity and used for authentication. If you installed TeamCity and logged into it, it means that built-in authentication is enabled and all user data is stored in TeamCity.

In the beginning the user database is empty and new users are either [added by the TeamCity administrator](managing-users-and-user-groups.md#Creating+New+User) or users are self\-registered: the default settings allow the users to register from the login page. All newly created users belong to the [All Users](user-group.md#%22All+Users%22+Group) group and have all roles assigned to this group. If some specific [roles](role-and-permission.md) are needed for the newly registered users, these roles should [be granted](managing-roles.md) via the __All Users__ group.

By default, the users are allowed to change their password on their profile page.

### Token-Based Authentication

Allows users to authenticate via [access tokens](managing-your-user-account.md#Managing+Access+Tokens) that they can create and invalidate themselves.

This authentication module is enabled by default.

### Windows Domain Authentication

Allows user login using Windows domain name and password.The credential check is performed on the TeamCity server side, so the server should be aware of the domain(s) users use to log in.The supported syntax for the username is `DOMAIN\user.name` as well as `<username>@<domain>`.

In addition to logging in using the login form, you can enable [NTLM HTTP Authentication](ntlm-http-authentication.md) single sign-on.    
If you select the "Microsoft Windows Domain" preset, in addition to the login via a Windows domain, the [Basic HTTP](#Basic+HTTP+Authentication) and [NTLM](ntlm-http-authentication.md) authentication modules are enabled by default.

#### Specifying Default Domain

To enable users to enter the system using the login form without specifying the domain as a part of the username, do the following:
1. Go to the __Administration | Authentication__ page.
2. Click the __edit__ link in the table next to the __Microsoft Windows domain__ authentication description.
3. Set the name in the __Default domain:__ field.
4. Click __Done__ and __Save__ your changes.

#### Registering New Users on Login

The default settings allow users to register from the login page and TeamCity user names for the new users will be the same as their Windows domain account.   
All newly created users belong to the [All Users](user-group.md#%22All+Users%22+Group) group and have all roles assigned to this group. If some specific [roles](role-and-permission.md) are needed for the newly registered users, these roles should [be granted](managing-roles.md) via the __All Users__ group.

To disable new user registration on login:
1. Go to the __Administration | Authentication__ page.
2. Click the __edit__ link in the table next to the __Microsoft Windows domain__ authentication description. Uncheck the __Allow user registration from the login page__ box.
3. Click the __edit__ link in the table next to the __NTLM HTTP__ authentication description. Uncheck the __Allow user registration from the login page__ box.

#### Linux-Specific Configuration

If your TeamCity server runs under Linux, JCIFS library is used for the Windows domain login. This only supports Windows domain servers with SMB (SMBv1) enabled. SMB2 is not supported.The library is configured using the properties specified in the `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/ntlm-config.properties` file. Changes to the file take effect immediately without the server restart.

JCIFS library settings which cannot be changed in runtime or settings to affect HTTP NTLM settings can only be set via a properties file passed via `-Djcifs.properties` JVM option.

If the default settings do not work for your environment, refer to [http://jcifs.samba.org/src/docs/api/](http://jcifs.samba.org/src/docs/api/) for all available configuration properties.   
If the library does not find the domain controller to authenticate against, consider adding the `jcifs.netbios.wins` property to the `ntlm-config.properties` file with the address of your WINS server. For other domain services locating properties, see [http://jcifs.samba.org/src/docs/resolver.html](http://jcifs.samba.org/src/docs/resolver.html).

### LDAP Authentication

Please refer to the [dedicated page](ldap-integration.md).


[//]: # (Internal note. Do not delete. "Configuring Authentication Settingsd70e480.txt")    


## HTTP Authentication Modules

### Basic HTTP Authentication

Please refer to [Accessing Server by HTTP](accessing-server-by-http.md) for details about basic HTTP authentication.

<tip>

For information on configuring Basic HTTP Authentication directly in the `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/auth-config.xml`, refer to the [earlier documentation version](https://confluence.jetbrains.com/display/TCD8/Configuring+Authentication+Settings).
</tip>


### NTLM HTTP Authentication

Please refer to the [dedicated page](ntlm-http-authentication.md).

### Bitbucket Cloud

In terms of 2020.2 EAP, users can sign in to TeamCity with a Bitbucket Cloud account.

To sign in, click the Bitbucket icon above the login form and, after the redirect, approve the TeamCity application. If your email is verified in Bitbucket and [in TeamCity](enabling-email-verification.md), you will be authenticated as this user. Otherwise, TeamCity will create a new user profile, unless this option is disabled. It is also possible to [map existing TeamCity users](#Mapping+users) with Bitbucket Cloud profiles.

Before enabling this module, you need to configure a [Bitbucket Cloud connection](integrating-teamcity-with-vcs-hosting-services.md) in the Root project's settings.

<table>

<tr>
<td>Setting</td>
<td>Description</td>
</tr>

<tr>

<td>

Allow creating new users on the first login

</td>

<td>

Enabled by default.  
If disabled, TeamCity will not create a new user when the provided external email is unrecognized. This is helpful if you use a publicly available Bitbucket Cloud repo but want to limit access to the TeamCity server.

</td>

</tr>

<tr>

<td>

Restrict authentication

</td>

<td>

A comma-separated list of [workspaces'](https://support.atlassian.com/bitbucket-cloud/docs/what-is-a-workspace/) IDs.

This list limits a set of users who can register or authenticate in TeamCity with their Bitbucket Cloud account. Together with the enabled _Allow creating new users on the first login_ option, this leaves an ability to automatically register unknown users but restricts it to those who work on your projects.

Leave empty to allow all Bitbucket Cloud users to access the TeamCity server.

>If you delete a user from an organization, remember to restrict their access or delete this user in TeamCity as well. Otherwise, they might be able to sign in to TeamCity with a preliminary created access token.
>
{type="warning"}

</td>

</tr>

</table>

### GitHub.com

In terms of 2020.2 EAP, users can sign in to TeamCity with a GitHub.com account.

To sign in, click the GitHub icon above the login form and, after the redirect, approve the TeamCity application. If your email is verified in GitHub and [in TeamCity](enabling-email-verification.md), you will be authenticated as this user. Otherwise, TeamCity will create a new user profile, unless this option is disabled. It is also possible to [map existing TeamCity users](#Mapping+users) with GitHub.com profiles.

Before enabling this module, you need to configure a [GitHub.com connection](integrating-teamcity-with-vcs-hosting-services.md) in the Root project's settings.

<table>

<tr>
<td>Setting</td>
<td>Description</td>
</tr>

<tr>

<td>

Allow creating new users on the first login

</td>

<td>

Enabled by default.  
If disabled, TeamCity will not create a new user when the provided external email is unrecognized. This is helpful if you use a publicly available GitHub.com repo but want to limit access to the TeamCity server.

</td>

</tr>

<tr>

<td>

Restrict authentication

</td>

<td>

A comma-separated list of [organizations'](https://support.atlassian.com/bitbucket-cloud/docs/what-is-a-workspace/) IDs.

This list limits a set of users who can register or authenticate in TeamCity with their GitHub account. Together with the enabled _Allow creating new users on the first login_ option, this leaves an ability to automatically register unknown users but restricts it to those who work on your projects.

Leave empty to allow all GitHub users to access the TeamCity server.

>If you delete a user from an organization, remember to restrict their access or delete this user in TeamCity as well. Otherwise, they might be able to sign in to TeamCity with a preliminary created access token.
>
{type="warning"}

</td>

</tr>

</table>

### GitHub Enterprise

In terms of 2020.2 EAP, users can sign in to TeamCity with a GitHub Enterprise account.

To sign in, click the GitHub icon above the login form and, after the redirect, approve the TeamCity application. If your email is verified in GitHub and [in TeamCity](enabling-email-verification.md), you will be authenticated as this user. Otherwise, TeamCity will create a new user profile, unless this option is disabled. It is also possible to [map existing TeamCity users](#Mapping+users) with GitHub Enterprise profiles.

Before enabling this module, you need to configure a [GitHub Enterprise connection](integrating-teamcity-with-vcs-hosting-services.md) in the Root project's settings.

<table>

<tr>
<td>Setting</td>
<td>Description</td>
</tr>

<tr>

<td>

Allow creating new users on the first login

</td>

<td>

Enabled by default.  
If disabled, TeamCity will not create a new user when the provided external email is unrecognized. This is helpful if you use a publicly available GitHub Enterprise server but want to limit access to the TeamCity server.

</td>

</tr>

<tr>

<td>

Restrict authentication

</td>

<td>

A comma-separated list of [organizations'](https://support.atlassian.com/bitbucket-cloud/docs/what-is-a-workspace/) IDs.

This list limits a set of users who can register or authenticate in TeamCity with their GitHub account. Together with the enabled _Allow creating new users on the first login_ option, this leaves an ability to automatically register unknown users but restricts it to those who work on your projects.

Leave empty to allow all GitHub users to access the TeamCity server.

>If you delete a user from an organization, remember to restrict their access or delete this user in TeamCity as well. Otherwise, they might be able to sign in to TeamCity with a preliminary created access token.
>
{type="warning"}

</td>

</tr>

</table>

>If you reconnect a TeamCity server from one GitHub Enterprise server to another, TeamCity might not be able to recognize external users after this operation. This case requires reconfiguring user profiles manually. If you encounter any issues, please [contact our support](https://confluence.jetbrains.com/display/TW/Feedback).
>
{type="warning"}

### GitLab.com

In terms of 2020.2 EAP, users can sign in to TeamCity with a GitLab.com account.

To sign in, click the GitLab icon above the login form and, after the redirect, approve the TeamCity application. If your email is verified in GitLab and [in TeamCity](enabling-email-verification.md), you will be authenticated as this user. Otherwise, TeamCity will create a new user profile, unless this option is disabled. It is also possible to [map existing TeamCity users](#Mapping+users) with GitLab.com profiles.

>If you want to be recognized in TeamCity by your email, make sure this email is set as _public_ in GitLab.

Before enabling this module, you need to configure a [GitLab.com connection](integrating-teamcity-with-vcs-hosting-services.md) in the Root project's settings.

<table>

<tr>
<td>Setting</td>
<td>Description</td>
</tr>

<tr>

<td>

Allow creating new users on the first login

</td>

<td>

Enabled by default.  
If disabled, TeamCity will not create a new user when the provided external email is unrecognized. This is helpful if you use a publicly available GitLab.com repo but want to limit access to the TeamCity server.

</td>

</tr>

<tr>

<td>

Restrict authentication

</td>

<td>

A comma-separated list of [groups'](https://docs.gitlab.com/ee/user/group/) IDs.

This list limits a set of users who can register or authenticate in TeamCity with their GitLab account. Together with the enabled _Allow creating new users on the first login_ option, this leaves an ability to automatically register unknown users but restricts it to those who work on your projects.

Leave empty to allow all GitLab users to access the TeamCity server.

>If you delete a user from an organization, remember to restrict their access or delete this user in TeamCity as well. Otherwise, they might be able to sign in to TeamCity with a preliminary created access token.
>
{type="warning"}

</td>

</tr>

</table>

### GitLab CE/EE

In terms of 2020.2 EAP, users can sign in to TeamCity with a GitLab CE/EE account.

To sign in, click the GitLab icon above the login form and, after the redirect, approve the TeamCity application. If your email is verified in GitLab and [in TeamCity](enabling-email-verification.md), you will be authenticated as this user. Otherwise, TeamCity will create a new user profile, unless this option is disabled. It is also possible to [map existing TeamCity users](#Mapping+users) with GitLab CE/EE profiles.

>If you want to be recognized in TeamCity by your email, make sure this email is set as _public_ in GitLab.

Before enabling this module, you need to configure a [GitLab CE/EE connection](integrating-teamcity-with-vcs-hosting-services.md) in the Root project's settings.

<table>

<tr>
<td>Setting</td>
<td>Description</td>
</tr>

<tr>

<td>

Allow creating new users on the first login

</td>

<td>

Enabled by default.  
If disabled, TeamCity will not create a new user when the provided external email is unrecognized. This is helpful if you use a publicly available GitLab CE/EE server but want to limit access to the TeamCity server.

</td>

</tr>

<tr>

<td>

Restrict authentication

</td>

<td>

A comma-separated list of [groups'](https://docs.gitlab.com/ee/user/group/) IDs.

This list limits a set of users who can register or authenticate in TeamCity with their GitLab account. Together with the enabled _Allow creating new users on the first login_ option, this leaves an ability to automatically register unknown users but restricts it to those who work on your projects.

Leave empty to allow all GitLab users to access the TeamCity server.

>If you delete a user from an organization, remember to restrict their access or delete this user in TeamCity as well. Otherwise, they might be able to sign in to TeamCity with a preliminary created access token.
>
{type="warning"}

</td>

</tr>

</table>

>If you reconnect a TeamCity server from one GitLab CE/EE server to another, TeamCity might not be able to recognize external users after this operation. This case requires reconfiguring user profiles manually. If you encounter any issues, please [contact our support](https://confluence.jetbrains.com/display/TW/Feedback).
>
{type="warning"}

## Mapping users

Each TeamCity user can connect own profile to one or more external Git hostings for authentication.   
You can connect to a VCS hosting in  __My settings & Tools | General | Authentication Settings__, if the respective authentication module is enabled on the server. After successful authorization, TeamCity will map your external account with the current user profile. Now, you can sign in to TeamCity with your VCS account.

Administrators who have a permission to change user profiles, can also map existing users with external accounts in __Administration | User Management | Users__.

 <seealso>
        <category ref="concepts">
            <a href="authentication-modules.md">Authentication Modules</a>
        </category>
</seealso>