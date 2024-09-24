[//]: # (title: TeamCity Data Backup)
[//]: # (auxiliary-id: TeamCity Data Backup)

The database of your TeamCity Cloud instance is backed up on a daily basis. The backed up files are usually store for 5 days. We constantly monitor active servers and can quickly restore the lost data in case the database gets corrupted due to any error.
{instance="tcc"}

## About Data Backup in TeamCity
{instance="tc"}

TeamCity provides several ways to back up its data:
* [Backup from the Web UI](creating-backup-from-teamcity-web-ui.md): an action in the web UI (can also be triggered via [REST API](https://www.jetbrains.com/help/teamcity/rest/manage-data-backup.html)) to create a backup while the server is running. It is recommended for regular maintenance backups. Some limitations on the backed up data apply (see the [related section](#Backing+up+Data) below). This option is also available on upgrade in the maintenance screen: on the first start of a newer version of the TeamCity server.
* [Backup via maintainDB command-line tool](creating-backup-via-maintaindb-command-line-tool.md): same as via the UI. To include all data, use the tool when the server [is stopped](creating-backup-via-maintaindb-command-line-tool.md#Performing+TeamCity+Data+Backup+with+maintainDB+Utility).
* [Manual backup](manual-backup-and-restore.md): is suitable if you want to manage the backup procedure manually. 
* You may need to [back up the build agent's data](backing-up-build-agent-s-data.md) only.

<note>

We strongly urge you to make the backup of TeamCity data before upgrading. Note that TeamCity server __does not support downgrading__.
</note>

## Backing up Data
{instance="tc"}

You can choose what data is backed up either in the UI or by adding the respective parameters in maintainDB.

TeamCity allows backing up the following data:
* [Data stored in the database](manual-backup-and-restore.md#Database+Data)
* Server settings, settings of projects and builds configurations (everything stored in [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/config`), and secure values
* Custom plugins (installed under [`<TeamCity Data Directory>`](teamcity-data-directory.md)`\plugins`) and database drivers (from [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/lib` directory)
* Supplementary data: settings history, triggers states, plugins data, and so on (everything under the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/system/pluginData` directory)
* Build logs
* Personal changes

The following data is __not included__ into backup:
* __Build artifacts__ (because of their size). These include build artifacts, internal NuGet feed packages, coverage report, finish build parameters, settings digest, and so on. If you need to back up the artifacts, save the contents of [artifacts directories](teamcity-configuration-and-maintenance.md) manually before [restoring TeamCity data from backup](restoring-teamcity-data-from-backup.md).
* For backup from the UI: __running builds and build queue state__. If you want to back up these, stop the TeamCity server and use the maintainDB tool.
* TeamCity application manual customizations under [`<TeamCity Home>`](teamcity-home-directory.md), including used server port number, which are stored in the [`<TeamCity Home>`](teamcity-home-directory.md)`/conf/server.xml` file.
* TeamCity application logs (under [`<TeamCity Home>`](teamcity-home-directory.md)`/logs`).
* Any manually created files under [`<TeamCity Data Directory>`](teamcity-data-directory.md) that do not fall into previously mentioned items.

[//]: # (Internal note. Do not delete. also https://youtrack.jetbrains.com/issue/TW-43056)

__The recommended approach__ is either to perform the [manual backup process](manual-backup-and-restore.md) or run a backup [from the UI](creating-backup-from-teamcity-web-ui.md) regularly (for example, automated via [REST API](https://www.jetbrains.com/help/teamcity/rest/manage-data-backup.html)) with the "Basic" level — this will ensure backing up all important data except build artifacts and build logs.

Build artifacts and logs (if necessary) can be backed up manually by copying files under [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/system/artifacts`.   
If logs are selected for backup, TeamCity will search for them in [all artifact directories](build-artifact.md) currently specified on the server.

Note that for large production TeamCity installations, exporting and importing of data from/to the database may not be an optimal solution and maintaining database backup via replication might be a better option; for example, see the corresponding [documentation](https://dev.mysql.com/doc/refman/8.0/en/replication.html) for MySQL database.

<seealso instance="tc">
        <category ref="installation">
            <a href="upgrading-teamcity-server-and-agents.md">Upgrade</a>
        </category>
</seealso>