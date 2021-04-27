[//]: # (title: Guest User)
[//]: # (auxiliary-id: Guest User)

TeamCity provides the ability to turn on the guest login allowing anonymous access to the TeamCity web UI.

>If you are using TeamCity Cloud, please be extremely cautious when enabling this functionality. Unlike the TeamCity On-Premises instances, most often installed in private environments, each TeamCity Cloud instance is globally available via its URL. If guest login is disabled on the instance (default behavior), only authorized users will be able to access it. If you enable it, anyone who knows your server address will be able to view its UI in the read-only mode. We suggest that you enable it for a Cloud server only if absolutely necessary and carefully manage the restrictions of the guest user roles.
>
{type="warning" product="tcc"}

A server administrator can [enable guest login](enabling-guest-login.md) on the __Administration | Authentication__ page.

Roles and groups for the guest user can be configured via __Guest user settings__ link available on the __Administration | Users__ page. By default, guest users have the __Project Viewer__ role for all the projects.

When guest user is enabled, any number of guest users can be logged in to TeamCity simultaneously without affecting each other's sessions. Thus, it can be useful for non-committers who just monitor the projects' status on the __Projects__ page.

Guest users do not have any personal settings, such as the __Changes__ page and the __Profile__ section (that is no way to receive notifications).

If guest login is enabled, you can construct a URL to the TeamCity web interface, so that no user login is required. Add the `&guest=1` parameter to a usual page URL. The login will be silently attempted on loading the page.

You can use guest login to download artifacts by adding `/guestAuth` before the URL path. For example,

```Shell
http://buildserver:8111/guestAuth/repository/download/<BuildConfigName>/<BuildID>:id/<artifacts>.zip

```

<seealso>
        <category ref="concepts">
            <a href="role-and-permission.md">Role and Permission</a>
            <a href="super-user.md" product="tc">Super User</a>
        </category>
        <category ref="admin-guide">
            <a href="enabling-guest-login.md">Enabling Guest Login</a>
        </category>
</seealso>
