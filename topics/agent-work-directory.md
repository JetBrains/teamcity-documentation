[//]: # (title: Agent Work Directory)
[//]: # (auxiliary-id: Agent Work Directory)

>This page is only relevant for [self-hosted build agents](teamcity-cloud-subscription-and-licensing.md#cloud-self-hosted-agents).
>
{type="note" product="tcc"}

_Agent work directory_ is the directory on a build agent that is used as a containing directory for the default [checkout directories](build-checkout-directory.md). By default, this is the `<[build_agent_home](agent-home-directory.md)>/work` directory.

To modify the default directory location, see `workDir` parameter in [Build Agent Configuration](build-agent-configuration.md).

<note>

Note that TeamCity assumes full control over the directory and can [delete subdirectories](build-checkout-directory.md#Automatic+Checkout+Directory+Cleaning) if they do not correspond to checkout directories of the recent builds.
</note>

For more information on handling the directories inside the agent work directory, refer to the [Build Checkout Directory](build-checkout-directory.md) page.


[//]: # (Internal note. Do not delete. "Agent Work Directoryd10e43.txt")    


<seealso>
        <category ref="concepts">
            <a href="build-agent.md">Build Agent</a>
            <a href="build-checkout-directory.md">Build Checkout Directory</a>
        </category>
</seealso>