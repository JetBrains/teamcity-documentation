[//]: # (title: Authentication Modules)
[//]: # (auxiliary-id: Authentication Modules)

There are two types of _authentication modules_ in TeamCity:
* __Credentials Authentication Modules__ authenticate users with a login/password pair specified on the login page.
* __HTTP Authentication Modules__ authenticate users with some information from a certain HTTP request.   
You can enable several _credentials authentication modules_ and several _HTTP authentication modules_ simultaneously.

On an attempt to sign in via the login form, TeamCity asks all the available _credentials authentication modules_ in the order they are specified in the settings; the first one who _can_ authenticate the user authenticates them. For any HTTP request, if there is no authenticated user yet, TeamCity asks all enabled _HTTP authentication modules_ in the order they are specified; the first one who can authenticate the user, authenticates them (if no _HTTP authentication module_ can authenticate the user for the specified HTTP request, TeamCity redirects the user to the login page).

TeamCity supports the following _credentials authentication modules_:
* __Built-in__ (cross-platform): Users and their passwords are maintained by TeamCity. New users are added by the TeamCity administrator (in the Administration area) or they can register themselves if the user registration at the first login is allowed by the administrator.
* __Microsoft Windows domain__ (cross-platform): All NT domain users that can sign in to the machine running the TeamCity server, can also sign in to TeamCity using the same credentials. That is, to sign in to TeamCity users should provide the __DOMAIN\username__ pair and their domain password.
* __LDAP server__ (cross-platform): Authentication is performed by directly logging into LDAP with credentials entered into the login form.
  {product="tc"}
  <anchor name="tokenBasedAuth"/>
  <anchor name="AuthenticationModules-tokenBasedAuth"/>
* __Token-based Authentication__ (cross-platform): Authentication via the personal access tokens that are maintained by TeamCity. This enables both an ability to authenticate with login/access-token instead of login/password when using the login form and token-based HTTP authentication.

The following _HTTP authentication modules_ are supported:
* __Basic HTTP__ (cross-platform): Allows accessing certain web server pages and perform actions from various scripts.
* __NTLM HTTP__ (only for Windows servers): Allows logging in using NTLM HTTP protocol. Depending on the client's web browser and operating system can provide an ability to log in without typing the user's credentials manually.
  {product="tc"}
* __GitHub.com__ and __GitHub Enterprise__: Allow authenticating using an existing GitHub user account. Allow limiting access to members of a [GitHub organization](https://docs.github.com/en/github/setting-up-and-managing-organizations-and-teams/about-organizations).
* __GitLab.com__ and __GitLab CE/EE__: Allow authenticating using an existing GitLab.com account. Allow limiting access to members of a [GitLab group](https://docs.gitlab.com/ee/user/group/).
* __Bitbucket Cloud__: Allows authenticating using an existing Bitbucket Cloud account.

Refer to [Configuring Authentication Settings](configuring-authentication-settings.md) for specific _authentication modules'_ configuration. See also [Accessing Server by HTTP](accessing-server-by-http.md) page for details about accessing a server from your scripts using _Token-Based Authentication_ or _basic HTTP authentication_.

<seealso>
        <category ref="external">
            <a href="https://plugins.jetbrains.com/docs/teamcity/custom-authentication-module.html" product="tc">Developing TeamCity Plugins: Custom Authentication Module</a>
        </category>
        <category ref="admin-guide">
            <a href="accessing-server-by-http.md">Accessing Server by HTTP</a>
            <a href="ldap-integration.md" product="tc">LDAP Integration</a>
            <a href="configuring-authentication-settings.md">Configuring Authentication Settings</a>
        </category>
</seealso>