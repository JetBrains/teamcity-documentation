[//]: # (title: Levels and Priority of Build Parameters)
[//]: # (auxiliary-id: Levels and Priority of Build Parameters;Project and Agent Level Build Parameters)

[Build parameters](configuring-build-parameters.md) can be defined at different levels (sorted from higher to lower priority):
* In a [custom build](running-custom-build.md) dialog.
* On the __Parameters__ page of __[Build Configuration Settings](creating-and-editing-build-configurations.md)__ or in a [build configuration template](build-configuration-template.md).
* On the __Parameters__ page of __Project Settings__: these affect all the build configurations and templates of the project and its subprojects.
* In the `<[Agent home](agent-home-directory.md)>/conf/buildAgent.properties` [configuration file](configure-agent-installation.md) of a build agent.

When TeamCity starts a build process, the following precedence of build parameters is used (sorted from higher to lower priority):
* Parameters from the [project- and agent-level build parameters](levels-and-priority-of-build-parameters.md) file
* [predefined](predefined-build-parameters.md) parameters
* parameters defined in the _Run Custom Build_ dialog
* parameters defined in build configuration settings
* parameters defined in a project (the parameters defined for a project will be inherited by all its subprojects and build configurations; if required, you can redefine them in a build configuration)
* parameters defined in a template (if any)
* parameters defined in the agent's `buildAgent.properties` file
* environment variables of the build agent process itself

The resultant set of parameters is also saved into a file which can be accessed by the build script. See the `teamcity.build.properties.file` system property or `TEAMCITY_BUILD_PROPERTIES_FILE` environment variable description in [Predefined Build Parameters](predefined-build-parameters.md) for details.

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

<anchor name="agentSpecific"/>

## Agent-Level Build Parameters
[//]: # (AltHead: agentSpecific)

To define parameters specific to a certain [build agent](build-agent.md), edit this agent's `<Agent Home>/conf/buildAgent.properties` [configuration file](configure-agent-installation.md). Refer to [this section](predefined-build-parameters.md#Agent+Properties) for more information on available predefined parameters for agents.

When defining system properties and environment variables in the `teamcity.default.properties` or `buildAgent.properties` file, use the following format:

```Plain Text
[env|system].<property_name>=<property_value>

```

For example, `env.CATALINA_HOME=C:\tomcat_6.0.13`.

<seealso>
        <category ref="admin-guide">
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
            <a href="predefined-build-parameters.md">List of Predefined Build Parameters</a>
        </category>
</seealso>
