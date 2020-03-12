[//]: # (title: Configuring Build Parameters)
[//]: # (auxiliary-id: Configuring Build Parameters)
[//]: # (Internal note. Do not delete. "Configuring Build Parametersd72e3.txt")    

_Build Parameters_ provide you with flexible means of sharing settings and a convenient way of passing settings into the build.

## Types of Build Parameters

Build parameters are name\-value pairs, defined by a user or provided by TeamCity, which can be used in a build.

There are three types of build parameters:
* Environment variables (defined using `env.` prefix) are passed into the spawned build process as environment.
* System properties (defined using `system." prefix) are passed into the build scripts of the supported runners (for example, Ant, MSBuild) as build\-tool specific variables.
* Configuration parameters (no prefix) are not passed into the build and are only meant to share settings within a build configuration. They are the primary means for customizing a build configuration which is based on a [template](build-configuration-template.md) or uses a [meta-runner](working-with-meta-runner.md).

There is a set of [predefined parameters](predefined-build-parameters.md) provided by TeamCity and administrators can also add custom parameters.

The parameters can be defined at different levels (in order of precedence):
* a specific build (via [Run Custom Build](triggering-a-custom-build.md) dialog)
* the __Parameters__ page of __Build Configuration Settings__ or [Build Configuration Template](build-configuration-template.md)
* the __Parameters__ page of __Project Settings__; these affect all the build configurations and templates of the project and its subprojects
* an agent (the `<`[`Agent home`](agent-home-directory.md)`>/conf/buildAgent.properties` file on the agent)

Any textual setting can reference a parameter which makes the string in the format of `%parameter.name%` be substituted with the actual value at the time of build. If there is a reference to a parameter which is not defined, it is considered an [implicit agent requirement](agent-requirements.md#Implicit+Requirements) so the build will only run on the agents with the parameter defined.

## Parameter name restrictions

__Since 2018.1__, the name of a configuration parameter should satisfy the following requirements:
* it should only contain the following charactes: `[a-zA-Z0-9._-*]`
* it should start with an ASCII letter

TeamCity will show warning on edit parameters page if a parameter name does not satisfy these requirements.

The references to parameters which names do not satisfy the above restrictions do not create an [implicit requirement](agent-requirements.md#Implicit+Requirements).

## Defining Build Parameters in Build Configuration

On the __Parameters__ page of Build Configuration settings you can define the required system properties and environment variables to be passed to the build script and environment when a build is started. Note that you can redefine them when launching a [Custom Build](triggering-a-custom-build.md).

Build Parameters defined in a Build Configuration are used only within this configuration. For other ways, refer to [Project and Agent Level Build Parameters](project-and-agent-level-build-parameters.md).

Any user\-defined build parameter (system property or environment variable) can reference other parameters by using the following format:


```Shell

%[env|system].property_name%
For example: system.tomcat.libs=%env.CATALINA_HOME%/lib/*.jar

```



## Using Build Parameters in Build Configuration Settings

In most Build Configuration settings you can use a reference to a Build Parameter instead of using the actual value. Before starting a build, TeamCity resolves all references with the available parameters. If there are references that cannot be resolved, they are left as is and a warning will appear in the build log.

To make a reference to a build parameter, use its name enclosed in percentage signs: for example, `%teamcity.build.number%`.

Any text appearing between percentage signs is considered a reference to a property by TeamCity. If the property cannot be found in the build configuration, the reference becomes an _implicit agent requirement_ and such build configuration can only be run on an agent with the property defined. The agent\-defined value will be used in the build.

If you want to prevent TeamCity from treating the text in the percentage signs as reference to a property, use two percentage signs. Every occurrence of "`%%`" in the values where property references are supported will be replaced to "`%`" before passing the value to the build. For example, if you want to pass "`%Y%m%d%H%M%S`" into the build, change it to "`%%Y%%m%%d%%H%%M%%S`".

#### Where References Can Be Used

<table><tr>

<td>

Group of settings


</td>

<td>

References notes


</td></tr><tr>

<td>

Build Runner settings, artifact specification


</td>

<td>

any of the properties that are passed into the build


</td></tr><tr>

<td>

User\-defined properties and Environment variables


</td>

<td>

any of the properties that are passed into the build


</td></tr><tr>

<td>

Build Number format


</td>

<td>

only [Predefined Server Build Properties](predefined-build-parameters.md)


</td></tr><tr>

<td>

VCS root and checkout rules settings


</td>

<td>

any of the properties that are passed into the build


</td></tr><tr>

<td>

VCS label pattern


</td>

<td>

`system.build.number` and [Server Build Predefined Properties](predefined-build-parameters.md)


</td></tr><tr>

<td>

Artifact dependency settings


</td>

<td>

only [Predefined Server Build Properties](predefined-build-parameters.md)


</td></tr></table>

If you reference a build parameter in a build configuration, and it is not defined there, it becomes an [agent requirement](agent-requirements.md) for the configuration. The build configuration will be run only on agents that have this property defined.

Password fields can also contain references to parameters, though in this case you cannot see the reference as it is masked as any password value.

For details on using and overriding parameters from dependencies, please refer to [this section](predefined-build-parameters.md#Dependencies+Properties).

## Using Build Parameters in VCS Labeling Pattern and Build Number

In Build number pattern and VCS labeling pattern, you can use the `%[env|system].property_name%` syntax to reference the properties that are known on the server\-side. These are [server](predefined-build-parameters.md#Server+Build+Properties) and [reference](predefined-build-parameters.md#Configuration+Parameters) predefined properties and properties defined in the settings of the build configuration on the __Parameters__ page.   
For example, VCS revision number: `%build.vcs.number%`.

## Using Build Parameters in the Build Scripts

All build parameters starting with `env.` prefix (environment variables) are passed into the build's process environment (omitting the prefix).

All build parameters starting with `system.` prefix (system properties) are passed to the supported __build script engines and can be referenced there by the property name__, without `system.` prefix.   
Note that this syntax will work __in the build script__ (__not__ in TeamCity settings):
* For [Ant](ant.md), [Maven](maven.md) and [NAnt](nant.md), use `$(<property name>)`
* For [MSBuild](msbuild.md) (Visual Studio 2005/2008 Project Files), use `$(<property name>)`. Note that MSBuild does not support names with dots ("`.`"), so you need to replace "`.`" with "`_`" when using the property inside a build script.
* For [Gradle](gradle.md): the TeamCity system properties can be accessed as Gradle properties (similar to the ones defined in the `gradle.properties` file) and are to be referenced as follows:   
    a) name allowed as Groovy identifier (the property name does not contain dots):
   
     ```Shell
        
        println "Custom user property value is $\{customUserProperty\}"
     
     ```

    b) name not allowed as Groovy identifier (the property name contains dots: for example, `build.vcs.number.1`): `project.ext["build.vcs.number.1"]`

<note>


Using `teamcity["<property name>"]` is not recommended, although still supported.

</note>

When TeamCity starts a build process, the following precedence of the build parameters is used (those on top have higher priority):
 * parameters from the [Project and Agent Level Build Parameters](project-and-agent-level-build-parameters.md) file.
 * [predefined](predefined-build-parameters.md) parameters.
 * parameters defined in the Run Custom Build dialog.
 * parameters defined in the Build Configuration.
 * parameters defined in the Project (the parameters defined for a project will be inherited by all its subprojects and build configurations. If required, you can redefine them in a build configuration).
 * parameters defined in a template (if any).
 * parameters defined in the agent's `buildAgent.properties` file.
 * environment variables of the Build Agent process itself.

The resultant set of parameters is also saved into a file which can be accessed by the build script. See `teamcity.build.properties.file` system property or `TEAMCITY_BUILD_PROPERTIES_FILE` environment variable description in [Predefined Build Parameters](predefined-build-parameters.md) for details.

 __  __
 
__See also:__

__Administrator's Guide__: [Project and Agent Level Build Parameters](project-and-agent-level-build-parameters.md) | [Predefined Build Parameters](predefined-build-parameters.md) | [Configuring Agent Requirements](configuring-agent-requirements.md) 


[//]: # (Internal note. Do not delete. "Configuring Build Parametersd72e417.txt")    

__ __