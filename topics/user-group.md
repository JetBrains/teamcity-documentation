[//]: # (title: User Group)
[//]: # (auxiliary-id: User Group)

To help you manage user accounts more efficiently, TeamCity provides User Groups configured using the __Administration | Groups__ page.

Here you can add, delete, edit groups, view parent groups, roles and all users belonging to the group. 

A user group allows you to:
* Assign [roles](role-and-permission.md) to all users included in the group at once: users get all the roles of the groups they belong to.
* Set the [notification rules](subscribing-to-notifications.md) for all users in the group: all the notification rules defined for the group are treated as default notification rules for the users included in this group.

You can create as many user groups as you need, and each user group can include any number of user accounts and even other user groups. A user account (or a whole user group) can be included into several user groups as well.

<anchor name="allusers"/>


#### "All Users" Group

__All Users__ is a special user group that is always present in the system. The group contains all registered users and no user can be removed from the group. You can modify roles and notification rules of the "All Users" group to make them default for all the users in the system.

All the newly registered users get roles and notification rules inherited from the group.

The [Guest User](guest-user.md) does not belong to the All Users group so roles and notification rules can be managed for the user separately.

 <seealso>
        <category ref="concepts">
            <a href="user-account.md">User Account</a>
            <a href="role-and-permission.md">Role and Permission</a>
        </category>
        <category ref="admin-guide">
            <a href="managing-user-accounts-groups-and-permissions.md">Managing User Accounts, Groups and Permissions</a>
        </category>
</seealso>