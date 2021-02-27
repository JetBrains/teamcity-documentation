[//]: # (title: Project and Agent Level Build Parameters)
[//]: # (auxiliary-id: Project and Agent Level Build Parameters)

In addition to defining [build parameters](configuring-build-parameters.md) in __Build Configuration Settings__, you can define them on the project or build agent level.

## Project Level Build Parameters

TeamCity allows you to define build parameters for a project, __all__ its subprojects, and build configurations in one place: __Project Settings | Parameters__ tab.

Note that if a build parameter P is defined in a build configuration and a build parameter with the same name exists on the project level, the following heuristics applies:

__Case 1__: Project A, Build Configuration from project A.   
Parameters defined in the build configuration have priority over the parameters with the same names defined on the project level.

__Case 2__: Project A, Template T from project A, build configuration from project A inherited from template T.   
Parameters of the build configuration have priority over the parameters with the same name defined in project A, and project-level parameters have priority over parameters with the same name defined in the template.

__Case 3__: Project A1, Project A2, Template T from project A1, build configuration from project A2 inherited from template T.   
Parameters of project A2 (the one build configuration belongs to) have priority over the parameters with the same names defined in the template.

You can also define parameters for only those build configurations of the project that use __the same VCS root__. To do that, create a text file named `teamcity.default.properties`, and check it into the VCS root. Ensure that the file appears directly in the [build working directory](build-working-directory.md) by specifying the appropriate [checkout rules](configuring-vcs-settings.md#Configure+Checkout+Rules). The name and path to the file can be customized via the `teamcity.default.properties` property of a build configuration.

The properties defined this way are not visible in the TeamCity web UI, but are passed directly to the build process.

<anchor name="agentSpecific"/>

## Agent Level Build Parameters
[//]: # (AltHead: agentSpecific)

To define agent-specific properties, edit the Build Agent's `buildAgent.properties` file (`<agent home>/conf/buildAgent.properties`). Refer to the [Agent-Specific Properties](predefined-build-parameters.md#Agent+Properties) page for more information.

When defining system properties and environment variables in the `teamcity.default.properties` or `buildAgent.properties` file, use the following format:

```Plain Text
[env|system].<property_name>=<property_value>

```

For example, `env.CATALINA_HOME=C:\tomcat_6.0.13`.

<seealso>
        <category ref="admin-guide">
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
            <a href="configuring-build-parameters.md">Defining and Using Build Parameters in Build Configuration</a>
            <a href="predefined-build-parameters.md">Predefined Build Parameters</a>
        </category>
</seealso>
