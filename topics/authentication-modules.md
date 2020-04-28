[//]: # (title: Authentication Modules)
[//]: # (auxiliary-id: Authentication Modules)

There are two types of _authentication modules_ in TeamCity:
* __Credentials Authentication Module__ authenticates users with a login/password pair specified on the login page.
* __HTTP Authentication Module__ authenticates users with some information from a certain HTTP request.
You can enable several _credentials authentication modules_ and several _HTTP authentication modules_ simultaneously.

On an attempt to log in via the login page, TeamCity asks all the available _credentials authentication modules_ in the order they are specified and the first one that can authenticate the user authenticates him/her. And for any HTTP request, if there is no authenticated user yet, TeamCity asks all enabled _HTTP authentication modules_ in the order they are specified and the first one that can authenticate the user, authenticates him/her (if no _HTTP authentication module_ can authenticate the user for the specified HTTP request, TeamCity redirects the user to the login page).

TeamCity supports the following _credentials authentication modules_:
* __Built\-in__ (cross-platform): Users and their passwords are maintained by TeamCity. New users are added by the TeamCity administrator (in the Administration area) or they can register themselves if the user registration at the first login is allowed by the administrator.
* __Microsoft Windows domain__ (cross\-platform): All NT domain users that can log on to the machine running the TeamCity server, can also log in to TeamCity using the same credentials. i.e. to log in to TeamCity users should provide domain and username (__DOMAIN\username__) and their domain password.
* __LDAP server__ (cross-platform): Authentication is performed by directly logging into LDAP with credentials entered into the login form.
  <anchor name="tokenBasedAuth"/>
* __Token-based Authentication__ (cross-platform): Authentication via the personal access tokens that are maintained by TeamCity. This enables both an ability to authenticate with login/access-token instead of login/password when using the login form and token-based HTTP authentication.

The following _HTTP authentication modules_ are supported:
* __Basic HTTP__ (cross\-platform): Allows accessing certain web server pages and perform actions from various scripts.
* __NTLM HTTP__ (only for Windows servers): Allows logging in using NTLM HTTP protocol. Depending on the client's web browser and operating system can provide an ability to log in without typing the user's credentials manually.

Refer to [Configuring Authentication Settings](configuring-authentication-settings.md) for specific _authentication modules_ configuration. See also [Accessing Server by HTTP](accessing-server-by-http.md) page for details about accessing a server from your scripts using _Token-Based Authentication_ or _basic HTTP authentication_.

<seealso>
        <category ref="external">
            <a href="https://plugins.jetbrains.com/docs/teamcity/custom-authentication-module.html">Developing TeamCity Plugins: Custom Authentication Module</a>
        </category>
        <category ref="admin-guide">
            <a href="accessing-server-by-http.md">Accessing Server by HTTP</a>
            <a href="ldap-integration.md">LDAP Integration</a>
            <a href="configuring-authentication-settings.md">Configuring Authentication Settings</a>
        </category>
</seealso>