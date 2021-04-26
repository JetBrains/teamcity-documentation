[//]: # (title: How To...)
[//]: # (auxiliary-id: viewpage.actionpageId113084582;How To...)

## Choose OS/Platform for TeamCity Server
{product="tc"}

Once the server/OS fulfills the [requirements](supported-platforms-and-environments.md#TeamCity+Server), TeamCity can run on any system. Please also review the [requirements](supported-platforms-and-environments.md) for the integrations you plan to use, for example the following functionality requires or works better when TeamCity server is installed under Windows:
* VCS integration with TFS
* VCS integration with VSS
* Windows domain logins (can also work under Linux, but may be less stable), especially NTLM HTTP authentication
* NuGet feed on the server (can also work under Linux, but may be less stable)
* Agent push to Windows machines

If you have no preference, Linux platforms may be more preferable due to more effective file system operations and the level of required general OS maintenance.

Final Operating System choice should probably depend more on the available resources and established practices in your organization.

## Estimate Hardware Requirements for TeamCity
{product="tc"}

The hardware requirements differ for the server and the agents.

The __agent__ hardware requirements are basically determined by the builds that are run. Running TeamCity agent software introduces a requirement for additional CPU time (but it can usually be neglected comparing to the build process CPU requirements) and additional memory: about 500Mb. The disk space required corresponds to the disk usage by the builds running on the agent (sources checkouts, downloaded artifacts, the disk space consumed during the build; all that combined for the regularly occurring builds). Although you can run a build agent on the same machine as the TeamCity server, the recommended approach is to use a separate machine (it may be virtual) for each build agent. If you chose to install several agents on the [same machine](setting-up-and-running-additional-build-agents.md#Installing+Several+Build+Agents+on+the+Same+Machine), consider the possible CPU, disk, memory or network bottlenecks that might occur. The [Performance Monitor](performance-monitor.md) build feature can help you in analyzing live data.

The __server__ hardware requirements depend on the server load, which in its turn depends significantly on the type of the builds and server usage. Consider the following general guidelines.   

<tip>

* If you decide to run an [external database](setting-up-an-external-database.md) on the same machine as the server, take into account hardware requirements with database engine requirements in mind.
* If you face some TeamCity-related performance issues, they should probably be investigated and addressed individually. For example, if builds generate too much data, the server disk system might be needing an upgrade both in size and speed characteristics.
</tip>

__Database Note:__  
When using the server extensively, the database performance starts to play a greater role.   
For reliability and performance reasons you should use an external database.   
See the [notes](setting-up-an-external-database.md) on choosing external database.    
The database size requirements naturally vary based on the amount of data stored (number of builds, number of tests, and so on). The active server database usage can be estimated at several gigabytes of data per year.

__Overview of the TeamCity hardware resources usage:__      
* CPU: TeamCity utilizes multiple cores of the CPU, so increasing number of cores makes sense. For non-trivial TeamCity usage at least 4 CPU cores are recommended.
* Memory: used by the TeamCity server main process and child processes (used for Maven integration, version control integration, Kotlin DSL execution). See the [notes](installing-and-configuring-the-teamcity-server.md#Setting+Up+Memory+settings+for+TeamCity+Server) on the main process memory usage. Generally, you will probably not need to dedicate more than 4G of memory to TeamCity server if you do not plan to run more than 100 concurrent builds (agents), support more than 200 online users or work with large repositories.
* HDD/disk usage: This sums up mainly from the temp directory usage (`<[TeamCity Home](teamcity-home-directory.md)>/temp` and OS default temp directory) and `<[TeamCity Data Directory](teamcity-data-directory.md)>/system` usage. Performance of the TeamCity server highly depends on the disk system performance. As TeamCity stores large amounts of data under `<[TeamCity Data Directory](teamcity-data-directory.md)>/system` (most notably, VCS caches and build results), it is important to ensure that the access to the disk is fast (in particular reading/writing files in multiple threads, listing files with attributes). Ensuring disk has good performance is especially important if you plan to store the Data Directory on a network drive. It is recommended to use local storage for `TeamCity Data Directory/system/caches` directory. See also [TeamCity Data Directory](teamcity-data-directory.md#Recommendations+as+to+choosing+Data+Directory+Location).
* Network: This mainly sums up from the traffic from VCS servers, to clients (web browsers, IDE, etc.) and to/from build agents (send sources, receive build results, logs and artifacts).

__The load on the server depends on:__   
* number of build configurations;
* number of builds in the history;
* number of the builds running daily;
* amount of data consumed and produced by the builds (size of the used sources and artifacts, size of the build log, number and output size of unit tests, number of inspections and duplicates hits, size and number of produced artifacts, and so on);
* clean-up rules configured
* number of agents and their utilization percentage;
* number of users having TeamCity web pages open;
* number of users logged in from IDE plugin;
* number and type of VCS roots as well as the configured checking for changes interval for the VCS roots. VCS checkout mode is relevant too: server checkout mode generates greater server load. Specific types of VCS also affect server load, but they can be roughly estimated based on native VCS client performance;
* number of changes detected by TeamCity per day in all the VCS roots;
* the size of the repositories TeamCity works with;
* total size of the sources checked out by TeamCity daily.    

A general example of hardware configuration capable to handle up to 100 concurrently running builds and running only TeamCity server can be:   
Server-suitable modern multicore CPU, 8Gb of memory, fast network connection, fast and reliable HDD, fast external database access. 

Based on our experience, a modest hardware likeIntel 3.2 GHz dual core CPU, 3.2Gb memory under Windows, 1Gb network adapter, single HDDcan provide acceptable performance for the following setup:
* 60 projects and 300 build configurations (with one forth being active and running regularly);
* more than 300 builds a day;
* about 2Mb log per build;
* 50 build agents;
* 50 web users and 30 IDE users;
* 100 VCS roots (mainly Perforce and Subversion using server checkout), average checking for changes interval is 120 seconds;
* more than 150 changes per day;
* Kotlin DSL is not used;
* the database (MySQL) is running on the same machine;
* TeamCity server process has `-Xmx1100m` JVM setting. 

The following configuration can provide acceptable performance for a more loaded TeamCity server:   
Intel Xeon E5520 2.2 GHz CPU (4 cores, 8 threads), 12Gb memory under Windows Server 2008 R2 x64, 1Gb network adapter, 3 HDD RAID1 disks (general, one for artifacts, logs and caches storage, and one for the database storage)

__Server load characteristics:__
* 150 projects and 1500 build configurations (with one third being active and running regularly);
* more than 1500 builds a day;
* about 4Mb log per build;
* 100 build agents;
* 150 web users and 40 IDE users;
* 250 VCS roots (mainly Git, Hg, Perforce and Subversion using agent-side checkout), average checking for changes interval is 180 seconds;
* more than 1000 changes per day;
* the database (MySQL) is running on the same machine;
* TeamCity server process has `-Xmx3700m` x64 JVM setting.

However, to ensure peak load can be handled well, more powerful hardware is recommended.

HDD free space requirements are mainly determined by the number of builds stored on the server and the artifacts size/build log size in each. Server disk storage is also used to store VCS-related caches and you can estimate that at double the checkout size of all the VCS roots configured on the server.

If the builds generate large number of data (artifacts/build log/test data), using fast hard disk for storing the `.BuildServer/system` directory and fast network between agents and server are recommended.

The general recommendation for deploying large-scale TeamCity installation is to start with a reasonable hardware while considering hardware upgrade. Then increase the load on the server (e.g. add more projects) gradually, monitoring the performance characteristics and deciding on necessary hardware or software improvements. There is also a [benchmark plugin](https://plugins.jetbrains.com/plugin/9127-benchmark) which can be used to estimate the number of simultaneous build the current server installation can handle. Anyway, best administration practices are recommended like keeping adequate disk defragmentation level, and so on.

Starting with an adequately loaded system, if you then increase the number of concurrently running builds (agents) by some factor, be prepared to increase CPU, database and HDD access speeds, amount of memory by the same factor to achieve the same performance.   
If you increase the number of builds per day, be prepared to increase the disk size.

If you consider cloud deployment for TeamCity agents (for example, on Amazon EC2),  also review [Setting Up TeamCity for Amazon EC2](setting-up-teamcity-for-amazon-ec2.md#Estimating+EC2+Costs)

A note on the agents' setup in JetBrains internal TeamCity installation:   
We use both separate machines each running a single agent and dedicated "servers" running several virtual machines each of them having a single agent installed. Experimenting with the hardware and software we settled on a configuration when each core7i physical machine runs 3 virtual agents, each using a separate hard disk. This stems form the fact that our (mostly Java) builds depend on HDD performance in the first place. But YMMV.

TeamCity is known to work well with 500\+ build agents (500 concurrently running builds actively logging build run-time data). In synthetic tests the server was functioning OK with as many as 1000 concurrent builds (the server with 8 cores, 32Gb of total memory running under Linux, and MySQL server running on a separate comparable machine). The load on the server produced by each build depends on the amount of data the build produces (build log, tests number and failure details, inspections/duplicates issues number, etc.). Keeping the amount of data reasonably constrained (publishing large outputs as build artifacts, not printing those into standard output; tweaking inspection profiles to report limited set of the most important inspection hits, etc.) will help scale the server to handle more concurrent builds.   
If you need much more agents/parallel builds, it is recommended to use [several nodes setup](multinode-setup.md). If a substantially large amount of agents is required, it is recommended to consider using several separate TeamCity instances and distributing the projects between them. We constantly work on TeamCity performance improvements and are willing to work closely with organizations running large TeamCity installations to study any performance issues and improve TeamCity to handle larger loads. See also a related post on the [maximum number of agents which TeamCity can handle](http://blog.jetbrains.com/teamcity/2015/08/benchmarking-teamcity/)

See also a related post: [description of a substantial TeamCity setup](http://blogs.jetbrains.com/teamcity/2011/09/05/improving-performance-and-scalability-of-your-teamcity-server/).

### Network Traffic between the Server and the Agents

The traffic mostly depends on the settings as some of them include transferring binaries between the agent and the server.   
The most important flows of traffic between the agent and the server are:
* agent retrieves commands from the server: these are typically build start tasks which basically include a dump of the build configuration settings and the full set of build parameters. The latter can be large (e.g. megabytes) in case of a large build chain. The parameters can be reviewed on the build's [Parameters tab](working-with-build-results.md#Parameters);
* agent periodically sends current status data to the server (this includes all the agents parameters which can be reviewed on the agent's [Agent Parameters](viewing-build-agent-details.md#Agent+Parameters) tab);
* during the build, the agent sends  build log messages and parameters data back to the server. These can be reviewed on the [Build Log](working-with-build-results.md#Build+Log) and [Parameters](working-with-build-results.md#Parameters) tabs of the build;
* (when the server-side checkout mode is used) the agent downloads the sources before the build (as a full or incremental patch) from the server;
* (when an [artifact dependency](artifact-dependencies.md) is configured) the agent downloads build artifacts of other builds from the server before starting a build;
* (when artifacts are configured for a build) the agent uploads build artifacts to the server;
* some runners (like coverage or code analysis) include automatic uploading of their results' reports to the server.

## Configuring TeamCity Server for Performance
{product="tc"}

Here are some recommendations to tweak TeamCity server setup for better performance. The list for [production server use](installing-and-configuring-the-teamcity-server.md#Configuring+Server+for+Production+Use) is a prerequisite:
* Regularly review reported Server Health reports (including hidden ones)
* Use a separate [reverse proxy](#Set+Up+TeamCity+behind+a+Proxy+Server) server (e.g. NGINX) to handle HTTPS
* Use a separate server for the external database and monitor the database performance
* Monitor the server's CPU and IO performance, increase hardware resources as necessary (see also [hardware notes](#Estimate+Hardware+Requirements+for+TeamCity))
* Make sure clean-up is configured for all the projects with a due retention policy, make sure clean-up completely finishes regularly (check Administration / Clean-Up page)
* Consider ensuring good IO performance for the `<[TeamCity Data Directory](teamcity-data-directory.md)>/system/caches` directory, e.g. by moving it to a separate local drive (or storing on a local drive you choose to store the TeamCity Data Directory on a network storage)
* Regularly archive obsolete projects
* Regularly review the installed not bundled plugins and remove those not essential for the server functioning
* Consider using agent-side checkout whenever possible
* Make sure the build logs are not huge (tens megabytes at most, better less than 10 Mb)
* If lots VCS roots are configured on the server, consider configuring [repository commit hooks](configuring-vcs-post-commit-hooks-for-teamcity.md) instead of using polling for changes or at least increase [VCS polling interval](configuring-vcs-roots.md#Common+VCS+Root+Properties) to 300 seconds or more
* If the server is often used by large number of users (e.g. more than 1000), consider reducing the frequency of background UI requests their by increasing [UI refresh intervals](teamcity-tweaks.md#Web+Page+Refresh+Interval)
* When regularly exceeding 500 concurrently running builds which log a lot of data, consider using [Several Nodes Setup](multinode-setup.md)

<anchor name="HowTo-RetrieveAdministratorPassword"/>

## Retrieve Administrator Password
{product="tc"}

On the first start with the empty database, TeamCity displays the Administrator Setup page which allows creating a user with full administrative permissions (assigning the [System Administrator](role-and-permission.md#Per-Project+Authorization+Mode) role).

If you want to regain access to the system and you cannot log in as a user with the System Administrator role, you can log in as a [super user](super-user.md) and change the existing administrator account password or create a new account with the System Administrator role.

It is also possible to use [REST API](https://www.jetbrains.com/help/teamcity/rest/curl-examples.html#Making+user+a+system+administrator) to add the System Administrator role to any existing user.

If you use built-in authentication and have correct email specified, you can [reset the password](managing-your-user-account.md#Changing+Your+Password) from the login page. 

## Estimate External Database Capacity
{product="tc"}

It is quite hard to provide the exact numbers when setting up or migrating to an external database, as the required capacity varies greatly depending on how TeamCity is used.

The database size and database performance are crucial aspects to consider.

__Database Size__

The size of the database will depend on:   
* how many builds are started every day
* how many test are reported from builds
* [clean-up](clean-up.md) rules (retention policy)
* clean-up schedule

We recommend the initial size of data spaces to be 4 GB. When migrating from the internal database, we suggest at least doubling the size of the current internal database. For example, the size of the external database (without the Redo Log files) of the internal TeamCity server in JetBrains is about 50 GB. Setting your database to grow automatically helps to increase file sizes to a predetermined limit when necessary, which minimizes the effort to monitor disk space.

Allocating 1 GB for the redo log (see the table below) and undo files is sufficient in most cases.

__Database Performance__

The following factors are to be taken into account:   
* type of database (RDBMS)
* number of agents (which actually means the number of builds running in parallel)
* number of web pages opened by all users
* [clean-up](clean-up.md) rules (retention policy)

It is advised to place the [`TeamCity Data Directory`](teamcity-data-directory.md) and database data files on physically different hard disks (even when both the TeamCity server and RDBMS share the same host).

Placing redo logs on a separate physical disk is also recommended especially in case of the high number of agents (50 and more).

__Database-specific considerations__

The _redo_ log (or a similar entity) naming for different RDBMS:

<table><tr>

<td>

RDBMS

</td>

<td>

Log name

</td></tr><tr>

<td>

Oracle

</td>

<td>

Redo Log

</td></tr><tr>

<td>

MS SQL Server

</td>

<td>

Transaction Log

</td></tr><tr>

<td>

PostgreSQL

</td>

<td>

WAL (write ahead log)

</td></tr><tr>

<td>

MySQL \+ InnoDB and Percona

</td>

<td>

Redo Log

</td></tr></table>

PostgreSQL: We recommend using version 9.2\+, which has a lot of query optimization features. Also see the information on the write-ahead-log (WAL) in the [PostgreSQL documentation](http://www.postgresql.org/docs/9.2/static/wal-internals.html)

Oracle: it is recommended to keep statistics on: all automatically gathered statistics should be enabled (since Oracle 10.0, this is the default setup). Also see the information on redo log files in the [Oracle documentation](https://docs.oracle.com/cd/B14117_01/server.101/b10752/iodesign.htm#26022).

MS SQL Server: it is NOT recommended to use the jTDS driver: it does not work with `nchar/nvarchar`, and to preserve unicode streams it may cause queries to take a long time and consume a lot of IO. Also see the information on redo log in the [Microsoft Knowledge base](https://support.microsoft.com/kb/2033523). If you use jTDS, please [migrate](setting-up-an-external-database.md#jTDS+driver).

MySQL: the query optimizer might be inefficient: some queries may get a wrong execution plan causing them to take a long time and consume huge IO.

## Estimate the Number of Required Build Agents

There are no precise data and the number of required build agents depends a lot on the server usage pattern, type of builds, team size, commitment of the team to CI process, and so on.   
The best way is to start with the default 3 agents and see how that plays with the projects configured, then estimate further based on that.

You might want to increase the number of agents when you see:
* builds waiting for an idle agent in the build queue;
* more changes included into each build than you find comfortable (for example, for build failures analysis);
* necessity for different environments.
We've seen patterns of having an agent per each 20 build configurations (types of builds). Or a build agent per 1-2 developers.

See also [notes](#Estimate+Hardware+Requirements+for+TeamCity) on maximum supported number of agents.
{product="tc"}

## Setup TeamCity in Replication/Clustering Environment
{product="tc"}

It is possible to add a [secondary TeamCity node](configuring-secondary-node.md) to ensure high availability and offload some operations from the main server. All nodes need to be connected to the same [`TeamCity Data Directory`](teamcity-data-directory.md) and the database.

To address fast disaster recovery scenarios, TeamCity supports active - failover (cold standby) approach: the data that the TeamCity server uses can be replicated and a solution put in place to start a new server using the same data if the currently active server malfunctions.

As to the data, the TeamCity server uses both database and file storage (Data Directory). You can browse through [TeamCity Data Backup](teamcity-data-backup.md) and [TeamCity Data Directory](teamcity-data-directory.md) pages in to get more information on TeamCity data storing. Basically, both the TeamCity Data Directory on the disk and the database which TeamCity uses must remain in a consistent state and thus must be replicated together.   
Only a single TeamCity server instance should use the database and Data Directory at any time.

Ensure that the distribution of the TeamCity failover/backup server is of exactly the same version as the main server. It is also important to ensure the same server environment/startup options like memory settings, and so on.

TeamCity agents farm can be reused between the main and the failover servers. Agents will automatically connect to the new server if you make the failover server to be resolved via the old server DNS name and agents connect to the server using the DNS name. See also information on [switching](#Switching+from+one+server+to+another) from one server to another.   
If appropriate, the agents can be replicated just as the server. However, there is no need to replicate any TeamCity-specific data on the agents except for the conf\buildAgent.properties file as all the rest of the data can typically be renewed from the server. In case of replicated agents farm, the replica agents just need to be connected to the failover server.

In case of two servers installations for redundancy purposes, they can use the same set of licenses as only one of them is running at any given moment.

## TeamCity Security Notes

The contents of this section have been moved to the [dedicated article](security-notes.md).

<anchor name="HowTo...-ConfigureNewlyInstalledMySQLServer"/>

<anchor name="HowTo-ConfigureNewlyInstalledMySQLServer"/>

## Configure Newly Installed MySQL Server
{product="tc"}

If MySQL server is going to be used with TeamCity in addition to the [basic setup](setting-up-an-external-database.md#MySQL), you should review and probably change some of the MySQL server settings. If MySQL is installed on Windows, the settings are located in `my.ini` file which usually can be found under MySQL installation directory. For Unix-like systems the file is called `my.cnf` and can be placed somewhere under `/etc` directory. Read more about configuration file location in [MySQL documentation](http://dev.mysql.com/doc/refman/5.5/en/option-files.html). Note: you'll need to restart MySQL server after changing settings in `my.ini|my.cnf`.

The following settings should be reviewed and/or changed:

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

### innodb_buffer_pool_size and innodb_log_file_size

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

We recommend to start with 2Gb and increase it if you experience slowness and have enough memory. After increasing buffer pool size you should also change size of the [`innodb_log_file_size`](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_log_file_size) setting (its value can be calculated as `innodb_log_file_size/N`, where N is the number of log files in the group (2 by default)):

```Shell
innodb_log_file_size=1024M

```

### innodb_file_per_table

For better performance you can enable the so-called [per-table tablespaces](http://dev.mysql.com/doc/refman/5.5/en/innodb-multiple-tablespaces.html). Note that once you add `innodb_file_per_table` option new tables will be created and placed in separate files, but tables created before enabling this option will still be in the shared tablespace. You'll need to reimport database for them to be placed in separate files.

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

Placing the MySQL log files on different disk sometimes helps improve performance. You can read about it in [MySQL documentation](http://dev.mysql.com/doc/refman/5.5/en/innodb-configuration.html).

### Setting The Binary Log Format

If the default MySQL binary logging format is not MIXED (it depends on the [version of MySQL](http://dev.mysql.com/doc/refman/5.1/en/binary-log-setting.html) you are using), then it should be explicitly set to __MIXED__:

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
{product="tc"}

For better TeamCity server performance, it is recommended to change some of the parameters of the newly installed PostgreSQL server. You can read more about PostgreSQL performance optimization in [PostgreSQL Wiki](https://wiki.postgresql.org/wiki/Performance_Optimization).

The parameters below can be changed in the `postgresql.conf` file located in the in PostgreSQL's data directory.

### shared_buffers

The default value of [http://www.postgresql.org/docs/current/static/runtime-config-resource.html#GUC-SHARED-BUFFERS](http://www.postgresql.org/docs/current/static/runtime-config-resource.html#GUC-SHARED-BUFFERS) parameter is too small and should be increased:


```Shell

shared_buffers=512MB

```

<anchor name="checkpoint_segments"/>

### checkpoint-related parameters
[//]: # (AltHead: checkpoint_segments)

For write-intensive applications such as TeamCity, it is recommended to change some of the [checkpoint-related parameters](http://www.postgresql.org/docs/current/static/runtime-config-wal.html#RUNTIME-CONFIG-WAL-CHECKPOINTS):

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

### synchronous_commit

If TeamCity is the only application using the PostgreSQL database, we recommend disabling the [http://www.postgresql.org/docs/current/static/runtime-config-wal.html#GUC-SYNCHRONOUS-COMMIT](http://www.postgresql.org/docs/current/static/runtime-config-wal.html#GUC-SYNCHRONOUS-COMMIT) parameter:

```Shell

synchronous_commit=off

```

<anchor name="HowTo-SetUpTeamCitybehindaProxyServer"/>

## Set Up TeamCity behind a Proxy Server
{product="tc"}

This section covers the recommended setup of reverse proxy servers installed in front of the TeamCity server web UI.

>We also recommend configuring HTTPS on the proxy level. Refer to your proxy server documentation for more details.
>
{type="tip"}

Consider the example:   
TeamCity server is installed at a _local_ URL [`http://teamcity.local:8111/tc`](http://teamcity.local:8111/tc).   
It is visible to the outside world as a _public_ URL [`http://teamcity.public:400/tc`](http://teamcity.public:400/tc).

To make sure TeamCity "knows" the actual absolute URLs used by the client to access the resources, you need to:
* set up a [reverse proxy](#Proxy+Server+Setup);
* configure the [Tomcat server](#TeamCity+Tomcat+Configuration) bundled with TeamCity.

These URLs are used to generate absolute URLs in the client redirects and other responses.

Note: An internal TeamCity server should work under the __same context__ (that is part of the URL after the host name) as it is visible from outside by an external address. See also the TeamCity server [context changing instructions](installing-and-configuring-the-teamcity-server.md#Changing+Server+Context).   
If you need to run the server under a different context, note that the context-changing proxy should conceal this fact from the TeamCity: for example, it should map server redirect URLs as well as cookies setting paths to the original (external) context.

### Proxy Server Setup

The proxy should be configured with the generic web security in mind. Headers like `Referer` and `Origin` and all unknown headers should be passed to the TeamCity web application in the unmodified form. For example, TeamCity relies on the `X-TC-CSRF-Token` header added by the clients.

### Apache

Versions 2.4.5 or later are recommended. Earlier versions do not support the WebSocket protocol, so you should use the settings described in the [previous documentation version](https://confluence.jetbrains.com/display/TCD8/How+To...).

When using Apache, make sure to use the [Dedicated "Connector" node approach](#Dedicated+%22Connector%22+Node+Approach) for configuring TeamCity server.

```Shell

LoadModule  proxy_module          /usr/lib/apache2/modules/mod_proxy.so
LoadModule  proxy_http_module     /usr/lib/apache2/modules/mod_proxy_http.so
LoadModule  headers_module        /usr/lib/apache2/modules/mod_headers.so
LoadModule  proxy_wstunnel_module /usr/lib/apache2/modules/mod_proxy_wstunnel.so

ProxyRequests       Off
ProxyPreserveHost   On
ProxyPass           /tc/app/subscriptions ws://teamcity.local:8111/tc/app/subscriptions connectiontimeout=240 timeout=1200
ProxyPassReverse    /tc/app/subscriptions ws://teamcity.local:8111/tc/app/subscriptions

ProxyPass           /tc http://teamcity.local:8111/tc connectiontimeout=240 timeout=1200
ProxyPassReverse    /tc http://teamcity.local:8111/tc

```

Note the order of the [ProxyPass rules](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html#proxypass): conflicting ProxyPass rules must be sorted starting with the longest URLs first.

By default, Apache allows only a limited number of parallel connections that may be insufficient when using the WebSocket protocol. For instance, it may result in the TeamCity server not responding when a lot of clients open the web UI. To fix it, you may need to fine-tune the Apache configuration.

For example, on Unix, you should switch to [mpm_worker](http://httpd.apache.org/docs/2.4/mod/worker.html) and configure the maximum number of simultaneous connections:

```Shell
<IfModule mpm_worker_module>
    ServerLimit 100
    StartServers 3
    MinSpareThreads 25
    MaxSpareThreads 75
    ThreadLimit 64
    ThreadsPerChild 25
    MaxClients 2500
    MaxRequestsPerChild 0
</IfModule>

```

On Windows, you may need to increase the [ThreadsPerChild](http://httpd.apache.org/docs/2.4/mod/mpm_common.html#threadsperchild) value as described in the [Apache documentation](http://httpd.apache.org/docs/2.4/platform/windows.html).

### NGINX

For the NGINX configuration below, use the ["RemoteIpValve" Approach](#Proxy-Tomcat-RemoteIpValve) for configuring the TeamCity server.

Versions 1.3 or later are recommended. Earlier versions do not support the WebSocket protocol, so you should use the settings described in the [previous documentation version](https://confluence.jetbrains.com/display/TCD8/How+To...).


```Java

http {
    # ... default settings here
    proxy_read_timeout     1200;
    proxy_connect_timeout  240;
    client_max_body_size   0;    # maximum size of an HTTP request. 0 allows uploading large artifacts to TeamCity
 
    map $http_upgrade $connection_upgrade { # WebSocket support
        default upgrade;
        '' '';
    }
     
    server {
        listen       400; # public server port
        server_name  teamcity.public; # public server host name
     
        location / { # public context (should be the same as internal context)
            proxy_pass http://teamcity.local:8111; # full internal address
            proxy_http_version  1.1;
            proxy_set_header    Host $server_name:$server_port;
            proxy_set_header    X-Forwarded-Host $http_host;    # necessary for proper absolute redirects and TeamCity CSRF check
            proxy_set_header    X-Forwarded-Proto $scheme;
            proxy_set_header    X-Forwarded-For $remote_addr;
            proxy_set_header    Upgrade $http_upgrade; # WebSocket support
            proxy_set_header    Connection $connection_upgrade; # WebSocket support
        }
    }
}

```

[//]: # (Internal note. Do not delete. "How To...d160e1234.txt")    

### Common misconfigurations

Check that your reverse proxy (or a similar tool) conforms to the following requirements:
* URLs with paths starting with a dot (`.`) are supported (path to hidden artifacts start contain the `.teamcity` directory).
* URLs with a colon (`:`) are supported (many TeamCity resources use the colon). Related [IIS setting](https://docs.microsoft.com/en-us/dotnet/api/system.web.configuration.httpruntimesection.requestpathinvalidcharacters?view=netframework-4.7.2). Symptom: build has no artifacts with the "_No user-defined artifacts in this build_" text even if there are artifacts.
* Maximum response length / time are not too restrictive (since TeamCity can serve large files to slow clients, the responses can be of Gb in size and hours in time).
* gzip Content-Encoding is fully supported. For example, certain IIS configurations can result in the "Loading data..." in the UI and 500 HTTP responses (see the related [issue](https://youtrack.jetbrains.com/issue/TW-56218)).

<anchor name="OtherServers"/>

### Other servers

Make sure to use a performant proxy with due (high) limits on request (upload) and response (download) size and timeouts (at least tens of minutes and gigabyte, according to the sizes of the codebase and artifacts).

It is recommended to use a proxy capable of working with the WebSocket protocol as this helps the UI to refresh quicker.

Generally, you need to configure the TeamCity server so that it "knows" about the original URL used by the client and can generate correct absolute URLs accessible for the client. The preferred way to achieve that is to pass the original `Host` header to TeamCity. An alternative method is to set `X-Forwarded-Host` header to the original `Host` header's value.

Note that whenever the value of the `Host` header is changed by the proxy (while it is recommended to preserve the original `Host` header value) and no `X-Forwarded-Host` header with the original `Host` value is provided, the values of the `Origin` and `Referer` headers should be mapped correspondingly if they contain the original `Host` header value (if they do not, they should not be set in order not to circumvent TeamCity [CSRF protection](csrf-protection.md)).

Select a proper approach from the section below and configure the proxy accordingly.

### TeamCity Tomcat Configuration

For a TeamCity Tomcat configuration, there are two options:
* Use a __dedicated "Connector" node__ in the server configuration with hard-coded public URL details and make sure the port configured in the connector is used only by the requests to the public URL configured.
* Configure the proxy to pass due request headers: `Host` or `X-Forwarded-Host` (original request host), `X-Forwarded-Proto` (original request protocol), `X-Forwarded-Port` (original request port) and __configure "RemoteIpValve"__ for the TeamCity Tomcat configuration.

<anchor name="Proxy-Tomcat-Connector"/>

### Dedicated "Connector" Node Approach

This approach can be used with any proxy configuration, provided the configured port is receiving requests only to the configured public URL.

Set up the proxying server to redirect all requests to `teamcity.public:400` to a dedicated port on the TeamCity server (`8111` in the example below) and change the "Connector" node in `<[TeamCity Home](teamcity-home-directory.md)>/conf/server.xml` file as below. Note that the "Connector" port configured this way should only be accessible via the configured proxy's address. If you also want to make TeamCity port accessible directly, use a separate "Connector" node with a dedicated `port` value for that.

```Shell

<Connector port="8111" protocol="org.apache.coyote.http11.Http11NioProtocol"
               connectionTimeout="60000"
               useBodyEncodingForURI="true"
               socket.txBufSize="64000"
               socket.rxBufSize="64000"
               tcpNoDelay="1"
               proxyName="teamcity.public"
               proxyPort="400"
               secure="false"
               scheme="http"
               />
```

When the public server address is __HTTPS__, use the `secure="true"` and `scheme="https"` attributes. If these attributes are missing, TeamCity will show a respective health report.

<anchor name="Proxy-Tomcat-RemoteIpValve"/>

### "RemoteIpValve" Approach
[//]: # (AltHead: Proxy-Tomcat-RemoteIpValve)

This approach can be used when the proxy server sets `X-Forwarded-Proto`, `X-Forwarded-Port` request headers to the values of the original URL. Also, while not critical for the most setups, this approach can be used to make sure the original client IP is passed to the TeamCity server correctly. This is important for legacy agents' [bidirectional communication](setting-up-and-running-additional-build-agents.md#Bidirectional+Communication).

Add the following into the Tomcat main `<Host>` node of the `conf\server.xml` file (see also Tomcat [doc](http://tomcat.apache.org/tomcat-8.5-doc/api/org/apache/catalina/valves/RemoteIpValve.html)):

```Shell

<Valve
   className="org.apache.catalina.valves.RemoteIpValve"
   remoteIpHeader="x-forwarded-for"
   protocolHeader="x-forwarded-proto"
   portHeader="x-forwarded-port"
   />

```

It is also recommended specifying the `internalProxies` attribute with the regular expression matching only the IP address of the proxy server. For example, `internalProxies="192\.168\.0\.1"`.

[//]: # (Internal note. Do not delete. "How To...d160e1383.txt")

## Enforce hiding stacktrace
{product="tc"}

If a user with access to your TeamCity server submits an invalid cross-site scripting (XSS) payload, the server will display the "Unexpected error" page containing a related stacktrace. To prevent exposing any sensible information about your environment via this stacktrace, you might want to disable its display. For this, set the `teamcity.web.runtimeError.showStacktrace` [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties) to `false`.

## Configure HTTPS for TeamCity Web UI
{product="tc"}

TeamCity does not provide out-of-the-box support for HTTPS access (see [TW-12976](http://youtrack.jetbrains.com/issue/TW-12976#comment=27-348823)). It is highly recommended to set up a reverse proxy like Nginx or Apache in front of TeamCity that would handle HTTPS and use HTTP TeamCity server port as the upstream. HTTPS-related configuration of the proxy is not specific for TeamCity and is generic as for any Web application. Make sure to configure the reverse proxy per [our recommendations](#Set+Up+TeamCity+behind+a+Proxy+Server). Generic web application best practices apply (like disabling http access to TeamCity at all).

For small servers, you can set up HTTPS via the internal [Tomcat means](https://tomcat.apache.org/tomcat-8.5-doc/ssl-howto.html), but this is not recommended as it may significantly increase the CPU load.

For configuring clients to access TeamCity server via HTTPS while using self-signed certificate, check the [related instructions](using-https-to-access-teamcity-server.md).

## Configure TeamCity to Use Proxy Server for Outgoing Connections
{product="tc"}

This section describes configuring TeamCity to use a proxy server for outgoing HTTP connections. To connect TeamCity behind a proxy to Amazon EC2 cloud agents, see [this section](setting-up-teamcity-for-amazon-ec2.md#Proxy+settings).

A TeamCity server can use a proxy server for certain outgoing HTTP connections to other services like issues trackers.

To point TeamCity to your proxy server, set the following [internal properties](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties):

```Shell

# For HTTP protocol
## The domain name or the IP address of the proxy host and the port:
teamcity.http.proxyHost=proxy.domain.com
teamcity.http.proxyPort=8080
 
## The hosts that should be accessed without going through the proxy, usually internal hosts. Provide a list of hosts, separated by the '|' character. The wildcard '*' can be used:
teamcity.http.nonProxyHosts=localhost|*.mydomain.com
 
## For an authenticated proxy add the following properties:
### Authentication type. "basic" and "ntlm" values are supported. The default is basic.
teamcity.http.proxyAuthenticationType=basic
### Login and Password for the proxy:
teamcity.http.proxyLogin=username
teamcity.http.proxyPassword=password
 
# For HTTPS protocol
## The domain name or the IP address of the proxy host and the port:
teamcity.https.proxyHost=proxy.domain.com
teamcity.https.proxyPort=8080
 
## The hosts that should be accessed without going through the proxy, usually internal hosts. Provide a list of hosts, separated by the '|' character. The wildcard '*' can be used:
teamcity.https.nonProxyHosts=localhost|*.mydomain.com
 
## For an authenticated proxy add the following properties:
### Authentication type. "basic" and "ntlm" values are supported. The default is basic.
teamcity.https.proxyAuthenticationType=basic
### Login and Password for the proxy:
teamcity.https.proxyLogin=login
teamcity.https.proxyPassword=password

```

## Configure TeamCity Agent to Use Proxy To Connect to TeamCity Server

This section covers the configuration of a proxy server for TeamCity agent-to-server connections.

<chunk include-id="agent-proxy-server">

On the TeamCity agent side, specify the proxy to connect to TeamCity server using the following properties in the [`buildAgent.properties`](build-agent-configuration.md) file:


```Shell

## The domain name or the IP address of the proxy host and the port
teamcity.http.proxyHost=123.45.678.9
teamcity.http.proxyPort=8080
 
## If the proxy requires authentication, specify the login and password
teamcity.http.proxyLogin=login
teamcity.http.proxyPassword=password
```

Note that the proxy has to be configured not to cache any TeamCity server responses; for example, if you use Squid, add "cache deny all" line to the `squid.conf` file.

</chunk>

## Install Multiple Agents on the Same Machine

See the [corresponding section](setting-up-and-running-additional-build-agents.md#Installing+Several+Build+Agents+on+the+Same+Machine) under agent installation documentation.

## Change Server Port
{product="tc"}

See [corresponding section](installing-and-configuring-the-teamcity-server.md#Changing+Server+Port) in server installation instructions.

## Test-drive Newer TeamCity Version before Upgrade
{product="tc"}

It's advised to try a new TeamCity version before upgrading your production server. The usual procedure is to [create a copy](#Create+a+Copy+of+TeamCity+Server+with+All+Data) of your production TeamCity installation, then [upgrade](upgrade.md) it, try the things out, and, when everything is checked, drop the test server and upgrade the main one. When you start the test server, remember to change the Server URL, disable Email and Jabber notifiers as well as [other features](#Copied+Server+Checklist) on the new server.

## Create a Copy of TeamCity Server with All Data
{product="tc"}

In case you want to preserve the original server as well as the copy, make sure to check the [licensing considerations](#Licensing+issues).

### Create a Server Copy

You can create a copy of the server using TeamCity backup functionality or manually. Manual approach is recommended for the complete server move.

### Use TeamCity Backup

1. Create a [backup](teamcity-data-backup.md) including everything. (You can skip the option to backup build logs is you are moving the artifacts, see below).
2. Install a new TeamCity server of the same version that you are already running. Ensure that:
   * the appropriate environment is configured (see the notes below) 
   * the server uses its own `<[TeamCity Data Directory](teamcity-data-directory.md)>` and its own [database](setting-up-an-external-database.md) (check the `<[TeamCity Data Directory](teamcity-data-directory.md)>config/database.properties` file)
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
3. Either perform clean [installation](installing-and-configuring-the-teamcity-server.md) or copy the TeamCity binaries ([`TeamCity Home Directory`](teamcity-home-directory.md)) into a new place (the `temp` and `work` subdirectories can be omitted during copying). Use exactly the same TeamCity version. If you plan to upgrade after copying, perform the upgrade only after you have the existing version up and running.
4. Copy `<[TeamCity Data Directory](teamcity-data-directory.md)>`. If you do not need the full copy, refer to the items below for options.
    * `.BuildServer/config` to preserve projects and build configurations settings
    * `.BuildServer/lib` and `.BuildServer/plugins` if you have them
    * files from the root of `.BuildServer/system` if you use an internal database and you do not want to move this database
    * `.BuildServer/system/artifacts` (optional) if you want build artifacts and build logs (including tests failure details) to be preserved on the new server
    * `.BuildServer/system/changes` (optional) if you want personal changes to be preserved on the new server
    * `.BuildServer/system/pluginData` (optional) if you want to preserve the state of various plugins, build triggers, and settings audit diff
    * `.BuildServer/system/caches` and `.BuildServer/system/caches` (optional) are not necessary to copy to the new server, they will be recreated on startup, but can take some time to be rebuilt (expect some slow down)
5. Artifacts directory is usually large. If you need to minimize the downtime of the server when moving it, you can use the generic approach for copying the data: use rsync, robocopy or alike tool to copy the data while the original server is running. Repeat the sync run several times until the amount of synced data reduces. Run the final sync after the original server shutdown. Alternative solution for the server move is to make the old data artifacts directory accessible to the new server and configure it as a second [location of artifacts](teamcity-configuration-and-maintenance.md). Then copy the files over from this second location to the main one while the server is running. Restart the server after copying completion.
6. Create a copy of the [database](setting-up-an-external-database.md), which your TeamCity installation is using, in a new schema or new database server. This can be done with database-specific tools or with the bundled maintainDB tool by [backing up](creating-backup-via-maintaindb-command-line-tool.md) database data and then [restoring](restoring-teamcity-data-from-backup.md) it.
7. Configure the new TeamCity installation to use proper `<[TeamCity Data Directory](teamcity-data-directory.md)>` and [database](setting-up-an-external-database.md) (`.BuildServer/config/database.properties` points to a copy of the database).
8. Perform the necessary [environment transfer](#Environment+transferring).

>If you want to do a quick check and do not need to preserve the build history on the new server, you can skip Step 6 (cloning database) and all the optional items of Step 4.

<anchor name="environment_transfer"/>

### Environment transferring
[//]: # (AltHead: environment_transfer)

Consider transferring the relevant environment if it was specially modified for an existing TeamCity installation. This might include:
* use the appropriate OS user account for running the TeamCity server process with properly configured settings, global and file system permissions
* use the same [TeamCity process launching options](configuring-teamcity-server-startup-properties.md), specifically check/copy environment variables starting with `TEAMCITY_`
* ensure any files/settings that were configured in the TeamCity web UI with absolute paths are accessible
* if relying on the OS-level user/machine settings like default SSH keys, cached VCS access credentials, transfer them as well
* consider replicating any special settings or exceptions related to the machine in the network configuration, and so on
* if the TeamCity installation was patched in any way (GroovyPlug plugin, native driver for MS SQL Server integrated security authentication), apply the same modifications to the installation copy
* if you run TeamCity with the OS startup (for example, Windows service), make sure the same configuration is performed on the new machine
* review and transfer settings in the `<[TeamCity Home](teamcity-home-directory.md)>\conf\teamcity-startup.properties` file
* consider any custom settings in `<[TeamCity Home](teamcity-home-directory.md)>\conf\server.xml`

<anchor name="copy_server_license"/>

### Licensing issues
{product="tc"}

A single TeamCity license __cannot be used on two running servers__ at the same time.
* A copy of the server created for redundancy/backup purposes can use the same license as only one of the servers will be running at a time.
* A copy of the server created for testing purposes requires an additional license. You can get the time-limited TeamCity [evaluation license](licensing-policy.md) once from the official TeamCity [download page](http://www.jetbrains.com/teamcity/download/). If you need an extension of the license or you have already evaluated the same TeamCity version, please [contact our sales department](http://www.jetbrains.com/company/contacts/index.html#Contacts_?TeamCity).
* A copy of the server intended to run at the same time as the main one regularly/for production purposes requires a separate license.

### Copied Server Checklist
{product="tc"}

If you are creating a copy (as opposed to moving the server this way), it is important to go through the checklist below:
* ensure the new server is configured to use another Data Directory and another database than the original server; check also "Artifact directories" setting on server's Global Settings;
* change server unique id by removing "uuid" attribute from XML of `<[TeamCity Data Directory](teamcity-data-directory.md)>\config\main-config.xml` file before the first start;
* ensure the same license keys are not used on several servers ([more on licensing](#Licensing+issues));
* update [Server URL](configuring-server-url.md) on the __Administration | Global Settings__ page to the actual URL of the server;
* check that you can successfully authenticate on the new server, use [super user](super-user.md) access if necessary;
* check that VCS servers, issue tracker servers, email and Jabber server and other server-accessed systems are accessible;
* check that any systems configured to push events to TeamCity server (like VCS hooks, automated build triggering, monitors, etc.) are updated to know about the new server;
* review the list of installed plugins to determine if their settings need changes;
* install new agents (or select some from the existing ones) and configure them to connect to the new server (using the new server URL);
* check that clients reading from the server (downloading artifact, using server's REST API, NuGet feed, etc.) are reconfigured, if necessary.

If you are creating a __test server__, you need to ensure that the users and production systems are not affected. Typically, this means you need to:
* disable Email, Jabber (in the "Administration &gt; Notifier" sections) and possibly also custom notifiers or change their settings to prevent the new server from sending out notifications;
* disable email verification (in the "Administration &gt; Authentication" section);
* be sure not to run any builds which change (for example, deploy to) production environments. This also typically includes Maven builds deploying to non-local repositories. You can prevent any builds from starting by pausing the [build queue](build-queue.md);
* disable cloud integration (so that it does not interfere with the main server);
* disable external artifact storage (as otherwise running/deleting builds and server clean-up will affect the storage which might be used by the production server);
* disable Docker registry clean-up (or just disable clean-up on the server);
* disable Commit Status Publishing;
* disable any plugins which push data into other non-copied systems based on the TeamCity events;
* disable functionality to [store project settings in VCS](storing-project-settings-in-version-control.md): set `teamcity.versionedSettings.enabled=false` internal property;
* consider significantly increasing [VCS checking for changes interval](configuring-vcs-roots.md#Common+VCS+Root+Properties) (server-wide default and overridden in the VCS roots) or changing settings of the VCS roots to prevent them from contacting production servers. See also [TW-47324](https://youtrack.jetbrains.com/issue/TW-47324).

See also the section below on [moving the server](#Move+TeamCity+Installation+to+a+New+Machine) from one machine to another.
{product="tc"}

[//]: # (Internal note. Do not delete. "How To...d160e1901.txt")  

## Move TeamCity Projects from One Server to Another
{product="tc"}

If you need to move data to a fresh server without existing data, it is recommended to [move the server](#Move+TeamCity+Installation+to+a+New+Machine) or [copy](#Create+a+Copy+of+TeamCity+Server+with+All+Data) it and then delete the data which is not necessary on the new server.

If you need to join the data with already existing data on the new server, there is a dedicated feature to move projects with most of the associated data from one server to another: [Projects Import](projects-import.md).

_Here are some notes on manual move of the settings in case you ever want to perform it_

It is possible to move _settings_ of a project or a build configuration to another server with simple file copying.

The two TeamCity servers (source and target) should be of exactly the same version (same build).

All the [identifiers](identifier.md) throughout all the projects, build configurations and VCS roots of both servers should be unique. If they are not, you can change them via web UI. If entities with the same id are present on different servers, the entities are assumed to be the same. For example this is useful for having global set of VCS roots on all the servers.

To move settings of the project and all its build configuration from one server to another:

From the [`TeamCity Data Directory`](teamcity-data-directory.md), copy the directories of corresponding projects (`.BuildServer\config\projects\<id>`) and all its parent projects to `.BuildServer\config\projects` of the target server. This moves project settings, build configuration settings, VCS roots defined in the projects preserving the links between them. If there are same-named files on the target server as those copied, this can happen in case of 
  a) id match: same entities already exist on the target server, in which case the clashing files can be excluded from copying, or 
  b) id clash: different entities happen to have same ids. In this case it should be resolved either by changing entity id on the source or target server to fulfill the uniqueness requirement.

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
{product="tc"}

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
* [install](setting-up-and-running-additional-build-agents.md) a new agent on the new machine
* copy `conf/buildAgent.properties` from the old installation to a new one
* start the new agent.
With these steps the agent will be recognized by TeamCity server as the same and will perform [clean checkout](clean-checkout.md) for all the builds.

Please also review the [section](agent-home-directory.md) for a list of directories that can be deleted without affecting builds consistency.

[//]: # (Internal note. Do not delete. "How To...d160e2180.txt")    

## Share the Build number for Builds in a Chain Build

A build number can be shared for builds connected by a [snapshot dependency](dependent-build.md#Snapshot+Dependency) or an [artifact dependency](dependent-build.md#Artifact+Dependency) using a reference to the following dependency property: `%dep.<btID>.system.build.number%`.

For example, you have build configurations A and B that you want to build in sync: use the same sources and take the same build number.   
Do the following:
1. Create build configuration C, then snapshot dependencies: __A on C__ and __B on C__.
2. Set the [Build number format](configuring-general-settings.md#Build+Number+Format) in A and B to:

```Shell

%dep.<btID>.system.build.number%
```

Where `<btID>` is the [ID of the build configuration](build-configuration.md) C. The approach works best when builds reuse is turned off via the [Snapshot Dependencies](snapshot-dependencies.md) snapshot dependency option set to off.

[Read more](predefined-build-parameters.md#Configuration+Parameters) about dependency properties.

Please watch/comment the issue related to sharing a build number [TW-7745](http://www.jetbrains.net/tracker/issue/TW-7745).

## Make Temporary Build Files Erased between the Builds

Update your build script to use path stored in `${teamcity.build.tempDir}` (Ant's style name) property as the temp directory. TeamCity agent creates the [directory](agent-home-directory.md#temp-dir) before the build and deletes it right after the build.

## Clear Build Queue if It Has Too Many Builds due to a Configuration Error

Try pausing the build configuration that has the builds queued. On build configuration pausing all its builds are removed form the queue.   
Also there is an ability to delete many builds from the build queue in a single dialog.

## Automatically create or change TeamCity build configuration settings

If you need a level of automation and web administration UI does not suite your needs, there several possibilities:
* use [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html)
* change configuration files directly on disk (see more at `<[TeamCity Data Directory](teamcity-data-directory.md)>`)
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

TeamCity has enough features to handle orchestration part of the deployments with the actual deployment logic configured in the build script / build runner. TeamCity supports a variety of generic build tools, so any specific tool can be run from within TeamCity. To ease specific tool usage, it is possible to wrap it into a meta-runner or write a custom plugin for that.

In general, setup steps for configuring deployments are:
1. Write a build script that will perform the deployment task for the binary files available on the disk. (e.g. use Ant or MSBuild for this. For typical deployment transports use [Deployer](deployers.md) runners). See also [Integrate with Build and Reporting Tools](#Integrate+with+Build+and+Reporting+Tools). You can use [Meta-Runner](working-with-meta-runner.md) to reuse a script with convenient UI.
2. Create a build configuration in TeamCity that will execute the build script and perform the actual deployment. If the deployment is to be visible or startable only by the limited set of users, place the build configuration in a separate TeamCity project and make sure the users have appropriate permissions in the project.
3. In this build configuration configure [artifact dependency](dependent-build.md#Artifact+Dependency) on a build configuration that produces binaries that need to be deployed.
4. Configure one of the available triggers in the deploying build configuration if you need the deployment to be triggered automatically (e.g. to deploy last successful of last pinned build), or use "Promote" action in the build that produced the binaries to be deployed.
5. Consider using [snapshot dependencies](dependent-build.md#Snapshot+Dependency) in addition to artifact ones and check [Build Chains](build-chain.md) tab to get the overview of the builds. In this case artifact dependency should use "Build from the same chain" option.
6. If you need to parametrize the deployment (e.g. specify different target machines in different runs), pass parameters to the build script using [custom build run dialog](triggering-a-custom-build.md). Consider using [Typed Parameters](typed-parameters.md) to make the custom run dialog easier to use or handle passwords.
7. If the deploying build is triggered manually consider also adding commands in the build script to pin and tag the build being deployed (via sending a [REST API](https://www.jetbrains.com/help/teamcity/rest/manage-builds.html#Build+Tags) request). You can also [use a build number](#Share+the+Build+number+for+Builds+in+a+Chain+Build) from the build that generated the artifact.

Further recommendations:
* Setup a separate build configurations for each target environment
* Use build's Dependencies tab for navigation between build producing the binaries and deploying builds/tasks
* If necessary, use parameter with "prompt" display mode to ask for "[confirmation](https://youtrack.jetbrains.com/issue/TW-8284#comment=27-290611)" on running a build
* [Change title](https://youtrack.jetbrains.com/issue/TW-20617#comment=27-527943) of the build configuration "Run" button


[//]: # (Internal note. Do not delete. "How To...d160e2430.txt")

## Use an External Tool that My Build Relies on
{product="tc"}

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
{product="tcc"}

If you need to use specific external tool to be installed on a build agent to run your builds, you have the following options:
* Check in the tool into the version control and use relative paths.
* Add environment preparation stage into the build script to get the tool form elsewhere.
* Create a separate build configuration with a single "fake" build which would contain required files as artifacts, then use artifact dependencies to send files to the target build.

## Integrate with Build and Reporting Tools

If you have a build tool or a tool that generates some report/provides code metrics which is not yet [supported by TeamCity](teamcity-documentation.md#TeamCity+2020.x+Supported+Platforms+and+Environments) or any of the [plugins](https://plugins.jetbrains.com/teamcity), most probably you can use it in TeamCity even without dedicated integration.
{product="tc"}

If you have a build tool or a tool that generates some report/provides code metrics which is not yet [supported by TeamCity](teamcity-cloud-documentation.md#TeamCity+Cloud+Supported+Platforms+and+Environments) or any of the [plugins](https://plugins.jetbrains.com/teamcity), most probably you can use it in TeamCity even without dedicated integration.
{product="tcc"}

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

A metrics value can be published as TeamCity statistics via [service message](service-messages.md#Reporting+Build+Statistics) and then displayed in a [custom chart](customizing-statistics-charts.md#Showing+Charts+Only+for+Specific+Build+Configurations+on+Project+Level). You can also configure [build failure condition](build-failure-conditions.md#Fail+build+on+metric+change) based on the metric.

If the tool reports code-attributing information like Inspections or Duplicates, TeamCity-bundled report can be used to display the results. A custom plugin will be necessary to process the tool-specific report into TeamCity-specific data model. Example of this can be found in [XML Test Reporting](xml-report-processing.md) plugin and FXCop plugin (see a link on [Open-source Bundled Plugins](https://confluence.jetbrains.com/display/TW/Open-source+Bundled+Plugins)).

See also [Import coverage results in TeamCity](#Import+coverage+results+in+TeamCity).

For advanced integration, a custom plugin will be necessary to store and present the data as required. See [Developing TeamCity Plugins](https://plugins.jetbrains.com/docs/teamcity/developing-teamcity-plugins.html) for more information on plugin development.

## Restore Just Deleted Project
{product="tc"}

TeamCity moves deleted projects settings directories (which are named after the project id) to `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/_trash` directory adding `"<internal ID>"` suffix to the directories.

To restore a project, find the project directory in the `_trash` directory and move it into regular projects settings directory: `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/projects` while removing the `".projectN"` suffix from the directory name.   
You can do this while server is running, it should pick up the restored project automatically.

Note that TeamCity preserves builds history and other data stored in the database for deleted projects/build configurations for 5 days after the deletion time. All the associated data (builds and test history, changes, etc.) is removed during the next clean-up after the [configurable](clean-up.md#Deleted+Build+Configurations+Clean-up) (5 days by default) timeout elapses. 

The `config/_trash` directory is not cleaned automatically and can be emptied manually if you are sure you do not need the deleted projects. No server restart is required.

## Transfer 3 Default Agents to Another Server
{product="tc"}

This is not possible.

Each TeamCity server (Professional and Enterprise) allows using 3 or more agents bound to the server without any agent licenses. In case of the Professional server, by default 3 agents are bound to the server instance: users do not pay for these agents, there is no license key for them.   
In case of the Enterprise server, the number of agents depends on your package and the agents are bound to the server license key.

So, the agents bound to the server cannot be transferred to another server.

If you need more build agents that are included with your TeamCity server, you can purchase additional build agent licenses and connect more agents in addition to those that come bound with the server.

See [more](licensing-policy.md) on licensing.

## Import coverage results in TeamCity

TeamCity comes bundled with IntelliJ IDEA/Emma and, JaCoCo coverage engines for Java and dotCover/NCover/PartCover for .NET.

However, there are plenty of other coverage tools out there, like [Cobertura](http://cobertura.sourceforge.net/index.html) and others which are not directly supported by TeamCity.

In order to achieve similar experience with these tools you can:
* publish a coverage HTML report as TeamCity artifact: most of the tools produce coverage report in HTML format, you can publish it as artifact and [configure report tab](including-third-party-reports-in-the-build-results.md) to show it in TeamCity. If artifact is published in the root artifact directory and its name is `coverage.zip` and there is `index.html` file in it, report tab will be shown automatically. As to running an external tool, check [Integrate with Build and Reporting Tools](#Integrate+with+Build+and+Reporting+Tools).
* extract coverage statistics from coverage report and publish [statistics values](custom-chart.md#Default+Statistics+Values+Provided+by+TeamCity) to TeamCity with help of [service message](service-messages.md#Reporting+Build+Statistics): if you do so, you'll see coverage chart on build configuration Statistics tab and also you'll be able to fail a build with the help of a build failure condition on a metric change (for example, you can fail build if the coverage drops).

<note>

You should not publish values CodeCoverageB, CodeCoverageL, CodeCoverageM, CodeCoverageC standing for block/line/method/class coverage percentage. TeamCity will calculate these values using their absolute parts. For example, CodeCoverageL will be calculated as CodeCoverageAbsLCovered divided by CodeCoverageAbsLTotal. You could publish these values but in this case they will lack decimal parts and will not be useful.
</note>

## Recover from "Data format of the Data Directory (NNN) and the database (MMM) do not match" error
{product="tc"}

If you get "Data format of the Data Directory (NNN) and the database (MMM) do not match." error on starting TeamCity, it means either the database or the TeamCity Data Directory were recently changed to an inconsistent state so they cannot be used together. Double-check the database and Data Directory locations and change them if they are not those where the server used to store the data.

If they are right, most probably it means that the server was upgraded with another database or Data Directory and the [consistent upgrade](upgrade.md#Upgrading+TeamCity+Server) requirement was not met for your main Data Directory and the database.

To recover from the state you will need backup of the consistent state made prior to the upgrade. You will need to restore that backup, ensure the right locations are used for the Data Directory and the database and perform the TeamCity upgrade.

## Debug a Build on a Specific Agent

In case a build fails on some agent, it is possible to debug it on this very agent to investigate agent-specific issues. Do the following:
1. Go to the __Agents__ page in the TeamCity web UI and [select the agent](viewing-build-agent-details.md).
2. [Disable the agent](build-agents-configuration-and-maintenance.md#Enabling%2FDisabling+Agents+via+UI) to temporarily remove it from the [build grid](build-grid.md). Add a comment (optional). To enable the agent automatically after a certain time period, check the corresponding box and specify the time.
3. [Select the build](working-with-build-results.md) to debug.
4. Open the [Custom Run](triggering-a-custom-build.md#Run+Custom+Build+dialog) dialog and specify the following options: 
    a. In the __Agent__ drop-down menu, select the disabled agent.
    b. It is recommended to select the __run as [Personal Build](personal-build.md)__ option to avoid intersection with regular builds.
5. When debugging is complete, enable the agent manually if automatic reenabling has not been configured.

You can also perform [remote debugging](remote-debug.md) of tests on an agent via the [IntelliJ IDEA plugin](intellij-platform-plugin.md) for TeamCity.

## Debug a Part of the Build (a build step)

If a build containing several steps fails at a certain step, it is possible to debug the step that breaks. Do the following:
1. Go to the build configuration and disable the build steps up to the one you want to debug.
2. [Select the build](working-with-build-results.md) to debug.
3. Open the [Custom Run](triggering-a-custom-build.md#Run+Custom+Build+dialog) dialog and select the __put the build to the [queue top](build-queue.md)__ to give you build the priority.
4. When debugging is complete, reenable the build steps.

## Watch Several TeamCity Servers with Windows Tray Notifier
{product="tc"}

TeamCity Tray Notifier is used normally to watch builds and receive notifications from a single TeamCity server. However, if you have more than one TeamCity server and want to monitor them with Windows Tray Notifier simultaneously, you need to start a separate instance of Tray Notifier for each of the servers from the command line with the `/allowMultiple` option:
* From the TeamCity Tray Notifier installation folder (by default, it's `C:\Program Files\JetBrains\TeamCity`) run the following command:


```Shell

JetBrains.TrayNotifier.exe /allowMultiple
```

Optionally, for each of the Tray Notifier instances you can explicitly specify the URL of the server to connect using the `/server` option. Otherwise, for each further tray notifier instance you will need to log out and change the server's URL via the UI.

```Shell

JetBrains.TrayNotifier.exe /allowMultiple /server:http://myTeamCityServer
```

See also [details](http://jetbrains.net/tracker/issue/TW-4230#comment=27-14194) in the issue tracker.

## Personal User Data Processing
{product="tc"}

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
{product="tc"}

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

If you want to encrypt the data used by TeamCity, it is recommended to use generic, non-TeamCity-specific tools for this as TeamCity does not provide dedicated functionality. TeamCity stores the data in the SQL database and on the file system. You can configure the database to store the data in encrypted form and use secure JDBC\-backed connection to the database (configured in the [`database.properties`](setting-up-an-external-database.md)).   
Also, you can configure encryption on the disk storage on the OS level.

### Logs and Debugging Data

If you want to ensure that you do not store the internal TeamCity logs for more than a limited amount of days, you can configure [internal logging](teamcity-server-logs.md) to rotate the log files each day and limit the number of files to keep. TeamCity agents generally do not operate with any user\-related data in a structural way, but if you need to ensure the logs are regularly rotated on the agent, you will need to configure [agent logging](viewing-build-agent-logs.md) the same way.

Per\-day rotation can be configured by adding `<param name="rotateOnDayChange" value="true"/>` line within all required `<appender name="..." class="jetbrains.buildServer.util.TCRollingFileAppender">` appenders. This change should be done to the default `conf\teamcity-server-log4j.xml` and also logging presets stored under `<TeamCity Data Directory>\config\_logging`.

TeamCity can also store diagnostics data like thread dumps which can record user-related data in unstructured way. It is recommended to review the content of the `<[TeamCity Home](teamcity-home-directory.md)>\logs` directory regularly and ensure that no old files are preserved in there. Also, the extra logs should be deleted after logging customization sessions like collecting debug logs, etc. There is a [known issue](https://youtrack.jetbrains.com/issue/TW-22596) that `logs\catalina.out` file if not rotated automatically at all. It is recommended to establish an automatic procedure to rotate the file regularly.

### Customizations

These notes only address bundled TeamCity functionality with the most common documented settings. You should assess your specific TeamCity installation considering customizations like the configured build scripts, installed plugins, external systems communicating with TeamCity via API, etc.

[//]: # (Internal note. Do not delete. "How To...d160e3129.txt")    
