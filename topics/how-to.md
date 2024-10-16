[//]: # (title: How To...)
[//]: # (auxiliary-id: viewpage.actionpageId113084582;How To...)

## Install Non-Bundled Version of Java
{instance="tc"}

TeamCity Server requires a Java SE JRE installation to run. A compatible JRE version is bundled in the TeamCity `.exe` installer but needs to be installed separately when using other distributions.

TeamCity selects the Java version to run the server process as follows:
* By default, if your TeamCity installation has a bundled JRE (the `<[TeamCity Home](teamcity-home-directory.md)>\jre` directory exists), it will be used to run the TeamCity server process. To use a different JRE, specify its path via the `TEAMCITY_JRE` environment variable.
* If there is no `<[TeamCity Home](teamcity-home-directory.md)>\jre` directory present, TeamCity looks for the `JRE_HOME` or `JAVA_HOME` environment variable pointing to the installation directory of JRE or JVM (Java SDK) respectively. If both variables are declared, JRE will be used.

The necessary steps to update the Java installation depend on the distribution used.

If your TeamCity installation has a bundled JRE (there is the `<[TeamCity Home](teamcity-home-directory.md)>\jre` directory), update it by installing a newer JRE per installation instructions and copying the content of the resulting directory to replace the content of the existing `<[TeamCity Home](teamcity-home-directory.md)>\jre` directory.   
If you also run a TeamCity agent from the `<[TeamCity Home](teamcity-home-directory.md)>\buildAgent` directory, install JDK (Java SDK) installation instead of JRE and copy the content of the JDK installation directory into `<[TeamCity Home](teamcity-home-directory.md)>\jre`.

<!--[//]: # (Internal note. Do not delete. "Installing and Configuring the TeamCity Serverd172e906.txt")-->

>If you use a different Java version, specified via an environment variable (`TEAMCITY_JRE`, `JRE_HOME`, or `JAVA_HOME`), make sure it is available for the process launching the TeamCity server (it is recommended to set a global OS environment variable). The variable should point to the home directory of the installed JRE or JVM (Java SDK) respectively.

## Update from 32-bit to 64-bit Java
{instance="tc"}

TeamCity server is bundled with the 64-bit JVM but can run under both 32- and 64-bit versions.

If you need to update 32-bit Java to 64-bit JVM, note that the memory usage is almost doubled when switching from 32- to 64-bit. Make sure to specify at least twice as much memory as for 32-bit JVM. Read how to [change memory settings](configure-server-installation.md#Configure+Memory+Settings+for+TeamCity+Server).

To update to 64-bit Java, either use the bundled version of Java or:
1. Update Java to be used by the server.
2. [Set JVM memory options](server-startup-properties.md). It is recommended to set the `-Xmx4g` option for 64-bit JVM.

## Install Non-Bundled Version of Tomcat
{instance="tc"}

The TeamCity Server distributions include a Tomcat version tested to work fine with the current version. You can use an alternative Tomcat version, but note that other combinations are not guaranteed to work correctly.

To use another version of the Tomcat web server instead of the bundled one, you need to perform the Tomcat upgrade/patch.

When upgrading the version of Tomcat to be used by TeamCity, we suggest that you:
* Back up the current [TeamCity Home](teamcity-home-directory.md).
* Delete/move from the TeamCity Home directories that are also present in the Tomcat distribution.
* Unpack the Tomcat distribution to the TeamCity Home directory.
* Copy TeamCity-specific files from the previously backed-up/moved directories to the TeamCity Home:
    * files under `bin` which are not present in the Tomcat distribution
    * review differences between the default Tomcat `conf` directory and one from TeamCity, update Tomcat files with TeamCity-specific settings (`teamcity-*` files, and portions of `server.xml`)
    * delete the default Tomcat `webapps/ROOT` directory and replace it with the one provided by TeamCity

## Autostart TeamCity Server on macOS
{instance="tc"}

Starting up the TeamCity server on macOS is quite similar to starting Tomcat on macOS.
1. Install TeamCity and make sure it works if started from the command line with `bin/teamcity-server.sh start`. This instruction assumes that TeamCity is installed to `/Library/TeamCity`.
2. Create the `/Library/LaunchDaemons/jetbrains.teamcity.server.plist` file with the following content:
    ```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "https://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
        <key>WorkingDirectory</key>
        <string>/Library/TeamCity</string>
        <key>Debug</key>
        <false/>
        <key>Label</key>
        <string>jetbrains.teamcity.server</string>
        <key>OnDemand</key>
        <false/>
        <key>KeepAlive</key>
        <true/>
        <key>ProgramArguments</key>
        <array>
            <string>/bin/bash</string>
            <string>--login</string>
            <string>-c</string>
            <string>bin/teamcity-server.sh run</string>
        </array>
        <key>RunAtLoad</key>
        <true/>
        <key>StandardErrorPath</key>
        <string>logs/launchd.err.log</string>
        <key>StandardOutPath</key>
        <string>logs/launchd.out.log</string>
    </dict>
    </plist>
    
    ```
3. Test your file by running:
    ```Shell
    launchctl load /Library/LaunchDaemons/jetbrains.teamcity.server.plist
    
    ```
   This command should start the TeamCity server (you can see this from `logs/teamcity-server.log` and in your browser).
4. If you don't want TeamCity to start under the root permissions, specify the `UserName` key in the `.plist` file, for example:
    ```XML
    <key>UserName</key>
    <string>teamcity_user</string>
    
    ```
The TeamCity server will now start automatically when the machine starts. To configure automatic start of a TeamCity build agent, see the [dedicated section](start-teamcity-agent.md#Automatic+Start).

## Automate TeamCity Server Installation
{instance="tc"}

For an automated TeamCity server installation, use the `.tar.gz` distribution.

Typically, you will need to unpack it and make the script perform the steps noted in [this section](configure-server-installation.md#Configuring+Server+for+Production+Use).

If you want to get a preconfigured server right away, put the files from a previously configured server into the [Data Directory](teamcity-data-directory.md). For each new server, you will need to:
* ensure it points to a new database (configured in `<Data Directory>\config\database.properties`);
* change the `<Data Directory>\config\main-config.xml` file not to have the `uuid` attribute in the root XML element (so the new one can be generated); set the appropriate value for the `rootURL` attribute.

<anchor name="HowTo-RetrieveAdministratorPassword"/>

## Retrieve Administrator Password
{instance="tc"}

On the first start with the empty database, TeamCity displays the Administrator Setup page which allows creating a user with full administrative permissions (assigning the [System Administrator](managing-roles-and-permissions.md#Per-Project+Authorization+Mode) role).

If you want to regain access to the system and you cannot log in as a user with the System Administrator role, you can log in as a [super user](super-user.md) and change the existing administrator account password or create a new account with the System Administrator role.

It is also possible to use [REST API](https://www.jetbrains.com/help/teamcity/rest/manage-users.html#Manage+User+Roles) to add the System Administrator role to any existing user.

If you use built-in authentication and have correct email specified, you can [reset the password](configuring-your-user-profile.md#Changing+Your+Password) from the login page.

## Setup TeamCity in Replication/Clustering Environment
{instance="tc"}

It is possible to add a [secondary TeamCity node](multinode-setup.md) to ensure high availability and offload some operations from the main server. All nodes need to be connected to the same [`TeamCity Data Directory`](teamcity-data-directory.md) and the database.

To address fast disaster recovery scenarios, TeamCity supports active - failover (cold standby) approach: the data that the TeamCity server uses can be replicated and a solution put in place to start a new server using the same data if the currently active server malfunctions.

As to the data, the TeamCity server uses both database and file storage (Data Directory). You can browse through [TeamCity Data Backup](teamcity-data-backup.md) and [TeamCity Data Directory](teamcity-data-directory.md) pages in to get more information on TeamCity data storing. Basically, both the TeamCity Data Directory on the disk and the database which TeamCity uses must remain in a consistent state and thus must be replicated together.   
Only a single TeamCity server instance should use the database and Data Directory at any time.

Ensure that the distribution of the TeamCity failover/backup server is of exactly the same version as the main server. It is also important to ensure the same server environment/startup options like memory settings, and so on.

TeamCity agents farm can be reused between the main and the failover servers. Agents will automatically connect to the new server if you make the failover server to be resolved via the old server DNS name and agents connect to the server using the DNS name. See also information on [switching](#Switching+from+one+server+to+another) from one server to another.   
If appropriate, the agents can be replicated just as the server. However, there is no need to replicate any TeamCity-specific data on the agents except for the conf\buildAgent.properties file as all the rest of the data can typically be renewed from the server. In case of replicated agents farm, the replica agents just need to be connected to the failover server.

In case of two servers installations for redundancy purposes, they can use the same set of licenses as only one of them is running at any given moment.

## Estimate Number of Required Build Agents

The number of required build agents depends on the server usage pattern, type of builds, team size, commitment of the team to CI process, and so on. In general, the best way is to start with three agents and see how they handle the projects on your server, and then make estimations for the future.

You might want to increase the number of agents when you see:
* builds waiting for an idle agent in the build queue;
* more changes included into each build than you find comfortable (for example, for build failures analysis);
* necessity for different environments.

We've seen patterns of having an agent per each 20 build configurations (types of builds). Or a build agent per 1-2 developers.

<!--## TeamCity Security Notes

The contents of this section have been moved to the [dedicated article](security-notes.md).-->

<anchor name="HowTo...-ConfigureNewlyInstalledMySQLServer"/>

<anchor name="HowTo-ConfigureNewlyInstalledMySQLServer"/>

## Configure Newly Installed MySQL Server
{instance="tc"}

If MySQL server is going to be used with TeamCity in addition to the [basic setup](set-up-external-database.md#MySQL), you should review and probably change some of the MySQL server settings. If MySQL is installed on Windows, the settings are located in `my.ini` file which usually can be found under MySQL installation directory. For Unix-like systems the file is called `my.cnf` and can be placed somewhere under `/etc` directory. Read more about configuration file location in [MySQL documentation](https://dev.mysql.com/doc/refman/8.0/en/option-files.html). Note: you'll need to restart MySQL server after changing settings in `my.ini|my.cnf`.

The following settings should be reviewed and/or changed:

### Table primary keys

TeamCity manages tables that do not have primary keys. For that reason, the MySQL server's [sql_require_primary_key](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_sql_require_primary_key) system variable must be set to `OFF`.

### InnoDB database engine

Make sure you're using InnoDB database engine for tables in TeamCity database. You can check what engine is used with help of this command:

```Shell

show table status like '<table name>';

```

or for all tables at once:

```Shell

show table status like '%';
```

### max_connections

You should ensure `max_connections` parameter has a bigger value than the one specified in TeamCity `<[TeamCity Home Directory](teamcity-home-directory.md)>/config/database.properties` file.

### innodb_buffer_pool_size and innodb_redo_log_capacity

Specifying a too small value in `innodb_buffer_pool_size` may significantly affect performance:

```Shell

# InnoDB, unlike MyISAM, uses a buffer pool to cache both indexes and
# row data. The bigger you set this the less disk I/O is needed to
# access data in tables. On a dedicated database server you may set this
# parameter up to 80% of the machine physical memory size. Do not set it
# too large, though, because competition of the physical memory may
# cause paging in the operating system. Note that on 32bit systems you
# might be limited to 2-3.5G of user level memory per process, so do not
# set it too high.
innodb_buffer_pool_size=2000M

```

We recommend to start with 2Gb and increase it if you experience slowness and have enough memory. After increasing buffer pool size you should also change size of the [`innodb_redo_log_capacity`](https://dev.mysql.com/doc/refman/8.0/en/innodb-parameters.html#sysvar_innodb_redo_log_capacity) setting.

```Shell
innodb_redo_log_capacity=2048M
```

For MySQL versions [prior to 8.0.30](https://dev.mysql.com/doc/refman/8.0/en/innodb-redo-log.html#innodb-redo-log-file-reconfigure), use the `innodb_log_file_size` and `innodb_log_files_in_group` parameters instead of `innodb_redo_log_capacity`. 

```Shell
# innodb_log_files_in_group=2 is set by default
innodb_log_file_size=1024M
```


### innodb_file_per_table

For better performance you can enable the so-called [per-table tablespaces](https://dev.mysql.com/doc/refman/8.0/en/innodb-file-per-table-tablespaces.html). Note that once you add `innodb_file_per_table` option new tables will be created and placed in separate files, but tables created before enabling this option will still be in the shared tablespace. You'll need to reimport database for them to be placed in separate files.

### innodb_flush_log_at_trx_commit

If TeamCity is the only application using MySQL database then you can improve performance by setting `innodb_flush_log_at_trx_commit` variable to `2` or `0`:

```Shell

# If set to 1, InnoDB will flush (fsync) the transaction logs to the
# disk at each commit, which offers full ACID behavior. If you are
# willing to compromise this safety, and you are running small
# transactions, you may set this to 0 or 2 to reduce disk I/O to the
# logs. Value 0 means that the log is only written to the log file and
# the log file flushed to disk approximately once per second. Value 2
# means the log is written to the log file at each commit, but the log
# file is only flushed to disk approximately once per second.
innodb_flush_log_at_trx_commit=2

```

Note: it is not important for TeamCity that database offers full ACID behavior, so you can safely change this variable.

### log files on different disk

Placing the MySQL log files on different disk sometimes helps improve performance. You can read about it in [MySQL documentation](https://dev.mysql.com/doc/refman/8.0/en/innodb-configuration.html).

### Setting The Binary Log Format

If the default MySQL binary logging format is not MIXED (it depends on the [version of MySQL](https://dev.mysql.com/doc/refman/8.0/en/binary-log-setting.html) you are using), then it should be explicitly set to __MIXED__:

```Shell

binlog-format=mixed

```

### Enable additional diagnostics

To get additional diagnostics data in case of some database-specific errors, grant more permissions for a TeamCity database user via SQL command:

```SQL

GRANT PROCESS ON *.* TO <teamcity-user-name>;

```

<anchor name="HowTo-ConfigureNewlyInstalledPostgreSQLServer"/>

## Configure Newly Installed PostgreSQL Server
{instance="tc"}

For better TeamCity server performance, it is recommended to change some of the parameters of the newly installed PostgreSQL server. You can read more about PostgreSQL performance optimization in [PostgreSQL Wiki](https://wiki.postgresql.org/wiki/Performance_Optimization).

The parameters below can be changed in the `postgresql.conf` file located in the in PostgreSQL's data directory.

### shared_buffers

The default value of [https://www.postgresql.org/docs/current/static/runtime-config-resource.html#GUC-SHARED-BUFFERS](https://www.postgresql.org/docs/current/static/runtime-config-resource.html#GUC-SHARED-BUFFERS) parameter is too small and should be increased:


```Shell

shared_buffers=512MB

```

### synchronous_commit

If TeamCity is the only application using the PostgreSQL database, we recommend disabling the [https://www.postgresql.org/docs/current/static/runtime-config-wal.html#GUC-SYNCHRONOUS-COMMIT](https://www.postgresql.org/docs/current/static/runtime-config-wal.html#GUC-SYNCHRONOUS-COMMIT) parameter:

```Shell

synchronous_commit=off

```

<anchor name="checkpoint_segments"/>

### checkpoint-related parameters
[//]: # (AltHead: checkpoint_segments)

For write-intensive applications such as TeamCity, it is recommended to change some of the [checkpoint-related parameters](https://www.postgresql.org/docs/current/static/runtime-config-wal.html#RUNTIME-CONFIG-WAL-CHECKPOINTS):

For __PostgreSQL 9.5 and later__:

```Shell

max_wal_size = 1500MB
checkpoint_completion_target=0.9

```

For versions __prior to PostgreSQL 9.5__:

```Shell

checkpoint_segments=32
checkpoint_completion_target=0.9
```

<anchor name="HowTo-SetUpTeamCitybehindaProxyServer"/>

## Set Up TeamCity behind a Proxy Server
{instance="tc"}

The contents of this section have been moved to the [dedicated article](configuring-proxy-server.md).

## Enforce hiding stacktrace
{instance="tc"}

If a user with access to your TeamCity server submits an invalid cross-site scripting (XSS) payload, the server will display the "Unexpected error" page containing a related stacktrace. To prevent exposing any sensible information about your environment via this stacktrace, you might want to disable its display. For this, set the `teamcity.web.runtimeError.showStacktrace` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) to `false`.

## Configure HTTPS for TeamCity Web UI
{instance="tc"}

TeamCity now allows [configuring HTTPS access](https-server-settings.md) easily. However, for large public facing TeamCity installations, it is still recommended that you set up a reverse proxy like NGINX or Apache in front of TeamCity that would handle HTTPS and use HTTP TeamCity server port as the upstream. 

HTTPS-related configuration of the proxy is not specific for TeamCity and is generic as for any web application. 
Make sure to configure the reverse proxy per [our recommendations](#Set+Up+TeamCity+behind+a+Proxy+Server). 
Generic web application best practices apply (like disabling http access to TeamCity at all).

For small servers, you can set up HTTPS via the internal [Tomcat means](https://tomcat.apache.org/tomcat-8.5-doc/ssl-howto.html), but this is not recommended as it may significantly increase the CPU load.

For configuring clients to access TeamCity server via HTTPS while using self-signed certificate, check the [related instructions](using-https-to-access-teamcity-server.md).

## Configure TeamCity to Use Proxy Server for Outgoing Connections
{instance="tc"}

The contents of this section have been moved to the [dedicated article](configuring-proxy-server.md).

## Configure TeamCity Agent to Use Proxy To Connect to TeamCity Server

This section covers the configuration of a proxy server for TeamCity agent-to-server connections.

<snippet id="agent-proxy-server">

On the TeamCity agent side, specify the proxy to connect to TeamCity server using the following properties in the [`buildAgent.properties`](configure-agent-installation.md) file:


```Shell

## The domain name or the IP address of the proxy host and the port
teamcity.http.proxyHost=123.45.678.9
teamcity.http.proxyPort=8080
 
## If the proxy requires authentication, specify the login and password
teamcity.http.proxyLogin=login
teamcity.http.proxyPassword=password
```

Note that the proxy has to be configured not to cache any TeamCity server responses; for example, if you use Squid, add "cache deny all" line to the `squid.conf` file.

</snippet>

## Install Multiple Agents on the Same Machine

See the [corresponding section](install-multiple-agents-on-one-machine.md) under agent installation documentation.

## Change Server Port
{instance="tc"}

See [corresponding section](configure-server-installation.md#Changing+Server+Port) in server installation instructions.

## Test-drive Newer TeamCity Version before Upgrade
{instance="tc"}

It's advised to try a new TeamCity version before upgrading your production server. The usual procedure is to [create a copy](#Create+a+Copy+of+TeamCity+Server+with+All+Data) of your production TeamCity installation, then [upgrade](upgrading-teamcity-server-and-agents.md) it, try the things out, and, when everything is checked, drop the test server and upgrade the main one. When you start the test server, remember to change the Server URL, disable the email notifier as well as [other features](#Copied+Server+Checklist) on the new server.

## Create a Copy of TeamCity Server with All Data
{instance="tc"}

In case you want to preserve the original server as well as the copy, make sure to check the [licensing considerations](#Licensing+issues).

### Create a Server Copy

You can create a copy of the server using TeamCity backup functionality or manually. Manual approach is recommended for the complete server move.

### Use TeamCity Backup

1. Create a [backup](teamcity-data-backup.md) including everything. (You can skip the option to backup build logs is you are moving the artifacts, see below).
2. Install a new TeamCity server of the same version that you are already running. Ensure that:
   * the appropriate environment is configured (see the notes below) 
   * the server uses its own `<[TeamCity Data Directory](teamcity-data-directory.md)>` and its own [database](set-up-external-database.md) (check the `<[TeamCity Data Directory](teamcity-data-directory.md)>config/database.properties` file)
3. [Restore the backup](restoring-teamcity-data-from-backup.md).
4. Move the artifacts (these are not included into the backup) by moving the content of `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/artifacts` directory from the old location to the new one. Since the artifacts can be large in the size, this can take a lot of time, so plan accordingly.
5. Perform the necessary [environment transfer](#Environment+transferring).

<note>

In order to ensure complete server transfer, it is recommended to copy the entire content of the TeamCity Data Directory from the original to the copied server. It is recommended to complete the copying before starting the new server.
</note>

### Copy Manually

If you do not want to use the bundled backup functionality or need manual control over the process, here is a general instruction on how to manually create a copy of the server:
1. Create a [backup](teamcity-data-backup.md) so that you can restore it if anything goes wrong.
2. Ensure the server is not running.
3. Either perform clean [installation](install-and-start-teamcity-server.md) or copy the TeamCity binaries ([`TeamCity Home Directory`](teamcity-home-directory.md)) into a new place (the `temp` and `work` subdirectories can be omitted during copying). Use exactly the same TeamCity version. If you plan to upgrade after copying, perform the upgrade only after you have the existing version up and running.
4. Copy `<[TeamCity Data Directory](teamcity-data-directory.md)>`. If you do not need the full copy, refer to the items below for options.
    * `.BuildServer/config` to preserve projects and build configurations settings
    * `.BuildServer/lib` and `.BuildServer/plugins` if you have them
    * files from the root of `.BuildServer/system` if you use an internal database and you do not want to move this database
    * `.BuildServer/system/artifacts` (optional) if you want build artifacts and build logs (including tests failure details) to be preserved on the new server
    * `.BuildServer/system/changes` (optional) if you want personal changes to be preserved on the new server
    * `.BuildServer/system/pluginData` (optional) if you want to preserve the state of various plugins, build triggers, and settings audit diff
    * `.BuildServer/system/caches` and `.BuildServer/system/caches` (optional) are not necessary to copy to the new server, they will be recreated on startup, but can take some time to be rebuilt (expect some slow down)
5. Artifacts directory is usually large. If you need to minimize the downtime of the server when moving it, you can use the generic approach for copying the data: use rsync, robocopy or alike tool to copy the data while the original server is running. Repeat the sync run several times until the amount of synced data reduces. Run the final sync after the original server shutdown. Alternative solution for the server move is to make the old data artifacts directory accessible to the new server and configure it as a second [location of artifacts](teamcity-configuration-and-maintenance.md). Then copy the files over from this second location to the main one while the server is running. Restart the server after copying completion.
6. Create a copy of the [database](set-up-external-database.md), which your TeamCity installation is using, in a new schema or new database server. This can be done with database-specific tools or with the bundled maintainDB tool by [backing up](creating-backup-via-maintaindb-command-line-tool.md) database data and then [restoring](restoring-teamcity-data-from-backup.md) it.
7. Configure the new TeamCity installation to use proper `<[TeamCity Data Directory](teamcity-data-directory.md)>` and [database](set-up-external-database.md) (`.BuildServer/config/database.properties` points to a copy of the database).
8. Perform the necessary [environment transfer](#Environment+transferring).

>If you want to do a quick check and do not need to preserve the build history on the new server, you can skip Step 6 (cloning database) and all the optional items of Step 4.

<anchor name="environment_transfer"/>

### Environment transferring
[//]: # (AltHead: environment_transfer)

Consider transferring the relevant environment if it was specially modified for an existing TeamCity installation. This might include:
* use the appropriate OS user account for running the TeamCity server process with properly configured settings, global and file system permissions
* use the same [TeamCity process launching options](server-startup-properties.md), specifically check/copy environment variables starting with `TEAMCITY_`
* ensure any files/settings that were configured in the TeamCity web UI with absolute paths are accessible
* if relying on the OS-level user/machine settings like default SSH keys, cached VCS access credentials, transfer them as well
* consider replicating any special settings or exceptions related to the machine in the network configuration, and so on
* if the TeamCity installation was patched in any way (GroovyPlug plugin, native driver for MS SQL Server integrated security authentication), apply the same modifications to the installation copy
* if you run TeamCity with the OS startup (for example, Windows service), make sure the same configuration is performed on the new machine
* review and transfer settings in the `<[TeamCity Home](teamcity-home-directory.md)>\conf\teamcity-startup.properties` file
* consider any custom settings in `<[TeamCity Home](teamcity-home-directory.md)>\conf\server.xml`

<anchor name="copy_server_license"/>

### Licensing issues
{instance="tc"}

A single TeamCity license __cannot be used on two running servers__ at the same time.
* A copy of the server created for redundancy/backup purposes can use the same license as only one of the servers will be running at a time.
* A copy of the server created for testing purposes requires an additional license. You can get the time-limited TeamCity [evaluation license](licensing-policy.md) once from the official TeamCity [download page](https://www.jetbrains.com/teamcity/download/). If you need an extension of the license or you have already evaluated the same TeamCity version, please [contact our sales department](https://www.jetbrains.com/company/contacts/index.html#Contacts_?TeamCity).
* A copy of the server intended to run at the same time as the main one regularly/for production purposes requires a separate license.

### Copied Server Checklist
{instance="tc"}

If you need to create a copy of a server (rather than move your server to a new location), follow the checklist below:

* Ensure the original and copied servers use different [Data Directories](teamcity-data-directory.md), databases, and artifact directories.
* Change the unique server ID by removing "uuid" attribute from XML of `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/main-config.xml` file before the first start.
* Ensure the same license keys are not used on several servers ([more on licensing](#Licensing+issues)).
* Update [Server URL](configuring-server-url.md) on the __Administration | Global Settings__ page to the actual URL of the server.
* Check that you can successfully authenticate on the new server, use [super user](super-user.md) access if necessary.
* Check that VCS servers, issue tracker servers, email server, and other server-accessed systems are accessible.
* Check that any systems configured to push events to TeamCity server (like VCS hooks, automated build triggering, monitors, etc.) are updated to know about the new server.
* Review the list of installed plugins to determine if their settings need changes.
* Install new agents (or select some from the existing ones) and configure them to connect to the new server (using the new server URL).
* Check that clients reading from the server (downloading artifact, using server's REST API, NuGet feed, etc.) are reconfigured, if necessary.

If you copy a production server to create a __test server__, you need to ensure that the users and production systems are not affected.

Below is the list of settings which should be changed __before__ the test server is first started:

* Disable cleanup process to avoid cleanup of files in external storages, such as S3 buckets or Docker Registry. To do this, locate the `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/main-config.xml` file and change the `<db-compact enabled="true">` line to `<db-compact enabled="false">`.


* Disable cloud integrations: the test server should not be able to start new cloud agents. Set the
`teamcity.cloud.integration.enabled=false` internal property in the `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/internal.properties` file (create this file if it does not already exist).

* Disable plugins that can modify external systems' states, such as commit status publisher. Create the file `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/disabled-plugins.xml` with the following content:

    ```XML
    <?xml version="1.0" encoding="UTF-8"?>
    <disabled-plugins>
      <disabled-plugin name="commit-status-publisher" />
      <disabled-plugin name="slackNotifier" />
      <disabled-plugin name="email" />
      <disabled-plugin name="webhooks" />
      <disabled-plugin name="searchBuildByNumber" />
    </disabled-plugins>
    ```
  
    Consider adding to this list any third-party plugins that push data to other non-copied systems based on the TeamCity events.

* Set the `teamcity.versionedSettings.enabled=false` internal property to turn off to prevent projects from [storing their settings in VCS](storing-project-settings-in-version-control.md).

After the test server starts:

* Navigate to **Administration | Authentication** and disable email verification.
* Ensure your test server does not run any builds that change (for example, deploy to) production environments. This also typically includes Maven builds deploying to non-local repositories. You can prevent any builds from starting by pausing the [build queue](working-with-build-queue.md).
* You may also want to significantly increase the [VCS checking for changes interval](configuring-vcs-roots.md#Common+VCS+Root+Properties) (both server-wide default and overridden values in the VCS roots) or change VCS roots' settings to prevent them from contacting production servers. See also [TW-47324](https://youtrack.jetbrains.com/issue/TW-47324).

See also the section below on [moving the server](#Move+TeamCity+Installation+to+a+New+Machine) from one machine to another.
{instance="tc"}

<!--[//]: # (Internal note. Do not delete. "How To...d160e1901.txt")  -->

## Move TeamCity Projects from One Server to Another
{instance="tc"}

If you need to move data to a fresh server without existing data, it is recommended to [move the server](#Move+TeamCity+Installation+to+a+New+Machine) or [copy](#Create+a+Copy+of+TeamCity+Server+with+All+Data) it and then delete the data which is not necessary on the new server.

If you need to join the data with already existing data on the new server, there is a dedicated feature to move projects with most of the associated data from one server to another: [Projects Import](projects-import.md).

_Here are some notes on manual move of the settings in case you ever want to perform it_

It is possible to move _settings_ of a project or a build configuration to another server with simple file copying.

The two TeamCity servers (source and target) should be of exactly the same version (same build).

All the [identifiers](identifier.md) throughout all the projects, build configurations and VCS roots of both servers should be unique. If they are not, you can change them via web UI. If entities with the same id are present on different servers, the entities are assumed to be the same. For example this is useful for having global set of VCS roots on all the servers.

To move settings of the project and all its build configuration from one server to another:

From the [`TeamCity Data Directory`](teamcity-data-directory.md), copy the directories of corresponding projects (`.BuildServer\config\projects\<id>`) and all its parent projects to `.BuildServer\config\projects` of the target server. This moves project settings, build configuration settings, VCS roots defined in the projects preserving the links between them. If there are same-named files on the target server as those copied, this can happen in case of 
  a) id match: same entities already exist on the target server, in which case the clashing files can be excluded from copying, or 
  b) id clash: different entities happen to have same ids. In this case it should be resolved either by changing entity ID on the source or target server to fulfill the uniqueness requirement.

The set of parent projects is to be identified manually based on the web UI or the directory names on disk (which be default will have the same prefix).

Note: It might make sense to keep the settings of the [root project](project.md#Root+Project) synchronized between all the servers (by synchronizing content of `.BuildServer\config\projects_Root` directory). For example, this will ensure same settings for the default clean-up policy on all the servers.

Further steps after projects copying might be:
* delete unused data in the copied parent projects (if any) on the target server
* use "server health" reports to identify duplicate VCS roots appeared in result of copying, if any
* archive the projects on the source server and adjust clean-up rules (to be able to see build's history, if necessary)

What is _not_ copied by the approach above:
* pausing comment and user of the paused build configurations
* archiving user of the archived projects
* global server settings (for example, Maven `settings.xml profiles`, tools like `handle.exe`, external change viewers, build queue priorities, issue trackers). These are stored under various files under `.BuildServer\config` directory and should be synchronized either on the file level or by configuring the same settings in the server administration UI.
* project association with agent pools
* templates from other projects which are not parents of the copied one. This configuration is actually deprecated in TeamCity 8.0 and is only supported as legacy. Templates used in several projects should be moved to the common parent project or root project.
* no data configured for the agents (build configurations that are allowed to run on the agent).
* no user-related or user group-related settings (like roles and notification rules)
* no state-related data like mutes and investigations.

## Move TeamCity Installation to a New Machine
{instance="tc"}

If you need to move an existing TeamCity installation to new hardware or a clean OS, you can install the same TeamCity version on the new machine, stop the old server, connect the new server to the same [`TeamCity Data Directory`](teamcity-data-directory.md) and make sure the server uses the [same environment](#Environment+transferring). Alternatively, you can follow the [instructions on copying](#Create+a+Copy+of+TeamCity+Server+with+All+Data) the server from one machine to another.

After the move, make the clients [use the new server address](#Switching+from+one+server+to+another), if changed.

You can use the existing [license keys](licensing-policy.md#Managing+Licenses) when you move the server from one machine to another (as long as there are no two servers running at the same time). As license keys are stored under `<[TeamCity Data Directory](teamcity-data-directory.md)>`, you transfer the license keys with all the other TeamCity settings data.

It is usually advised not to combine TeamCity update with any other actions, like environment or hardware changes, and perform the changes one at a time so that, if something goes wrong, the cause can be easily tracked.

### Switching from one server to another
 
 Note that `<[TeamCity Data Directory](teamcity-data-directory.md)>` and database should be used by a single TeamCity instance at any given moment. If you configured a new TeamCity instance to use the same data, ensure you shutdown and disable the old TeamCity instance before starting the new one.

Generally it is recommended to use a domain name to access the server (in the agent configuration and when users access the TeamCity web UI). This way you can update the DNS entry to make the address resolve to the IP address of the new server and after all cached DNS results expire, all clients will automatically use the new server. You might need to reduce the DNS server cache/lease time in advance before the change to make the clients "understand" the change fast.

However, if you need to use another server domain address, you will need to:
* Switch agents to the new URL (requires updating the `serverUrl` property in `buildAgent.properties` on each agent). If you want to install agents anew but preserve agent's name and authentication status, you can install a new agent and copy the `conf\buildAgent.properties` file from an old agent (checking that any paths in it are updated accordingly).
* Upon the new server startup, remember to update the [Server URL](configuring-server-url.md) on __Administration__ | __Global Settings__ page.
* Notify all TeamCity users on the new address
* Consider updating settings of external services if they depend on the request originating address

## Move TeamCity Agent to a New Machine

Apart from the binaries, TeamCity agent installation stores its configuration and data left from the builds it run. Usually the data from the previous builds makes preparation for the future builds a bit faster, but it can be deleted if necessary. The configuration is stored under `conf` and `launcher\conf` directories. The data collected by previous build is stored under `work` and `system` directories.

The simplest way to move agent installation into a new machine or new location is to:
* stop existing agent
* [install](install-and-start-teamcity-agents.md) a new agent on the new machine
* copy `conf/buildAgent.properties` from the old installation to a new one
* start the new agent.
With these steps the agent will be recognized by TeamCity server as the same and will perform [clean checkout](clean-checkout.md) for all the builds.

Please also review the [section](agent-home-directory.md) for a list of directories that can be deleted without affecting builds consistency.

<!--[//]: # (Internal note. Do not delete. "How To...d160e2180.txt")-->    

## Share the Build number for Builds in a Chain Build

A build number can be shared for builds connected by a [snapshot dependency](dependent-build.md#Snapshot+Dependency) or an [artifact dependency](dependent-build.md#Artifact+Dependency) using a reference to the following dependency property: `%dep.<btID>.system.build.number%`.

For example, you have build configurations A and B that you want to build in sync: use the same sources and take the same build number.   
Do the following:
1. Create build configuration C, then snapshot dependencies: __A on C__ and __B on C__.
2. Set the [Build number format](configuring-general-settings.md#Build+Number+Format) in A and B to:

```Shell
%dep.<btID>.system.build.number%
```

Where `<btID>` is the [ID of the build configuration](managing-builds.md) C. The approach works best when builds reuse is turned off via the [Snapshot Dependencies](snapshot-dependencies.md) snapshot dependency option set to off.

[Read more](predefined-build-parameters.md#Predefined+Configuration+Parameters) about dependency properties.

Please watch/comment the issue related to sharing a build number [TW-7745](https://youtrack.jetbrains.com/issue/TW-7745).

## Make Temporary Build Files Erased between the Builds

Update your build script to use path stored in `${teamcity.build.tempDir}` (Ant's style name) property as the temp directory. TeamCity agent creates the [directory](agent-home-directory.md#temp-dir) before the build and deletes it right after the build.

## Clear Build Queue if It Has Too Many Builds due to a Configuration Error

Try pausing the build configuration that has the builds queued. On build configuration pausing all its builds are removed form the queue.   
Also there is an ability to delete many builds from the build queue in a single dialog.

## Automatically create or change TeamCity build configuration settings

If you need a level of automation and web administration UI does not suite your needs, there several possibilities:

* use [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html)
* change configuration files directly on disk (see more at `<[TeamCity Data Directory](teamcity-data-directory.md)>`)
  {instance="tc"}
* write a TeamCity Java plugin that will perform the tasks using [open API](https://plugins.jetbrains.com/docs/teamcity/developing-teamcity-plugins.html).



## Attach Cucumber Reporter to Ant Build

If you use Cucumber for Java applications testing you should run cucumber with `--expand` and special `--format` options. More over you should specify RUBYLIB environment variable pointing on necessary TeamCity Rake Runner ruby scripts:


```Shell

<<target name="features">
     <java classname="org.jruby.Main" fork="true" failonerror="true">
       <classpath>
         <pathelement path="${jruby.home}/lib/jruby.jar"/>
         <pathelement path="${jruby.home}/lib/ruby/gems/1.8/gems/jvyaml-0.0.1/lib/jvyamlb.jar"/>
         ....
       </classpath>
       <jvmarg value="-Xmx512m"/>
       <jvmarg value="-XX:+HeapDumpOnOutOfMemoryError"/>
       <jvmarg value="-ea"/>
       <jvmarg value="-Djruby.home=${jruby.home}"/>
       <arg value="-S"/>
       <arg value="cucumber"/>
       <arg value="--format"/>
       <arg value="Teamcity::Cucumber::Formatter"/>
       <arg value="--expand"/>
       <arg value="."/>
       <env key="RUBYLIB"
            value="${agent.home.dir}/plugins/rake-runner/rb/patch/common${path.separator}${agent.home.dir}/plugins/rake-runner/rb/patch/bdd"/>
       <env key="TEAMCITY_RAKE_RUNNER_MODE" value="buildserver"/>
     </java>
   </target>

```

Please check the RUBYLIB path separator (';' for Windows, ':' for Linux, or '$\{path.separator\}' in ant for safety).   
If you are launching Cucumber tests using the Rake build language, TeamCity will add all necessary command line parameters and environment variables automatically. This tip works in TeamCity version &gt;= 5.0.

## Get Last Successful Build Number

Use URL like this:

```Shell

http://<your TeamCity server>/app/rest/buildTypes/id:<ID of build configuration>/builds/status:SUCCESS/number

```

The build number will be returned as a plain-text response.   
For `<ID of build configuration>`, see [Identifier](identifier.md).   
This functionality is provided by [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html)

## Set up Deployment for My Application in TeamCity

TeamCity has multiple features to handle orchestration part of the deployments with the actual deployment logic configured in the build script / build runner. TeamCity supports a variety of generic build tools, so any specific tool can be run from within TeamCity. To ease specific tool usage, it is possible to wrap it into a meta-runner or write a custom plugin for that.

See the following articles for more information: [](deploy-build.md), [](deployment-build-configuration.md).

In general, setup steps for configuring deployments are:
1. Write a build script that will perform the deployment task for the binary files available on the disk. (e.g. use Ant or MSBuild for this. For typical deployment transports use [Deployer](deployers.md) runners). See also [Integrate with Build and Reporting Tools](#Integrate+with+Build+and+Reporting+Tools). You can use [Meta-Runner](working-with-meta-runner.md) to reuse a script with convenient UI.
2. Create a build configuration in TeamCity that will execute the build script and perform the actual deployment. If the deployment is to be visible or startable only by the limited set of users, place the build configuration in a separate TeamCity project and make sure the users have appropriate permissions in the project.
3. In this build configuration configure [artifact dependency](dependent-build.md#Artifact+Dependency) on a build configuration that produces binaries that need to be deployed.
4. Configure one of the available triggers in the deploying build configuration if you need the deployment to be triggered automatically (e.g. to deploy last successful of last pinned build), or use "Deploy" action in the build that produced the binaries to be deployed.
5. Consider using [snapshot dependencies](dependent-build.md#Snapshot+Dependency) in addition to artifact ones and check [Build Chains](build-chain.md) tab to get the overview of the builds. In this case artifact dependency should use "Build from the same chain" option.
6. If you need to parametrize the deployment (e.g. specify different target machines in different runs), pass parameters to the build script using [custom build run dialog](running-custom-build.md). Consider using [Typed Parameters](typed-parameters.md) to make the custom run dialog easier to use or handle passwords.
7. If the deploying build is triggered manually consider also adding commands in the build script to pin and tag the build being deployed (via sending a [REST API](https://www.jetbrains.com/help/teamcity/rest/manage-finished-builds.html#Manage+Build+Tags) request or a [Service Message](service-messages.md#Adding+and+Removing+Build+Tags)). You can also [use a build number](#Share+the+Build+number+for+Builds+in+a+Chain+Build) from the build that generated the artifact.

Further recommendations:
* Setup a separate build configurations for each target environment
* Use build's Dependencies tab for navigation between build producing the binaries and deploying builds/tasks
* If necessary, use parameter with "prompt" display mode to ask for "[confirmation](https://youtrack.jetbrains.com/issue/TW-8284#comment=27-290611)" on running a build
* [Change title](https://youtrack.jetbrains.com/issue/TW-20617#comment=27-527943) of the build configuration "Run" button. Buttons of [Deployment configurations](deployment-build-configuration.md) automatically change their titles to "Deploy".


<!--[//]: # (Internal note. Do not delete. "How To...d160e2430.txt")-->

## Use an External Tool that My Build Relies on
{instance="tc"}

If you need to use specific external tool to be installed on a build agent to run your builds, you have the following options:
* Install and register the tool in TeamCity:
  1. Install the tool on all the agents that will run the build. This can be done manually or via an automated script. For simple file distribution also see [Installing Agent Tools](installing-agent-tools.md)
  2. Add a property into `buildAgent.properties` file (or add environment variable to the system) with the tool home location as the value.
  3. Add agent requirement for the property in the build configuration.
  4. Use the property in the build script.
* Check in the tool into the version control and use relative paths.
* Add environment preparation stage into the build script to get the tool form elsewhere.
* Create a separate build configuration with a single "fake" build which would contain required files as artifacts, then use artifact dependencies to send files to the target build.

## Use an External Tool that My Build Relies on
{instance="tcc"}

If you need to use specific external tool to be installed on a build agent to run your builds, you have the following options:
* Check in the tool into the version control and use relative paths.
* Add environment preparation stage into the build script to get the tool form elsewhere.
* Create a separate build configuration with a single "fake" build which would contain required files as artifacts, then use artifact dependencies to send files to the target build.

## Integrate with Build and Reporting Tools

If you have a build tool or a tool that generates some report/provides code metrics which is not yet [supported by TeamCity](supported-platforms-and-environments.md) or any of the [plugins](https://plugins.jetbrains.com/teamcity), most probably you can use it in TeamCity even without dedicated integration.

The integration tasks involved are collecting the data in the scope of a build and then reporting the data to TeamCity so that they can be presented in the build results or in other ways.

__Data collection__

The easiest way for a start is to modify your build scripts to make use of the selected tool and collect all the required data.   
If you can run the tool from a command line console, then you can run it in TeamCity with a [command line runner](command-line.md). This will give you detection of the messages printed into standard error output. The build can be marked as failed is the exit code is not zero or there is output to standard error via [build failure condition](build-failure-conditions.md).    
If the tool has launchers for any of the supported build scripting engines like Ant, Maven or MSBuild, then you can use corresponding runner in TeamCity to start the tool. See also [Use an External Tool that My Build Relies on](#Use+an+External+Tool+that+My+Build+Relies+on) for the recommendations on how to run an external tool.

You can also consider creating a [Meta Runner](working-with-meta-runner.md) to let the tool have dedicated UI in TeamCity.

For an advanced integration a custom [TeamCity plugin](https://plugins.jetbrains.com/docs/teamcity/build-runner-plugin.html) can be developed in Java to ease tool configuration and running.

__Presenting data in TeamCity__

The build progress can be reported to TeamCity via [service messages](service-messages.md#Reporting+Build+Progress) and build status text can also be [updated](service-messages.md#Reporting+Build+Status).

For testing tools, if they are not yet [supported](testing-frameworks.md) you can report tests progress to TeamCity from the build via [test-related service messages](service-messages.md#Reporting+Tests) or generate one of the supported [XML reports](xml-report-processing.md) in the build and let it be imported via a service message of configured XML Report Processing build feature.

To present the results for a generic report, the approach might be to generate HTML report in the build script, pack it into archive and publish as a build artifact. Then configure a [report tab](including-third-party-reports-in-the-build-results.md) to display the HTML report as a tab on build's results.

A metrics value can be published as TeamCity statistics via [service message](service-messages.md#Reporting+Build+Statistics) and then displayed in a [custom chart](customizing-statistics-charts.md#Showing+Charts+Only+for+Specific+Build+Configurations+on+Project+Level). You can also configure [build failure condition](build-failure-conditions.md#Fail+Build+on+Metric+Change) based on the metric.

If the tool reports code-attributing information like Inspections or Duplicates, TeamCity-bundled report can be used to display the results. A custom plugin will be necessary to process the tool-specific report into TeamCity-specific data model. Example of this can be found in [XML Test Reporting](xml-report-processing.md) plugin and FXCop plugin (see a link on the [Bundled Open-Source Plugins](https://plugins.jetbrains.com/docs/teamcity/developing-teamcity-plugins.html#Bundled+Open-Source+Plugins) page).

See also [Import coverage results in TeamCity](importing-arbitrary-coverage-results-to-teamcity.md).

For advanced integration, a custom plugin will be necessary to store and present the data as required. See [Developing TeamCity Plugins](https://plugins.jetbrains.com/docs/teamcity/developing-teamcity-plugins.html) for more information on plugin development.

## Restore Just Deleted Project
{instance="tc"}

TeamCity moves deleted projects settings directories (which are named after the project id) to `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/_trash` directory adding `"<internal ID>"` suffix to the directories.

To restore a project, find the project directory in the `_trash` directory and move it into regular projects settings directory: `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/projects` while removing the `".projectN"` suffix from the directory name.   
You can do this while server is running, it should pick up the restored project automatically.

Note that TeamCity preserves builds history and other data stored in the database for deleted projects/build configurations for 5 days after the deletion time. All the associated data (builds and test history, changes, etc.) is removed during the next clean-up after the [configurable](teamcity-data-clean-up.md#Deleted+Build+Configurations+Clean-up) (5 days by default) timeout elapses. 

The `config/_trash` directory is not cleaned automatically and can be emptied manually if you are sure you do not need the deleted projects. No server restart is required.

## Transfer 3 Default Agents to Another Server
{instance="tc"}

This is not possible.

Each TeamCity server (Professional and Enterprise) allows using 3 or more agents bound to the server without any agent licenses. In case of the Professional server, by default 3 agents are bound to the server instance: users do not pay for these agents, there is no license key for them.   
In case of the Enterprise server, the number of agents depends on your package and the agents are bound to the server license key.

So, the agents bound to the server cannot be transferred to another server.

If you need more build agents that are included with your TeamCity server, you can purchase additional build agent licenses and connect more agents in addition to those that come bound with the server.

See [more](licensing-policy.md) on licensing.

## Recover from "Data format of the Data Directory (NNN) and the database (MMM) do not match" error
{instance="tc"}

If you get "Data format of the Data Directory (NNN) and the database (MMM) do not match." error on starting TeamCity, it means either the database or the TeamCity Data Directory were recently changed to an inconsistent state so they cannot be used together. Double-check the database and Data Directory locations and change them if they are not those where the server used to store the data.

If they are right, most probably it means that the server was upgraded with another database or Data Directory and the [consistent upgrade](upgrading-teamcity-server-and-agents.md#Upgrading+TeamCity+Server) requirement was not met for your main Data Directory and the database.

To recover from the state you will need backup of the consistent state made prior to the upgrade. You will need to restore that backup, ensure the right locations are used for the Data Directory and the database and perform the TeamCity upgrade.

## Debug a Build on a Specific Agent

In case a build fails on some agent, it is possible to debug it on this very agent to investigate agent-specific issues. Do the following:
1. Go to the __Agents__ page in the TeamCity web UI and [select the agent](viewing-build-agent-details.md).
2. [Disable the agent](build-agents-configuration-and-maintenance.md#Enabling%2FDisabling+Agents+via+UI) to temporarily remove it from the <tooltip term="build-grid">_build grid_</tooltip>. Add a comment (optional). To enable the agent automatically after a certain time period, check the corresponding box and specify the time.
3. [Select the build](working-with-build-results.md) to debug.
4. Open the [Custom Run](running-custom-build.md) dialog and specify the following options: 
    a. In the __Agent__ drop-down menu, select the disabled agent.
    b. It is recommended to select the __run as [Personal Build](personal-build.md)__ option to avoid intersection with regular builds.
5. When debugging is complete, enable the agent manually if automatic reenabling has not been configured.

You can also perform [remote debugging](remote-debug.md) of tests on an agent via the [IntelliJ IDEA plugin](intellij-platform-plugin.md) for TeamCity.

## Debug a Part of the Build (a build step)

If a build containing several steps fails at a certain step, it is possible to debug the step that breaks. Do the following:
1. Go to the build configuration and disable the build steps up to the one you want to debug.
2. [Select the build](working-with-build-results.md) to debug.
3. Open the [Custom Run](running-custom-build.md) dialog and select the __put the build to the [queue top](working-with-build-queue.md#Moving+Builds+to+Top)__ to give you build the priority.
4. When debugging is complete, reenable the build steps.

## Watch Several TeamCity Servers with Windows Tray Notifier
{instance="tc"}

TeamCity Tray Notifier is used normally to watch builds and receive notifications from a single TeamCity server. However, if you have more than one TeamCity server and want to monitor them with Windows Tray Notifier simultaneously, you need to start a separate instance of Tray Notifier for each of the servers from the command line with the `/allowMultiple` option:
* From the TeamCity Tray Notifier installation directory (by default, it's `C:\Program Files\JetBrains\TeamCity`) run the following command:


```Shell

JetBrains.TrayNotifier.exe /allowMultiple
```

Optionally, for each of the Tray Notifier instances you can explicitly specify the URL of the server to connect using the `/server` option. Otherwise, for each further tray notifier instance you will need to log out and change the server's URL via the UI.

```Shell

JetBrains.TrayNotifier.exe /allowMultiple /server:http://myTeamCityServer
```

See also [details](https://youtrack.jetbrains.com/issue/TW-4230#comment=27-14194) in the issue tracker.

## Personal User Data Processing
{instance="tc"}

In relation to the TeamCity product, JetBrains does not collect any personal data of the on-premises TeamCity installation users. The related documents governing relationship of customers with JetBrains are available on the official website: [privacy policy](https://www.jetbrains.com/company/privacy.html), [terms of purchase](https://www.jetbrains.com/store/terms/), [TeamCity license agreement](https://www.jetbrains.com/teamcity/buy/license.html).

The notes below can be useful when assessing how your usage of TeamCity complies with the General Data Protection Regulation (GDPR) (EU) 2016/679 regulation. These notes are meant to address the most basic questions and can serve as an input to the assessment of your specific TeamCity installation.

The notes are based on TeamCity 2017.2.4 which is actual at the moment of GDPR enforcement date. Please update your TeamCity instance at least to this version, as previous versions might contain issues not in line with the notes below.

### TeamCity and Users' Personal Data

The most important user-related data stored by TeamCity is:
* full name and username - stored in the database and shown on the user's profile and whenever the user is referenced. When a user triggers a build, these are also stored in the build's parameters and passed into the build
* user's email - stored in the database and shown on the user's profile, used to send out notifications
* IP address of the clients accessing the server - can appear in the [internal logs](teamcity-server-logs.md)

TeamCity internal logs can also record some unstructured user-related information (for example, submitted by the user or sent by the browser with the HTTP requests, retrieved according to the configured settings from the users source like LDAP).

### Deleting the User Data

When you want to delete personal data of a specific user, the best way to do it is to delete the user in TeamCity. This way all the references to the user will continue to store the numeric user id, while all the other user information will not be stored anymore. Note that [Audit](tracking-user-actions.md) records will mention internal numeric user id after the user deletion.

If the user triggered any builds (i.e. had the "Run build" permission in any of the projects which are still present on the server), the user's username and full name were recorded in the build's `teamcity.build.triggeredBy` parameters as text values as those were part of the build's "environment". If you need to remove those, you can either delete the related builds (and all the builds which artifact- or snapshot-depend on them), or delete parameters of those affected builds (the parameters are stored in archived files under &lt;TeamCity Data Directory&gt;\system\artifacts\*\*\*\.teamcity\properties directories).

After the user deletion and other data cleaning, make sure to [reset search index](search.md#Resetting+Search+Index) to prune possibly cached data of the deleted user from the search index.
{instance="tc"}

There are several other places which can hold user-related data

If user had "Edit project" permission, the full name / username can appear in:
* some audit entries (saved with TeamCity versions before 2017.2.1) — [TW-52215](https://youtrack.jetbrains.com/issue/TW-52215)
* some build logs in "Wait for pending persist tasks to complete" build log line (saved with TeamCity versions before 2017.2.1) — [TW-52872](https://youtrack.jetbrains.com/issue/TW-52872)
* commit comments in the repository storing versioned settings store name of the user doing the change in UI

VCS usernames in VCS-related data :
* in VCS changes visible in UI or stored in the database
* in the local `.git` repository clones on the server and agents

Username can also appear in access credentials configured in different integrations like VCS roots, issue tracker, database access, etc. (these are stored in the settings files and audit diff files in the TeamCity Data Directory and VCS roots usernames are also stored in the database for the current and previous versions of the VCS roots)

To ensure user's details are not stored by TeamCity you might want to check the TeamCity-backing storage that no occurrences of the data are stored: the database, Data Directory and the TeamCity home directory (logs, and memory dumps which are regularly placed under the "bin" directory).

### User Agreement

If you want the users to accept a special agreement before using your TeamCity instance, you can install a [dedicated plugin](https://plugins.jetbrains.com/plugin/10722-terms-of-service) developed by JetBrains for this purpose. Refer to the plugin's documentation for more details.

### Encryption

If you want to encrypt the data used by TeamCity, it is recommended to use generic, non-TeamCity-specific tools for this as TeamCity does not provide dedicated functionality. TeamCity stores the data in the SQL database and on the file system. You can configure the database to store the data in encrypted form and use secure JDBC\-backed connection to the database (configured in the [`database.properties`](set-up-external-database.md)).   
Also, you can configure encryption on the disk storage on the OS level.

### Logs and Debugging Data

If you want to ensure that you do not store the internal TeamCity logs for more than a limited amount of days, you can configure [internal logging](teamcity-server-logs.md) to rotate the log files each day and limit the number of files to keep. TeamCity agents generally do not operate with any user\-related data in a structural way, but if you need to ensure the logs are regularly rotated on the agent, you will need to configure [agent logging](viewing-build-agent-logs.md) the same way.

Per\-day rotation can be configured by adding `<param name="rotateOnDayChange" value="true"/>` line within all required `<appender name="..." class="jetbrains.buildServer.util.TCRollingFileAppender">` appenders. This change should be done to the default `conf\teamcity-server-log4j.xml` and also logging presets stored under `<TeamCity Data Directory>\config\_logging`.

TeamCity can also store diagnostics data like thread dumps which can record user-related data in unstructured way. It is recommended to review the content of the `<[TeamCity Home](teamcity-home-directory.md)>\logs` directory regularly and ensure that no old files are preserved in there. Also, the extra logs should be deleted after logging customization sessions like collecting debug logs, etc. There is a [known issue](https://youtrack.jetbrains.com/issue/TW-22596) that `logs\catalina.out` file if not rotated automatically at all. It is recommended to establish an automatic procedure to rotate the file regularly.

### Customizations

These notes only address bundled TeamCity functionality with the most common documented settings. You should assess your specific TeamCity installation considering customizations like the configured build scripts, installed plugins, external systems communicating with TeamCity via API, etc.

<!--[//]: # (Internal note. Do not delete. "How To...d160e3129.txt") -->   
