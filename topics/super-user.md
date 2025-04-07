[//]: # (title: Super User Access)
[//]: # (help-id: Super User Access;Super User)

The **Super User** login mode provides full System Administrator access to the server UI, ideal for cases like lost credentials or adjusting authentication settings. The Super User token is also required to access the server maintenance page at startup, which prompts for manual confirmation before certain actions (for example, database upgrades after a server update).

Super Users have all [System Administrator permissions](managing-roles-and-permissions.md) but lack personal settings like notifications. Multiple Super Users can log in simultaneously without interfering with each other's sessions.

## Super User Token

To log in as a Super User, enter the authentication token as the password without specifying a username. A new token is generated each time the server starts and is recorded in both the server console and [`teamcity-server.log`](teamcity-server-logs.md) file (search for "Super User authentication token").

```Shell
[2024-10-24 13:10:10,459]   INFO -  jetbrains.buildServer.STARTUP - Upgrade from version 1011 to version 1012 is required
[2024-10-24 13:10:10,459]   INFO -  jetbrains.buildServer.STARTUP - Backup of this version is possible
[2024-10-24 13:10:10,465]   INFO -  jetbrains.buildServer.STARTUP - Current stage: Data upgrade is required (administrator login is required to proceed)
[2024-10-24 13:10:10,466]   INFO -  jetbrains.buildServer.STARTUP - Administrator can login from web UI using super user authentication token (better use a private browser window)
[2024-10-24 13:10:10,466]   INFO -   jetbrains.buildServer.SERVER - Super user authentication token: 12345678910 (use empty username with the token as the password to access the server)
```


Instead of logging in with an empty username, you can also go to `<TeamCity_server_URL>/login.html?super=1` and enter the token. Accessing this page prints the token to the server log again for your convenience.



## Disable Super User


Super User login is enabled by default but can be disabled by setting the `teamcity.superUser.disable=true` [internal property](server-startup-properties.md#TeamCity+Internal+Properties). In emergencies, remove this setting in the [<`TeamCity Data Directory>`](teamcity-data-directory.md)`/config/internal.properties` file and restart TeamCity to restore Super User access.

<seealso>
        <category ref="concepts">
            <a href="guest-user.md">Guest User</a>
        </category>
</seealso>
