[//]: # (title: Managing Users and User Groups)
[//]: # (auxiliary-id: Managing Users and User Groups)

## Creating New User

The __Administration | Users__ page provides the _Create user account_ option.

When creating a user account when [several authentication modes enabled](configuring-authentication-settings.md#Advanced+Mode) on the server, only a username is required.

If only the [default authentication](authentication-modules.md) is used, the password is required as well. Any new user is automatically added to the [All Users group](user-group.md#%22All+Users%22+Group) and inherits the roles and permissions defined for this group. If you do not use [per-project permissions](role-and-permission.md#Authorization+Mode), you can specify here whether a user should have administrative permissions or not. Otherwise, you can assign roles to this user [later](#Assigning+Roles+to+Users).

### Editing User Account

To edit/delete a user account, click its name on the __Users__ tab of the __Administration | Users__ page and use the corresponding option. The page provides several tabs allowing you to modify various user account settings

### General

The _General_ tabs allows modifying the user's name, email address and password if you have appropriate permissions. Users can change their own username only if free registration is allowed. The administrator can always change the username of any user.

#### Authentication Settings

The __Authentication Settings__ section appears if several authentication modules are enabled on the server. Here you can edit usernames for different authentication modules such as LDAP and Windows Domain.   
  


 <anchor name="vcsUsername"/>

### VCS Usernames


This tab allows viewing and editing default usernames for different VCSs used by the current user.   
Multiple usernames are supported for a VCS root type and for a separate VCS root: several newline\-separated values can be used for each VCS username.

The names set here will be used to:
* show builds with changes committed by a user with such a VCS username on the [Changes](viewing-your-changes.md) page
* highlight such builds on the Projects page if the appropriate [option is selected](managing-your-user-account.md#Customizing+UI),
* notify the user on such builds when the __Builds affected by my changes__ option is selected in [notifications settings](subscribing-to-notifications.md#What+Will+Be+Watched).

## Groups

Use this tab to review the groups the user belongs to, and add/remove the user from groups.

## Roles

_This tab is available only if per\-project permissions are enabled on the server Administration / Authentication page_.    
Use this tab to view the roles assigned to the user directly and those inherited from groups. The roles assigned directly can be modified/removed here. See also [Assigning Roles to Users](#Assigning+Roles+to+Users) below.



<anchor name="assigningRoles"/>

### Assigning Roles to Users

[//]: # (AltHead: assigningRoles)

<tip>

To be able to grant roles to users on per\-project basis, enable per\-project permissions on the __Administration | Authentication__ page.   
The __Administration | Roles__ page lists all existing roles detailing their permissions.
</tip>

There are several ways to assign roles to one or several users:
* To assign a role to a specific user, on the __Users__ tab for the user click _View roles_ in the corresponding column. In the Roles tab, click __Assign role__.
* To assign a role to multiple users, on the __Users__ tab, check the boxes next to the usernames and use the __Assign roles__ button at the bottom of the page.
* To assign a role to all users in a group, on the __Groups__ tab click _View roles_ for the group in question, then assign a role on the group level.   

When assigning a role, you can:
* Select whether a role should be granted globally, or in particualr projects.
* Replace existing roles with the newly selected. This will remove all roles assigned to user(s)/group and replace them with the selected one instead.

## Notification Rules

This tab displays [notification rules](subscribing-to-notifications.md) for the user.

### Own rules

The rules configured by the user are displayed here and can be modified.

### Inherited rules

This section displays the rules inherited by the user from the groups they belong to.

## Managing User Groups

### Creating New Group

Open the __Administration | Groups__ page and сlick __Create new group__. In the dialog, specify the group name. TeamCity will create an editable Group Key, which is a unique group identifier.

When creating a group, you can select the parent group(s) for it. All roles and notification rules configured for the parent group will be automatically assigned to the current group. To place the current group to the top level, deselect all groups in the list.

### Editing Group Settings

To edit a group, click its name on the __Groups__ tab. You can modify the group name and description as well as parent group(s), and change the list of users, roles and permissions, and notification settings for the group.

To delete a group, click __Delete__ next to its name in the list. This action will only delete the group itself and won't affect the users of this group.

<tip>

The __All Users__ group includes all users and cannot be deleted. However, you can modify its roles and notification settings.
</tip>

The __Roles__ tab allows you to view and edit (assign/unassign) default roles for the current group. These roles will be automatically assigned to all users in the group. Default roles for a user group are divided in two groups:
* roles inherited from a parent group. Inherited roles can not be unassigned from the group.
* roles assigned explicitly to the group

To assign a role for the current group explicitly, click the __Assign role__ link.    
To view permissions granted to a role, click the __View roles permissions__ link.    
You can also specify notification rules to be applied to all users in the current group.   

To learn more about notification rules, refer to [Subscribing to Notifications](subscribing-to-notifications.md).

### Adding Multiple Users to Group

On the __Users__ page, select users, click the __Add to groups__ button at the bottom, and specify the groups  to add the users to. Note that all these users will inherit the roles defined for the group.

 __  __

__See also:__

__Concepts__: [User Account](user-account.md) | [User Group](user-group.md) | [Role and Permission](role-and-permission.md)

__ __