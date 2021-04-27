[//]: # (title: Viewing Build Agent Details)
[//]: # (auxiliary-id: Viewing Build Agent Details)

To view the state and information about an agent, click its name or navigate to the __Agents__ page, find the agent in the list of connected, disconnected or authorized agents and click its name.

For each connected agent TeamCity provides the following information:

## Agent Summary
* __Status__: [learn more about an agent's status](build-agent.md). 
* __Details__:
  * the agent name
  * the agent host IP address 
  * the port used by the TeamCity server to connect to the agent
  * the communication [protocol](setting-up-and-running-additional-build-agents.md) used for data transfers between the agent and the server
  * the agent operating system
  * CPU rank: the result of the bundled CPU benchmark. Note that the benchmark results can depend on the JVM version and options used by the agent. For example, `-server` JVM option has a significant influence on the results. CPU benchmark also affects the way how the server distributes builds among agents. If a build duration estimate cannot be calculated for an agent (there were no builds in the history ran on this agent), TeamCity chooses the fastest agent basing on the CPU benchmark value.
  * the [pool](configuring-agent-pools.md) the agent belongs to
  * the agent launcher version
* If there is a __running build__ on the agent, the page displays information on the build with the link to the [build results](working-with-build-results.md).
* __Miscellaneous__: this section provides the options to
  * [Clean sources on the agent](clean-checkout.md)
  * Open Remote Desktop: available for agents running on the Windows operating system if the RDP client is installed and registered on the agent.
  * Reboot Agent Machine: available to users with _Reboot build agent machines_ permissions. Click the link and confirm the reboot action. By default, the TeamCity agent will wait until the current build finishes. Deselect the checkbox and click Reboot to restart the agent immediately.    
  Additional configuration of the reboot command is possible. See [Agent Reboot](#Agent+Reboot).
  * Dump threads on agent

### Agent Reboot
[//]: # (AltHead: Configuring Agent Reboot Command)

>This section is only relevant for [self-hosted build agents](teamcity-cloud-subscription-and-licensing.md#cloud-self-hosted-agents).
>
{type="note" product="tcc"}

Agent reboot is performed by executing an OS-specific command. Under certain circumstances the command might need customization specific to the OS environment. Additional configuration might be required if the default reboot command fails.

To tweak the agent reboot, add the `teamcity.agent.reboot.command` agent configuration parameter to the [`buildagent.properties`](build-agent-configuration.md) file with the command to execute when reboot is required. Example configuration:

```Shell
teamcity.agent.reboot.command=sudo shutdown -r 60

```

or

```Shell
teamcity.agent.reboot.command=shutdown -r -t 60 -c "TeamCity Agent reboot command" -f

```

## Build History

Shows the builds that were run on the agent.

## Compatible Configurations

Displays compatible and incompatible build configurations with the reason of incompatibility.

## Build runners

Lists build runners supported by build agent.

## Agent logs

The page allows viewing and downloading the logs.

## Agent Parameters

The tab lists system properties, environment variables, and configuration parameters. Refer to the [Configuring Build Parameters](configuring-build-parameters.md) page for more information on different types of parameters.

<seealso>
        <category ref="installation">
            <a href="installation.md">Installing and Running Additional Build Agents</a>
        </category>
        <category ref="concepts">
            <a href="build-agent.md">Build Agent</a>
            <a href="run-configuration-policy.md">Run Configuration Policy</a>
        </category>
</seealso>