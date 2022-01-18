[//]: # (title: Using Build Parameters)
[//]: # (auxiliary-id: Using Build Parameters)

This article explains how to use [build parameters](configuring-build-parameters.md) to share settings within a build.

## Using Build Parameters in Build Configuration Settings

In most build configuration settings, you can use a [reference to a build parameter](configuring-build-parameters.md#Parameter+References) instead of using the actual plain-text value. Before starting a build, TeamCity resolves all references with the available parameters. If there are references that cannot be resolved, they are left as is, and a respective warning appears in the build log.

To reference a build parameter, use its name enclosed in percentage characters: for example, `%\teamcity.build.number%`.

>Any text enclosed in percentage characters will be interpreted by TeamCity as a reference to a parameter. If the parameter cannot be found in the build configuration, this reference becomes an _[implicit agent requirement](agent-requirements.md#Implicit+Requirements)_ and such build configuration can only be run on an agent with this parameter defined. The agent-defined value will be used in the build.  
If you want to prevent TeamCity from treating the text in the percentage characters as a reference to a parameter, use two percentage characters. Every occurrence of `%\%` in the values where parameter references are supported will be replaced with `%` before passing the value to the build. For example, if you want to pass `%\Y%m%\d%H%\M%S` into the build, change it to `%\%Y%\%m%\%d%\%H%\%M%\%S`.

Password fields can also contain references to parameters. Note that in this case, you cannot see the reference when entering or editing it, as it is masked by asterisks.

For details on reusing or overriding parameters within a build chain, refer to [this section](predefined-build-parameters.md#Dependency+Parameters).

## Using Build Parameters in VCS Labeling Pattern and Build Number

In the [build number](build-number.md) pattern or [VCS labeling](vcs-labeling.md) pattern, you can use the `%<prefix>.parameter_name%` syntax to reference any parameter known by TeamCity:
* Predefined parameters of a [server](predefined-build-parameters.md#Predefined+Server+Build+Parameters) or [build configuration](predefined-build-parameters.md#Predefined+Configuration+Parameters).
* Custom build parameters added on the __Build Configuration Settings | Parameters__ page.

For example, a VCS revision number can be specified as `%\build.vcs.number%`.

## Using Build Parameters in Build Scripts

All build parameters starting with the `env.` prefix (<emphasis tooltip="environment-variable">environment variables</emphasis>) are passed into a build's process environment without the `env.` prefix.

All build parameters starting with the `system.` prefix (<emphasis tooltip="system-property">system properties</emphasis>) are passed to the build script engines and can be referenced there by the parameter name, without the `system.` prefix. However, you need to use this prefix to access these parameters in the TeamCity UI or, for example, in the [Command Line](command-line.md) runner.

The following syntax will work _in a build script_ (_not_ in TeamCity settings):
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

## Example Workflow

Let's try to define a build parameter on a build configuration level and use it in a build step.

In this example use case, we will add a configuration parameter containing your server URL. Then, we will use its value as the base part of the URL to download a certain file via a command line. To do this:
1. Go to __Build Configuration Settings | Parameters__.
2. Click __Add new parameter__.
3. Enter the parameter's name and value. Leave _Configuration parameter_ as its _Kind_.  
   * Name: `serverUrlBase`
   * Value: `https://<yourServerDomain>.com/`
     <img src="add-config-parameter.png" width="460" alt="Add configuration parameter"/>
4. Save the parameter.
5. Go to __Build Steps__.
6. Click __Add build step__.
7. Choose the [Command Line](command-line.md) runner type.
8. In the _Custom script_ field, enter the following command:
  ```Shell
  curl -o libraries_%build.number%.tar.gz %serverUrlBase%libraries.tar.gz

  ```
  For a build with the number `1234`, this command will be resolved as follows:
  ```Shell
  curl -o libraries_1234.tar.gz https://<yourServerDomain>.com/libraries.tar.gz

  ```
9. Save the build step and run a new build.

When executing this step in a build, TeamCity will download the `libraries.tar.gz` archive from your server and save it in the [checkout directory](build-checkout-directory.md) as an archive with the new name, depending on the current build's number. TeamCity generates a unique build number for each starting build and saves it as this build's `build.number` parameter, which we referenced in the new archive name. Such predefined parameters can be accessed within build steps in the same way as parameters you add manually. You can find the list of other predefined parameters [here](predefined-build-parameters.md).

You can follow this guide in your own configuration: just substitute the archive name and server URL with real values.

Parametrized build settings and scripts is one of the main advantages of TeamCity, and using them will significantly boost your pipelines' capabilities.

 <seealso>
        <category ref="admin-guide">
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
            <a href="predefined-build-parameters.md">List of Predefined Build Parameters</a>
        </category>
</seealso>