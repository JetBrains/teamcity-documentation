[//]: # (title: Build Agent)
[//]: # (auxiliary-id: Build Agent)

A TeamCity _build agent_ is a piece of software which listens for the commands from the TeamCity server and starts the actual build processes. It is [installed and configured](install-and-start-teamcity-agents.md) separately from the TeamCity server. An agent can be installed on the same computer as the server or on a different machine (the latter is a preferred setup for server performance reasons); an agent can run the same operating system (OS) as the TeamCity server or a different OS.
{product="tc"}

A TeamCity _build agent_ is a piece of software which listens for the commands from the TeamCity server and starts the actual build processes. There are two types of agents in TeamCity Cloud: JetBrains-hosted and self-hosted. The first ones are maintained and configured by JetBrains. They are started on-demand as soon as each new build requires to be run. The second ones are [stored and configured](install-and-start-teamcity-agents.md) by the customer. Both types of agents can be successfully combined in one TeamCity Cloud installation. Please see [Subscription and Licensing](teamcity-cloud-subscription-and-licensing.md) on details between these agents in terms of the TeamCity Cloud subscription.
{product="tcc"}

A TeamCity build agent contains [two processes](configuring-build-agent-startup-properties.md):   
* Agent launcher — a Java process that launches the agent process.
* Agent — the main process for a build agent; runs as a child process for the agent launcher.

An agent typically checks out the source code, downloads artifacts of other builds and runs the build process. An agent can run a single build at a time. The number of agents basically limits the number of parallel builds and environments in which your build processes are run.   
An agent can run builds of any compatible build configuration.

The TeamCity server monitors all the connected agents and assigns queued builds to the agents based on [compatibility requirements](agent-requirements.md), [agent pools](configuring-agent-pools.md), build configuration restrictions configured for an agent and the selection algorithm described [here](build-queue.md).

## Build Agent Status

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

</td></tr><tr>

<td id="agent-authorization">

__Authorized/ Unauthorized__

</td>

<td>

Agents are manually authorized via the web UI on the __Agents__ page (except for the agents from the machines launched by the [cloud integrations](teamcity-integration-with-cloud-solutions.md)). Only authorized build agents can run builds. The number of authorized agents at any given time cannot exceed the number of [agent licenses](licensing-policy.md#Number+of+Agents) entered on the server. When an agent is unauthorized, a license is freed and a different build agent can be authorized. Purchase additional licenses to expand the number of agents that can concurrently run builds. When a new agent is registered on the server for the first time, it is __unauthorized__ by default and requires manual authorization to run the builds.
{product="tc"}

Agents are manually authorized via the web UI on the __Agents__ page. Only authorized build agents can run builds. The number of authorized agents at any given time cannot exceed the number of agent licenses entered on the server. When an agent is unauthorized, a license is freed and a different build agent can be authorized. Purchase additional licenses to expand the number of agents that can concurrently run builds. When a new agent is registered on the server for the first time, it is __unauthorized__ by default and requires manual authorization to run the builds.
{product="tcc"}

If a build agent is installed and running on the same computer as the TeamCity build server, it is authorized automatically.

</td></tr><tr>

<td id="enable-agent">

__Enabled/ Disabled__

</td>

<td>

Agents are manually enabled/disabled via the [web UI](build-agents-configuration-and-maintenance.md#Enabling%2FDisabling+Agents+via+UI). The TeamCity server only distributes builds to agents that are enabled.

 Agent disabling does not affect (stop) the build which is currently running on the agent.

__Disabled__ agents can still run builds when the build is assigned to a special agent (for example, by [triggering a custom build](running-custom-build.md)). This feature is generally used to temporarily remove agents from the <emphasis tooltip="build-grid">build grid</emphasis> to investigate agent-specific issues.

</td></tr></table>

All agents connected to the server must have unique agent names.

Only users with certain roles can manage agents. See [this article](managing-roles-and-permissions.md) for more information.

For a build agent configuration, refer to [this section](configure-agent-installation.md).

## Agent Upgrade
{product="tc"}

A TeamCity agent is upgraded automatically when necessary. The process involves downloading new agent files from the TeamCity server and restarting the agent on the new files. In order to successfully accomplish this, the user under whose account the agent runs should have [enough](system-requirements.md#Common+Requirements) permissions.

Typically, an agent upgrade happens when:
* the server is [upgraded](upgrading-teamcity-server-and-agents.md)
* an agent plugin is [added](installing-additional-plugins.md) or [updated](https://plugins.jetbrains.com/docs/teamcity/plugins-packaging.html#PluginsPackaging-AgentUpgradeonUpdatingPlugins) on the server
* [a new tool is installed](installing-agent-tools.md)

## Agent Upgrade
{product="tcc"}

JetBrains hosted agents are upgraded automatically when the server is upgraded.

The upgrade of self-hosted agents is also automated and involves downloading new agent files from the TeamCity server and restarting the agent on the new files. In order to successfully accomplish this, the user under whose account the agent runs should have [enough](system-requirements.md#Common+Requirements) permissions.

Typically, an agent upgrade happens when the server is upgraded.

<seealso>
        <category ref="installation">
            <a href="install-and-start-teamcity-agents.md">Setting up Additional Build Agents</a>
        </category>
        <category ref="concepts">
            <a href="agent-work-directory.md">Agent Work Directory</a>
            <a href="managing-roles-and-permissions.md">Roles and Permissions</a>
        </category>
        <category ref="admin-guide">
            <a href="configuring-agent-pools.md">Agent Pools</a>
            <a href="assigning-build-configurations-to-specific-build-agents.md">Assigning Build Configurations to Specific Build Agents</a>
            <a href="licensing-policy.md" product="tc">Licensing Policy</a>
        </category>
</seealso>