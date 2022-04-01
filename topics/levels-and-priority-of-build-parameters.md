[//]: # (title: Levels and Priority of Build Parameters)
[//]: # (auxiliary-id: Levels and Priority of Build Parameters;Project and Agent Level Build Parameters)

[Build parameters](configuring-build-parameters.md) can be defined at different levels (sorted from higher to lower priority):
* Parameters defined in the _[Run Custom Build](running-custom-build.md)_ dialog.
* Parameters [defined in Build Configuration Settings](using-build-parameters.md#Using+Build+Parameters+in+Build+Configuration+Settings).
* Parameters [defined in Project Settings](#Project-Level+Build+Parameters).  
  The parameters defined for a project will be inherited by all its subprojects and build configurations. If required, you can redefine them in a single build configuration.
* Parameters defined in a [build configuration template](build-configuration-template.md).
* Parameters [defined in an agent's configuration file](#Agent-Level+Build+Parameters).
* Environment variables of the build agent process itself.
* [Predefined build parameters](predefined-build-parameters.md).

On the start of a build process, the resultant set of parameters is saved into a file which can be accessed by the build script. To learn more about this file, see the `teamcity.build.properties.file` <emphasis tooltip="system-property">system property</emphasis> or `TEAMCITY_BUILD_PROPERTIES_FILE` <emphasis tooltip="environment-variable">environment variable</emphasis> descriptions in [Predefined Build Parameters](predefined-build-parameters.md).

## Project-Level Build Parameters

TeamCity allows you to define build parameters for a project, __all__ its subprojects, and build configurations in one place: __Project Settings | Parameters__.

If a build parameter with the same name is defined both in a build configuration and on the project level, the following rules apply:

<table>

<tr><td></td><td></td><td></td></tr>

<tr><td>

__Case 1__

</td><td>

Project A, Build Configuration from project A

</td><td>

Parameters defined in the build configuration have priority over the parameters with the same names defined on the project level.

</td></tr>

<tr><td>

__Case 2__

</td><td>

Project A, Template T from project A, build configuration from project A inherited from template T

</td><td>

Parameters of the build configuration have priority over the parameters with the same name defined in project A. Project-level parameters have priority over parameters with the same name defined in the template.

</td></tr>

<tr><td>

__Case 3__

</td><td>

Project A1, Project A2, Template T from project A1, build configuration from project A2 inherited from template T

</td><td>

Parameters of project A2 (the one build configuration belongs to) have priority over the parameters with the same names defined in the template.

</td></tr>

</table>

### Expected Parameter Format

When defining <emphasis tooltip="system-property">system properties</emphasis> or <emphasis tooltip="environment-variable">environment variables</emphasis> in the `teamcity.default.properties` file, use the following format: `system.<property_name>=<property_value>` or `env.<property_name>=<property_value>`. For example, `env.CATALINA_HOME=C:\tomcat_6.0.13`.

The names of parameters must contain only the `[a-zA-Z0-9._-*]` characters and start with an ASCII letter.

### Set Parameters for Build Configurations Using Same VCS Root

You can also define parameters for only those build configurations of the project that use the same VCS root. To do that, create a text file named `teamcity.default.properties` and check it in to the VCS root. Ensure that the file appears directly in the [Build Working Directory](build-working-directory.md) by specifying the appropriate [checkout rules](configuring-vcs-settings.md#Configure+Checkout+Rules). The name and path to the file can be customized via the `teamcity.default.properties` parameter of a build configuration.  
The parameters defined this way are not visible in the TeamCity UI, but are passed directly to the build process.

<anchor name="agentSpecific"/>

## Agent-Level Build Parameters
[//]: # (AltHead: agentSpecific)

To define parameters specific to a certain [build agent](build-agent.md), you need to edit this agent's `<Agent Home>/conf/buildAgent.properties` [configuration file](configure-agent-installation.md). Refer to [this section](predefined-build-parameters.md#Predefined+Agent+Build+Parameters) for more information on available predefined parameters for agents.

The [expected format](#Expected+Parameter+Format) is the same as at the project level: `[system|env].<property_name>=<property_value>`.

<seealso>
        <category ref="admin-guide">
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
            <a href="using-build-parameters.md">Using Build Parameters</a>
            <a href="predefined-build-parameters.md">List of Predefined Build Parameters</a>
        </category>
</seealso>
