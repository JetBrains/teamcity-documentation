[//]: # (title: Guest User)
[//]: # (auxiliary-id: Guest User)
TeamCity provides the ability to turn on the guest login allowing anonymous access to the TeamCity web UI.

A server administrator can [enable guest login](enabling-guest-login.md) on the __Administration | Authentication__ page.

Roles and groups for the guest user can be configured via __Guest user settings__ link available on the __Administration | Users__ page. By default, guest users have the __Project Viewer__ role for all the projects.

When guest user is enabled, any number of guest users can be logged in to TeamCity simultaneously without affecting each other's sessions. Thus, it can be useful for non\-committers who just monitor the projects status on the __Projects__ page.

Guest users do not have any personal settings, such as __Changes__ Page and Profile section (i.e. no way to receive notifications).

If guest login is enabled, you can construct a URL to the TeamCity Web interface, so that no user login is required:
* Add `&guest=1` parameter to a usual page URL. The login will be silently attempted on loading the page.

You can use guest login to download artifacts:
* Use `/guestAuth` before the URL path. For example:


```Shell
http://buildserver:8111/guestAuth/action.html?add2Queue=bt7

```

 <seealso>
        <category ref="concepts">
            <a href="role-and-permission.md">Role and Permission</a>
            <a href="super-user.md">Super User</a>
        </category>
        <category ref="admin-guide">
            <a href="enabling-guest-login.md">Enabling Guest Login</a>
        </category>
</seealso>
