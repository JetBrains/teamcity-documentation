[//]: # (title: Levels and Priority of Build Parameters)
[//]: # (auxiliary-id: Levels and Priority of Build Parameters;Project and Agent Level Build Parameters)

[Build parameters](configuring-build-parameters.md) can be defined at different levels (sorted from higher to lower priority):
When TeamCity starts a build process, the following precedence of build parameters is used (sorted from higher to lower priority):
* Parameters defined in the _[Run Custom Build](running-custom-build.md)_ dialog.
* Parameters [defined in Build Configuration Settings](using-build-parameters.md#Using+Build+Parameters+in+Build+Configuration+Settings).
* Parameters [defined in Project Settings](#Project-Level+Build+Parameters). The parameters defined for a project will be inherited by all its subprojects and build configurations. If required, you can redefine them in a single build configuration.
* Parameters defined in a [build configuration template](build-configuration-template.md).
* Parameters [defined in the agent's configuration file](#Agent-Level+Build+Parameters).
* Environment variables of the build agent process itself.
* [Predefined build parameters](predefined-build-parameters.md).

The resultant set of parameters is saved into a file which can be accessed by the build script. To learn more about this file, see the `teamcity.build.properties.file` <emphasis tooltip="system-properties">system property</emphasis> or `TEAMCITY_BUILD_PROPERTIES_FILE` <emphasis tooltip="environment-variables">environment variable</emphasis> descriptions in [Predefined Build Parameters](predefined-build-parameters.md).

## Project-Level Build Parameters

TeamCity allows you to define build parameters for a project, __all__ its subprojects, and build configurations in one place: __Project Settings | Parameters__.

If a build parameter with the same name is defined both in a build configuration and on the project level, the following rules apply:
* __Case 1__: Project A, Build Configuration from project A.   
  Parameters defined in the build configuration have priority over the parameters with the same names defined on the project level.
* __Case 2__: Project A, Template T from project A, build configuration from project A inherited from template T.   
  Parameters of the build configuration have priority over the parameters with the same name defined in project A, and project-level parameters have priority over parameters with the same name defined in the template.
* __Case 3__: Project A1, Project A2, Template T from project A1, build configuration from project A2 inherited from template T.   
  Parameters of project A2 (the one build configuration belongs to) have priority over the parameters with the same names defined in the template.

You can also define parameters for only those build configurations of the project that use __the same VCS root__. To do that, create a text file named `teamcity.default.properties` and check it in to the VCS root. Ensure that the file appears directly in the [Build Working Directory](build-working-directory.md) by specifying the appropriate [checkout rules](configuring-vcs-settings.md#Configure+Checkout+Rules). The name and path to the file can be customized via the `teamcity.default.properties` parameter of a build configuration.  
The parameters defined this way are not visible in the TeamCity UI, but are passed directly to the build process.

When defining <emphasis tooltip="system-properties">system properties</emphasis> or <emphasis tooltip="environment-variables">environment variables</emphasis> in the `teamcity.default.properties` file, use the following format:

```Plain Text
[env|system].<property_name>=<property_value>

```

For example, `env.CATALINA_HOME=C:\tomcat_6.0.13`.

<anchor name="agentSpecific"/>

## Agent-Level Build Parameters
[//]: # (AltHead: agentSpecific)

To define parameters specific to a certain [build agent](build-agent.md), edit this agent's `<Agent Home>/conf/buildAgent.properties` [configuration file](configure-agent-installation.md). Refer to [this section](predefined-build-parameters.md#Agent+Properties) for more information on available predefined parameters for agents.

The expected format is the same as at the project level: `[env|system].<property_name>=<property_value>`.

<seealso>
        <category ref="admin-guide">
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
            <a href="using-build-parameters.md">Using Build Parameters</a>
            <a href="predefined-build-parameters.md">List of Predefined Build Parameters</a>
        </category>
</seealso>
