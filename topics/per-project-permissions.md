<chunk include-id="permissions">

## List of Permissions

### Server-level Permissions

The following permissions relate to per-server features of TeamCity and cannot be associated with specific projects:

<table>

<tr>
<td>
Permission
</td>
<td>
Description
</td>
</tr>

<tr>
<td>
Administer build agent machines
</td>
<td>

Allows rebooting machines on which TeamCity [agents](build-agent.md) are installed and viewing their logs and [threat dumps](reporting-issues.md#Agent+Thread+Dump).

<note>

If this option is disabled, but a user has the _"Administer project agent machines"_ permission granted for certain [projects](project.md), the user will be able to administer machines of agents belonging to the [pools](agent-pools.md) assigned to these projects.

</note>

</td>
</tr>

<tr>
<td>

Assign/unassign users to groups or change groups hierarchy

</td>
<td>

Allows managing [user groups](user-group.md) and modifying their hierarchy.

</td>
</tr>

<tr>
<td>
Authorize agent
</td>
<td>

Allows [authorizing or unauthorizing](build-agent.md#agent-authorization) build agents.

</td>
</tr>

<tr>
<td>
Change agent run configuration policy
</td>
<td>

Allows assigning agents to specific [build configurations](build-configuration.md) by establishing the agents' [run configuration policies](run-configuration-policy.md).

</td>
</tr>

<tr>
<td>
Change backup settings and control backup process
</td>
<td>

Grants access to the __Backup__ administration page and [backup options](creating-backup-from-teamcity-web-ui.md).

</td>
</tr>

<tr>
<td>
Change server settings
</td>
<td>

Allows [managing tools](installing-agent-tools.md). Grants access to the following Server Administration features: [Global Settings](teamcity-configuration-and-maintenance.md), [Authentication](configuring-authentication-settings.md), [Updates](upgrade.md#Automatic+Update), [Nodes Configuration](multinode-setup.md), [Notifiers](notifier.md), [Diagnostics](teamcity-monitoring-and-diagnostics.md), [Plugins List](installing-additional-plugins.md#Installing+a+plugin+via+Web+UI).

</td>
</tr>

<tr>
<td>
Change user/group notification rules
</td>
<td>

Allows modifying [Notification Rules](managing-users-and-user-groups.md#Notification+Rules) for groups or individual users.

</td>
</tr>

<tr>
<td>
Clean sources on agent
</td>
<td>

Grants access to the ["Clean source on this agent"](clean-checkout.md#Enforcing+Clean+Checkout) option for [build agents](build-agent.md).

<note>
With this permission disabled, a user will still be able to clean sources on agents that can run allowed build configurations, if this user has the _"Clean build configuration sources"_ permission.
</note>

</td>
</tr>

<tr>
<td>
Configure server data cleanup
</td>
<td>

Allows modifying settings on the [Clean-up Settings](clean-up.md) administration page.

</td>
</tr>

<tr>
<td>
Create user account
</td>
<td>

Allows [creating accounts](managing-users-and-user-groups.md#Creating+New+User) for new users.

</td>
</tr>

<tr>
<td>
Create user group
</td>
<td>

Allows [creating new user groups](managing-users-and-user-groups.md#Creating+New+Group).

</td>
</tr>

<tr>
<td>
Delete user account
</td>
<td>

Allows [deleting user accounts](managing-users-and-user-groups.md#Editing+User+Account).

</td>
</tr>

<tr>
<td>
Delete user group
</td>
<td>

Allows [deleting user groups](managing-users-and-user-groups.md#Editing+Group+Settings).

</td>
</tr>

<tr>
<td>
Enable/disable agent
</td>
<td>

Allows [enabling and disabling](build-agent.md#enable-agent) build agents.

</td>
</tr>

<tr>
<td>
Import projects
</td>
<td>

Allows [importing projects](projects-import.md) from backup files.

</td>
</tr>

<tr>
<td>
Manage agent pools
</td>
<td>

Allows [managing agent pools](agent-pools.md#Managing+Agent+Pools).

</td>
</tr>

<tr>
<td>
Manage roles
</td>
<td>

Allows [managing user roles](managing-users-and-user-groups.md#Roles).

</td>
</tr>

<tr>
<td>
Manage server licenses
</td>
<td>

Grants access to the [Licenses](licensing-policy.md#Managing+Licenses) server administration page.

</td>
</tr>

<tr>
<td>
Modify user group
</td>
<td>

Allows [changing configuration of user groups](managing-users-and-user-groups.md#Editing+Group+Settings).

</td>
</tr>

<tr>
<td>
Modify user profile and roles
</td>
<td>

Allows changing configuration of [user accounts](managing-users-and-user-groups.md#Editing+User+Account) and [user roles](managing-users-and-user-groups.md#Roles).

</td>
</tr>

<tr>
<td>
Remove agent
</td>
<td>

Allows removing any [build agent](build-agent.md). An agent must be disconnected before removing.

<note>

If this permission is disabled for a user, but the user has the _"Remove project agent"_ permission granted for certain [projects](project.md), he/she will be able to remove agents belonging to the [pools](agent-pools.md) assigned to these projects.

</note>

</td>
</tr>

<tr>
<td>
Reorder builds in queue
</td>
<td>

Allows [reodering builds in a build queue](build-queue.md#Ordering+Build+Queue) by dragging and enables the __Move to top__ button.

</td>
</tr>

<tr>
<td>
View agent usage statistics
</td>
<td>

Allows viewing [Build Agents' Workload Statistics](viewing-agents-workload.md#Build+Agents%27+Workload+Statistics) and [Load Statistics Matrix](viewing-agents-workload.md#Load+Statistics+Matrix).

</td>
</tr>

<tr>
<td>
View agent details
</td>
<td>

Grants access to viewing details of any [build agent](build-agent.md).

<note>

If this permission is disabled for a user, but the user has the _"View project agent details"_ permission granted for certain [projects](project.md), he/she will be able to view details of agents belonging to the [pools](agent-pools.md) assigned to these projects.

</note>

</td>
</tr>

<tr>
<td>
View all registered users
</td>
<td>

Allows viewing names of all users registered on the server (for example, in the list of users or when [assigning investigations](investigating-and-muting-build-problems.md#Investigating+or+Muting+Build+Problems)).

<note>

If this permission is disabled for a user, the user will still be able to see names of the users of the projects he/she has access to.

</note>

</td>
</tr>

<tr>
<td>
View audit log
</td>
<td>

Grants access to the [Audit](tracking-user-actions.md) page.

</td>
</tr>

<tr>
<td>
View cloud images and instances
</td>
<td>

Allows seeing added [cloud images and instances](teamcity-integration-with-cloud-solutions.md).

</td>
</tr>

<tr>
<td>
View server errors
</td>
<td>

Allows seeing all critical errors (global and project-level) as warnings in TeamCity UI.

Without this permission, Project Administrators will only see critical errors related to their projects.

</td>
</tr>

<tr>
<td>
View usage statistics
</td>
<td>

Grants access to the __Usage Statistics__ server administration page that allows enabling/disabling collection of statistics for TeamCity improvement.

</td>
</tr>

<tr>
<td>
View user profile
</td>
<td>

Allows viewing existing [user profiles](managing-users-and-user-groups.md).

</td>
</tr>

</table>

### Project-level Permissions

The following permissions can be associated with specific projects. Actions, allowed by these permissions, become available to a user only in specific [projects](project.md) (or all projects, if the permission is applied to the [Root project](project.md#Root+Project)) and for agents that belong to [pools](agent-pools.md) assigned to these projects.

<table>

<tr>
<td>
Permission
</td>
<td>
Description
</td>
</tr>

<tr>
<td>
Administer project agent machines
</td>
<td>

Allows rebooting machines on which TeamCity [agents](build-agent.md) are installed and viewing their logs and [threat dumps](reporting-issues.md#Agent+Thread+Dump).

</td>
</tr>

<tr>
<td>
Archive/dearchive project
</td>
<td>

Allows [archiving and dearchiving projects](archiving-projects.md).

</td>
</tr>

<tr>
<td>
Assign/unassign investigation
</td>
<td>

Allows [assigning and unassigning investigations](investigating-and-muting-build-problems.md#Investigating+or+Muting+Build+Problems) on users.

</td>
</tr>

<tr>
<td>
Authorize project agent
</td>
<td>

Allows [authorizing or unauthorizing](build-agent.md#agent-authorization) build agents that belong to [pools](agent-pools.md) assigned to permitted projects.

</td>
</tr>

<tr>
<td>
Change agent pools associated with project
</td>
<td>

Allows assigning [pools](agent-pools.md) to permitted projects and removing the project's association with any assigned pool.

</td>
</tr>

<tr>
<td>
Change agent run configuration policy for project
</td>
<td>

Allows assigning agents to specific [build configurations](build-configuration.md) in the permitted project by establishing the agents' [run configuration policies](run-configuration-policy.md).

</td>
</tr>

<tr>
<td>
Change build status
</td>
<td>

Allows changing [build status manually](changing-build-status-manually.md).

</td>
</tr>

<tr>
<td>
Change cleanup rules
</td>
<td>

Allows modifying clean-up rules on the [Clean-up Rules](clean-up.md#Project+Clean-up+Rules) page in the settings of permitted projects.

<note>

If a user has the _"Configure server data cleanup"_ permission, this user will be able to edit clean-up rules on both the server level and in projects he/she has access to.

</note>

</td>
</tr>

<tr>
<td>
Change user roles in project
</td>
<td>

Allows managing [roles](managing-roles.md) of users in permitted projects. Users with this permission will be able to manage only roles that grant those permissions they have themselves.

</td>
</tr>

<tr>
<td>
Clean build configuration sources
</td>
<td>

Allows [enforcing clean checkout](clean-checkout.md#Enforcing+Clean+Checkout) for a build configuration in a permitted project: either via the __Actions__ menu in the Build Configuration Home, or via the _Clean sources on this agent_ option in the __Agent Summary__.

<note>
With this permission disabled, a user will still be able to clean sources on agents, if this user has the _"Clean sources on agent"_ permission.
</note>

</td>
</tr>

<tr>
<td>
Comment build
</td>
<td>

Allows commenting builds in permitted projects.

</td>
</tr>

<tr>
<td>
Create/delete VCS root
</td>
<td>

Allows creating [VCS roots](configuring-vcs-roots.md) in the permitted projects.

</td>
</tr>

<tr>
<td>
Create subproject
</td>
<td>

Allows creating [subprojects](project.md#Project+Hierarchy) in permitted projects.

</td>
</tr>

<tr>
<td>
Customize build parameters
</td>
<td>

Allows [customizing build parameters](triggering-a-custom-build.md#Parameters) when running a build in a permitted projects.

</td>
</tr>

<tr>
<td>
Customize build revisions
</td>
<td>

Allows [selecting a build branch and revision](triggering-a-custom-build.md#Changes) when running a custom build in a permitted projects.

</td>
</tr>

<tr>
<td>
Delete subproject
</td>
<td>

Allows deleting [subprojects](project.md#Project+Hierarchy) in permitted projects.

</td>
</tr>

<tr>
<td>
Edit VCS change description
</td>
<td>

Allows editing description of a [VCS change in TeamCity](viewing-your-changes.md). 

</td>
</tr>

<tr>
<td>
Edit project
</td>
<td>

Allows [modifying settings](creating-and-editing-projects.md) of permitted projects. Overwrites all other permissions that concern project settings.

</td>
</tr>

<tr>
<td>
Enable/disable agents associated with project
</td>
<td>

Allows [enabling and disabling](build-agent.md#enable-agent) build agents.

<note>

To be able to enable/disable agents assigned to a pool, the user must have this permission granted for all the projects that are assigned to this pool.

</note>

</td>
</tr>

<tr>
<td>
Enable/disable enforced settings in project
</td>
<td>

Allows enabling and disabling settings, [enforced by a build template](build-configuration-template.md#Enforcing+settings+inherited+from+template), in the permitted projects.

</td>
</tr>

<tr>
<td>
Enable/disable versioned settings
</td>
<td>

Allows changing the state of options in [Versioned Settings](storing-project-settings-in-version-control.md#Synchronizing+Settings+with+VCS) of the permitted projects.

</td>
</tr>

<tr>
<td>
Manage project's agent cloud profiles
</td>
<td>

Grants access to the __[Cloud Profiles](agent-cloud-profile.md)__ section of the permitted projects.

</td>
</tr>

<tr>
<td>
Manually label/merge build sources
</td>
<td>

Enables the __[Label this build sources](vcs-labeling.md#Manual+VCS+labeling)__ and __[Merges this build sources](working-with-feature-branches.md#Manual+branch+merging)__ actions for builds in the permitted projects.

</td>
</tr>

<tr>
<td>
Mute/unmute problems in project
</td>
<td>

Allows [muting and unmuting problems](investigating-and-muting-build-problems.md#Investigating+or+Muting+Build+Problems) for build configurations in the permitted projects.

</td>
</tr>

<tr>
<td>
Pause/activate build configuration
</td>
<td>

Enables the __[Pause/Activate](build-configuration.md#Build+Configuration+State)__ actions for build configurations in the permitted projects.

</td>
</tr>

<tr>
<td>
Pin/unpin build
</td>
<td>

Allows [pinning/unpinning builds](pinned-build.md) in the permitted projects.

</td>
</tr>

<tr>
<td>
Remove finished build
</td>
<td>

Allows [removing finished builds](working-with-build-results.md) in the permitted projects.

</td>
</tr>

<tr>
<td>
Remove project agent
</td>
<td>

Allows removing [build agents](build-agent.md) that belong to the permitted [agent pools](agent-pools.md). The agent pool is considered _permitted_ for a user, if this user has this permission enabled for all the projects assigned to this [agent pool](agent-pools.md).

An agent must be disconnected before removing.

</td>
</tr>

<tr>
<td>
Run build
</td>
<td>

Allows [running builds](working-with-build-results.md) in the permitted configurations.

</td>
</tr>

<tr>
<td>
Start/stop cloud agent
</td>
<td>

Allows starting/stopping [cloud agents](agent-cloud-profile.md) that belong to the permitted [agent pools](agent-pools.md). The agent pool is considered _permitted_ for a user, if this user has this permission enabled for all the projects assigned to this [agent pool](agent-pools.md).


</td>
</tr>

<tr>
<td>
Stop / remove from queue any personal build
</td>
<td>

Allows [stopping](build-state.md#Canceled%2FStopped+build) [personal builds](personal-build.md) or [removing them from the queue](ordering-build-queue.md#Removing+Builds+From+Build+Queue), in the permitted projects.

</td>
</tr>

<tr>
<td>
Stop build / remove from queue
</td>
<td>

Allows [stopping](build-state.md#Canceled%2FStopped+build) builds or [removing them from the queue](ordering-build-queue.md#Removing+Builds+From+Build+Queue), in the permitted projects.

</td>
</tr>

<tr>
<td>
Tag build
</td>
<td>

Allows [adding tags to builds](build-tag.md) in the permitted projects.

</td>
</tr>

<tr>
<td>
View build configuration settings
</td>
<td>

Allows viewing the [settings of build configurations](build-configuration.md#Build+Configuration+Settings) in the permitted projects.

</td>
</tr>

<tr>
<td>
View build runtime parameters and data
</td>
<td>

Allows viewing [runtime parameters](working-with-build-results.md#Parameters) and data of build in the permitted configurations.

Grants access to the __Parameters__ tab of the build results.

</td>
</tr>

<tr>
<td>
View cloud images and instances
</td>
<td>

Allows viewing images and instances in the [cloud profiles](agent-cloud-profile.md) of the permitted projects.

</td>
</tr>

<tr>
<td>
View project agents details
</td>
<td>

Allows viewing [details](build-agents-configuration-and-maintenance.md#Viewing+TeamCity+agents+details) of [build agents](build-agent.md) that belong to the permitted [agent pools](agent-pools.md). The agent pool is considered _permitted_ for a user, if this user has this permission enabled for all the projects assigned to this [agent pool](agent-pools.md).

</td>
</tr>

<tr>
<td>
View project and all parent projects
</td>
<td>

Allows viewing permitted projects and all their parent projects.

</td>
</tr>


</table>

</chunk>