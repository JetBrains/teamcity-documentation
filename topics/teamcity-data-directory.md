[//]: # (title: TeamCity Data Directory)
[//]: # (auxiliary-id: TeamCity Data Directory)

TeamCity Data Directory is the directory on the file system used by the TeamCity server to store configuration, build results, and current operation files. The directory is the primary storage for all the configuration settings and holds the data critical to the TeamCity installation.

For TeamCity Cloud instances, this directory is fully operated by the TeamCity team.
{product="tcc"}

The build history, users and their data and some other data are stored in the [database](set-up-external-database.md). See notes on [backup](manual-backup-and-restore.md) for the description of the data stored in the directory and the database.
{product="tc"}

Note that in this documentation and other TeamCity materials the directory is often referred to as `.BuildServer`. If you have a different name for it, replace `.BuildServer` with the actual name.
{product="tc"}

Note that in this documentation and other TeamCity materials the directory is often referred to as `.BuildServer`.
{product="tcc"}

<anchor name="SpecifyLocationoftheTeamCityDataDirectory"/>

## Location of the TeamCity Data Directory
{product="tc"}

The currently used Data Directory location can be seen on the __Administration | Global Settings__ page for a running TeamCity server instance. Clicking the __Browse__ link opens the __Administration | Global Settings | Browse Data Directory__ tab allowing the user to upload new or modify the existing files in the directory.

The current Data Directory location is also available in the `logs/teamcity-server.log` file (look for "_TeamCity Data Directory:_" line on the server startup).

### Configuring Location

There are three ways to configure the location of the TeamCity Data Directory:
* __by selecting it in the UI form on the first server startup__. The specified Data Directory is then saved into `<[TeamCity home directory](teamcity-home-directory.md)>/conf/teamcity-startup.properties` file.
* manually, using the `TEAMCITY_DATA_PATH` __environment variable__. The variable can be either system-wide or defined for the user under whom the TeamCity server is started.
* manually, by specifying the `teamcity.data.path` __[JVM property](server-startup-properties.md#JVM+Options)__.

If during the first startup TeamCity finds the Data Directory location configured as the environment variable, it skips the related startup screen and uses the detected path.

If the `TEAMCITY_DATA_PATH` environment variable is not set and the `<[TeamCity home directory](teamcity-home-directory.md)>/conf/teamcity-startup.properties` file does not define it either, the default TeamCity Data Directory location will be the user's home directory (for example, it is `$HOME/.BuildServer` under Linux and `%\USERPROFILE%/.BuildServer` under Windows).

### Recommendations as to choosing Data Directory Location

Since the Data Directory stores all the server and configured projects settings, it is important that it is not available for reading and writing to the OS users without the corresponding level of access. See the related [security notes](security-notes.md#Server+and+Data).

By default, the `system` directory stores all the [artifacts](build-artifact.md) and build logs of the builds in the history and can be quite large, so it is recommended to place TeamCity Data Directory on a non-system disk. Refer to the [Clean-Up](teamcity-data-clean-up.md) page to configure automatic cleaning of older builds. If a single local disk cannot store all the artifacts, you can add another disk and configure [multiple artifacts paths](teamcity-configuration-and-maintenance.md#artifact-directories).

Note that TeamCity assumes reliable and persistent read/write access to the TeamCity Data Directory and can malfunction if the Data Directory becomes inaccessible. This malfunction can affect TeamCity operation while the directory is unavailable and may also corrupt data of the currently running builds. While TeamCity should be able to tolerate occasional Data Directory inaccessibility, under rare circumstances the data stored in the directory might still be corrupted or partially lost.

<anchor name="caches_folder"/>
It is recommended to store `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/caches` on a local disk or even a separate dedicated disk, especially if TeamCity Data Directory is located on a network storage. You can either create a symlink to the `caches` directory from the main directory or redefine its path via the `teamcity.caches.path` JVM system property which can be specified in TEAMCITY_SERVER_OPTS environment variable, for instance:

```
TEAMCITY_SERVER_OPTS=-Dteamcity.caches.path=<path to local caches directory>
```
  
The `caches` directory stores local clones of the VCS repositories, and it is important to provide good performance for it. If the directory content is lost, it will be rebuilt and no data will be lost.

<warning>

__The Data Directory should not be located under the [TeamCity Home Directory](teamcity-home-directory.md).__

We highly recommend placing the Data Directory outside of the TeamCity installation directory to prevent any data loss.

</warning>

[//]: # (Internal note. Do not delete. "TeamCity Data Directoryd311e146.txt")


<anchor id="TeamCityDataDirectory-centralRepository" auxiliary-id="TeamCityDataDirectory-centralRepository"/>

### Upload Configuration Files to a Version Control

You can configure TeamCity to push files from the `.BuildServer/config` directory (excluding the `_trash` subfolder) to an external VCS repository. This setup allows you to:

* Monitor all configuration changes. Each edit to server configuration files is automatically pushed as a separate commit, enabling you to review and investigate the change history.
* Restore your server if its configuration files become corrupted. To roll back, shut down the TeamCity server and pull files from an earlier repository revision into the `.BuildServer/config` directory.

To set up a repository that should store your server's configuration files:

<img src="dk-settings-repo.png" width="706" alt="Settings repository"/>

1. Go to **Administration | Configs Repository**.
2. Tick **Commit changes in configuration files to the configs repository**.
3. Enter the path to your repository in the SSH format.
    > Configuration files contain scrambled passwords, database parameters, and other sensitive data that must be stored securely. Therefore, use a local Git repository on the same machine as your TeamCity server, or an external Git repository accessible only to your TeamCity server administrators.
    > 
    {style="warning"}
4. Enter a fully specified name (`heads/refs/<name>`) of the repository branch that should store configuration files.
5. Upload a private key to allow TeamCity to access your repository via SSH.

If you're using the [Autoincrementer plugin](https://plugins.jetbrains.com/plugin/9057-autoincrementer), we recommend that you update it to the latest version.


 <anchor name="data_directory_structure"/>

## Structure of TeamCity Data Directory
{product="tc"}

The `config` subdirectory of TeamCity Data Directory contains the configuration of your TeamCity projects, and the `system` subdirectory contains build logs, artifacts, and database files (if internal database (HSQLDB) is used which is default). You can also review information on [Manual Backup and Restore](manual-backup-and-restore.md) to understand better which data is stored in the database, and which is on the file system.
* __`BuildServer/config`__ — a directory where projects, build configurations and general server settings are stored.
  * `_trash` — backup copies of deleted projects, it is OK to delete them manually. For details on restoring the projects check [How To](how-to.md#Restore+Just+Deleted+Project).
  * `notifications` — notification templates and notification configuration settings.
  * `logging` — [internal server logging](teamcity-server-logs.md) configuration files, new files can be added to the directory manually.
     <anchor name="projects_folder"/>
  * `projects` — a directory which contains all project-related settings. Each project has its own directory. Project hierarchy is not used and all the projects have a corresponding directory residing directly under "projects".
    * `<projectID>` — a directory containing all the settings of a project with the `<projectID>` ID (including build configuration settings and excluding subproject settings). New directories can be created provided they have mandatory nested files. The _Root_ directory contains settings of the [root project](project.md#Root+Project). Whenever `*.xml.N` files occur under the directory, they are backup copies of corresponding files created when a project configuration is changed via the web UI. These backup copies are not used by TeamCity.
      * `buildNumbers` — a directory which contains `<buildConfigurationID>.buildNumbers.properties` files which store the current build number counter for the corresponding build configuration.
      * `buildTypes` — a directory with `<buildConfiguration or template ID>.xml` files with corresponding build configuration or template settings.
      * `pluginData` — a directory to store optional and plugin-related project-level settings. Bundled plugins settings and auxiliary project settings like custom project tabs are stored in the `plugin-settings.xml` file in the directory. Credentials stored outside of VCS per Versioned settings are stored in the `secure/credentials.json` file.
      * `vcsRoots` — a directory which contains project's VCS roots settings in the files `<VcsRootID>.xml`.
      * `project-config.xml` — the project configuration file containing the project settings, such as [parameters](configuring-build-parameters.md) and [clean-up rules](teamcity-data-clean-up.md).
  * `main-config.xml` — server-wide configuration settings.
  * `database.properties` — database connection settings, see more at [Setting up an External Database](set-up-external-database.md).
  * `license.keys` — a file which stores the license keys entered into TeamCity.
  * `change-viewers.properties` — [External Changes Viewer](external-changes-viewer.md) configuration properties, if available.
  * `internal.properties` — file for specifying various [internal TeamCity properties](server-startup-properties.md). It is __not__ present by default and needs to be created if necessary.
  * `auth-config.xml` — a file storing server-wide authentication-related settings.
  * `ldap-config.properties` — [LDAP authentication](ldap-integration.md) configuration properties.
  * `ntlm-config.properties` — [Windows domain authentication](configuring-authentication-settings.md#Windows+Domain+Authentication) configuration properties.
  * `issue-tracker.xml` — issue tracker integration settings.
  * `cloud-profiles.xml` — Cloud (for example, Amazon EC2) integration settings.
  * `backup-config.xml` — web UI backup configuration settings.
  * `roles-config.xml` — roles-permissions assignment file.
  * `database.*.properties` — default template connection settings files for different external databases.
  * `*.dtd` — DTD files for the XML configuration files.
  * `*.dist` — default template configuration files for the corresponding files without `.dist`. See [below](#Direct+Modifications+of+Configuration+Files).
* __`.BuildServer/plugins`__ — a directory where TeamCity plugins can be stored to be loaded automatically on the TeamCity start. New plugins can be added to the directory. Existing ones can be removed while the server is not running. The structure of a plugin is described in [Plugins Packaging](https://plugins.jetbrains.com/docs/teamcity/plugins-packaging.html).
  * `.tools` — create this directory to centralize tools to be installed on all agents. Any folder or `.zip` file under this folder will be distributed to all agents and appear under [Agent Home Directory](agent-home-directory.md) folder.
<anchor name="systemDir"/>
* __`.BuildServer/system`__ — a directory where build results data is stored. The content of the directory is generated by TeamCity and is not meant for manual editing.
   
  * <anchor name="artifacts"/>`artifacts` — the [default directory](teamcity-configuration-and-maintenance.md) where the builds' artifacts, logs and other data are stored. The format of the artifact storage is `<project ID>/<build configuration name>/<internal_build_id>` (read more about the [internal build ID](build-results-page.md#Internal+Build+ID)). If necessary, the files in each build's directory can be added/removed manually — this will be reflected in the corresponding build's artifacts.
     * `.teamcity` subdirectory stores build's [hidden artifacts](build-artifact.md#Hidden+Artifacts) and build logs (see below). The files can be deleted manually, if necessary, but it is not recommended as the build will lose the corresponding features backed by the files (like the build log, displaying/using finished build parameters including for the build reuse as snapshot dependency, coverage reports, and so on)      
    <anchor name="rawLogs"/>
       *  `logs` subdirectory stores the [build log](build-log.md) in an internal format. The build log stores the build output, compilation errors, test output, and test failure details. The files can be removed manually, if necessary, but corresponding builds will lose build log and failure details (as well as test failure details).
  * `messages` — a directory stores the files which could not be moved (see the server log on the server start for details).
  * `changes` — a directory where the [remote run](personal-build.md) changes are stored in internal format. Name of the files inside the directory contains internal personal change id. The files can be deleted manually, if necessary, but corresponding personal builds will lose personal changes in UI and when affected queued builds try to start, they fail or run without personal patch.
  *   <anchor name="pluginData"/> `pluginData` — a directory storing various data concerning builds, current system state, and so on. It is not advised to delete or modify this directory. The content of this directory corresponds to the data stored in the database, so when the database is restored, this directory should be restored to the same state to be consistent with the database.
    * `audit` — directory holding history of the build configuration changes and used to display diff of the changes. Also stores related data in the database.
  * `caches` — a directory with internal caches (of the VCS repository contents, search index, other). It can be [manually deleted](teamcity-monitoring-and-diagnostics.md#Caches) to clear caches: they will be restored automatically as needed. It is safer to delete the directory while server is not running.
     * `.unpacked` — directory that is created automatically to store unpacked server-side plugins. Should not be modified while the server is running. Can be safely deleted if the server is not running.
  * `buildserver.*` — a set of files pertaining to the embedded HSQLDB.
* __`.BuildServer/backup`__ — default directory to store backup archives created via [web UI](creating-backup-from-teamcity-web-ui.md). The files in this directory are not used by TeamCity and can be safely removed if they were already copied for safekeeping.
* __`.BuildServer/lib/jdbc`__ — directory that TeamCity uses to search for [database drivers](set-up-external-database.md). Create the directory if necessary. TeamCity does not manage the files in the directory, it only scans it for `.jar` files that store the necessary driver.

## Direct Modifications of Configuration Files
{product="tc"}

The files under the `config` directory can be edited manually (unless explicitly noted). The changes will be taken into account without the server restart. TeamCity monitors these files for changes and rereads them automatically when modifications or new files are detected. Bear in mind that it is easy to break the physical or logical structure of these files, so edit them with extreme caution. Always [back up](teamcity-data-backup.md) your data before making any changes.

Note that the format of the files can change with newer TeamCity versions, so the files updating procedure might need adjustments after an upgrade.

The [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html) has means for most common settings editing and is more stable in terms of functioning after the server upgrade.

  
<anchor name="distfiles"/>
   
### .dist Template Configuration Files

Many configuration files meant for manual editing use the following convention:
* Together with the file (suppose named `fileName`) there comes a file `fileName.dist`. The `.dist` files are meant to store default server settings, so that you can use them as a sample for `fileName` configuration. The `.dist` files should not be edited manually as they are overwritten on every server start. Also, `.dist` files are used during the server upgrade to determine whether the `fileName` files were modified by user, or the latter can be updated.

### XML Structure and References

If you plan to modify the configuration manually, note that there are entries interlinked by _ids_. Examples of such entries are __build configuration -&gt; VCS roots__ links and __Project -&gt; parent project__ links. All the entries of the same type must have unique ids in the entire server. New entries can be added only if their ids are unique.

See also the related [section](how-to.md#Move+TeamCity+Projects+from+One+Server+to+Another) on moving projects between TeamCity servers.

<seealso product="tc">
        <category ref="installation">
            <a href="teamcity-data-backup.md">TeamCity Data Backup</a>
        </category>
</seealso>
