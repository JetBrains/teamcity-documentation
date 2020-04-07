[//]: # (title: Role and Permission)
[//]: # (auxiliary-id: Role and Permission)

User access levels are handled by assigning different _roles_ to users thus granting them respective _permissions_.

A _permission_ is an _authorization_ to perform particular operations, for example, to run a build or modify build configuration settings.   
A _role_ is a set of _permissions_ that can be granted to a user in one or all projects thus controlling access to the projects and various features in the Web UI.

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## Authorization Mode

TeamCity authorization supports two modes: __simple__ and __per-project__.
* In the __simple__ mode, there are only three types of authorization levels: guest, logged-in user, and administrator.   
* In the __per-project__ mode, you can assign users roles in projects or server-wide. The set of permissions in roles is editable.

Permissions within a role granted at the project level are automatically propagated in all the subprojects of this project.   
The __View project and all parent projects__ permission allows viewing not only the project (with its subprojects) but its parent projects too.

## Changing Authorization Mode

Unless explicitly configured, the simple authorization mode is used in TeamCity Professional and per-project is used in TeamCity Enterprise.   
To change the authorization mode, go to __Administration | Authentication__ and enable/disable the _Enable per-project permissions_ option.

## Simple Authorization Mode

<table>

<tr>

<td>

Role

</td>

<td>

Description

</td>

</tr>
<tr>

<td>

Administrator

</td>

<td>

User with no restrictions; corresponds to the [System Administrator](#system-administrator) role in the per-project authorization mode.

</td>

</tr>
<tr>

<td>

Logged-in user

</td>

<td>

Corresponds to the default [Project Developer](#project-developer) role granted for all projects in the per-project authorization mode.

</td>

</tr>
<tr>

<td>

Guest user

</td>

<td>

Corresponds to the default [Project Viewer](#project-viewer) role granted for all projects in the per-project authorization mode.

</td></tr></table>

## Per-Project Authorization Mode

Roles are assigned to users by administrators on a per-project basis: a user can have different roles in different projects, and hence, the permissions are project-based. A user can have a role in a specific project or in all available projects, or no roles at all. You can [associate a user account with a set of roles](managing-users-and-user-groups.md). A role can also be granted to a user group. This means that the role is automatically granted to all the users that are included into the group (both directly or through other groups).

You can add roles and assign permissions to them in __Administration | Roles__. Click __Add permission__ under any role to see the list of all available permissions, select a required permission, and click __Add__. To add multiple permissions, hold the __CTRL__ key when selecting them.

By default, TeamCity provides the following per-project roles:

<table>

<tr>

<td>

Role

</td>

<td>

Description

</td></tr><tr>

<td>

<anchor name="system-administrator"/>

__System Administrator__

</td>

<td>

Has no restrictions in permissions, and has all of the [Project Administrator's](#project-administrator) permissions. Сan create and manage user accounts, authorize build agents, set up projects and build configurations, edit the TeamCity server settings, manage TeamCity licenses, configure server data clean-up rules, change VCS roots, and so on.


</td></tr><tr>

<td>

<anchor name="project-viewer"/>

__Project Viewer__

</td>

<td>

Has read-only access to projects and can only _view_ the project, its parent, and subprojects. Does not have permissions to [view agent details](build-agents-configuration-and-maintenance.md#Viewing+TeamCity+agents+details).

</td></tr><tr>

<td>

<anchor name="project-administrator"/>

__Project Administrator__

</td>

<td>

Can customize general settings of a project and settings of build configurations, assign roles to the project users, create subprojects. Has all the [Project Developer's](#project-developer) permissions.

With the enabled "_Change user / group notification rules in project_" permission, can edit notification rules for users and user groups assigned to their projects.

__Prior to TeamCity 10__, this role included the [Agent Manager](#agent-manager) role.   
__Since TeamCity 10__, [project-level agent management permissions](#Project-level+Agent+Management+Permissions) replace the inherited Agent Manager role. If old settings (set prior to TeamCity 10) are preserved in your TeamCity installation after upgrade to version 10 or later, we recommend excluding the Agent Manager role from the Project Administrator role manually and configuring agent management permissions instead.


</td></tr><tr>

<td>

<anchor name="project-developer"/>

__Project developer__


</td>

<td>

Usually commits changes to a project. Can start/stop builds, reorder builds in the build queue, label the build sources, review agent details, start investigation of a failed build.


</td></tr><tr>

<td>

<anchor name="agent-manager"/>

__Agent manager__


</td>

<td>

Can customize and manage [Build Agents](build-agent.md), change the run configuration policy, [enable/disable](build-agents-configuration-and-maintenance.md#Enabling%2FDisabling+Agents+via+UI) build agents, and [pause/resume build queue](build-queue.md#Pausing%2FResuming+Build+Queue).

__Prior to TeamCity 10.0__, this role was included into the [Project Administrator](#project-administrator) role. __Since TeamCity 10__, project-level agent management permissions [have been introduced](#Project-level+Agent+Management+Permissions).


</td></tr></table>

When per-project permissions are enabled, server administrators can modify the roles, delete them, or add new roles with any combination of permissions right in the TeamCity Administration web UI, or by modifying the `roles-config.xml` file stored in the \<[TeamCity Data Directory](teamcity-data-directory.md)\>\/config directory. When assigning roles to users, the _View role permissions_ link in the web UI displays the list of permissions for each role in accordance with their current configuration.

#### Per-Project Permissions
The following permissions may be assigned to a new or existing per-project role and will only affect the associated project:

* Run build
* Stop build / remove from queue
* Stop / remove from queue any personal build
* Pin / unpin build
* Tag build
* Pause / activate build configuration
* Comment build
* Clean build configuration sources
* Assign / unassign investigation
* View project and all parent projects
* View build configuration settings
* View VCS file content
* Edit VCS change description
* Edit project
* Change cleanup rules
* Customize build parameters
* Customize build revisions
* Change user roles in project
* Change user / group notification rules in project
* Manually label / merge build sources
* Archive / dearchive project
* Mute / unmute problems in project
* Change build statu* s
* View build runtime*  parameters and data
* Create subproject
* Delete subproject
* Create / delete VCS root
* Change agent pools associated with project
* Remove finished build
* Enable / disable versioned settings
* Enable / disable enforced settings in project
* Enable / disable agents associated with project
* Start / Stop cloud agent
* View cloud images and instances
* Manage project's agent cloud profiles
* View project agent usage statistics
* Change agent run configuration policy for project
* Administer project agent machines (e.g. reboot, view agent logs, etc.)
* Remove project agent
* Authorize project agent
* View project agents details

#### Non-Project Related Permissions
The following permissions may be applied to a new or existing role, but are not applied per-project:

* Reorder builds in queue
* Create user account
* Delete user account
* Modify user profile and roles
* Change user / group notification rules
* Manage server licenses
* Change server settings
* Change agent run configuration policy
* Clean sources on agent
* Enable / disable agent
* Authorize agent
* View agent details
* Configure server data cleanup
* Change own profile
* View agent usage statistics
* Remove agent
* Create user group
* Delete user group
* Modify user group (name, description and roles)
* Assign / unassign users to groups or change groups hierarchy
* Manage roles (create, delete, change permissions)
* Administer build agent machines (e.g. reboot, view agent logs, etc.)
* View audit log
* View server errors
* View usage statistics
* Change backup settings and control backup process
* Import projects
* Manage agent pools
* View user profile
* View all registered users
* Manage server installation: view logs, restart, etc.
* View server settings

## Project-level Agent Management Permissions

TeamCity has the following project-level permissions to perform a task on an agent:
* Enable/disable agents associated with project
* Start/Stop cloud agent for project
* Change agent run configuration policy for project
* Administer project agent machines (for example, reboot, view agent logs)
* Remove project agent
* Authorize project agent

<tip>

With the _"Authorize project agent"_ permission and the [maximum number of agents](agent-pools.md#Managing+Agent+Pools) setting for an agent pool, you can set up the system in a way which allows project administrators to run new agents and authorize/add them to their pools without involving the global system administrator.

</tip>

All project-level agent management permissions are by default added to the [Project Administrator](#project-administrator) role.

A user can perform a task controlled by one of these permissions on all the agents belonging to some [pool](agent-pools.md) provided this permission is granted to the user in all the projects associated with this pool. For example, a user with the _"Enable/disable agents associated with project"_ permission granted in some projects can enable or disable agents which belong to the pools of the related projects if the permission is granted in __all the projects__ associated with the pools. 

__  __

__See also:__

__Concepts__: [User Account](user-account.md)   
__Administrator's Guide__: [Enabling Guest Login](enabling-guest-login.md) | [Managing Users and User Groups](managing-users-and-user-groups.md)

__ __
