[//]: # (title: Configuring Build Agent Startup Properties)
[//]: # (auxiliary-id: Configuring Build Agent Startup Properties)

In TeamCity a [build agent](build-agent.md) contains two processes:
* Agent Launcher – a Java process that launches the agent process
* Agent – the main process for a Build Agent; runs as a child process for the agent launcher
Whether you run a build agent via the `agent.bat|sh` script or as a Windows service, at first the agent launcher starts and then it starts the agent.

<note>

You do not need to specify any of the options unless you are advised to do by the TeamCity support team or you know what you are doing.
</note>

## Agent Properties

For both processes above you can customize the final agent behavior by specifying system properties and variables for the agent to run with.

### Build Agent Is Run Via Script

Before you run the `<Agent Home>\bin\agent.bat|sh` script, set the following environment variables:
* `TEAMCITY_AGENT_MEM_OPTS` – set agent memory options (JVM options)
* `TEAMCITY_AGENT_OPTS` – additional agent JVM options

### Build Agent Is Run As Service

In the `<Agent Home>\launcher\conf\wrapper.conf` file, add the following lines (one per option):

```Plain Text
wrapper.app.parameter.<N>

```


<note>
 
You should add additional lines _before_ the following line in the `wrapper.conf` file: `wrapper.app.parameter.N=jetbrains.buildServer.agent.AgentMain`.

Make sure to renumber all lines after the inserted ones.
</note>

## Agent Launcher Properties

It's rare that you would ever need these. Most probably you would need affecting main agent process properties described above.

### Build Agent Is Run Via Script

Before you run the `<Agent Home>\bin\agent.bat|sh` script, set the `TEAMCITY_LAUNCHER_OPTS` environment variable.

### Build Agent Is Run As Service

In the `<Agent Home>\launcher\conf\wrapper.conf` file, add the following lines (one per option, the `N` number should increase):


```Plain Text
wrapper.java.additional.<N>

```


<note>

Make sure to renumber all lines after the inserted ones.
</note>


[//]: # (Internal note. Do not delete. "Configuring Build Agent Startup Propertiesd71e106.txt")    


<seealso>
        <category ref="concepts">
            <a href="configuring-teamcity-server-startup-properties.md">Configuring TeamCity Server Startup Properties</a>
        </category>
        <category ref="admin-guide">
            <a href="agent-home-directory.md">Agent Home Directory</a>
        </category>
</seealso>