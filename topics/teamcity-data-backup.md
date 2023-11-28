[//]: # (title: TeamCity Data Backup)
[//]: # (auxiliary-id: TeamCity Data Backup)

The database of your TeamCity Cloud instance is backed up on a daily basis. The backed up files are usually store for 5 days. We constantly monitor active servers and can quickly restore the lost data in case the database gets corrupted due to any error.
{product="tcc"}

<anchor name="About+Data+Backup+in+TeamCity"/>

## Backup Alternatives
{product="tc"}

TeamCity provides the following alternatives for backing up data:

<table>
<tr>
<td><p><b>Backup method</b></p></td>
<td><p><b>Advantages/disadvantages</b></p></td>
</tr>

<tr>
<td><p><a href="creating-backup-from-teamcity-web-ui.md"><i>Web UI</i></a></p></td>
<td><p>
<list>
<li><p>Zero downtime</p></li>
<li><p>Database agnostic</p></li>
<li><p>Can be triggered by <a href="https://www.jetbrains.com/help/teamcity/rest/manage-data-backup.html">REST API</a></p></li>
<li><p>Does not include running builds and build queue state</p></li>
<li><p>Does not include all types of data</p></li>
</list>
</p></td>
</tr>

<tr>
<td><p><a href="creating-backup-via-maintaindb-command-line-tool.md"><i>maintainDB tool</i></a></p></td>
<td><p>
<list>
<li><p>Main use case requires stopping TeamCity server</p></li>
<li><p>Database agnostic</p></li>
<li><p>Includes running builds and build queue state (when server is stopped)</p></li>
<li><p>Does not include all types of data</p></li>
</list>
</p></td>
</tr>

<tr>
<td><p><a href="manual-backup-and-restore.md"><i>Manual</i></a></p></td>
<td><p>
<list>
<li><p>Requires stopping TeamCity server</p></li>
<li><p>Database backup with database-specific tools</p></li>
<li><p>Includes running builds and build queue state</p></li>
<li><p>Can include all types of data</p></li>
</list>
</p></td>
</tr>

</table>

__The recommended approach__ is either to perform the [manual backup process](manual-backup-and-restore.md) or run a backup [from the UI](creating-backup-from-teamcity-web-ui.md) regularly (for example, automated via [REST API](https://www.jetbrains.com/help/teamcity/rest/manage-data-backup.html)) with the "Basic" level — this will ensure backing up all important data except build artifacts and build logs.

> If you only need to back up data from a build agent, see the procedure for [](backing-up-build-agent-s-data.md).
>
{type="tip"}

<note>

We strongly urge you to make the backup of TeamCity data before upgrading. Note that TeamCity server __does not support downgrading__.
</note>

<anchor name="Backing+up+Data"/>

## What Data is Backed Up
{product="tc"}

Depending on the chosen backup method, the following data can be backed up:

<table>
<tr>
<td width="280"><p><b>Type of Data</b></p></td>
<td><p><b>Web UI</b></p></td>
<td><p><b>maintainDB</b></p></td>
<td><p><b>Manual</b></p></td>
</tr>

<tr>
<td><p>Database</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
</tr>

<tr>
<td><p>Server settings, project settings, and build settings</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
</tr>

<tr>
<td><p>Supplementary data</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
</tr>

<tr>
<td><p>Build logs</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
</tr>

<tr>
<td><p>Personal builds</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
</tr>

<tr>
<td><p>Custom plugins</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
</tr>

<tr>
<td><p>Database drivers</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
</tr>

<tr>
<td><p>Running builds and build queue state</p></td>
<td><p><b>No</b></p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
</tr>

<tr>
<td><p>Build artifacts</p></td>
<td><p><b>No</b></p></td>
<td><p><b>No</b></p></td>
<td><p>Yes</p></td>
</tr>

<tr>
<td><p>TeamCity server logs</p></td>
<td><p><b>No</b></p></td>
<td><p><b>No</b></p></td>
<td><p>Yes</p></td>
</tr>

<tr>
<td><p>Manual customizations under <code>&lt;TeamCity Home&gt;</code></p></td>
<td><p><b>No</b></p></td>
<td><p><b>No</b></p></td>
<td><p>Yes</p></td>
</tr>

<tr>
<td><p>Manually created files under <code>&lt;TeamCity Data Directory&gt;</code></p></td>
<td><p><b>No</b></p></td>
<td><p><b>No</b></p></td>
<td><p>Yes</p></td>
</tr>

</table>

The types of data from the preceding table are, as follows:

* Database — [data stored in the database](manual-backup-and-restore.md#Database+Data). In the case of Web UI or maintainDB backup, the data is extracted in a database-agnostic format, which makes it possible to restore the data to a different database if required (database migration). On the other hand, for enterprise production systems, exporting data from the database might not be an efficient solution and data replication using database-specific tools is probably more performant. For example, see the corresponding [documentation](https://dev.mysql.com/doc/refman/8.0/en/replication.html) for MySQL database.

* Server settings, project settings, and build settings — includes everything under `<[TeamCity Data Directory](teamcity-data-directory.md)>/config`), including secure values.
* Supplementary data — settings history, triggers states, plugins data, and everything under the `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/pluginData` directory.
* Build logs — are usually stored together with build artifacts (by default, under `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/artifacts`).
   > If build logs are selected for backup, TeamCity will search for them in [all artifact directories](build-artifact.md) currently specified on the server.
   >
   {type="note"}
* [Personal builds](personal-build.md) — the history of VCS changes (commits) related to personal builds.
* Custom plugins — files under `<[TeamCity Data Directory](teamcity-data-directory.md)>/plugins`.
* Database drivers — files under `<[TeamCity Data Directory](teamcity-data-directory.md)>/lib`.
* Running builds and build queue state — requires the TeamCity server to be stopped. You can back up this type of data using the maintainDB tool.

* Build artifacts — are not normally included in the TeamCity backup. Because of the large data volumes involved, it usually makes sense to implement a dedicated artifacts storage strategy, for example:
   * By customizing the directory for [artifacts storage](teamcity-configuration-and-maintenance.md#artifact-directories), configuring it to use the mount point of a redundant storage medium.
   * By configuring [external artifacts storage](configuring-artifacts-storage.md#external-artifacts-storage) to store data in the cloud.
   > Build artifacts and logs can (if necessary) be backed up manually by copying files under `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/artifacts` (default location).
   >
   {type="tip"}

* [TeamCity server logs](teamcity-server-logs.md) — files under `<[TeamCity Home](teamcity-home-directory.md)>/logs`.
* Manual customizations under `<[TeamCity Home](teamcity-home-directory.md)>` — including the server port number, which is configured in the `<[TeamCity Home](teamcity-home-directory.md)>/conf/server.xml` file.
* Manually created files under `<[TeamCity Data Directory](teamcity-data-directory.md)>` — any files under `<TeamCity Data Directory>` that have not already been mentioned.

[//]: # (Internal note. Do not delete. also https://youtrack.jetbrains.com/issue/TW-43056)

<seealso product="tc">
        <category ref="installation">
            <a href="upgrading-teamcity-server-and-agents.md">Upgrade</a>
        </category>
</seealso>