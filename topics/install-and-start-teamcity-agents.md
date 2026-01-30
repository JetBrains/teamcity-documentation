[//]: # (title: Install and Start TeamCity Agents)
[//]: # (help-id: Install and Start TeamCity Agents;Setting up and Running Additional Build Agents)

<primary-label ref="java-update" instance="tc"/>

>This section is about [self-hosted build agents](teamcity-cloud-subscription-and-licensing.md#cloud-self-hosted-agents). [JetBrains-hosted build agents](supported-platforms-and-environments.md#JetBrains-Hosted+Agents) are maintained by the TeamCity Cloud team and require no actions from users.
>
{type="note" instance="tcc"}

A TeamCity _build agent_ is a piece of software which listens for the commands from the TeamCity server and starts the actual build processes. A production TeamCity setup requires installing additional build agents on dedicated machines. Before that, make sure to read notes on [agent-server communication](#Agent-Server+Data+Transfer), [system requirements](system-requirements.md#TeamCity+Agent+Requirements), [conflicting software](known-issues.md#Conflicting+Software), and [security](security-notes.md#Build+Agents).
{instance="tc"}

A TeamCity _build agent_ is a piece of software which listens for the commands from the TeamCity server and starts the actual build processes. A production TeamCity setup requires installing additional build agents on dedicated machines. Before that, make sure to read notes on [agent-server communication](#Agent-Server+Data+Transfer), [system requirements](system-requirements.md#TeamCity+Agent+Requirements), [security](security-notes.md#Build+Agents), [conflicting software](known-issues.md#Conflicting+Software), and the [licensing policy](teamcity-cloud-subscription-and-licensing.md) on adding new self-hosted agents.
{instance="tcc"}

If you install TeamCity bundled with a Tomcat servlet container, or use the TeamCity installer for Windows, both the server and one build agent are installed on the same machine. This is not a recommended setup for [production purposes](configure-server-installation.md#Configuring+Server+for+Production+Use) because of [security concerns](security-notes.md). Moreover, the build procedure can slow down the responsiveness of the web UI and overall TeamCity server functioning.
{instance="tc"}

<anchor name="SettingupandRunningAdditionalBuildAgents-ServerDataTransfers"/>

<anchor name="SettingupandRunningAdditionalBuildAgents-Agent-ServerDataTransfers"/>


## Common Build Agent Concepts


<include from="server-administrator-guide.md" element-id="basic-agent-info"/>

* A TeamCity build agent contains <a href="configuring-build-agent-startup-properties.md">two processes</a>: agent launcher (a Java process that launches the agent process) and agent (the main process for a build agent that runs as a child process for the agent launcher).



## Build Agent Statuses

In TeamCity, a build agent can have following statuses:

<table><tr>

<td width="200">

Status

</td>

<td>

Description

</td></tr><tr>

<td>

__Connected/ Disconnected__

</td>

<td>

An agent is connected if it is registered on the TeamCity server and responds to server commands, otherwise it is __disconnected__. This status is determined automatically.

>If an agent stays disconnected during 14 days, its state changes to _Unauthorized_. If you try to reconnect it to the server, you will have to authorize it again.  
>The default timeout duration (14 days) can be adjusted by changing the `teamcity.server.cleanup.agents.inactivityDays` [internal property](server-startup-properties.md#TeamCity+Internal+Properties).
>
{type="note" instance="tc"}

</td></tr><tr>

<td>

__Authorized/ Unauthorized__
{id="agent-authorization"}

</td>

<td>

Agents are manually authorized via the web UI on the __Agents__ page (except for the agents from the machines launched by the [cloud integrations](teamcity-integration-with-cloud-solutions.md)). Only authorized build agents can run builds. The number of authorized agents at any given time cannot exceed the number of [agent licenses](licensing-policy.md#Number+of+Agents) entered on the server. When an agent is unauthorized, a license is freed and a different build agent can be authorized. Purchase additional licenses to expand the number of agents that can concurrently run builds. When a new agent is registered on the server for the first time, it is __unauthorized__ by default and requires manual authorization to run the builds.
{instance="tc"}

Agents are manually authorized via the web UI on the __Agents__ page. Only authorized build agents can run builds. The number of authorized agents at any given time cannot exceed the number of agent licenses entered on the server. When an agent is unauthorized, a license is freed and a different build agent can be authorized. Purchase additional licenses to expand the number of agents that can concurrently run builds. When a new agent is registered on the server for the first time, it is __unauthorized__ by default and requires manual authorization to run the builds.
{instance="tcc"}

If a build agent is installed and running on the same computer as the TeamCity build server, it is authorized automatically.

</td></tr><tr>

<td>

__Enabled/ Disabled (Disabled for maintenance for cloud agents)__
{id="enable-agent"}

</td>

<td>

Agents are manually enabled/disabled via the [web UI](build-agents-configuration-and-maintenance.md#Enabling%2FDisabling+Agents+via+UI). The TeamCity server only distributes builds to agents that are enabled.

Agent disabling does not affect (stop) the build which is currently running on the agent.

__Disabled__ agents can still run builds when the build is assigned to a special agent (for example, by [triggering a custom build](running-custom-build.md)). This feature is generally used to temporarily remove agents from the <tooltip term="build-grid">_build grid_</tooltip> to investigate agent-specific issues.

</td></tr></table>

All agents connected to the server must have unique agent names.

Only users with certain roles can manage agents. See [this article](managing-roles-and-permissions.md) for more information.

For a build agent configuration, refer to [this section](configure-agent-installation.md).


## Agent-Server Data Transfer

[//]: # (AltHead: Server-Agent Data Transfers)

A TeamCity agent connects to the TeamCity server via the URL configured as the `serverUrl` agent property. This is called unidirectional agent-to-server connection.

Agents use unidirectional agent-to-server connection via the polling protocol: an agent establishes an HTTP(S) connection to the TeamCity Server, and polls the server periodically for server commands.

>It is recommended using __HTTPS__ for agent-to-server communications (check related [server configuration notes](how-to.md#Configure+HTTPS+for+TeamCity+Web+UI)). If the agents and the server are deployed in a secure environment, agents can be configured to use plain HTTP URL for connections to the server as this reduces transfer overhead. Note that the data travelling through the connection established from an agent to the server includes build settings, repository access credentials and keys, repository sources, build artifacts, build progress messages, and build log. In case of using the HTTP protocol that data can be compromised via the "[man in the middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)" attack.
>
{type="warning" instance="tc"}

<!--[//]: # (Internal note. Do not delete. "Setting up and Running Additional Build Agentsd283e376.txt")-->

<anchor name="SettingupandRunningAdditionalBuildAgents-InstallingAdditionalBuildAgents"/>

## Connecting Local Agents to TeamCity Server

After you [install a build agent locally](install-teamcity-agent.md), it needs to be [configured](configure-agent-installation.md) and connected to your TeamCity server or cloud instance. Watch this video for a quick guide:

<video src="https://youtu.be/dvyDCzOJJZw"
title="TeamCity tutorial — How to connect local agents to your TeamCity server"/>

>Please note that the _Use authentication token..._ option referenced in the video is currently available only for TeamCity Cloud instances.
>
{type="warning" instance="tc"}


## Cloud Agents

Hosting TeamCity agents in the cloud allows you implement a highly scalable solutions with new agents spinning up on demand and winding down when there are no builds to process. See the [](teamcity-integration-with-cloud-solutions.md) section for more information on cloud-hosted TeamCity agents.


## Agent Upgrade
{instance="tc"}

TeamCity agents are automatically upgraded when needed. Typically, this happens when:

* the server is [upgraded](upgrading-teamcity-server-and-agents.md)
* an agent plugin is [added](installing-additional-plugins.md) or [updated](https://plugins.jetbrains.com/docs/teamcity/plugins-packaging.html#PluginsPackaging-AgentUpgradeonUpdatingPlugins) on the server
* [a new tool is installed](installing-agent-tools.md)

Note that updating agent plugins and receiving new files following the server upgrade may trigger an agent restart for the changes to take effect. If agents run under user accounts with [sufficient permissions](system-requirements.md#Common+Requirements), all restarts happen automatically and do not require your input.


## Agent Upgrade
{instance="tcc"}

Both JetBrains-hosted and self-hosted agents upgrade automatically when the server is upgraded. Note that receiving new files following the server upgrade may trigger an agent restart for the changes to take effect. If your self-hosted agents run under user accounts with [sufficient permissions](system-requirements.md#Common+Requirements), all restarts happen automatically and do not require your input.


## Agent Priority
{instance="tcc"}

If you have a mix of [JetBrains-hosted](supported-platforms-and-environments.md#JetBrains-Hosted+Agents) and [self-hosted](supported-platforms-and-environments.md#Self-Hosted+Agents) agents, TeamCity uses the following set of rules to pick an optimal agent that is the most balanced in terms of both performance and price:

* Self-hosted agents have priority over JetBrains-hosted agents
* [Per-month agents](managing-subscription-and-resources.md#Prepay+JetBrains-Hosted+Build+Agents+Monthly) have priority over per-minute agents
* If agents on any platform can run a build, TeamCity prioritizes Linux agents first, then Windows, and lastly, macOS.
* Priorities of AWS-hosted agents depend on their instance types. Smaller agents have priority over larger ones.
* Agents with the latest OS versions have priority over agents with older versions.
* Agents installed on x86_64 machines have priority over ARM agents.

You can manually lower or raise the priority of a self-hosted agent by modifying its integer `teamcity.agent.priority` property. This property accepts values in the `-10000` ~ `10000` range with the default value of `0`. For [EC2 build agents](setting-up-teamcity-for-amazon-ec2.md), you can set this property on the Cloud Image settings page:

<img src="dk-agentpriority.png" width="706" alt="Set the image priority for a EC2 Cloud Image"/>

For other agent types, add the following line to the [&lt;TeamCity_Agent_Home&gt;/conf/buildAgent.properties](configure-agent-installation.md) file:

```XML
teamcity.agent.priority=54
```

Note that TeamCity recognizes agent properties only after the agent is fully booted and connected to the server. For that reason, priorities for non-EC2 cloud agents apply only to active/running instances. Currently, only EC2 cloud images relay agent priorities before instances start.

## Agent Priority
{instance="tc"}

TeamCity selects agents using multiple criteria, including CPU count, past performance, and agent source (local self-hosted agents have the highest priority, followed by [cloud agents](teamcity-integration-with-cloud-solutions.md), and [Kubernetes executor pods](kubernetes-executor.md) ranked lowest). You can override this logic by setting the integer `teamcity.agent.priority` property (`–10,000` to `10,000`; default: `0`).

* For [AWS-hosted cloud agents](setting-up-teamcity-for-amazon-ec2.md), you can set this property on the Cloud Image settings page:

    <img src="dk-agentpriority.png" width="706" alt="Set the image priority for a EC2 Cloud Image"/>

* For other agent types, add the following line to the [&lt;TeamCity_Agent_Home&gt;/conf/buildAgent.properties](configure-agent-installation.md) file:

    ```XML
    teamcity.agent.priority=54
    ```

Note that TeamCity recognizes agent properties only after the agent is fully booted and connected to the server. For that reason, priorities for non-EC2 cloud agents apply only to active/running instances. Currently, only EC2 cloud images relay agent priorities before instances start.



## Generating Authentication Token
{instance="tcc"}

The recommended approach to connecting a self-hosted agent to a TeamCity Cloud instance is to generate a unique authentication token for this agent. To do this, go to __Agents__, open the __Install Build Agents__ menu in the upper right corner of the screen, and click _Use authentication token_. There are two options:

* _Generate plain-text token_: you need to copy the generated token and enter it in the [build agent configuration](configure-agent-installation.md) file. On Windows, you will be prompted to enter it right in the _Configure Build Agent Properties_ installation dialog.
* _Download config_: enter an agent name (`name` attribute in the [build agent config](configure-agent-installation.md)) and download the entire config file. Place it as the `buildAgent.properties` file in the build agent directory.

Please generate own token or configuration file per each self-hosted agent.



## Debug Agents Remotely

<snippet id="agents-terminal">

After an agent was installed and connected, you can invoke a terminal for this agent's machine directly from the TeamCity UI. This functionality lets you remotely view agent logs, check installed software, and debug specific agent issues.

To invoke a terminal, click **Agents** in the TeamCity header, choose the required agent, and click **Open terminal**.

<img src="dk-agentTerminal-2023-11.png" width="706" alt="Agent Terminal Window"/>

You can also open this terminal from the [](build-results-page.md). In this case, the terminal opens in the [checkout directory](build-checkout-directory.md) instead of the `$HOME` folder.

<img src="dk-terminal-in-checkout-folder.png" width="706" alt="Agent Terminal Window"/>

When a terminal opens, you can click the **Open in a separate tab** link to get a bigger client area.



The **Open terminal** button is available for all types of agent machines (Linux, Windows, and macOS) and invokes terminals under the same user identity who starts TeamCity agents.

To ensure your build agent is idle while you perform maintenance, disable it but do not stop it since the terminal session requires a [running](start-teamcity-agent.md) build agent. [Stopping a build agent](start-teamcity-agent.md#Stop+Build+Agent) freezes a previously open terminal tab, preventing users from typing new commands.

For cloud agents that are automatically terminated after idling for a certain period of time, you may want to click the ["Disable for maintenance..."](build-agents-configuration-and-maintenance.md#Enabling%2FDisabling+Agents+via+UI) button to keep the agent's machine running and prevent it from shutting down while you are still investigating a build problem. 

<!-- Executing terminal commands is a valid activity that prevents automatic agent shutdown. However, if the terminal is not used, the agent will shut down as scheduled. Shortly before this, TeamCity will display a pop-up notification.

<img src="dk-agent-terminal-warning.png" width="706" alt="Terminal shutdown warning"/>

-->

> The "Open interactive terminal" link opens in the `<SERVER-URL>/plugins/teamcity-agent-terminal/agentTerminal.html?agentId:<ID>` URL in a separate panel or a browser tab. If your server is [behind a proxy](multinode-setup.md#Proxy+Configuration), ensure your proxy configuration allows websocket connections to this page.
> 
{type="note" instance="tc"}

The **Open terminal** link is visible only to users whose [role permissions](managing-roles-and-permissions.md) include the *"Invoke interactive agent terminals"* permission. This permission should be granted for all projects associated with the agent pool of the corresponding agent. Users with the "Project Administrator" and "System Administrator" roles have such a permission by default. As an additional precaution, each request to open a terminal is written as a new "Agent actions | Connect to agent" activity in the [audit log](tracking-user-actions.md).

</snippet>