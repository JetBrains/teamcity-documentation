[//]: # (title: Managing Roles)
[//]: # (auxiliary-id: Managing Roles)
If [per-project permissions](role-and-permission.md) are enabled in your installation, you can view the existing roles, modify them  and create new ones in the TeamCity web UI using the __Administration | Roles__ link (in the __User Management__ section of __Settings__).

Using the __Roles__ page you can:
* create new roles
* delete existing roles
* add/delete permissions from existing roles
* include/exclude one role permissions

<note>

The role settings are global.
</note>

You can also configure roles and permissions using the `roles-config.xml` file stored in \<[TeamCity Data Directory](teamcity-data-directory.md)\>\/config directory.

 <seealso>
       <category ref="concepts">
            <a href="role-and-permission.md">Role and Permission</a>
        </category>
</seealso>