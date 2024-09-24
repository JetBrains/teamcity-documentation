[//]: # (title: Restoring TeamCity Data from Backup)
[//]: # (auxiliary-id: Restoring TeamCity Data from Backup)

TeamCity administrators are able to restore [backed up data](creating-backup-via-maintaindb-command-line-tool.md) via the TeamCity UI or by manually using the `maintainDB` command-line utility.

## Before Restoring

<note>

__Server Copying__

If you are creating a copy of the server, make sure to go through [copied server checklist](how-to.md#Copied+Server+Checklist) after restoration.
</note>

You can restore backed up data into the same or a different database; from/to any of the [supported databases](supported-platforms-and-environments.md#Databases) (for example, you can restore data from a HSQL database to a PostgreSQL database, as well as restore a backup of a PostgreSQL database to a new PostgreSQL database).

A newer version of TeamCity can be used to restore the backup created with any previous TeamCity version (provided that the TeamCity version is later than 6.0).

During the restoration of a large database, you might want to configure database-specific settings to make bulk data changes faster (like setting SQL Server "Recovery Model" to "Simple"). Consult your DBA for more details.

A TeamCity [backup file](teamcity-data-backup.md#Backing+up+Data) __does not contain build artifacts__. To back up the build logs and artifacts, copy the contents of the [artifact directory](build-artifact.md#Artifacts+Storage) ([`<TeamCity Data Directory>`](teamcity-data-directory.md)`/system/artifacts` by default) from the old location to the new one manually. The general compatibility rule of the data under `system/artifacts` is that files created by older TeamCity versions can be read by newer versions, but not necessarily vice versa.

When [external artifacts storage](configuring-artifacts-storage.md#external-artifacts-storage) is enabled, the [artifacts directory](teamcity-configuration-and-maintenance.md#artifact-directories) of the TeamCity Data Directory contains metadata about artifacts mappings, so make sure they are restored. 

See also details on the directories in the [TeamCity Data Directory](teamcity-data-directory.md) description.

Note that it is important to __copy artifacts before the server start__ as reindexing build metadata 
is launched automatically on starting the server restored from backup. 
If some artifacts are missing during indexing, this could cause some parts of the TeamCity server  to fail
(for example, a NuGet feed). In case you copied artifacts after the server start (or copying of artifacts was not completed before the server start), 
you might need to [reindex build metadata manually](common-problems.md#Problems+with+TeamCity+NuGet+Feed) and wait until reindexing is finished.

## Performing Restore

TeamCity can automatically restore the backed up data to _a new TeamCity installation_ (for example, to create a copy of an existing server). This process relies on the `maintainDB` utility, but performs all the necessary operations under the hood: 

1. Open the TeamCity URL (by default, [`http://localhost/`](http://localhost/){nullable="true"} for `exe` installations and [`http://localhost:8111/`](http://localhost:8111/){nullable="true"} for `tar.gz`) in a browser.
2. On the _TeamCity First Start_ page, enter the path to the [Data Directory](teamcity-data-directory.md) and click __Restore from backup__.
3. Enter an absolute path to the backup directory on the TeamCity server or upload a ZIP archive with the backed up data.
4. Choose the target database. If you use an external database, configure its address and credentials.
5. Proceed with the restoration.

TeamCity will restore the data and display the `maintainDB` utility log. If the version of the backed up data format is earlier than the current one, TeamCity will propose to upgrade it.

<note>

__Limitations of automatic restoration__

* Only builds with both database and configuration present in the backup file can be restored automatically. Some backup files with the [custom scope](creating-backup-from-teamcity-web-ui.md#backup-scope) might not be restored properly.
* In case automatic restoration faces an error (for example, not enough database memory), you will need to clean the database manually and then try restoring again.

In both cases, we recommend that you restore TeamCity manually via `maintainDB`, as described below.

</note>

<anchor name="maintaindb_restore"/>

To restore backed up files to _an existing TeamCity installation_ or to overcome the limitation of the automatic restore, use the `maintainDB` utility manually. This section describes __only some__ of the `maintainDB` options. For the complete list of all available options, run `maintainDB` from the command line with no parameters. See also maintainDB [startup options](creating-backup-via-maintaindb-command-line-tool.md#maintainDB+Startup+Options).

To perform restore from a backup file via `maintainDB`:
1. Install the TeamCity server from a `tar.gz` or `.exe` installation package. Do not start the TeamCity server.
2. Create a new empty [TeamCity Data Directory](teamcity-data-directory.md).
3. Choose one of the options:   
    * To restore the backup into a __new external database__, [create and configure an empty database](set-up-external-database.md), configure a `database.properties` file with the database settings to be passed to the `restore` command later on and either place it into the `/config` subdirectory of the newly created [TeamCity Data Directory](teamcity-data-directory.md) or anywhere on your file system outside the TeamCity Data Directory.   
    * To restore the data into the __same database the backup was created from__, proceed to the next step.   
4. Place the required [database drivers](set-up-external-database.md#Database-specific+Steps) into the `lib/jdbc` subdirectory of the newly created [TeamCity Data Directory](teamcity-data-directory.md) directory.
5. Use the `maintainDB` utility located in the [`<TeamCity Home>`](teamcity-home-directory.md)`/bin` directory to run the `restore` command:   

    a. To restore the backup into a __new external database__
    
    * if the  `database.properties` file is in the TeamCity Data Directory:
    
    ```
    maintainDB.[cmd|sh] restore -A <absolute path to the newly created TeamCity Data Directory> -F <path to the TeamCity backup file> -T <config/database.properties>
    ``` 
    
   * if the `database.properties` file is outside the TeamCity Data Directory:
    
    ```
    maintainDB.[cmd|sh] restore -A <absolute path to the newly created TeamCity Data Directory> -F <path to the TeamCity backup file> -T <absolute path to the database.properties file of the target database on the file system outside data dir>
    ```
    
    b. To restore the data into the __same database the backup was created from__:
    
    
    ```
    maintainDB.[cmd|sh] restore -A <absolute path to the newly created TeamCity Data Directory> -F <path to the TeamCity backup file>
    ```
    
   c. To restore the backup into the __internal database__:
    
    
    ```
    maintainDB.[cmd|sh] restore -A <absolute path to the newly created TeamCity Data Directory> -I -F <path to the TeamCity backup file>
    ```

6. If the process completes successfully, to __complete the restoration__ you need to __copy__ over [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/system/artifacts` from the old directory. The directory stores build artifacts and those are not included into the backup file.

 
Notes on the `restore` command options:
* The `-A` argument can be omitted if you have the [`TEAMCITY_DATA_PATH`](teamcity-data-directory.md) environment variable set.
* The `-F` argument can be an absolute path or a path relative to the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/system/backup` directory.
* The `-T` argument must point to the `database.properties` file created in step 3.
* If the `-T` argument is not specified and the `database.properties` file is present in the newly created `<TeamCity Data Directory>/config` and the backup file, the database is restored using the properties file in `<TeamCity Data Directory>/config`.
* By default, if no other option except `-F` is specified, all the backed up scopes will be restored from the backup file. To restore only specific scopes from the backup file, use the corresponding options of the `maintainDB` utility: `-D`, `-C`, `-U`, `-L`, and `-P`.
<tip>

To get the reference for the available options of `maintainDB`, run the utility without any command or option.
</tip>

## Restoring database only

When restoring the database but preserving the more recent TeamCity Data Directory, it is important to make sure the data is consistent:
* [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/system/pluginData` (_supplementary data_) should be restored as well to be consistent with the data stored in the database.
* [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/system/caches` should be cleared before the server start.

Before restoring a TeamCity database to an existing server, make sure the TeamCity server is not running.

To restore a TeamCity database only from a backup file to an existing server:
1. [Create and configure the database](set-up-external-database.md), placing the `database.properties` file into the `config` subdirectory of the `TeamCity Data Directory`.
2. Ensure that the required database drivers are present in the `/lib/jdbc` subdirectory.
3. Use the `maintainDB` utility located in the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/bin` directory (only available in TeamCity `.tar.gz` and `.exe` distributions).
4. If the _supplementary data_ is present in the backup, delete the content of the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/system/pluginData` directory (consider backing it up in another location first).
5. Use the `restore` command (The `-T` argument must point to the `database.properties` file created in step 1):

    ```
    maintainDB.[cmd|sh] restore -A <absolute path to TeamCity Data Directory> -F <path to the TeamCity backup file> -T <path to the database.properties file of the target database> -D
    ```
6. See the `maintainDB` utility console output. You may have to copy the `database.properties` file manually if requested.
7. Delete the contents of the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/system/caches` directory.

## Resuming restore after interruption

The restore may be interrupted due to the following reasons:
* Lack of space on the file system or in the database
* Insufficient permissions to the file system or the database

The interruption occurs when one of tables or indexes failed to be restored, which is indicated in the `maintainDB` utility console output.   
__Before resuming the restore__, manually delete the incorrectly restored object from the database.

To resume the backup restore after an interruption:   
Run the `maintainDB` utility with the `restore` command with the required options and the `--continue` option:


```
maintainDB.[cmd|sh] restore <all previously used restore options> --continue

```


<!--[//]: # (Internal note. Do not delete. "Restoring TeamCity Data from Backupd270e422.txt")-->    

<seealso>
        <category ref="admin-guide">
            <a href="creating-backup-via-maintaindb-command-line-tool.md">Creating Backup via maintainDB command-line tool</a>
        </category>
</seealso>
