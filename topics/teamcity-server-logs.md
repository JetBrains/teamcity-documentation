[//]: # (title: TeamCity Server Logs)
[//]: # (auxiliary-id: TeamCity Server Logs)

TeamCity Server keeps a log of internal activities that can be examined to investigate an issue with the server behavior or get internal error details.

The logs are stored in plain text files in a disk directory on the TeamCity server machine (usually in `<[TeamCity Server home](teamcity-home-directory.md)>/logs`). The files are appended with messages when TeamCity is running.

While the server is running, the logs can be viewed in the web UI on the `Server Logs` tab of __Administration | Diagnostics__ section.

<anchor name="enable-server-logs-debug"/>

>__Enable Debug in Server Logs__  
>In the web UI, go to __Administration | Diagnostics__ page. On the __Troubleshooting__ tab, choose an active logging preset. To view logs, go to the __Server Logs__ tab.  
>If it is not possible to enable debug logging mode from the TeamCity web UI, refer to [this section](#Changing+Logging+Configuration) to learn how to adjust logging options manually.
> 
{id="enable-debug-in-server-logs" auxiliary-id="enable-debug-in-server-logs"}

## General Logging Description

TeamCity uses [log4j 2.x library](http://logging.apache.org/log4j/2.x) for the logging and its settings can be [customized](#Changing+Logging+Configuration). Versions prior to TeamCity 2022.04 used Log4j 1.x for the logging.

By default, log files are located under the `<[TeamCity Server home](teamcity-home-directory.md)>/logs` directory.

The most important log files are:

<table>

<tr><td></td><td></td></tr>

<tr>

<td>

`teamcity-server.log`

</td>

<td>

General server log

</td></tr><tr>

<td>

`teamcity-activities.log`

</td>

<td>

Log of user-initiated and main build-related events

</td></tr><tr>

<td>

`teamcity-vcs.log`

</td>

<td>

Log of [VCS](configuring-vcs-roots.md)-related activity

</td></tr><tr>

<td>

`teamcity-cleanup.log`

</td>

<td>

Contains [clean-up](teamcity-data-clean-up.md)-related log

</td></tr><tr>

<td>

`teamcity-commit-status.log`

</td>

<td>

Contains [](commit-status-publisher.md)-related log. Apply the `debug-commit-status` preset to write DEBUG-level messages to this log. 

</td></tr><tr>

<td>

`teamcity-notifications.log`

</td>

<td>

[Notifications](adding-notification-rules.md)-related log

</td></tr><tr>

<td>

`teamcity-pull-requests.log`

</td>

<td>

[](pull-requests.md)-related log. Apply the `debug-pull-requests` preset to include DEBUG-level messages to this log.

</td></tr><tr>

<td>

`teamcity-clouds.log`

</td>

<td>

(off by default) [Cloud-integration](teamcity-integration-with-cloud-solutions.md)-related log

</td></tr><tr>

<td>

`teamcity-sql.log`

</td>

<td>

(off by default) Log of SQL queries, see [details](reporting-issues.md#Database-related+Slowdowns)

</td></tr><tr>

<td>

`teamcity-http-auth.log`

</td>

<td>

(off by default) Log with messages related to [NTLM](ntlm-http-authentication.md) and other authentication for HTML requests

</td></tr><tr>

<td>

`teamcity-xmlrpc.log`

</td>

<td>

(off by default) Log of messages sent by the server to agents and IDE plugins via XML-RPC

</td></tr><tr>

<td>

`vcs-content-cache.log`

</td>

<td>

(off by default) Log related to individual file content requests from VCS

</td></tr><tr>

<td>

`teamcity-rest.log`

</td>

<td>

(off by default) [REST-API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html) related logging

</td></tr><tr>

<td>

`teamcity-freemarker.log`

</td>

<td>

(off by default) [Notification templates](customizing-notification-templates.md) processing-related logging

</td></tr><tr>

<td>

`teamcity-agentPush.log`

</td>

<td>

(off by default) Logging related to [agent push](install-and-start-teamcity-agents.md) operations

</td></tr><tr>

<td>

`teamcity-remote-run.log`

</td>

<td>

(off by default) Logging related to [personal builds](personal-build.md) processing on the server

</td></tr><tr>

<td>

`teamcity-svn.log`

</td>

<td>

(off by default) [SVN integration](subversion.md) log

</td></tr><tr>

<td>

`teamcity-tfs.log`

</td>

<td>

(off by default) [TFS integration](team-foundation-version-control.md) log

</td></tr><tr>

<td>

`teamcity-starteam.log`

</td>

<td>

(off by default) [StarTeam integration](starteam.md) log

</td></tr><tr>

<td>

`teamcity-clearcase.log`

</td>

<td>

(off by default) [ClearCase integration plugin](https://plugins.jetbrains.com/plugin/13210-vcs-support-clearcase) log

</td></tr><tr>

<td>

`teamcity-ldap.log`

</td>

<td>

[LDAP](ldap-integration.md)-related log

</td></tr><tr>

<td>

`teamcity-nuget.log`

</td>

<td>

[NuGet](nuget.md)-related log

</td></tr><tr>

<td>

`teamcity-maintenance.log`

</td>

<td>

(off by default) logs of back-up/ restore/ migration performed with [maintainDB tool](migrating-to-external-database.md#Full+Migration)

</td></tr><tr>

<td>

`teamcity-maintenance-truncation.log`

</td>

<td>

(off by default) contains extended information on possible data truncation during back-up/ restore/ migration performed with [maintainDB tool](migrating-to-external-database.md)

</td></tr><tr>

<td>

`teamcity-versioned-settings.log`

</td>

<td>

(off by default) contains information on [synchronization of the project settings](storing-project-settings-in-version-control.md) with the version control

</td></tr><tr>

<td>

`teamcity-ws.log`

</td>

<td>

logs related to communication between browsers and the TeamCity server using the [WebSocket connection](server-health.md#WebSocket+connection+issues)

</td></tr><tr>

<td>

`teamcity-issue-trackers.log`

</td>

<td>

logs related to communication between TeamCity and configured [issue tracker](integrating-teamcity-with-issue-tracker.md)

</td></tr></table>

Other files can also be created on [changing Logging Configuration](#Changing+Logging+Configuration).

Some of the files can have `.N` extensions — that are files with previous logging messages copied on main file rotation. See [`<DefaultRolloverStrategy/>`](#Changing+Logging+Settings) for preserving more files.

## Logging-related Diagnostics UI

Users with System Administrator role can view and download the server logs right from the TeamCity UI using __Administration | Diagnostics | Server Logs__.

<anchor name="TeamCityServerLogs-loggingPreset"/>

## Changing Logging Configuration

While TeamCity is running, active logging settings can be changed by selecting between available _logging presets_.

>We highly recommend using the Default debug logging preset unless otherwise is suggested by a TeamCity support engineer. Using a different preset creates an extra load on the file system and might slow down various TeamCity operations.
>
{style="note"}

The active logging preset is changed in the __Administration | Diagnostics | Troubleshooting | Debug logging__ section. Choosing a preset changes logging configuration immediately, and the preset is preserved after a server restart, until manually changed on this page again. It is strongly advised to return to the Default preset once the necessary logs were collected.

The available presets are stored in the files with the `.xml` extension under the `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/_logging` directory. New files can be added into the directory and existing files can be modified (using the [`.dist` convention](teamcity-data-directory.md#.dist+Template+Configuration+Files)). New presets can also be uploaded via __Diagnostics | Logging Presets__.

If it is not possible to enable debug logging mode via logging presets (for example, to get the logging during server initialization) or to make persistent changes to the logging, you can back up the `conf/teamcity-server-log4j.xml` file and copy/rename the `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/_logging/debug-general.xml` file over `conf/teamcity-server-log4j.xml` before the server start.

### Changing Logging Settings

If you want to fine-tune the log4j configuration, you can edit `<[TeamCity Server home](teamcity-home-directory.md)>/conf/teamcity-server-log4j.xml` file. If the server is running, the log4j configuration file will be reloaded automatically and the logging configuration will be changed on the fly (some log4j restrictions still apply, so for a massive change consider restarting the server).

If you change settings in other preset files, like `debug-all.xml` or `debug-vcs.xml`, the settings won't be picked up automatically. To enforce the change, switch logging preset to some other value, like `<Default>` and then switch back to `debug-all`. 

Most useful settings of log4j configuration:

To change the minimum log level to save in the file, tweak the `level` attribute of the `Logger` element.

```Plain Text
<Loggers>
    <Logger name="jetbrains.buildServer" level="INFO">
...

```

The logs are rotated by default. When debug is enabled, it makes sense to increase the `max` attribute of `<DefaultRolloverStrategy/>` element to affect the number of preserved log files. While doing so, ensure there is sufficient free disk space available.

```Plain Text
<DelegateAppender>
    ...
    <DefaultRolloverStrategy max="20" fileIndex="min"/>
...

```

For detailed description of `<DefaultRolloverStrategy/>` and other supported attributes, see [RollingFileAppender](https://logging.apache.org/log4j/2.x/manual/appenders.html#RollingFileAppender) in Apache documentation.

## Reading Logs

Each message has a timestamp and level (`ERROR`, `WARN`, `INFO`, `DEBUG`).

Example:
```Shell
[2010-11-19 23:22:35,657] INFO - s.buildServer.SERVER - Agent vmWin2k3-3 has been registered with id 19, not running a build

```

* `ERROR` means an operation failed and some data was lost or action not performed. Generally, there should be no ERRORs in the log.
* `WARN` generally means that an operation failed, but will be retried or the operation is considered not important. Some amount of WARNs is OK. But you can review the log for such warnings to better understand what is going OK and what is not.
* `INFO` is an informational message that just reports on the current activities.
* `DEBUG` is only useful for issue investigation, for example, to be analyzed by TeamCity developers.

### General Logging Configuration

By default, TeamCity searches for log4j configuration in the `.../conf/teamcity-server-log4j.xml` file (this resolves to `<[TeamCity Server home](teamcity-home-directory.md)>/conf/teamcity-server-log4j.xml` for TeamCity `.exe` and `.tar.gz` distributions when run from `bin`). If no such file is present, the default log4j configuration is used. The logs are saved to the `../logs` directory by default.

If necessary, you can change the path to the configuration by adding the `log4j2.configurationFile` JVM option or [internal property](server-startup-properties.md). The path to the `logs` directory is configured via the `TEAMCITY_LOGS_PATH` environment variable, or, alternatively, via the `teamcity_logs` JVM option or [internal property](server-startup-properties.md).   
For example: `log4j2.configurationFile=file:../conf/teamcity-server-log4j.xml` and `teamcity_logs=../logs/`.   
Default values can be looked up in the `bin/teamcity-server` script available in the `.exe` and `tar.gz` distributions.

If you start TeamCity by the means other than the bundled `teamcity-server` or `runAll` scripts, make sure to pass the above-mentioned options to the server JVM. See also the [recommendations](install-and-start-teamcity-server.md) on installing TeamCity into not bundled web server.

The default `teamcity-server-log4j.xml` file content can be found in the `.exe` and `tar.gz` distributions. The one with debug enabled can be found under `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/_logging/debug-general.xml` name after server's first start. 

<seealso>
        <category ref="troubleshooting">
            <a href="viewing-build-agent-logs.md">Viewing Build Agent Logs</a>
        </category>
        <category ref="admin-guide">
            <a href="reporting-issues.md">Reporting Issues</a>
        </category>
</seealso>
