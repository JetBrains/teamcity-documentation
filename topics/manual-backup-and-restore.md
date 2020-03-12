[//]: # (title: Manual Backup and Restore)
[//]: # (auxiliary-id: Manual Backup and Restore)

## Server Manual Backup

Other ways to create a backup are [available](teamcity-data-backup.md). You can use the instructions on this page if you want fine-grained control over the backup process or need to use a specific procedure for your TeamCity backups. Manual backups can also be the most performant way to create and restore backups as they can be tailored to the specific infrastructure in use.

<note>

Before performing the backup procedures, it is recommended to __stop__ the TeamCity server. This way you ensure the backup taken is consistent. If you create a backup while the TeamCity server is running, currently updated data (mostly related to queued and running builds as well as settings updates) can appear in inconsistent state after restore. When performing backup while the server is running first create the database backup and then that of the files in the TeamCity Data Directory.
</note>

The following data needs to be backed up:

### TeamCity Data Directory

[`TeamCity Data Directory`](teamcity-data-directory.md) stores:
* server settings, projects and build configurations with their settings (i.e. all that is configured via the Administration web UI)
* build artifacts by default, unless a different location is [configured](build-artifact.md). Build logs are also stored as a part of the build artifacts
* current operation files, internal data structure, and so on

For more details on the directory structure and data, refer to the [TeamCity Data Directory](teamcity-data-directory.md) section.

If necessary, you can exclude parts of the directory from the backup to save space: you will only lose the excluded data. You may safely exclude the `system/caches` directory from the backup — the necessary data will be rebuilt from scratch on TeamCity startup.

If you decide to skip backing up data under `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>/system` directory, make sure you note the most recent files in each of the `artifacts`, `messages`, and `changes` subdirectories and save this information. It will be needed if you decide to restore the database backup with the TeamCity Data Directory corresponding to a newer state than that of the database.


[//]: # (Internal note. Do not delete. "Manual Backup and Restored203e71.txt")    


The `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>/system/buildserver.*` files store the internal database (HSQLDB) data. You need to back them up if you use HSQLDB (the default setting not suitable for production use).

### Database Data

<anchor name="database_data"/>

The database stores all information on the build results (build history and all the build\-associated data except for artifacts and build logs), VCS changes, agents, build queue, user accounts and user permissions, and so on.
* If you use the HSQLDB, the internal database (default setting, not recommended for production), the database is stored in the files residing directly in the `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>/system` folder. All files from the directory can be backed up. You may also refer to the [HSQLDB backup notes](http://hsqldb.org/doc/guide/ch05.html#N10F02).
* If you use an external database, back up your database schema used by TeamCity with database-specific tools. For the external database connection settings used by TeamCity, refer to the `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>/config/database.properties` file. You can also see the [corresponding installation section](setting-up-an-external-database.md).

### Application Files

You do not need to back up TeamCity application directory (web server alone with the web application), provided you still have the original distribution package and you did not:
* place any custom libraries for TeamCity to use
* install any non-default TeamCity plugins directly into web application files
* make any startup script/configuration changes.

If you feel you need to back up the application files:
* If you use a _non\-war_ distribution: back up __everything__ under `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>` except for the `temp` and `work` directories.
* If you use the _war_ distribution, follow the backup procedure of the servlet container used.


### Log files

If you need [TeamCity server log files](teamcity-server-logs.md) (which are mainly used for problem solving or debug purposes), back up the  `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>/logs` directory.

<note>

You may also want to back up TeamCity Windows Service settings, if they were modified.
</note>

## Manual Restoration of Server Backup

If you need to restore backup created with the web UI or `maintainDB` utility, please refer to [Restoring TeamCity Data from Backup](restoring-teamcity-data-from-backup.md). This section describes restoration of a manually created backup.

You should always restore both the data in the `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>` and data in the database. Both the database and the directory should be backed up/restored in sync.

### TeamCity Data Directory Restoration

You can simply put the previously backed up files back to their original places. However, it is important that no extra files are present when restoring the backup. The simplest way to achieve this is to restore the backup into a clean Data Directory. If this is not possible, make sure the files created after the backup was done are cleared. Especially the newly created files under the `artifacts`, `messages`, `changes` directories under `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>/system`.

### TeamCity Database Restoration

The database must be restored using the database\-specific tools and methods. You might also want to use database-specific methods to make the restore faster (like setting SQL Server "Recovery Model" to "Simple").

Prior to restoring, make sure there are no extra tables in the schema used by TeamCity.

### Restoration to New Server

If you want to run a copy of the server, make sure the servers use distinct data directories and databases. For an external database, make sure you modify settings in the `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>/config/database.properties` file to point to a different database.

### Restoring Build Logs

Build logs located in the `/logs` subdirectory of [`/artifacts`](teamcity-data-directory.md#artifacts) from [multiple artifact directories](build-artifact.md) are backed up and restored into the single directory `system/<project ID>/<build configuration name>/<internal_build_id>/.teamcity/logs`.

__ __
