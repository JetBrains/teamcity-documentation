[//]: # (title: Creating Backup from TeamCity Web UI)
[//]: # (auxiliary-id: Creating Backup from TeamCity Web UI)

TeamCity allows creating a backup of TeamCity data via the Web UI. To create a backup file, navigate to the __Administration | Backup__ page, specify backup parameters as described below, and start the backup process.

## Backup Settings

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

Backup file


</td>

<td>

Specify the name for the backup file, the extention (`.zip`) will be added automatically. By default, TeamCity will store the backup file in the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/backup` directory. For security reasons, you cannot explicitly change this path in the UI. To modify this setting, specify an absolute or relative path (the path should be relative to [TeamCity Data Directory](teamcity-data-directory.md)) in the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/config/backup-config.xml` file. For example:

```Shell
<backup-settings>
  ...
  <general>
    <backup-dir path="C:/TC-Backups"/>
  </general>
  ...
</backup-settings>
```

</td></tr><tr>

<td>

add timestamp suffix

</td>

<td>

Check this option to automatically add time stamp suffix to the specified filename. This may be useful to differentiate your backup files, if you don't clean up old backups.

<note>

* If the directory where backup files are stored already contains a file with the name specified above, TeamCity won't run backup: you will need either to specify another name, or enable _time stamp suffix_ option, which allows you to avoid this.
* Time stamp suffix has a specific format: sorting backup files alphabetically will also sort them chronologically.
</note>
  
</td></tr><tr>

<td id="backup-scope">

<anchor name="CreatingBackupfromTeamCityWebUI-backup_scope"/>

Backup scope

</td>

<td>

Specify what kind of data you want to back up. The contents of the backup file depending on the scope is described right in the UI when you select a scope. Note that the size of the backup file and the time the backup process will take depends on the scope you select.

To reduce the resulting file size and the time spent on the backup, select the "basic" scope, which includes server settings, projects and builds configurations, plugins and database. However, you'll be able to restore only the settings which were backed up.

For the full backup suitable for most of the needs, it is recommended to use the Custom scope with all the items selected except for "build logs" and then backup the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/system/artifacts` location as a usual file system.

Build artifacts are not included into the backup due to their size. It is recommended to either back up the [artifacts directories](teamcity-configuration-and-maintenance.md) separately or use a redundant storage for the artifacts. Build logs are stored as a part of build artifacts, so there is no need to back up the build logs if you implement a separate backup of the artifacts locations.

</td></tr></table>

When you start the backup, TeamCity will display its status and details of the current process including progress and estimates.

<note>

__Important Note__

* Running and queued builds are not included into a backup created during server running. To include these builds, consider using the different [backup](creating-backup-via-maintaindb-command-line-tool.md) approach when the server is not running.
* The backup process takes time that depends on how many builds there are in the system. During this process the system's state can change, for example, some builds may finish, other builds that were waiting in the build queue may start, new builds may appear in the build queue. Note that these changes will not influence the backup. TeamCity will back up only the data actual by the time the backup process was started.
* The resulting backup file is a `*.zip` archive which has a specific structure that does not depend on the OS or database type you use. Thus, you can use the backup file to restore your data even on a different Operating System, or with a different database. If you change the contents of this file manually, TeamCity will not be able to restore your data.

</note>


## Back up History

The __History__ tab of the __Administration | Backup__ page allows reviewing the list of created backup files, their size and date when the files were created.

Note that only backup files created from web UI are shown here. Backups created with the [utility](creating-backup-via-maintaindb-command-line-tool.md) are not displayed on the __History__ tab.

 <seealso>
        <category ref="installation">
            <a href="upgrading-teamcity-server-and-agents.md">Upgrade</a>
        </category>
        <category ref="concepts">
            <a href="teamcity-data-directory.md">TeamCity Data Directory</a>
        </category>
        <category ref="admin-guide">
            <a href="teamcity-data-backup.md">TeamCity Data Backup</a>
        </category>
</seealso>