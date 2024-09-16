[//]: # (title: Viewing Build Agent Logs)
[//]: # (auxiliary-id: Viewing Build Agent Logs)

>This page is only relevant for [self-hosted build agents](teamcity-cloud-subscription-and-licensing.md#cloud-self-hosted-agents).
>
{type="note" instance="tcc"}

To analyze agent-specific cases, there are internal log files saved by the TeamCity agent process into `<TeamCity agent home>/logs` directory on the agent machine.

When the agent is connected to TeamCity server, you can __browse and download__ the agent logs in TeamCity web UI, on the __Logs__ tab for an agent.

If you need to customize the logging, see below.

## Log Files

TeamCity uses [Log4j 2.x](http://logging.apache.org/log4j/2.x) for internal logging of events. The default build agent Log4j configuration file is `<agent home>/conf/teamcity-agent-log4j2.xml`.

> Note: versions of TeamCity prior to 2022.04 used Log4j 1.x and the Log4j configuration file was located at `<agent home>/conf/teamcity-agent-log4j.xml` 

See the comments in the Log4j configuration file for enabling the DEBUG mode. Build agent logs are placed into `<agent home>/logs` directory. Normally, you do not need to restart the agent for the updated logging configuration to be applied.

<table><tr>

<td>

File name

</td>

<td>

Description

</td></tr><tr>

<td>

`teamcity-agent.log`

</td>

<td>

General build agent log

</td></tr><tr>

<td>

`teamcity-build.log`

</td>

<td>

`stdout` and `stderr` output of builds run by the agent

</td></tr><tr>

<td>

`teamcity-vcs.log`

</td>

<td>

VCS-related logging (for checkout mode "Automatically on agent")

</td></tr><tr>

<td>

`upgrade.log`

</td>

<td>

log of the build agent upgrade (logged by the upgrading process)

</td></tr><tr>

<td>

`launcher.log`

</td>

<td>

log of the agent's monitoring/launching process

</td></tr><tr>

<td>

`wrapper.log`

</td>

<td>

(only present when the agent is run as Windows service or by Java Service Wrapper) output of the process build agent launching process

</td></tr></table>

## Generic Debug Logging

To enable general debug logging on an agent, change the `jetbrains.buildServer` category logging priority in the `<agent home>/conf/teamcity-agent-log4j2.xml` file:

```XML
<Logger name="jetbrains.buildServer" level="DEBUG">
    <AppenderRef ref="ROLL"/>
</Logger>
```

If you're using TeamCity version < 2022.04 then the following replacement should be done in the `<agent home>/conf/teamcity-agent-log4j.xml` file:
```XML
<category name="jetbrains.buildServer">
    <priority value="DEBUG"/>
    <appender-ref ref="ROLL"/>
</category>
```

Then, see `teamcity-agent.log*` files.   

## VCS Debug Logging

To enable detailed VCS logging on an agent, change the VCS category logging priority in the `<agent home>/conf/teamcity-agent-log4j2.xml` file:

```XML
<Logger name="jetbrains.buildServer.VCS" level="DEBUG">
    <AppenderRef ref="ROLL.VCS"/>
</Logger>
```

If you're using TeamCity version < 2022.04 then the following replacement should be done in the `<agent home>/conf/teamcity-agent-log4j.xml` file:

```XML
<category name="jetbrains.buildServer.VCS">
    <priority value="DEBUG"/>
    <appender-ref ref="ROLL.VCS"/>
</category>
```

Then, see `teamcity-vcs.log*` files.   

## Specific Debug Logging

To get dump of the data sent from the agent to the server, enable an agent XML-RPC log, by uncommenting the line below in the `<agent home>/conf/teamcity-agent-log4j2.xml` file.

```XML
<Logger name="jetbrains.buildServer.XMLRPC" level="DEBUG">
    <AppenderRef ref="ROLL.XMLRPC"/>
</Logger>

```

If you're using TeamCity version < 2022.04 then the following replacement should be done in the `<agent home>/conf/teamcity-agent-log4j.xml` file.

```XML
<category name="jetbrains.buildServer.XMLRPC">
    <priority value="DEBUG"/>
    <appender-ref ref="ROLL.XMLRPC"/>
</category>

```

Then, see `teamcity-xmlrpc.log`.   

## Advanced Logging Configuration
{instance="tc"}

You can configure location of the logs by altering the value of the `teamcity_logs` property (passed to JVM via `-D` option). You can also change the Log4j configuration file location by changing the value of the `log4j2.configuration` property. See the corresponding documentation [section](configuring-build-agent-startup-properties.md) on how to pass the options.

For additional options on tweaking logging, refer to the [TeamCity Server Logs](teamcity-server-logs.md#Changing+Logging+Configuration) page.

<seealso>
        <category ref="troubleshooting">
            <a href="reporting-issues.md">Reporting Issues</a>
        </category>
        <category ref="admin-guide" instance="tc">
            <a href="teamcity-server-logs.md">TeamCity Server Logs</a>
        </category>
</seealso>
