[//]: # (title: Install and Start TeamCity Agents)
[//]: # (auxiliary-id: Install and Start TeamCity Agents;Setting up and Running Additional Build Agents)

>This section is about [self-hosted build agents](teamcity-cloud-subscription-and-licensing.md#cloud-self-hosted-agents). [JetBrains-hosted build agents](teamcity-cloud-subscription-and-licensing.md#cloud-jb-hosted-agents) are maintained by the TeamCity Cloud team and require no actions from users.
>
{type="note" product="tcc"}

A TeamCity _build agent_ is a piece of software which listens for the commands from the TeamCity server and starts the actual build processes. A production TeamCity setup requires installing additional build agents on dedicated machines. Before that, make sure to read notes on [agent-server communication](#Agent-Server+Data+Transfer), [system requirements](system-requirements.md#TeamCity+Agent+Requirements), [conflicting software](known-issues.md#Conflicting+Software), and [security](security-notes.md#Build+Agents).
{product="tc"}

A TeamCity _build agent_ is a piece of software which listens for the commands from the TeamCity server and starts the actual build processes. A production TeamCity setup requires installing additional build agents on dedicated machines. Before that, make sure to read notes on [agent-server communication](#Agent-Server+Data+Transfer), [system requirements](system-requirements.md#TeamCity+Agent+Requirements), [security](security-notes.md#Build+Agents), [conflicting software](known-issues.md#Conflicting+Software), and the [licensing policy](teamcity-cloud-subscription-and-licensing.md) on adding new self-hosted agents.
{product="tcc"}

If you install TeamCity bundled with a Tomcat servlet container, or use the TeamCity installer for Windows, both the server and one build agent are installed on the same machine. This is not a recommended setup for [production purposes](configure-server-installation.md#Configuring+Server+for+Production+Use) because of [security concerns](security-notes.md). Moreover, the build procedure can slow down the responsiveness of the web UI and overall TeamCity server functioning.
{product="tc"}

<anchor name="SettingupandRunningAdditionalBuildAgents-ServerDataTransfers"/>
<anchor name="SettingupandRunningAdditionalBuildAgents-Agent-ServerDataTransfers"/>

## Agent-Server Data Transfer

[//]: # (AltHead: Server-Agent Data Transfers)

A TeamCity agent connects to the TeamCity server via the URL configured as the `serverUrl` agent property. This is called unidirectional agent-to-server connection.

Agents use unidirectional agent-to-server connection via the polling protocol: an agent establishes an HTTP(S) connection to the TeamCity Server, and polls the server periodically for server commands.

>It is recommended using __HTTPS__ for agent-to-server communications (check related [server configuration notes](how-to.md#Configure+HTTPS+for+TeamCity+Web+UI)). If the agents and the server are deployed in a secure environment, agents can be configured to use plain HTTP URL for connections to the server as this reduces transfer overhead. Note that the data travelling through the connection established from an agent to the server includes build settings, repository access credentials and keys, repository sources, build artifacts, build progress messages, and build log. In case of using the HTTP protocol that data can be compromised via the "[man in the middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)" attack.
>
{type="warning" product="tc"}

[//]: # (Internal note. Do not delete. "Setting up and Running Additional Build Agentsd283e376.txt")

## Generating Authentication Token
{product="tcc"}

The recommended approach to connecting a self-hosted agent to a TeamCity Cloud instance is to generate a unique authentication token for this agent. To do this, go to __Agents__, open the __Install Build Agents__ menu in the upper right corner of the screen, and click _Use authentication token_. There are two options:

* _Generate plain-text token_: you need to copy the generated token and enter it in the [build agent configuration](configure-agent-installation.md) file. On Windows, you will be prompted to enter it right in the _Configure Build Agent Properties_ installation dialog.
* _Download config_: enter an agent name (`name` attribute in the [build agent config](configure-agent-installation.md)) and download the entire config file. Place it as the `buildAgent.properties` file in the build agent directory.

Please generate own token or configuration file per each self-hosted agent.

<anchor name="SettingupandRunningAdditionalBuildAgents-InstallingAdditionalBuildAgents"/>