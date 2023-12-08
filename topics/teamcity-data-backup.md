[//]: # (title: TeamCity Data Backup)
[//]: # (auxiliary-id: TeamCity Data Backup)

The database of your TeamCity Cloud instance is backed up on a daily basis. The backed up files are usually store for 5 days. We constantly monitor active servers and can quickly restore the lost data in case the database gets corrupted due to any error.
{product="tcc"}

<anchor name="About+Data+Backup+in+TeamCity"/>

## Backup Alternatives
{product="tc"}

TeamCity provides the following alternatives for backing up data:

<table header-style="none">

<tr>
<td><p><a href="creating-backup-from-teamcity-web-ui.md"><i>Web UI</i></a></p></td>
<td><p>
<list>
<li><p>Zero downtime</p></li>
<li><p>Database agnostic</p></li>
<li><p>Can be triggered by <a href="https://www.jetbrains.com/help/teamcity/rest/manage-data-backup.html">REST API</a></p></li>
<li><p>Does not include running builds and build queue state</p></li>
<li><p>Does not include manual customizations</p></li>
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
<li><p>Does not include manual customizations</p></li>
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
<li><p>Can include manual customizations</p></li>
</list>
</p></td>
</tr>

</table>

For example, one possible backup strategy would be to perform a full manual backup on a regular schedule, complemented by more frequent Basic level Web UI backups (which could be automated using the TeamCity REST API).

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
<td><p>Server settings, project settings, and build settings (<b>secure values</b>)</p></td>
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

### Types of Data

The types of data from the preceding table are, as follows:

<anchor name="database_data"/>

* _Database_ — The database stores all information on the build results (build history and all the build-associated data except for artifacts and build logs), VCS changes, agents, build queue, user accounts and user permissions, and so on.

  In the case of Web UI or maintainDB backup, the data is extracted in a database-agnostic format, which makes it possible to restore the data to a different database if required (database migration). On the other hand, for enterprise production systems, exporting data from the database might not be an efficient solution and data replication using database-specific tools is probably more performant. For example, see the corresponding [documentation](https://dev.mysql.com/doc/refman/8.0/en/replication.html) for MySQL database.

* _Server settings, project settings, and build settings_ — includes everything under `<[TeamCity Data Directory](teamcity-data-directory.md)>/config`), including secure values.
   > Secure values under this directory include data such as authentication tokens, SSH keys, and passwords.
   >
   {type="warning"}
* _Supplementary data_ — settings history, triggers states, plugins data, and everything under `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/pluginData`.
* _Build logs_ — are usually stored together with build artifacts (by default, under `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/artifacts`).
   > If build logs are selected for backup, TeamCity will search for them in [all artifact directories](build-artifact.md) currently specified on the server.
   >
   {type="note"}
* _[Personal builds](personal-build.md)_ — the history of VCS changes (commits) related to personal builds.
* _Custom plugins_ — files under `<[TeamCity Data Directory](teamcity-data-directory.md)>/plugins`.
* _Database drivers_ — files under `<[TeamCity Data Directory](teamcity-data-directory.md)>/lib`.
* _Running builds and build queue state_ — requires the TeamCity server to be stopped. You can back up this type of data using the maintainDB tool.

* _Build artifacts_ — are not normally included in the TeamCity backup. Because of the large data volumes involved, it usually makes sense to implement a dedicated artifacts storage strategy, for example:
   * By customizing the directory for [artifacts storage](teamcity-configuration-and-maintenance.md#artifact-directories), configuring it to use the mount point of a redundant storage medium.
   * By configuring [external artifacts storage](configuring-artifacts-storage.md#external-artifacts-storage) to store data in the cloud.
   > Build artifacts and logs can (if necessary) be backed up manually by copying files under `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/artifacts` (default location).
   >
   {type="tip"}

* _[TeamCity server logs](teamcity-server-logs.md)_ — files under `<[TeamCity Home](teamcity-home-directory.md)>/logs`.
* _Manual customizations under `<[TeamCity Home](teamcity-home-directory.md)>`_ — including the server port number, which is configured in the `<[TeamCity Home](teamcity-home-directory.md)>/conf/server.xml` file.
* _Manually created files under `<[TeamCity Data Directory](teamcity-data-directory.md)>`_ — any files under `<TeamCity Data Directory>` that have not already been mentioned.

[//]: # (Internal note. Do not delete. also https://youtrack.jetbrains.com/issue/TW-43056)

<seealso product="tc">
        <category ref="installation">
            <a href="upgrading-teamcity-server-and-agents.md">Upgrade</a>
        </category>
</seealso>