[//]: # (title: Role and Permission)
[//]: # (auxiliary-id: Role and Permission)

User access levels are handled by assigning different roles to users.

A _role_ is a set of _permissions_ that can be granted to a user in one or all projects thus controlling access to the projects and various features in the Web UI.   
A _permission_ is an _authorization_ granted to a TeamCity user to perform particular operations, for example, to run a build or modify build configuration settings.

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## Authorization Mode

TeamCity authorization supports two modes: __simple__ and __per\-project__.
* In the __simple__ mode, there are only three types of authorization levels: guest, logged\-in user, and administrator.   
* In the __per\-project__ mode, you can assign users _Roles_ in projects or server\-wide. The set of permissions in roles is editable.

Permissions within a role granted at the project level are automatically propagated in all the subprojects of this project.

The __View project and all parent projects__ permission allows you to view not only the project (with its subprojects) but its parent projects too.

## Changing Authorization Mode

Unless explicitly configured, the simple authorization mode is used when TeamCity is working in the Professional mode and per\-project is used when working in the Enterprise mode.   
To change the authorization mode, use the __Enable per\-project permissions__ checkbox on the __Administration | Authentication__ page.

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

Users with no restrictions; corresponds to the __System Administrator__ role in the per\-project authorization mode

</td></tr><tr>

<td>

Logged\-in user

</td>

<td>

Corresponds to the default __Project Developer__ role granted for all projects in the per\-project authorization mode


</td></tr><tr>

<td>

Guest user

</td>

<td>

Corresponds to the default __Project Viewer__ role granted for all projects in the per\-project authorization mode


</td></tr></table>

## Per-Project Authorization Mode

Roles are assigned to users by administrators on a per\-project basis \- a user can have different roles in different projects, and hence, the permissions are project\-based. A user can have a role in a specific project or in all available projects, or no roles at all. You can [associate a user account with a set of roles](managing-users-and-user-groups.md). A role can also be granted to a user group. This means that the role is automatically granted to all the users that are included into the group (both directly or through other groups).

By default, TeamCity provides the following roles:

<table><tr>

<td>

Role

</td>

<td>

Description

</td></tr><tr>

<td>

 __System Administrator__


</td>

<td>

TeamCity System Administrators have no restrictions in their permissions, and have all of the project administrator's permissions. They can create and manage users' accounts, authorize build agents and set up projects and build configurations, edit the TeamCity server settings, manage TeamCity licenses, configure server data clean\-up rules, change VCS roots, and so on.


</td></tr><tr>

<td>

__Project Viewer__

</td>

<td>

Project Viewer has read\-only access to projects and can only _view_ the project, its parent and subprojects. Project Viewer does not have permissions to [view agent details](build-agents-configuration-and-maintenance.md#Viewing+TeamCity+agents+details).

</td></tr><tr>

<td>

 __Project Administrator__


</td>

<td>

Project Administrator is a person who can customize general settings of a project and settings of build configurations, assign roles to the project users, create subprojects, and who has all the project developer's permissions. __Prior to TeamCity 10__, this role included in the Agent Manager role, __since TeamCity 10__ [agent management permissions](#Agent+Management+Permissions) (see below) replace the inherited Agent Manager role.


</td></tr><tr>

<td>

__Project Developer__


</td>

<td>

Project Developer is a person who usually commits changes to a project. He/she can start/stop builds, reorder builds in the build queue, label the build sources, review agent details, start investigation of a failed build.


</td></tr><tr>

<td>

__Agent Manager__


</td>

<td>

The Agent Manager role grants a user permissions for customizing and managing [Build Agents](build-agent.md); changing the run configuration policy, [enabling/disabling](build-agents-configuration-and-maintenance.md#Enabling%2FDisabling+Agents+via+UI) build agents, and [pausing/resuming build queue](build-queue.md#Pausing%2FResuming+Build+Queue).

__Prior to TeamCity 10.0__, this role is included in the _Project Administrator_ role.      
__Since TeamCity 10__, new agent management permissions have been introduced. See the [section below](#Agent+Management+Permissions).


</td></tr></table>

When per\-project permissions are enabled, server administrators can modify the roles, delete them, or add new roles with any combination of permissions right in the TeamCity Administration web UI, or by modifying the `roles-config.xml` file stored in the \<[TeamCity Data Directory](teamcity-data-directory.md)\>\/config directory. When assigning roles to users, the _View role permissions_ link in the web UI displays the list of permissions for each role in accordance with their current configuration.

## Agent Management Permissions

TeamCity has 6 permissions to perform a task on an agent: a user must have a specific permission granted in a project. A user can perform a task controlled by one of these permissions on all the agents belonging to some [pool](agent-pools.md) provided this permission is granted to the user in all the projects associated with this pool. For example, a user with 'Enable / disable agents associated with project' permission in some projects can enable or disable agents which belong to the pools of the related projects if the permission is granted in __all the projects__ associated with the pools. 

In new installations, these project\-related permissions are added to the Project Administrator role and the Agent manager role is no longer included in it.

In the existing installations after [upgrade](upgrade.md), the new permissions are added to the Agent Manager role (which is included in the Project Administrator role). It is recommended to remove the inherited Agent Manager role manually and add the required permission(s) to the Project Administrator role.

 
Agent Management Permissions:

* Enable / disable agents associated with project
* Start / Stop cloud agent for project
* Change agent run configuration policy for project
* Administer project agent machines (for example, reboot, view agent logs)
* Remove project agent
* Authorize project agent. Their permission and the [Managing Agent Pools](agent-pools.md#Managing+Agent+Pools) setting for an agent pool enable you to set up the system in a way which allows project administrators to run new agents and authorize/add them to their pools without involving the global system administrator.




__  __

__See also:__



__Concepts__: [User Account](user-account.md)

__Administrator's Guide__: [Enabling Guest Login](enabling-guest-login.md) | [Managing Users and User Groups](managing-users-and-user-groups.md) 