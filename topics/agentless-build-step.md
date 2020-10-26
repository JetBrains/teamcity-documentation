[//]: # (title: Agentless Build)
[//]: # (auxiliary-id: Agentless Build)

>This functionality is introduced in terms of TeamCity 2020.2 Early Access Program.

_Agentless build steps_ are [steps](configuring-build-steps.md) that can run virtually, without an [agent](build-agent.md).

A build agent is usually required to run a build from start to finish. However, if a running build does not need its agent for some remaining steps, it can send an instruction to "detach" the agent. This agent becomes available and can be instantly assigned to another build.

This approach allows saving agents' work time and is optimal for configurations that use third-party tools for finalizing a build: for example, to deploy a project. Such a build can finish outside  TeamCity; the TeamCity server will detect its status reports directly, without using an agent as a mediator.

Let's consider an example build that consists of three steps: compilation, testing, and deployment. All of them are processed by an agent even though the deployment step is actually performed by the external software. The agent is reporting results to the TeamCity server and stays assigned to the build until it is finished.

<img src="agent-depend-build.png" alt="Regular build" width="556"/>

With the agentless approach, the agent does not need to handle the final deployment step and can run other builds from the queue. The TeamCity server will catch the following reports directly from the external tool.

<img src="agentless-build.png" alt="Build with agentless step" width="518"/>

<seealso>
        <category ref="admin-guide">
            <a href="detaching-build-from-agent.md">Detaching Build from Agent</a>
        </category>
</seealso>