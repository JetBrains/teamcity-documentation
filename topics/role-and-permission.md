[//]: # (title: Role and Permission)
[//]: # (auxiliary-id: Role and Permission)

User access levels are handled by assigning different _roles_ to users thus granting them respective _permissions_.

A _permission_ is an _authorization_ to perform particular operations, for example, to run a build or modify build configuration settings.   
A _role_ is a set of _permissions_ that can be granted to a user in one or all projects thus controlling access to the projects and various features in the Web UI.

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

<td id="system-administrator">

__System Administrator__

</td>

<td>

Has no restrictions in permissions, and has all of the [Project Administrator's](#project-administrator) permissions. Сan create and manage user accounts, authorize build agents, set up projects and build configurations, edit the TeamCity server settings, manage TeamCity licenses, configure server data clean-up rules, change VCS roots, and so on.


</td></tr><tr>

<td id="project-viewer">

__Project Viewer__

</td>

<td>

Has read-only access to projects and can only _view_ the project, its parent, and subprojects. Does not have permissions to [view agent details](build-agents-configuration-and-maintenance.md#Viewing+TeamCity+Agents+Details).

</td></tr><tr>

<td id="project-administrator">

__Project Administrator__

</td>

<td>

Can customize general settings of a project and settings of build configurations, assign roles to the project users, create subprojects. Has all the [Project Developer's](#project-developer) permissions.

With the enabled "_Change user / group notification rules in project_" permission, can edit notification rules for users and user groups assigned to their projects.

</td></tr><tr>

<td id="project-developer">

__Project developer__

</td>

<td>

Usually commits changes to a project. Can start/stop builds, reorder builds in the build queue, label the build sources, review agent details, start investigation of a failed build.

</td></tr><tr>

<td id="agent-manager">

__Agent manager__

</td>

<td>

Can customize and manage [Build Agents](build-agent.md), change the run configuration policy, [enable/disable](build-agents-configuration-and-maintenance.md#Enabling%2FDisabling+Agents+via+UI) build agents, and [pause/resume build queue](build-queue.md#Pausing%2FResuming+Build+Queue).


</td></tr></table>

When per-project permissions are enabled, server administrators can modify the roles, delete them, or add new roles with any combination of permissions right in the TeamCity Administration web UI, or by modifying the `roles-config.xml` file stored in the \<[TeamCity Data Directory](teamcity-data-directory.md)\>\/config directory. When assigning roles to users, the _View role permissions_ link in the web UI displays the list of permissions for each role in accordance with their current configuration.

## Project-level Agent Management Permissions

TeamCity has the following project-level permissions to perform a task on an agent:
* Enable/disable agents associated with project
* Start/Stop cloud agent for project
* Change agent run configuration policy for project
* Administer project agent machines (for example, reboot, view agent logs)
* Remove project agent
* Authorize project agent

<tip>

With the _"Authorize project agent"_ permission and the [maximum number of agents](configuring-agent-pools.md#Managing+Agent+Pools) setting for an agent pool, you can set up the system in a way which allows project administrators to run new agents and authorize/add them to their pools without involving the global system administrator.

</tip>

All project-level agent management permissions are by default added to the [Project Administrator](#project-administrator) role.

A user can perform a task controlled by one of these permissions on all the agents belonging to some [pool](configuring-agent-pools.md) provided this permission is granted to the user in all the projects associated with this pool. For example, a user with the _"Enable/disable agents associated with project"_ permission granted in some projects can enable or disable agents which belong to the pools of the related projects if the permission is granted in __all the projects__ associated with the pools. 

<seealso>
        <category ref="concepts">
            <a href="user-account.md">User Account</a>
        </category>
        <category ref="admin-guide">
            <a href="enabling-guest-login.md">Enabling Guest Login</a>
            <a href="managing-users-and-user-groups.md">Managing Users and User Groups</a>
        </category>
</seealso>