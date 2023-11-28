[//]: # (title: Creating Backup via maintainDB command-line tool)
[//]: # (auxiliary-id: Creating Backup via maintainDB command-line tool)

TeamCity [.tar.gz and .exe distributions](install-and-start-teamcity-server.md) provide the `maintainDB.cmd|sh` utility located in the `<[TeamCity Home](teamcity-home-directory.md)>/bin` directory. This command-line tool enables you to back up the server data, [restore it](restoring-teamcity-data-from-backup.md), and [migrate between different databases](migrating-to-external-database.md). You can also back up data using the [UI](creating-backup-from-teamcity-web-ui.md).

## maintainDB Backup Procedure

To back up TeamCity server data using the `maintainDB.cmd|sh` tool:

1. Before proceeding, review the [alternative backup procedures](teamcity-data-backup.md#Backup+Alternatives) (particularly with respect to the [scope of the backed-up data](teamcity-data-backup.md#What+Data+is+Backed+Up)), to ensure that the `maintainDB.cmd|sh` backup procedure meets your backup needs.
   > Some types of data are not backed up by this procedure. See [](teamcity-data-backup.md#What+Data+is+Backed+Up) to decide whether additional manual backup steps are required.
   >
   {type="warning"}
2. Review your artifacts backup strategy. The `maintainDB.cmd|sh` tool does _not_ back up build artifacts, because a production system is usually configured with [external artifacts storage](configuring-artifacts-storage.md) that has dedicated backup facilities.
   > If necessary, you can manually back up build artifacts and build logs by copying files from `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/artifacts`, which is the default location for artifacts storage.
   >
   {type="tip"}
3. Stop the TeamCity server.
   > If the backup process is started while the TeamCity server is running, the running builds and queued builds are not included in the backup.
   >
   {type="note"}
4. Decide on the backup scope:
    * `--basic` — back up only the most essential data
    * `--all` — back up the maximum amount of data possible with maintainDB
    * Custom — choose from the options: `-D -C -U -L -P`
   > For more details, see [backup scope options](#Backup+Scope+Options).
   >
   {type="tip"}
5. From the `<TeamCity Home>/bin` directory, enter the following command to back up TeamCity from the `<TeamCity Data Directory>`, saving the backup to the `<Backup File Name>` file with timestamp suffix:
    <tabs>
    <tab title="Linux"><p/>

    ```Plain Text
    ./maintainDB.sh backup \
        -A <TeamCity Data Directory> \
        -D -C -U -L -P \
        -F <Backup File Name> --timestamp
    ```

    </tab>
    <tab title="Windows"><p/>

    ```Plain Text
    .\maintainDB.cmd backup -A <TeamCity Data Directory> -D -C -U -L -P -F <Backup File Name> --timestamp
    ```

    </tab>
    </tabs>
6. Restart the TeamCity server.

## Backup File Location and Format

The default directory for backup files is the `<TeamCity Data Directory>/backup`.

<note>

If not specified otherwise with the `-A` option, TeamCity will read the TeamCity Data Directory path from the `TEAMCITY_DATA_PATH` environment variable, or the default path (`$HOME/.Buildserver`) will be used.
</note>

The default format of the backup file name is `TeamCity_Backup_<timestamp>.zip`. The `<timestamp>` suffix is added in the `YYYYMMDD_HHMMSS` format.

<anchor name="Performing+TeamCity+Data+Backup+with+maintainDB+Utility"/>

## maintainDB Command Options

This section describes some of the `maintainDB` options. For the complete list of options, run `maintainDB` from the command line with no parameters.

### Backup Scope Options

<table>
<tr>
<td width="200"><p><b>Option</b></p></td>
<td><p><b>Description</b></p></td>
</tr>

<tr>
<td><p><code>-D</code></p> <p><code>--include-database</code></p></td>
<td><p>Includes database data in backup/restore</p></td>
</tr>

<tr>
<td><p><code>-C</code></p> <p><code>--include-config</code></p></td>
<td><p>Includes server, projects and build configurations settings, and plugins in backup/restore</p></td>
</tr>

<tr>
<td><p><code>-U</code></p> <p><code>--include-supplementary-data</code></p></td>
<td><p>Includes supplementary plugin data in backup/restore</p></td>
</tr>

<tr>
<td><p><code>-L</code></p> <p><code>--include-build-logs</code></p></td>
<td><p>Includes build logs in backup/restore</p></td>
</tr>

<tr>
<td><p><code>-P</code></p> <p><code>--include-personal-changes</code></p></td>
<td><p>Includes changes from personal builds in backup/restore</p></td>
</tr>

<tr>
<td><p><code>--basic</code></p></td>
<td><p>Combines the <code>-D -C -U</code> options</p></td>
</tr>

<tr>
<td><p><code>--all</code></p></td>
<td><p>Combines the <code>-D -C -U -L -P</code> options</p></td>
</tr>

</table>


[//]: # (Internal note. Do not delete. "Creating Backup via maintainDB command-line toold102e196.txt")    

<anchor name="maintainDB+Startup+Options"/>

## JVM Startup Options for maintainDB

If you customize TeamCity server startup options via `TEAMCITY_SERVER_OPTS/TEAMCITY_SERVER_MEM_OPTS` environment variables or use custom JDK installation to run the server, you might need to run `maintainDB` script with related options added into `TEAMCITY_MAINTAINDB_OPTS/TEAMCITY_MAINTAINDB_MEM_OPTS` environment variables and run the script with all the same environment as the TeamCity server, so that the same JVM is used.
 
 <seealso>
        <category ref="installation">
            <a href="set-up-external-database.md">Setting up an External Database</a>
            <a href="migrating-to-external-database.md">Migrating to an External Database</a>
        </category>
</seealso>
