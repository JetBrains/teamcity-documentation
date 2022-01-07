[//]: # (title: Configuring Agent Pools)
[//]: # (auxiliary-id: Configuring Agent Pools;Agent Pools)

To manage build agents efficiently, you can group them into multiple [agent pools](agent-pool.md).

## Required Permissions

To be able to add/remove pools and set maximum number of agents in the pool, you need to have the "_Manage agent pools_" permission granted to the System Administrator and Agent Manager [roles](managing-roles-and-permissions.md) in the default TeamCity [per-project authorization mode](managing-roles-and-permissions.md#Per-Project+Authorization+Mode).

Assigning and unassigning projects and agents to/from pools is restricted by the "_Change agent pools associated with project_" permission, which by default is a part of the Project Administrator role. Users can perform the operations on the pool only if they have the "_Change agent pools associated with project_" permission for _all projects_ associated with _all pools_ affected by the operation.

See also related [agent management permissions](managing-roles-and-permissions.md#Project-level+Agent+Management+Permissions).

## Managing Agent Pools

The __Agents | Pools__ tab allows managing pools in TeamCity.

To create a new agent pool, specify its name.

By default, a pool can contain an unlimited number of agents. You can set a maximum number of agents in the pool (not applicable for the Default pool). If the maximum number of agents is reached, TeamСity will not allow adding any new agents to this pool. This includes moving agents from other pools and automatic authorization of cloud agents. New cloud agents will not start if the target pool is full. 

To populate a pool with agents, click __Assign agents__ and select them from the list. Since an agent can belong to one pool only, assigning it to a pool will remove it from its previous pool. If this may cause compatibility problems, TeamCity will give you a warning. Removing an agent from a custom pool will return it to the Default pool. You can also assign cloud agents to a specific pool when adding an image to a cloud profile.

To assign a pool to a cloud agent, you need to configure it in the cloud image details of the [agent cloud profile](agent-cloud-profile.md). Note that agents from all cloud profiles of the current project are automatically combined into a _[project pool](agent-cloud-profile.md#Adding+Agent+Image)_.
{product="tc"}

<note product="tc">

Only the cloud agent images configured in the `<Root>` project can be moved using __Assign agents__.
</note>

When you have configured agent pools, you can:
* filter the build queue by pools;
* use grouping by pool on the [Agent Matrix and Agent Statistics](viewing-agents-workload.md) pages.

<seealso>
        <category ref="concepts">
            <a href="agent-pool.md">Agent Pool</a>
            <a href="build-agent.md">Build Agent</a>
            <a href="build-queue.md">Build Queue</a>
            <a href="agent-requirements.md">Agent Requirements</a>
        </category>
        <category ref="admin-guide">
            <a href="viewing-agents-workload.md">Viewing Agents Workload</a>
            <a href="agent-cloud-profile.md" product="tc">Agent Cloud Profile</a>
        </category>
</seealso>