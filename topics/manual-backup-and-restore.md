[//]: # (title: Manual Backup and Restore)
[//]: # (auxiliary-id: Manual Backup and Restore)

You can use the instructions on this page if you want fine-grained control over the backup process or need to use a specific procedure for your TeamCity backups. Manual backups can also be the most performant way to create and restore backups as they can be tailored to the specific infrastructure in use.

<anchor name="Server+Manual+Backup"/>

## Manual Backup of TeamCity Server

### Manual Backup Procedure

To back up TeamCity server data manually:

1. Before proceeding, review the [alternative backup procedures](teamcity-data-backup.md#Backup+Alternatives) (particularly with respect to the [scope of the backed-up data](teamcity-data-backup.md#What+Data+is+Backed+Up)), to ensure that the manual backup procedure meets your backup needs.
2. Stop the TeamCity server.
   > If you perform a manual backup while the TeamCity server is running, currently updated data (mostly related to queued and running builds as well as settings updates) can appear in an inconsistent state after restore.
   >
   {type="warning"}
3. If TeamCity uses an [external database](set-up-external-database.md) (recommended for production systems), back up the TeamCity database schema using database-specific tools.
   > To check what kind of database TeamCity is using, see the `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/database.properties` file.
   >
   {type="tip"}
   > If TeamCity uses the (default) HSQLDB internal database, it stores the HSQLDB data in the `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/buildserver.*` files.
   >
   {type="note"}
4. Review your artifacts backup strategy. In a production system, build artifacts are often stored in the cloud by configuring [external artifacts storage](configuring-artifacts-storage.md#external-artifacts-storage). If this is the case, you can use the backup facilities from your cloud provider to create a backup of the artifacts storage.
   > By default, TeamCity stores build artifacts and build logs under `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/artifacts`.
   >
   {type="tip"}
6. Back up the `<[TeamCity Data Directory](teamcity-data-directory.md)>` using a file archiving tool (Zip or similar) and copy the resulting archive file to a safe and secure location.
   > Some of the contents of the `<[TeamCity Data Directory](teamcity-data-directory.md)>` are not strictly needed for backup. If you want to reduce the size of this file, see [](#Reducing+Backup+Size).
   >
   {type="tip"}
7. (Optional) In some cases, you might need to back up additional files (for example, custom files under `[<TeamCity Home>](teamcity-home-directory.md)`). For details, see [](#Optional+Backup+Data).
8. Restart the TeamCity server.


### Optional Backup Data

The following data might need to be backed up in some cases:

<dl>

<dt><p>Application Files</p></dt>
<dd><p>You do not need to back up the TeamCity application directory (web server along with the web application), provided you still have the original distribution package and you did not:</p>
<list>
<li><p>place any custom libraries for TeamCity to use</p></li>
<li><p>install any non-default TeamCity plugins directly into web application files</p></li>
<li><p>make any startup script/configuration changes</p></li>
</list>
<p>If you feel you need to back up the application files:</p>
<list>
<li><p>If you use a <i>non-war</i> distribution: back up <b>everything</b> under <code>&lt;TeamCity Home&gt;</code> except for the <code>temp</code> and <code>work</code> directories.</p></li>
<li><p>If you use the <i>war</i> distribution, follow the backup procedure of the servlet container used.</p></li>
</list>
</dd>

<dt><p>Log Files</p>
</dt>
<dd><p>If you need <a href="teamcity-server-logs.md">TeamCity server log files</a> (which are mainly used for problem-solving or debug purposes), back up the  <code>&lt;TeamCity Home&gt;/logs</code> directory.</p>
<note><p>You may also want to back up TeamCity Windows Service settings, if they were modified.</p>
</note>
</dd>

</dl>


<anchor name="TeamCity+Data+Directory+Backup"/>

### Reducing Backup Size

To reduce the size of the backup, you can exclude certain parts of the `<[TeamCity Data Directory](teamcity-data-directory.md)>`, as follows:

* `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/caches` directory — you can safely exclude the `system/caches` directory from the backup, because the cache gets rebuilt on TeamCity startup.

[//]: # (Internal note. Do not delete. "Manual Backup and Restored203e71.txt")


## Manual Restoration of Server Backup

If you need to restore backup created with the web UI or `maintainDB` utility, please refer to [Restoring TeamCity Data from Backup](restoring-teamcity-data-from-backup.md). This section describes restoration of a manually created backup.

You should always restore both the data in the `<[TeamCity Data Directory](teamcity-data-directory.md)>` and data in the database. Both the database and the directory should be backed up/restored in sync.

### TeamCity Data Directory Restoration

You can simply put the previously backed up files back to their original places. However, it is important that no extra files are present when restoring the backup. The simplest way to achieve this is to restore the backup into a clean Data Directory. If this is not possible, make sure the files created after the backup was done are cleared. Especially the newly created files under the `artifacts`, `messages`, `changes` directories under `<[TeamCity Data Directory](teamcity-data-directory.md)>/system`.

### TeamCity Database Restoration

The database must be restored using the database-specific tools and methods. You might also want to use database-specific methods to make the restore faster (like setting SQL Server "Recovery Model" to "Simple").

Prior to restoring, make sure there are no extra tables in the schema used by TeamCity.

### Restoration to New Server

If you want to run a copy of the server, make sure the servers use distinct data directories and databases. For an external database, make sure you modify settings in the `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/database.properties` file to point to a different database.

### Restoring Build Logs

Build logs located in the `/logs` subdirectory of [`/artifacts`](teamcity-data-directory.md#artifacts) from [multiple artifact directories](build-artifact.md) are backed up and restored into the single directory `system/<project ID>/<build configuration name>/<internal_build_id>/.teamcity/logs`.
