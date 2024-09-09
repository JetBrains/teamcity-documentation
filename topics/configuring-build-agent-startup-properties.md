[//]: # (title: Configuring Build Agent Startup Properties)
[//]: # (auxiliary-id: Configuring Build Agent Startup Properties)

>This page is only relevant for [self-hosted build agents](teamcity-cloud-subscription-and-licensing.md#cloud-self-hosted-agents).
>
{type="note" product="tcc"}

In TeamCity, a [build agent](build-agent.md) contains two processes:
* Agent launcher — a Java process that launches the agent process.
* Agent — the main process for a build agent; runs as a child process for the agent launcher.

Whether you run a build agent via the `agent.bat|sh` script or as a Windows service, the agent launcher starts first, and then it starts the agent itself. Remember that running an agent via script is the recommended approach but you might want to run it as a service in certain cases; refer to [this page](install-and-start-teamcity-agents.md) for more information.

>Please avoid changing the startup options unless you are advised to do so by the TeamCity support team or you are absolutely confident in consequences of these changes.
>
{style="note"}

## Agent Properties

For both processes mentioned above, you can customize the final agent behavior by specifying system properties and variables for the agent to run with.

### Build Agent is Run via Script

Before you run the `[<Agent_Home>](agent-home-directory.md)/bin/agent.bat|sh` script, set the following [environment variables](https://en.wikipedia.org/wiki/Environment_variable) in your OS:
* `TEAMCITY_AGENT_MEM_OPTS` — set agent memory JVM options (for 64-bit agents, consider setting `-Xmx` to 512 MB)
* `TEAMCITY_AGENT_OPTS` — additional agent JVM options

### Build Agent is Run as Service

In the `[<Agent_Home>](agent-home-directory.md)/launcher/conf/wrapper.conf` file, add the following lines (one per option):

```Plain Text
wrapper.app.parameter.<N>

```

<note>
 
You should add these extra lines _before_ the following line in the `wrapper.conf` file: `wrapper.app.parameter.N=jetbrains.buildServer.agent.AgentMain`.

Make sure to renumber all lines after the inserted ones.
</note>

### Build Agent is Run by Launchd (macOS)

If a TeamCity agent process is started by a [launchd](start-teamcity-agent.md#Automatic+Agent+Start+Under+macOS) script, you need to modify the corresponding `.plist` file to set up environment variables. The following `.plist` file entry sets JVM `XMX` parameter to increase the maximum memory allocation pool for a build agent to 512 MB.

```XML
<key>EnvironmentVariables</key>
    <dict>
        <key>TEAMCITY_AGENT_MEM_OPTS</key>
        <string>-Xmx512m</string>
    </dict>
```

## Agent Launcher Properties

Only rare cases might require changing agent launcher properties. Before modifying these, make sure your problem cannot be solved by changing the main agent process properties described [above](#Agent+Properties).

### Build Agent is Run via Script
{id="build-agent-is-run-via-script-1"}

Before you run the `[<Agent_Home>](agent-home-directory.md)/bin/agent.bat|sh` script, set the `TEAMCITY_LAUNCHER_OPTS` environment variable.

### Build Agent is Run as Service
{id="build-agent-is-run-as-service-1"}

In the `[<Agent_Home>](agent-home-directory.md)/launcher/conf/wrapper.conf` file, add the following lines (one per option, the `N` number should increase):

```Plain Text
wrapper.java.additional.<N>

```

<note>

Make sure to renumber all lines after the inserted ones.
</note>



[//]: # (Internal note. Do not delete. "Configuring Build Agent Startup Propertiesd71e106.txt")    

<seealso>
        <category ref="concepts">
            <a href="server-startup-properties.md" product="tc">Configuring TeamCity Server Startup Properties</a>
        </category>
        <category ref="admin-guide">
            <a href="agent-home-directory.md">Agent Home Directory</a>
            <a href="install-and-start-teamcity-agents.md">Setting up and Running Additional Build Agents</a>
        </category>
</seealso>