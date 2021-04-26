[//]: # (title: Creating Backup via maintainDB command-line tool)
[//]: # (auxiliary-id: Creating Backup via maintainDB command-line tool)

TeamCity [.tar.gz and .exe distributions](installing-and-configuring-the-teamcity-server.md) provide the `maintainDB.bat|sh` utility located in the `<[TeamCity Home](teamcity-home-directory.md)>/bin` directory. This command-line tool enables you to back up the server data, [restore it](restoring-teamcity-data-from-backup.md), and [migrate between different databases](migrating-to-an-external-database.md). You can also create data backup using the [web UI](creating-backup-from-teamcity-web-ui.md).

## Before You Backup

Before backing up data, it is recommended to shut down the TeamCity server to include all builds into the backup. If the backup process is started when the TeamCity server is up, running and queued builds are not included into the backup.

## Backup File Location and Format

The default directory for backup files is the `<TeamCity Data Directory>\backup`.

<note>

If not specified otherwise with the `-A` option, TeamCity will read the TeamCity Data Directory path from the `TEAMCITY_DATA_PATH` environment variable, or the default path (`$HOME\.Buildserver`) will be used.
</note>

The default format of the backup file name is `TeamCity_Backup_<timestamp>.zip`; the `<timestamp>` suffix is added in the `YYYYMMDD_HHMMSS` format.

## Performing TeamCity Data Backup with maintainDB Utility

This section describes some of the `maintainDB` options. For a complete list of all available options, run `maintainDB` from the command line with no parameters.

__To create a data backup file__, from the command line start `maintainDB` utility with the `backup` command:

```Plain Text
maintainDB.[cmd|sh] backup
```

TeamCity data backup has [some limitations](teamcity-data-backup.md#Backing+up+Data). By default, if you run `maintainDB` utility with no optional parameters, only the database, server settings, projects and builds configurations, plugins and supplementary data (settings history, triggers states, plugins data, and so on) will be backed up, omitting build logs and personal changes.

### Configuring backup scope

To configure the scope of data to include in the backup file, use the following options:
* `-C` or `--include-config` — includes build configurations settings
* `-D` or `--include-database` — includes database
* `-L` or `--include-build-logs` — includes build logs
* `-P` or `--include-personal-changes` — includes personal changes
* `-U` or `--include-supplementary-data` — includes supplementary (plugins') data

Specifying different combinations of the above options, you can control the content of the backup file. For example, to create backup with all supported types of data, run

```Plain Text
maintainDB backup -C -D -L -P -U
```

[//]: # (Internal note. Do not delete. "Creating Backup via maintainDB command-line toold102e196.txt")    

### maintainDB Usage Examples for Data Backup

__To create a backup file with a custom name__, run maintainDB with `-F` or `--backup-file` option and specify desired backup file name without extension:

```Plain Text
maintainDB.cmd backup -F <backup file custom name>
or
maintainDB.cmd backup --backup-file <backup file custom name>

```

Executing the command above will create a new ZIP file with the specified name in the default backup directory.

__To add a timestamp suffix to a custom filename__, add `-M` or `--timestamp` option:

```Plain Text
maintainDB.cmd backup -F <backup file custom name> -M
or
maintainDB.cmd backup -F <backup file custom name> --timestamp

```

__To create a backup file in a custom directory__, run maintainDB with the `-F` option:

```Plain Text
maintainDB backup -F <absolute path to the custom backup directory>
or
maintainDB backup --data-dir <absolute path to the custom backup directory>
```

## maintainDB Startup Options

If you customize TeamCity server startup options via `TEAMCITY_SERVER_OPTS/TEAMCITY_SERVER_MEM_OPTS` environment variables or use custom JDK installation to run the server, you might need to run `maintainDB` script with related options added into `TEAMCITY_MAINTAINDB_OPTS/TEAMCITY_MAINTAINDB_MEM_OPTS` environment variables and run the script with all the same environment as the TeamCity server, so that the same JVM is used.
 
 <seealso>
        <category ref="installation">
            <a href="setting-up-an-external-database.md">Setting up an External Database</a>
            <a href="migrating-to-an-external-database.md">Migrating to an External Database</a>
        </category>
</seealso>
