[//]: # (title: Super User)
[//]: # (auxiliary-id: Super User)

The __Super user__ login allows you to access the server UI with System Administrator permissions. For example, if you forgot the credentials or need to fix authentication-related settings. The login is performed using an authentication token that can be found in the server logs.

Also, Super user token is used to access the server maintenance pages displayed on the server start when a manual action is required to proceed with the server startup.

The authentication token is automatically generated on every server start. The token is printed in the server console and [`teamcity-server.log`](teamcity-server-logs.md) under the `TeamCity\logs` directory (search for the "Super user authentication token" text). The line is printed on the server start and on any login page submit without a username specified.

To log in as a super user, use an empty username and the authentication token as the password on the login page.

A super user is not a usual TeamCity user and does not have any personal settings, such as __Changes__ page and Profile section (that is there is no way to receive notifications). The super user has all [System Administrator permissions](role-and-permission.md).

Any number of super users can log in to TeamCity simultaneously without affecting each other's sessions.

Instead of using an empty username, you can also go to the `<TeamCity_server_URL>/login.html?super=1` page and enter the super user authentication token. On loading the super user login page, the super user token is printed into the server log and console again for your convenience.

The super user login is enabled by default, but it can be disabled by specifying the `teamcity.superUser.disable=true` [internal property](server-startup-properties.md#TeamCity+Internal+Properties).

<seealso>
        <category ref="concepts">
            <a href="guest-user.md">Guest User</a>
        </category>
</seealso>
