[//]: # (title: Backing up Build Agent's Data)
[//]: # (auxiliary-id: Backing up Build Agent's Data)

>This page is related only to [self-hosted build agents](teamcity-cloud-subscription-and-licensing.md#cloud-self-hosted-agents). Data of [JetBrains-hosted build agents](teamcity-cloud-subscription-and-licensing.md#cloud-jb-hosted-agents) is backed up automatically.
>
{type="note" instance="tcc"}

To back up build agent's data:
	
1. __Build Agent configuration__   
Back up the [`<Agent Home Directory>`](agent-home-directory.md)`/conf/buildAgent.properties` file.   
You may also wish to back up any other configuration files changed (the build agent's configuration is specified in [`<Agent Home Directory>`](agent-home-directory.md)`/conf` and [`<Agent Home Directory>`](agent-home-directory.md)`/launcher/conf` directories).
	
2. __Log files__   
If you need build agent log files (mainly used for problem-solving or debug purposes), back up the [`<Agent Home Directory>`](agent-home-directory.md)`/logs` directory.

<note>

You may also wish to back up the build agent's Windows Service settings, if they were modified.
</note>
