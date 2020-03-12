[//]: # (title: Agent Pools)
[//]: # (auxiliary-id: Agent Pools)

## Concept

Instead of having one common set of agents, you can break them into separate groups called _agent pools_. A pool is a named set of agents to which you can assign projects.
* An agent can belong to __one pool only__.
* A project can use __several pools__ for its builds.

The number of agents authorized by the TeamCity server is limited by the number of [agent licenses](licensing-policy.md#Number+of+Agents). By default, all newly authorized agents are included into the __Default pool__. 

With the help of agent pools you can bind specific agents to specific projects. Project builds can be run only on build agents from the pools assigned to the project. Agent pools can also help to calculate the required agents capacity.

You can find all agent pools configured in TeamCity at the __Agents | Pools__ tab.

## Required Permissions

To be able to add/remove pools and set maximum number of agents in the pool, you need to have the _"Manage agent pools"_ permission granted to the System Administrator and Agent Manager [roles](role-and-permission.md) in the default TeamCity [per-project authorization mode](role-and-permission.md#Per-Project+Authorization+Mode).

Assigning and unassigning projects and agents to/from pools is restricted by the _"Change agent pools associated with project"_ permission, which by default is a part of the Project administrator role. Users can perform the operations on the pool only if they have the _"Change agent pools associated with project"_ permission for __all projects__ associated with __all pools__ affected by the operation.

See also related [agent management permissions](role-and-permission.md#Project-level+Agent+Management+Permissions).

## Managing Agent Pools

The __Agents | Pools__ tab allows managing pools in TeamCity.

To create a new agent pool, specify its name. 

By default, a pool can contain an unlimited number of agents. You can set a maximum number of agents in the pool (not applicable for the Default pool). If the maximum number of agents is reached, TeamСity will not allow adding any new agents to this pool. This includes moving agents from other pools and automatic authorization of cloud agents. New cloud agents will not start if the target pool is full. 

To populate a pool with agents, click __Assign agents__ and select them from the list. Since an agent can belong to one pool only, assigning it to a pool will remove it from its previous pool. If this may cause compatibility problems, TeamCity will give you a warning. Removing an agent from a custom pool will return it to the Default pool. You can also assign cloud agents to a specific pool when adding an image to a cloud profile.

<note>

Only the cloud agent images configured in the `<Root>` project can be moved using __Assign agents__. To assign a pool to a cloud agent, configure it using [Agent Cloud profile](agent-cloud-profile.md), in the cloud image details.
</note>

When you have configured agent pools, you can:
* Filter the build queue by pools.
* Use grouping by pool on the [Agent Matrix and Agent Statistics](viewing-agents-workload.md) pages.

__  __

__See also:__

__Concepts__: [Build Agent](build-agent.md) | [Agent Requirements](agent-requirements.md)   
__Administrator's Guide__: [Viewing Agents Workload](viewing-agents-workload.md)

__ __