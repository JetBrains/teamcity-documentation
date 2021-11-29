[//]: # (title: Agent Requirements)
[//]: # (auxiliary-id: Agent Requirements)

Agent requirements are used in TeamCity to specify whether a [build configuration](build-configuration.md) can run on a particular [build agent](build-agent.md) besides [agent pools](configuring-agent-pools.md) and specified build configuration restrictions.

## Agent Requirements Based on Environment Variables and Properties

When a build agent registers on the TeamCity server, it provides information about its configuration, including its environment variables, system properties, and additional settings specified in the `buildAgent.properties` file.

The administrator can specify required environment variables and system properties for a build configuration on the __Build Configuration Settings | Agent Requirements__ page. For instance, if a particular build configuration must run on a build agent running Windows, the administrator specifies this by adding a requirement that the `teamcity.agent.jvm.os.name` system property on the build agent must contain the `Windows` string.

If the properties and environment variables on the build agent do not fulfill the requirements specified by the build configuration, then the build agent is incompatible with this build configuration. The __Agent Requirements__ page lists both compatible and incompatible agents. Multiple agent requirements for a single parameter can be added. The conditions are treated as 'and' to determine compatible agents. 

It is possible to disable a configured requirement using the corresponding option in the requirements list.

Sometimes the build configuration may become incompatible with a build agent if the build runner for this configuration cannot be initialized on the build agent. For instance, .NET build runners do not initialize on UNIX systems.

## Implicit Requirements

Any reference (a name enclosed in `%` characters) to an unknown parameter is considered an _implicit requirement_, which means that the build will only run if:
* an agent provides this parameter,
* or the parameter is defined on the build configuration (or project) level. The priority of the build configuration's value is higher than of the value defined on an agent.

For instance, if you define a build runner parameter as a reference to another property: `%\env.JDK_16%/lib/*.jar`, this will implicitly add an agent requirement for the referenced property: that is, `env.JDK_16` must be defined. To define such properties on the agent, you can:
* Specify them in the [`buildAgent.properties`](configure-agent-installation.md) file.
* Set the [environment variable](predefined-build-parameters.md#Agent+Environment+Variables) `JDK_16` on the build agent.
* Specify the value on the __Parameters__ page of a build configuration (or in the __Project Settings__). The same value of the property will be used for all build agents.

 <seealso>
        <category ref="concepts">
            <a href="build-agent.md">Build Agent</a>
            <a href="build-configuration.md">Build Configuration</a>
        </category>
        <category ref="admin-guide">
            <a href="assigning-build-configurations-to-specific-build-agents.md">Assigning Build Configurations to Specific Build Agents</a>
            <a href="configuring-build-agent-startup-properties.md">Configuring Build Agent Startup Properties</a>
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
            <a href="configuring-agent-requirements.md">Configuring Agent Requirements</a>
        </category>
</seealso>