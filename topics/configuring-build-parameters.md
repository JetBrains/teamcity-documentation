[//]: # (title: Configuring Build Parameters)
[//]: # (auxiliary-id: Configuring Build Parameters)
[//]: # (Internal note. Do not delete. "Configuring Build Parametersd72e3.txt")

_Build parameters_ are name-value pairs, defined by a user or provided by TeamCity, which can be used in a build. They help flexibly share settings and pass them to build steps.

This article explains how to configure build parameters. See how to [use them inside build settings and build scripts](using-build-parameters.md).

<anchor name="ConfiguringBuildParameters-ConfigurationParameters"/>
<anchor name="ConfiguringBuildParameters-BuildParameters"/>

## Types of Build Parameters

There are three types of build parameters:
* [Environment variables](#Environment+Variables)
* [System properties](#System+Properties)
* [Configuration parameters](#Configuration+Parameters)

### Environment Variables

Environment variables can be passed into a spawned build process as into an environment.

They are defined by the `env.` prefix.

### System Properties

System properties can be passed into build scripts of [certain runners](#Using+Build+Parameters+in+Build+Scripts) as variables specific to a build tool.

They are defined by the `system.` prefix.

### Configuration Parameters

Configuration parameters are not passed into a build process and are only meant to share settings within a build configuration. They are the primary means for customizing a build configuration which is based on a [template](build-configuration-template.md) or uses a [meta-runner](working-with-meta-runner.md).

They come with no prefix.

<anchor name="parameter-reference"/>

## Parameter Name Restrictions

The names of configuration parameters must contain only the `[a-zA-Z0-9._-*]` characters and start with an ASCII letter.

## Predefined and Custom Parameters

TeamCity provides a set of [predefined parameters](predefined-build-parameters.md). Administrators can add custom parameters, as described below.

## Defining Build Parameters in Build Configuration

In __Build Configuration Settings | Parameters__, you can define the required system properties and set environment variables to be passed to the build script and environment when a build is started. You can redefine them in a single build run by launching a [custom build](running-custom-build.md).

Build parameters defined in a build configuration are used only within this configuration. See how to define them on a [project or agent level](levels-and-priority-of-build-parameters.md).

Any user-defined build parameter (system property or environment variable) can reference other parameters by using the following format:

```Shell

%[env|system].property_name%
For example: system.tomcat.libs=%\env.CATALINA_HOME%/lib/*.jar

```

Read more about [parameter references](#Parameter+References).

You can also choose the [parameter's type](typed-parameters.md), so the parameter is displayed as a UI field in the _Run Custom Build_ dialog. This way, you can quickly change the parameter's value in the current build run via the UI.

## Parameter References

Any textual setting in TeamCity can reference a build parameter. If you enter a string in the `%\parameter.name%` format, TeamCity will substitute it with the actual value during the build.

If a build references a parameter which is not defined, TeamCity considers it an [implicit agent requirement](agent-requirements.md#Implicit+Requirements): the build will only run on the agents with this parameter defined.  
The references to parameters which names do not satisfy the [above restrictions](#Parameter+Name+Restrictions) do not create an [implicit requirement](agent-requirements.md#Implicit+Requirements).

See [where you can use parameter references](using-build-parameters.md#Where+References+Can+Be+Used).

<seealso>
        <category ref="admin-guide">
        <a href="using-build-parameters.md">Using Build Parameters</a>
            <a href="levels-and-priority-of-build-parameters.md">Project- and Agent-Level Build Parameters</a>
            <a href="predefined-build-parameters.md">Predefined Build Parameters</a>
            <a href="typed-parameters.md">Typed Parameters</a>
            <a href="configuring-agent-requirements.md">Configuring Agent Requirements</a>
        </category>
</seealso>