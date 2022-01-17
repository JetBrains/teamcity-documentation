[//]: # (title: Using Build Parameters)
[//]: # (auxiliary-id: Using Build Parameters)

This article explains how to use [build parameters](configuring-build-parameters.md) to share settings within a build.

## Using Build Parameters in Build Configuration Settings

In most build configuration settings, you can use a [reference to a build parameter](configuring-build-parameters.md#Parameter+References) instead of using the actual value. Before starting a build, TeamCity resolves all references with the available parameters. If there are references that cannot be resolved, they are left as is, and a respective warning appears in the build log.

To reference a build parameter, use its name enclosed in percentage signs: for example, `%\teamcity.build.number%`.

>Any text enclosed in percentage characters will be interpreted by TeamCity as a reference to a parameter. If the parameter cannot be found in the build configuration, this reference becomes an _implicit agent requirement_ and such build configuration can only be run on an agent with this parameter defined. The agent-defined value will be used in the build.  
If you want to prevent TeamCity from treating the text in the percentage characters as a reference to a parameter, use two percentage characters. Every occurrence of `%\%` in the values where parameter references are supported will be replaced with `%` before passing the value to the build. For example, if you want to pass `%\Y%m%\d%H%\M%S` into the build, change it to `%\%Y%\%m%\%d%\%H%\%M%\%S`.

Password fields can also contain references to parameters: in this case, you cannot see the reference as it is masked by asterisks as any secure value.

For details on reusing or overriding parameters withing a build chain, refer to [this section](predefined-build-parameters.md#Dependency+Parameters).

### Where References Can Be Used

This is where [parameter references](configuring-build-parameters.md#Parameter+References) can be used in TeamCity:

<table><tr>

<td>

Settings

</td>

<td>

Description

</td></tr><tr>

<td>

Build runner settings, artifact specification

</td>

<td>

Any of the parameters that are passed into the build.

</td></tr><tr>

<td>

User-defined parameters and environment variables

</td>

<td>

Any of the parameters that are passed into the build.

</td></tr><tr>

<td>

Build number format

</td>

<td>

Only [predefined server build parameters](predefined-build-parameters.md).

</td></tr><tr>

<td>

VCS root and checkout rules settings

</td>

<td>

Any of the parameters that are passed into the build.

</td></tr><tr>

<td>

VCS label pattern

</td>

<td>

`system.build.number` and [predefined server build parameters](predefined-build-parameters.md#Predefined+Server+Build+Parameters).

</td></tr><tr>

<td>

Artifact dependency settings

</td>

<td>

Only [predefined server build parameters](predefined-build-parameters.md#Predefined+Server+Build+Parameters).

</td></tr></table>

## Using Build Parameters in VCS Labeling Pattern and Build Number

In the [build number](build-number.md) pattern or [VCS labeling](vcs-labeling.md) pattern, you can use the `%[env|system].parameter_name%` syntax to reference any parameter known by TeamCity:
* Predefined parameters of a [server](predefined-build-parameters.md#Server+Build+Properties) or [build configuration](predefined-build-parameters.md#Predefined+Configuration+Parameters).
* Custom build parameters added on the __Build Configuration Settings | Parameters__ page.

For example, a VCS revision number can be specified as `%\build.vcs.number%`.

## Using Build Parameters in Build Scripts

All build parameters starting with the `env.` prefix (<emphasis tooltip="environment-variable">environment variables</emphasis>) are passed into a build's process environment, without the `env.` prefix.

All build parameters starting with the `system.` prefix (<emphasis tooltip="system-property">system properties</emphasis>) are passed to the build script engines and can be referenced there by the parameter name, without the `system.` prefix. However, you need to use this prefix to access these parameters in the TeamCity UI or, for example, in the [Command Line](command-line.md) runner.

The following syntax will work _in the build script_ (_not_ in TeamCity settings):
* For [Ant](ant.md), [Maven](maven.md), and [NAnt](nant.md), use `${<parameter_name>}`.
* For [.NET](net.md), use `$(<parameter_name>)`.
    * MSBuild does not support names with dots (`.`), so you need to replace `.` with `_` when using the parameter inside a build script.
    * The `nuget push` and `nuget delete` commands do not support parameters.
* For [Gradle](gradle.md), the TeamCity system properties can be accessed as Gradle properties (similar to the ones defined in the `gradle.properties` file) and are to be referenced as follows:
    * The name allowed as a Groovy identifier (the property name does not contain dots):

     ```Shell
        
        println "Custom user property value is $\{customUserProperty\}"
     
     ```

    * The name not allowed as a Groovy identifier (the property name contains dots: for example, `build.vcs.number.1`): `project.ext["build.vcs.number.1"]`

Using the `teamcity["<parameter_name>"]` syntax is not recommended, but supported for compatibility.