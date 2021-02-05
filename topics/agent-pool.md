[//]: # (title: Agent Pool)
[//]: # (auxiliary-id: Agent Pool)

Instead of having one common set of [build agents](build-agent.md), you can break them into separate groups called _agent pools_. A pool is a named set of agents to which you can assign projects.
* An agent can belong to _one pool only_.
* A project can use _several pools_ for its builds.

The number of agents authorized by the TeamCity server is limited by the number of [agent licenses](licensing-policy.md#Number+of+Agents). By default, all newly authorized agents are included into the _Default pool_.
{product="tc"}

The number of agents authorized by the TeamCity server is limited by the number of agent licenses. By default, all newly authorized agents are included into the _Default pool_.

With the help of agent pools you can bind specific agents to specific projects. Project builds can be run only on build agents from the pools assigned to the project. Agent pools can also help to calculate the required agents' capacity.

You can find all agent pools configured in TeamCity on the __Agents | Pools__ tab.

<seealso>
        <category ref="admin-guide">
            <a href="configuring-agent-pools.md">Configuring Agent Pools</a>
        </category>
</seealso>