[//]: # (title: Projects Import)
[//]: # (help-id: Projects Import)

Projects Import adds projects with all their data and user accounts from a backup file to an existing TeamCity server, effectively merging two servers into one.


<include from="common-templates.md" element-id="env-imported-encryption-key-warning">
<var name="operation-name-ev" value="import projects exported"/>
</include>


<anchor name="ProjectsImport-ProjectsImportorServerMove"/>

## Projects Import or Server Move
{instance="tc"}

Import is a disruptive operation and [not all data is imported](#Data+not+included+into+import), so use it only when you need to add projects to a server that already hosts other projects. To move all the data of a server to a different machine, use [server move](how-to.md#Move+TeamCity+Installation+to+a+New+Machine) instead.

## Importing projects
{instance="tc" help-id="ProjectImport-ImportingProjects"}

1. On the source server, [create a usual backup](creating-backup-from-teamcity-web-ui.md) file containing the projects to import. The __[major version](upgrading-teamcity-server-and-agents.md#Upgrading+TeamCity+Server) of the source and target servers must be the same__.
2. On the target server, go to __Server Administration | Project Import__, upload the backup file, and follow the wizard. TeamCity displays the results when the import finishes.

<warning>

Backup files do not include build artifacts. To complete the import, copy them to the target server by running the [provided scripts](#Moving+artifacts+and+logs): check details in the import log.

</warning>

>Projects Import does not support external artifact storages. If you use one, move the externally stored artifacts to their new locations manually, using the build IDs mapping generated during the import. Contact [TeamCity support](troubleshooting.md) for details.

### Selecting projects for import

After selecting a backup file, choose which projects to import. TeamCity analyzes them and reports whether each project will be imported, merged, or skipped.

* The project is __imported__ if it is new for the target server. All its entities (build configurations, templates, builds, and so on) and their data are created on the target server.
* The project is __merged__ if it already exists on the target server, that is, the source and target projects share the same [UUID](identifier.md#Universally+Unique+IDs) and [external ID](identifier.md#External+IDs). Existing entities remain intact, and only the entities that are new for the target are imported with their data.   
  Data of the existing entities is neither imported nor merged: new changes are not added to an existing VCS root, and a template that exists on both servers keeps its target settings. This also means you cannot import missing builds into an existing build configuration — to do that, delete this build configuration in the UI and reimport its project.
* The project is __skipped__ if a [conflict](#Conflicts) occurs: its [UUID](identifier.md) is new but its [external ID](identifier.md#External+IDs) already exists on the target, or the source and target projects have the same UUID but different external IDs.

### Defining import scope

You can import project settings, builds and changes history, and user accounts in any combination. Since an imported project can use settings of its parents, TeamCity also imports the VCS roots, templates, meta-runners, and other project-related settings of the parent projects. Existing objects on the target server are never overwritten.

### Configuration files import

For each imported or merged project, TeamCity copies the configuration files that are new for the target server to its [Data Directory](teamcity-data-directory.md). Existing files are not changed.

The following files are imported:
* Configuration XML files of the project with its build configurations, templates, and VCS roots, as well as its subprojects.
* All files from the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/plugins` directory.
* Build number files of the newly added build configurations.

### Importing users, groups, and tokens
{help-id="ProjectImport-ImportUsersAndGroups"}

#### User accounts

TeamCity users must have unique usernames, so when users are in the import scope, TeamCity compares usernames to see whether each user will be imported or merged. In both cases, it transfers all the user-related data (personal builds, changes, test mutes, and investigations) and the [user account settings](configuring-your-user-profile.md) — roles, permissions, VCS names, notification settings, and so on, both system-wide and related to the imported projects.

* A user whose username is new for the target server is __imported__. Such users appear on the target server in a separate group marked _Imported &lt;Import Date Time&gt;_.
* A user whose username exists on both servers can be __merged__. The existing data remains intact, and only the data that is new for the target is added.

Merging relies on usernames, so if the same username belongs to different people on the two servers, their information is merged anyway. Merging can also extend a user's permissions on the target server: a system administrator role granted on the source server is added to the target user, while project roles are added only for the projects within the import scope.

The __Project Import | Import scope | Users__ section reports how many conflicts were found, so that you can review them and decide whether to merge. TeamCity lists users that have the same username but different emails on both servers, and counts users that share both a username and an email. If [email verification](enabling-email-verification.md) is enabled, such users are compared based on their verified emails. These options are active only when users with verified emails are present on the source server, the target server, or both.

#### User groups

Groups work the same way: new groups are imported and the existing ones can be merged. If a [conflict](#Conflicts) occurs — a group exists on both servers but with different roles — the target group may get additional roles after the import, and so do all its members.

The __Project Import | Import scope | Groups__ section reports how many conflicting groups were found. You can view all groups that share a group key and decide whether to merge them. The ["All Users" group](creating-and-managing-user-groups.md#%22All+Users%22+Group) is always listed as conflicting because it exists on every TeamCity server by default.

#### Access tokens

When users are in the import scope, whether TeamCity transfers their [access tokens](configuring-your-user-profile.md#Managing+Access+Tokens) depends on the [token scope](configuring-your-user-profile.md#token-scope):

* Tokens limited to the projects selected for import (the _Limit per project_ scope) are imported by default. On the target server, these tokens retain their permissions only for the imported projects.
* Tokens limited to any other project are not imported. For example, a token scoped to the parent project of an imported project is skipped, even though TeamCity imports the settings this parent project shares with its children.
* Tokens that grant the same permissions as their owner (the _Same as current user_ scope) are imported only with explicit consent: select the corresponding checkbox in the __Project Import | Import scope | Users__ section. Since such tokens are not limited to any project, on the target server they grant every permission that their owner has there — which is why they are left out by default.

For a merged user, imported tokens are added to the tokens that this user already has on the target server.

### Conflicts
{help-id="Projects Import Conflicts"}

TeamCity does not import entities from the backup file if they conflict with an entity on the target server. Before the import, TeamCity analyzes the backup file and displays all detected conflicts on the __Import Scope__ configuration page.

It is __highly recommended that you resolve all conflicts__ before proceeding, as unresolved conflicts may result in unpredictable behavior after the import. For example, a build configuration can report a critical error if the VCS root it depends on was skipped, or refer to the wrong template if the templates from the source and target servers had conflicting [external IDs](identifier.md#External+IDs).

If you import without resolving the conflicts, you can find the conflicting files in the `conflictingFiles` directory under the import results logs.

<anchor name="ProjectsImport-Dataexcludedfromimport"/>

## Importing projects
{instance="tcc"}

1. On the source TeamCity On-Premises server, create a [backup file](https://www.jetbrains.com/help/teamcity/creating-backup-from-teamcity-web-ui.html) containing the projects to import.
2. On the target server, go to __Server Administration | Project Import__, upload the backup file, and follow the wizard. TeamCity displays the results when the import finishes.

## Data not included into import

The import has the following limitations:
* Agents and agent pools are not imported ([TW-39797](https://youtrack.jetbrains.com/issue/TW-39797)).
* Settings are merged on a per-file basis: new files are added, but no settings files are merged. For example, if a project being imported already exists on the target server, its parameters, project features, and plugin settings are not merged.
* If you use the "Store secure values (like passwords or API tokens) outside of VCS" option for versioned settings, credentials are not imported for the projects that already exist on the server.
* Audit records are imported only if users are in the import scope.
* Running builds and the build queue are not included in the backup, and thus not imported.
* Internal IDs, such as build IDs, are not preserved. This means that URLs to the build results pages from the old server appear broken even if redirected to the new server.
* Backup files do not contain artifacts and logs (build logs are stored under build artifacts), so these are not imported automatically. TeamCity provides scripts to move them [manually](#Moving+artifacts+and+logs).
{instance="tc"}
* Global server settings (authentication schemes, custom roles, and so on) are not imported.
* Build artifacts and logs cannot be imported to TeamCity Cloud.

<note>

An import may take significant time. There can be only one import process per server.

</note>
    
## Moving artifacts and logs
{instance="tc" help-id="ProjectImport-MovingArtifactsAndLogs"}

Artifacts and logs are not imported from the backup file, but you can copy or move them from the source to the target server using the `.bat` and `.sh` scripts from the `projectsImport-<date>` directory under the TeamCity logs. These scripts accept the source and target [artifact directories](build-artifact.md) via the command line, and the rest is done automatically. You can run the scripts while the server is running.

It may take some time for TeamCity to display the imported build artifacts.


## Moving artifacts and logs
{instance="tcc" help-id="ProjectImport-MovingArtifactsAndLogs"}

Restoring artifacts and logs of imported projects is available only on TeamCity On-Premises instances. See this article for more information: [Migrate from TeamCity On-Premises to TeamCity Cloud](migrate-from-teamcity-on-premises-to-teamcity-cloud.md#Migration+Process).

## Viewing Import Results

Each import process creates the `projectsImport-<date>` directory under the TeamCity logs, allowing you to view the import results. This directory contains:

* the `conflictingFiles` directory with all the data that has been merged
* mappings of the fields in the source and target databases
* scripts for copying artifacts and logs (see the section [above](#Moving+artifacts+and+logs))
{instance="tc"}
* the import report listing the import results, including the information on the data that has not been imported (if any)
