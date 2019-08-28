[//]: # (title: Assigning Build Configurations to Specific Build Agents)
[//]: # (auxiliary-id: Assigning Build Configurations to Specific Build Agents)
It is sometimes necessary to manage the [Build Agents](build-agent.md)' workload more effectively. For example, if the time\-consuming performance tests are run, the Build Agents with low hardware resources may slow down. As a result, more builds will enter the [Build Queue](build-queue.md), and the feedback loop can become longer than desired. To avoid such situation, you can:

1. [Establish a run configuration policy](#Agent+pools) for an agent, which defines the build configurations to run on this agent.
2. Define special [Agent Requirements](agent-requirements.md), to restrict the pool of agents, on which a build configuration can run the builds. These requirements are:			
   * [Build Agent name](#Agent+pools). If the name of a build agent is made a requirement, the build configuration will run builds on this agent only.		
   * [Build Agent property](#Agent+pools). If a certain property, for example, a capability to run builds of a certain configuration or an operating system, is made a requirement, the build configuration will run builds on the agents that meet this requirement.


* You can modify these parameters when setting up the project or build configuration, or at any moment you need. The changes you make to the build configurations are applied on the fly.	
* You can specify a particular build agent to run a build on when [Triggering a Custom Build](triggering-a-custom-build.md).



## Agent pools


You could split agents into pools. Each project could be associated to a number of pools. See [Agent Pools](agent-pools.md).


## Establishing a Run Configuration Policy

__To establish a Build Agent's run configuration policy:__
	
1. Click the __Agents__ and select the desired build agent.
2. Click the __Compatible Configurations__ tab.
3. Select __Run selected configurations only__ and tick the desired build configurations names to run on the build agent.


## Making Build Agent Name and Property a Build Configuration Requirement

__To make a build configuration run the builds on a build agent with the specified name and properties:__
	
1. Click __Administration__ and select the desired build configuration.
2. Click Agent Requirements (see [Configuring Agent Requirements](configuring-agent-requirements.md)).
3. Click the __Add requirement for a property__ link, type the `agent.name` property, set its condition to __equals__ and specify the build agent's name.
4. Click the __Add requirement for a property__ link and add the required property, condition, and value. For example, if you have several Linux\-only builds, you can add the `teamcity.agent.jvm.os.name` property and set the __starts with__ condition and the `linux` value.


<tip>

You can also use the condition __contains__, however, it may include more than one specific build agent (for example, a build configuration with a requirement `agent.name` __contains__ `Agent10`, will run on agents named __Agent10__, __Agent10a__, and __Agent10b__).
</tip>


[//]: # (Internal note. Do not delete. "Assigning Build Configurations to Specific Build Agentsd17e193.txt")    


 __  __
 
__See also:__



__Concepts__: [Build Agent](build-agent.md) | [Agent Requirements](agent-requirements.md) | [Run Configuration Policy](run-configuration-policy.md)   

__Administrator's Guide__: [Triggering a Custom Build](triggering-a-custom-build.md)
