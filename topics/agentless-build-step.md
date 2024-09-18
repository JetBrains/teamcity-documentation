[//]: # (title: Agentless Build Step)
[//]: # (auxiliary-id: Agentless Build Step)

_Agentless build steps_ are [steps](configuring-build-steps.md) that can run without an [agent](build-agent.md), in an external software.

Normally, a running build occupies a build agent until the very finish, even if its final steps are executed outside TeamCity. However, if a build does not need its agent for some remaining steps, it can detach from this agent. The agent becomes available and can be instantly assigned to another build.

This approach allows saving agents' work time and is optimal for configurations that use third-party tools for finalizing a build: for example, to run numerous tests or deploy a project. Such a build can finish outside TeamCity; the TeamCity server will detect its status reports directly, without using an agent as a mediator.

Let's consider an example build that consists of three steps: compilation, testing, and deployment. All of them are processed by an agent even though the deployment step is actually performed by the external software. The agent is just polling the external software and reporting build statuses to the TeamCity server.

<img src="agent-depend-build.png" alt="Regular build" width="556"/>

With the agentless approach, the agent does not need to handle the final deployment step and can run other builds from the queue. The TeamCity server will catch the following reports directly from the external tool.

<img src="agentless-build.png" alt="Build with agentless step" width="518"/>

<video src="https://youtu.be/O7LdxtiBkLY"
title="New in TeamCity 2020.2: Agentless Build Steps"/>

<seealso>
        <category ref="admin-guide">
            <a href="detaching-build-from-agent.md">Detaching Build from Agent</a>
        </category>
</seealso>