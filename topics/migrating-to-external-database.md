[//]: # (title: Migrating to External Database)
[//]: # (auxiliary-id: Migrating to an External Database)

This article covers the steps required to migrate TeamCity data from the database of one type to another. For details on using an external database from the first TeamCity start, as well as the general external database information and database-specific configuration steps, refer to [this article](set-up-external-database.md).

The most typical case for migration is when you evaluated TeamCity with the default internal database and need to switch to an external database to prepare your TeamCity installation for production use. The recommended steps are also applicable when switching from one external database to another. You can also follow them to move between database servers of the same type, but in that case, the database-specific data transfer is regularly more preferable.

<note>

Database migration cannot be combined with the server upgrade. If you want to upgrade at the same time, you should first [upgrade](upgrading-teamcity-server-and-agents.md), run the new version of TeamCity, and then migrate to another database.
</note>

<!--[//]: # (Internal note. Do not delete. "Migrating to an External Databased212e19.txt")-->    

There are several ways to migrate data into a new database:
* [Switch](#Switch+with+No+Data+Migration) with no data migration: build configurations settings will be preserved, but not the historical builds data or users.
* [Full migration](#Full+Migration): all the data is preserved except for any database-stored data provided by the third-party plugins.
* [Backup and then restore](#Backup+and+Restore): the same as full migration, but using the two-step approach.

## Switch with No Data Migration

If you want a fast switch to an external database and _do not want to preserve existing data_ like users and builds on the server, follow the steps below. After the switch, the server will start with an empty database, but preserve all the _settings_ stored under TeamCity Data Directory (see [details](manual-backup-and-restore.md#TeamCity+Data+Directory+Backup) on what is stored where).

1. [Create and configure an external database](set-up-external-database.md) to be used by TeamCity.
2. Shut down the TeamCity server.
3. [Create a backup copy](teamcity-data-backup.md) of the [`<TeamCity Data Directory>`](teamcity-data-directory.md) used by the server.
4. Clean up the `system` directory: you __must__ remove the `messages` and `artifacts` directories from the `system` directory of your [`<TeamCity Data Directory>`](teamcity-data-directory.md); you __may__ delete the old HSQLDB files: `buildserver.*` to remove the no longer needed internal storage data.
5. Start the TeamCity server.

>If you see the __TeamCity Maintenance__ screen, click _"I'm a server administrator, show me the details"_ and enter the [Super User Token](super-user.md). Follow the instructions to create a new TeamCity database.

## Full Migration

These steps describe switching to another database while preserving all the data. This is done by the TeamCity migration tool — `maintainDB` command-line utility.

The `maintainDB.[cmd|sh]` shell/batch script is located in the [`<TeamCity Home  Directory>`](teamcity-home-directory.md)`/bin` directory and is used for migrating as well as for [backing up](creating-backup-via-maintaindb-command-line-tool.md) and [restoring](restoring-teamcity-data-from-backup.md) TeamCity data. The utility is only available in the TeamCity `.tar.gz` and .`exe` distributions.

TeamCity supports __HSQLDB__, __MySQL__, __Oracle__, __PostgreSQL__, and __Microsoft SQL Server__; the migration is possible between any of these databases.

The target database must be empty before the migration process (it must NOT contain any tables).

<note>

If an error occurs during migration, do not use the new database as it may result in the database data corruption or various errors that can uncover later. In case of an error, investigate the reason logged into the console or in the migration logs (see below), and, if a solution is found, clean the target database and repeat the migration process.
</note>

To migrate all your existing data to a new external database:

1. [Create and configure an external database](set-up-external-database.md) to be used by TeamCity and install the database driver into TeamCity. __Do not modify any TeamCity settings at this stage__.

2. Shut down the TeamCity server.

3. Create a temporary properties file with a custom name (for example, `database.<database_type>.properties`) for the target database using the corresponding template ([`<TeamCity Data Directory>`](teamcity-data-directory.md)`/config/database.<database_type>.properties.dist`). Configure the properties and place the file into any temporary directory. __Do not modify the original `database.<database_type>.properties` file__.

4. Run the `maintainDB` tool with the `migrate` command and specify the absolute path to the newly created target database properties file with the `-T` option:

     ```Shell
     maintainDB.[cmd|sh] migrate -T <path to database.properties file>
     ```

    If you don't have the `TEAMCITY_DATA_PATH` environment that points to the [TeamCity Data Directory](teamcity-data-directory.md), add the `-A` parameter to the command call:

     ```Shell
     maintainDB.[cmd|sh] migrate -A <path to TeamCity Data Directory> -T <path to database.properties file>
     ```


    Upon the successful completion of the database migration, the temporary file will be copied to the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/config/database.properties` file which will be used by TeamCity. The temporary file can be safely deleted. If you are migrating between external databases, the original `database.properties` file for the source database will be replaced with the file specified via the `-T` option. The original `database.properties` file will be automatically renamed to `database.properties.before.<timestamp>`.



5. Start the TeamCity server. This must be the same TeamCity version that was run last time (TeamCity [upgrade](upgrading-teamcity-server-and-agents.md) must be performed as a separate procedure).

    After you make sure the migration succeeded, you __may__ delete the old HSQLDB files: `buildserver.*` to remove the no longer needed internal storage data.

<anchor name="backup_restore"/>

## Backup and Restore
[//]: # (AltHead: backup_restore)

You can [create a backup](teamcity-data-backup.md) and then [restore it](restoring-teamcity-data-from-backup.md) using different target database settings. You will probably need to specify restore options to restore only the database data.

## Troubleshooting

* Extended information during migration execution is logged into the `logs\teamcity-maintenance.log` file. Also, `logs\teamcity-maintenance-truncation.log` contains extended information on possible data truncation during the migration process.
* If you encounter an "_Out of memory_" error, try increasing the number in the `-Xmx512m` parameter in the `maintainDB` script. On a 32-bit platform, the maximum is about 1300 MB.    
    Alternatively, run HSQLDB in the standalone mode via

    ```Java
    java -Xmx256M -cp ..\webapps\ROOT\WEB-INF\lib\hsqldb.jar org.hsqldb.Server -database.0
    <TeamCity Data Directory>\system\buildserver -dbname.0 buildserver
    ```

    and then run the migration tool pointing to the database as the source: `jdbc:hsqldb:hsql://localhost/buildserver sa ''`

* If you get "_The input line is too long_" error while running the tool, change the script to use an alternative classpath method.  
    For `maintainDB.bat`, remove the lines below the "_Add all JARs from WEB-INF\lib to classpath_" comment and uncomment the lines below the "_Alternative classpath: Add only necessary JARs_" comment.

<seealso>
        <category ref="installation">
            <a href="common-problems.md">Common database-related problems</a>
            <a href="set-up-external-database.md">Setting up an External Database</a>
        </category>
        <category ref="concepts">
            <a href="teamcity-data-directory.md">TeamCity Data Directory</a>
        </category>
        <category ref="admin-guide">
            <a href="teamcity-data-backup.md">TeamCity Data Backup</a>
        </category>
</seealso>