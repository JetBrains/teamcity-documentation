[//]: # (title: Creating and Managing User Groups)
[//]: # (auxiliary-id: Creating and Managing User Groups)

## What is User Group in TeamCity
{id="User+Group" auxiliary-id="User Group"}

User groups help manage user accounts more efficiently:
* Assign [roles](managing-roles-and-permissions.md) to all users included in the group at once: users get all the roles of the groups they belong to.
* Set [notification rules](adding-notification-rules.md) for all users in the group: all the notification rules defined for the group are treated as default notification rules for the users included in this group.

You can create as many user groups as you need, and each user group can include any number of user accounts and even other user groups. A user account (or a whole user group) can be included into several user groups as well.

<anchor name="allusers"/>

## "All Users" Group

__All Users__ is a special user group that is always present in the system. The group contains all registered users and no user can be removed from the group. You can modify roles and notification rules of the _All Users_ group to make them default for all the users in the system.

All the newly registered users get roles and notification rules inherited from the group.

The [Guest User](guest-user.md) does not belong to the _All Users_ group, so roles and notification rules can be managed for the user separately.

## Creating New User Group

Open the __Administration | Groups__ page and сlick __Create new group__. In the dialog, specify the group name. TeamCity will create an editable _group key_, which is a unique group identifier.

When creating a group, you can select the parent group(s) for it. All roles and notification rules configured for the parent group will be automatically assigned to the current group. To place the current group to the top level, deselect all groups in the list.

## Editing Group Settings

To edit a group, click its name on the __Groups__ tab. You can modify the group name and description as well as parent group(s), and change the list of users, roles and permissions, and notification settings for the group.

To delete a group, click __Delete__ next to its name in the list. This action will only delete the group itself and won't affect the users of this group.

>The __All Users__ group includes all users and cannot be deleted. However, you can modify its roles and notification settings.

The __Roles__ tab allows you to view and edit (assign/unassign) default roles for the current group. These roles will be automatically assigned to all users in the group. Default roles for a user group are divided in two groups:
* roles inherited from a parent group. Inherited roles can not be unassigned from the group.
* roles assigned explicitly to the group

To assign a role for the current group explicitly, click the __Assign role__ link. To view permissions granted to a role, click the __View roles permissions__ link. You can also specify notification rules to be applied to all users in the current group.

To learn more about notification rules, refer to [Subscribing to Notifications](adding-notification-rules.md).
