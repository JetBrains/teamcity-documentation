[//]: # (title: List of Predefined Build Parameters)
[//]: # (auxiliary-id: List of Predefined Build Parameters;Predefined Build Parameters)

TeamCity provides a number of predefined [build parameters](configuring-build-parameters.md) which are ready to be used in the settings of a build configuration or in build scripts.

Predefined build parameters can come from several scopes:
* [Server build parameters](#Predefined+Server+Build+Parameters) are generated on the server side in the scope of a particular build. For example, a build number.
* [Agent build parameters](#Predefined+Agent+Build+Parameters) are provided on the agent side in the scope of a particular build right before the build start. For example, a path to a file which contains a list of build's changes.
* [Agent environment parameters](#Predefined+Agent+Environment+Parameters) are provided by an agent on connection to the TeamCity server. They are not specific to any build and characterize the agent environment (for example, the path to the .NET framework). These are mainly used in [agent requirements](agent-requirements.md).

All these parameters are _passed to a build_.

There is also a special type of server-side build parameters that can be referenced by other parameters but that are _not passed to a build_ directly. See the [list of such parameters](#Predefined+Configuration+Parameters).

>__Tips__:
>* To see the up-to-date list of available properties when defining a text-value parameter anywhere in TeamCity, you can either click ![paramsPopupHover.gif](paramsPopupHover.gif) next to the text field, or enter `%` in the text field.
>* System properties can be [referenced](using-build-parameters.md#Using+Build+Parameters+in+Build+Configuration+Settings) as `%\system.propertyName%`.

## Predefined Server Build Parameters


Here is the list of server build parameters (<emphasis tooltip="system-property">system properties</emphasis>) predefined in TeamCity and the respective environment variables.

<table><tr>

<td width="280">

System Property

</td>

<td width="230">

Environment variable

</td>

<td>

Description

</td></tr><tr>

<td>

`teamcity.version`

</td>

<td>

`TEAMCITY_VERSION`

</td>

<td>

The version of the TeamCity server. This property can be used to determine if the build is run within TeamCity.

</td></tr><tr>

<td>

`teamcity.projectName`

</td>

<td>

`TEAMCITY_PROJECT_NAME`

</td>

<td>

The name of the project the current build belongs to.

</td></tr><tr>

<td>

`teamcity.buildConfName`

</td>

<td>

`TEAMCITY_BUILDCONF_NAME`

</td>

<td>

The name of the build configuration the current build belongs to.

</td></tr><tr>

<td>

`teamcity.buildType.id`

</td>

<td>

none

</td>

<td>

The [unique ID](identifier.md) used by TeamCity to reference the build configuration the current build belongs to.

</td></tr><tr>

<td>

`teamcity.configuration.properties.file`

</td>

<td>

none

</td>

<td>

The full name (including the path) of the file containing all the build parameters in alphabetical order.

</td></tr><tr>

<td>

`build.is.personal`

</td>

<td>

`BUILD_IS_PERSONAL`

</td>

<td>

Is set to `true` if the build is [personal](personal-build.md). Is not defined otherwise.

</td></tr><tr>

<td>

`build.number`

</td>

<td>

`BUILD_NUMBER`

</td>

<td>

The build number assigned to the build by TeamCity. The parameter uses the special [build number format](configuring-general-settings.md#Build+Number+Format).

</td></tr><tr>

<td>

none

</td>

<td>

`BUILD_URL`

</td>

<td>

The link to the current build.

</td></tr><tr>

<td>

`teamcity.build.id`

</td>

<td>

none

</td>

<td>

The internal unique ID used by TeamCity to reference builds.

</td></tr><tr>

<td>

`teamcity.auth.userId`

</td>

<td>

none

</td>

<td>

A generated username that can be used to [download artifacts](configuring-dependencies.md) of other build configurations. Valid only during the build. ([Read more details](artifact-dependencies.md#Build-level+authentication)).

</td></tr><tr>

<td>

`teamcity.auth.password`

</td>

<td>

none


</td>

<td>

A generated password that can be used to download artifacts of other build configurations. Valid only during the build. ([Read more details](artifact-dependencies.md#Build-level+authentication)).

</td></tr><tr>

<td>

`build.vcs.number.<VCS_root_ID>`

</td>

<td>

`BUILD_VCS_NUMBER_<VCS_root_ID>`

</td>

<td>

The latest VCS revision included in the build for the specified root. See [this article](configuring-vcs-roots.md) for the `<VCS_root_ID>` description. If there is one root in a build configuration, the `build.vcs.number` parameter (without the VCS root ID) is also provided.

Note that this value is a VCS-specific (for example, for SVN it is a revision number and for CVS it is a timestamp).

</td></tr></table>

## Predefined Agent Build Parameters

Agent build parameters (<emphasis tooltip="system-property">system properties</emphasis>) are unique for each build. They are calculated on the agent right before the build start and then passed to the build.

<table><tr>

<td width="280">

System Property

</td>

<td width="230">

Environment Variable

</td>

<td>

Description

</td></tr><tr>

<td>

`teamcity.build.checkoutDir`

</td>

<td>

none

</td>

<td>

The [checkout directory](build-checkout-directory.md) used for the build.

</td></tr><tr>

<td>

`teamcity.build.workingDir`

</td>

<td>

none

</td>

<td>

The [working directory](build-working-directory.md) where the build is started. This is the path where a TeamCity build runner is supposed to start a process. This is a runner-specific property, thus it has a different value for each step.

</td></tr><tr>

<td>

`teamcity.build.tempDir`

</td>

<td>

none

</td>

<td>

A full path of the build temp directory generated by TeamCity. The directory is cleaned after the build.

</td></tr><tr>

<td>

`teamcity.build.properties.file`

</td>

<td>

`TEAMCITY_BUILD_PROPERTIES_FILE`

</td>

<td>

A full name (including the path) of the file containing all the `system.*` properties passed to the build. The `system.` prefix is omitted. The file uses the [Java properties](https://java.sun.com/j2se/1.4.2/docs/api/java/util/Properties.html#load(java.io.InputStream)) file format (for example, special characters are backslash-escaped).

</td></tr><tr>

<td>

`teamcity.build.changedFiles.file`

</td>

<td>

none

</td>

<td>

A full path to the file with information about changed files included in the build.

This property could be useful if you want to [support risk test reordering](https://plugins.jetbrains.com/docs/teamcity/risk-tests-reordering-in-custom-test-runner.html) in your custom runner for tests. This file is only available if there were changes in the build. It is not available for [history builds](history-build.md).

</td></tr></table>

## Predefined Agent Environment Parameters

These agent-specific parameters are defined on each build agent and vary depending on its environment. Aside from standard parameters (for example, `teamcity.agent.jvm.os.name` or `teamcity.agent.jvm.os.arch` provided by the JVM running on an agent), agents can have parameters based on installed applications. TeamCity can automatically detect applications like .NET Framework or Visual Studio and add the corresponding <emphasis tooltip="system-property">system properties</emphasis> and <emphasis tooltip="environment-variable">environment variables</emphasis>.

If additional applications/libraries are available in the environment, the system administrator can manually define the respective parameters manually, in the `<Agent Home>/conf/buildAgent.properties` file.

These parameters can be used for setting build configuration options, defining build configuration requirements (for example, check if a certain parameter exists), and inside build scripts. For more information on how to reference these properties, see [this section](configuring-build-parameters.md#Custom+Build+Parameters).

>In the TeamCity UI, the values of parameters of a given agent can be viewed in __Agents | Agent Settings | Agent Parameters__.

Here is the list of agent environment parameters predefined in TeamCity:

<table><tr>

<td width="280">

Agent Parameter

</td>

<td>

Description

</td></tr><tr>

<td>

`teamcity.agent.name`

</td>

<td>

The name of the agent as specified in the `buildAgent.properties` [agent configuration file](configure-agent-installation.md). It can be used to set a requirement for a build configuration to run or not run on a particular build agent.

</td></tr><tr>

<td>

`teamcity.agent.work.dir`

</td>

<td>

The path to the [Agent Work Directory](agent-work-directory.md).

</td></tr><tr>

<td>

`teamcity.agent.work.dir.freeSpaceMb`

</td>

<td>

Free space available in the [Agent Work Directory](agent-work-directory.md).

</td></tr><tr>

<td>

`teamcity.agent.home.dir`

</td>

<td>

The path to the [Agent Home Directory](agent-home-directory.md).

</td></tr><tr product="tc">

<td>

`teamcity.agent.tools.dir`

</td>

<td>

The path to the [Tools](installing-agent-tools.md) directory on the agent.

</td></tr><tr>

<td>

`teamcity.agent.jvm.os.version`

</td>

<td>

The corresponding [JVM property](https://java.sun.com/j2se/1.5.0/docs/api/java/lang/System.html#getProperties()).

</td></tr><tr>

<td>

`teamcity.agent.jvm.user.country`

</td>

<td>

The corresponding [JVM property](https://java.sun.com/j2se/1.5.0/docs/api/java/lang/System.html#getProperties()).

</td></tr><tr>

<td>

`teamcity.agent.jvm.user.home`

</td>

<td>

The corresponding [JVM property](https://java.sun.com/j2se/1.5.0/docs/api/java/lang/System.html#getProperties()).

</td></tr><tr>

<td>

`teamcity.agent.jvm.user.timezone`

</td>

<td>

The corresponding [JVM property](https://java.sun.com/j2se/1.5.0/docs/api/java/lang/System.html#getProperties()).

</td></tr><tr>

<td>

`teamcity.agent.jvm.user.name`

</td>

<td>

The corresponding [JVM property](https://java.sun.com/j2se/1.5.0/docs/api/java/lang/System.html#getProperties())).

</td></tr><tr>

<td>

`teamcity.agent.jvm.user.language`

</td>

<td>

The corresponding [JVM property](https://java.sun.com/j2se/1.5.0/docs/api/java/lang/System.html#getProperties()).

</td></tr><tr>

<td>

`teamcity.agent.jvm.user.variant`

</td>

<td>

The corresponding [JVM property](https://java.sun.com/j2se/1.5.0/docs/api/java/lang/System.html#getProperties()).

</td></tr><tr>

<td>

`teamcity.agent.jvm.file.encoding`

</td>

<td>

The corresponding [JVM property](https://java.sun.com/j2se/1.5.0/docs/api/java/lang/System.html#getProperties()).

</td></tr><tr>

<td>

`teamcity.agent.jvm.file.separator`

</td>

<td>

The corresponding [JVM property](https://java.sun.com/j2se/1.5.0/docs/api/java/lang/System.html#getProperties()).

</td></tr><tr>

<td>

`teamcity.agent.jvm.path.separator`

</td>

<td>

The corresponding [JVM property](https://java.sun.com/j2se/1.5.0/docs/api/java/lang/System.html#getProperties()).

</td></tr><tr>

<td>

`teamcity.agent.jvm.specification`

</td>

<td>

The corresponding [JVM property](https://java.sun.com/j2se/1.5.0/docs/api/java/lang/System.html#getProperties()).

</td></tr><tr>

<td>

`teamcity.agent.jvm.version`

</td>

<td>

The corresponding [JVM property](https://java.sun.com/j2se/1.5.0/docs/api/java/lang/System.html#getProperties()).

</td></tr><tr>

<td>

`teamcity.agent.jvm.java.home`

</td>

<td>

See the [section below](#Java-Related+Environment+Variables) for details.

</td></tr><tr>

<td>

`teamcity.agent.os.arch.bits`

</td>

<td>

The agent's OS bitness.

</td></tr><tr>

<td>

`DotNetFramework<version>[_x86|_x64]`

</td>

<td>

This parameter is defined if the corresponding version(s) of .NET Framework runtime is installed.

</td></tr><tr>

<td>

`DotNetFramework<version>[_x86|_x64]_Path`

</td>

<td>

This parameter's value is set to the corresponding framework runtime version(s) path(s).

Note that this parameter is defined only for the latest installed version per major release.   
For example, if you have 3.5, 4.5, and 4.8 versions installed, this parameter will only be defined for 3.5 and 4.8. 4.5 will be omitted as there is a later available version of .NET Framework 4. To explicitly define such a version, consider using the [`DotNetFrameworkTargetingPack<version>_Path`](#DotNetFrameworkTargetingPack) parameter instead.

</td></tr><tr>

<td>

`DotNetFrameworkSDK<version>[_x86|_x64]`

</td>

<td>

This parameter is defined if the corresponding version(s) of .NET Framework SDK is installed.

</td></tr><tr>

<td>

`DotNetFrameworkSDK<version>[_x86|_x64]_Path`

</td>

<td>

The path of the corresponding framework SDK version.

</td></tr><tr>

<td>

<anchor name="DotNetFrameworkTargetingPack"/>

`DotNetFrameworkTargetingPack<version>_Path`

</td>

<td>

The path to the corresponding Reference assemblies (AKA Targeting Pack) location.

</td></tr><tr>

<td>

`WindowsSDK<version>`

</td>

<td>

This property is defined if the corresponding version of Windows SDK is installed.

</td></tr><tr>

<td>

`WindowsSDK<version>_Path`

</td>

<td>

This property value is the path of the corresponding version of Windows SDK.

</td></tr><tr>

<td>

`VS[2003|2008|2013|2017]`

</td>

<td>

This property is defined if the corresponding version(s) of Visual Studio is installed

</td></tr><tr>

<td>

`VS[2003|2008|2013|2017]_Path`

</td>

<td>

The path to the directory that contains `devenv.exe`.

</td></tr><tr>

<td>

`teamcity.dotnet.nunitlauncher<version>`

</td>

<td>

The path to the directory that contains the standalone NUnit test launcher, `NUnitLauncher.exe`. The version number refers to the version of .NET Framework under which the test will run. The version equals the version of .NET Framework.

</td></tr><tr>

<td>

`teamcity.dotnet.nunitlauncher.msbuild.task`

</td>

<td>

The path to the directory that contains the MSBuild task `dll` providing the NUnit task for MSBuild, Visual Studio (sln).

</td></tr><tr>

<td>

`teamcity.dotnet.msbuild.extensions2.0`

</td>

<td>

The path to the directory that contains MSBuild 2.0 listener and tasks assemblies.

</td></tr><tr>

<td>

`teamcity.dotnet.msbuild.extensions4.0`

</td>

<td>

The path to the directory that contains MSBuild 4.0 listener and tasks assemblies.

</td></tr><tr>

<td>

`teamcity.agent.ownPort`

</td>

<td>

The [agent port](configure-agent-installation.md) used by the TeamCity server to connect to the agent.

</td></tr><tr product="tc">

<td>

`teamcity.agent.protocol`

</td>

<td>

The [protocol](install-and-start-teamcity-agents.md#Agent-Server+Data+Transfer) used for data transfers between the agent and the server.

</td></tr><tr>

<td>

`teamcity.agent.cpuBenchmark`

</td>

<td>

The [CPU benchmarking](viewing-build-agent-details.md#Agent+Summary) result for the agent.

</td></tr><tr>

<td>

`teamcity.agent.hardware.cpuCount`

</td>

<td>

The number of processors in the build agent system.

</td></tr><tr>

<td>

`teamcity.agent.hostname`

</td>

<td>

The name of the build agent host.

</td></tr></table>

>__Tips__:  
>* Make sure to replace `.` with `_` when using parameters in MSBuild scripts. For example, use `teamcity_dotnet_nunitlauncher_msbuild_task` instead of `teamcity.dotnet.nunitlauncher.msbuild.task`.
>* The `_x86` and `_x64` parameter suffixes are used to designate the specific version of the framework.
>* The `teamcity.dotnet.nunitlauncher` parameters cannot be hidden or disabled.


[//]: # (Internal note. Do not delete. "Predefined Build Parametersd257e948.txt")

### Java-Related Environment Variables

When a build agent starts, it detects the installed JDK and JRE and then defines Java-related environment variables as described [below](#Defining+Java-Related+Environment+Variables). If a started agent already has the Java-related environment variables set, they are not redefined.

These variables can be used in build scripts as usual <emphasis tooltip="environment-variable">environment variables</emphasis>.

#### Detecting Java on Agent

An agent searches and launches all Java installations to verify they are valid. It determines the Java version and bitness based on the output.

The following locations are searched (some locations are common for all OSs, some are OS-specific):
* A custom directory on the agent, if defined. See [how to define a custom directory](#Defining+Custom+Directory+to+Search+for+Java).
* The [agent tools](installing-agent-tools.md) directory, `<Agent Home Directory>/tools`, is checked for containing a JRE or JDK. By default, the subdirectories of `/tools` are not scanned. To search the subdirectories, define `teamcity.agent.java.search.path=%\agent.tools.NAME%/INNER_PATH` in the `buildAgent.properties` file.  
  For Unix and macOS, remember to [set the executable bit](https://plugins.jetbrains.com/docs/teamcity/plugins-packaging.html) on the files for TeamCity to be able to launch the discovered Java.
{product="tc"}  
* In the paths specified by the `JAVA_HOME`, `JDK_HOME`, `JRE_HOME` environment variables, if defined.
* The OS-specific locations:  

  <tabs>
  <tab title="Windows">
  
  * The Windows Registry is searched for the Java installed with the Java installer.
  * The `C:\Program Files` and `C:\Program Files (x86)` directories are searched for `Java` and `JavaSoft` subdirectories.
  * `C:\Java`
  
  </tab>
  <tab title="Unix">

  * `/usr/local/java`
  * `/usr/local`
  * `/usr/java`
  * `/usr/lib/jvm`
  * `/usr`
  
  </tab>
  <tab title="macOS">

  * `/System/Library/Frameworks/JavaVM.framework/Versions/<Java Version>/Home`
  * `/Library/Java/JavaVirtualMachines/Versions/<Java Version>/Home`
  * `/Library/Java/JavaVirtualMachines/<Java Version>/Contents/Home`   

  </tab>
  </tabs>
  
* In the path specified by the `PATH` environment variable, if defined.

#### Defining Custom Directory to Search for Java

You can define a custom directory on an agent to search for Java installations. To do this, add the `teamcity.agent.java.search.path` property to the [`buildAgent.properties`](configure-agent-installation.md) file.

You can define a list of directories: the type of separator character depends on the OS.

#### Defining Java-Related Environment Variables

For each version of Java, the following variable is defined: `JDK_<major>_<minor>[_x64]`. For example, `env.JDK_1_6` (Java 6) or `env.JDK_14_0_x64` (Java 14 64-bit).

The `JDK` variables are defined when the JDK is found. Before Java 11, the `JRE` variables are defined when the JRE is found but the JDK is not.   
The `_x64` variables point to 64-bit Java only. The variables without the `_x64` suffix may point to both 32-bit or 64-bit installations but 32-bit ones are preferred.  
If several installations with the same major version and the same bitness, but different minor version/update are found, the latest one is selected.

In addition, the following variables are defined:
* `JAVA_HOME` — for the latest JDK installation (but 32-bit one is preferred).
* `JDK_HOME` — the same as `JAVA_HOME`.
* `JRE_HOME` — for the latest JRE or JDK installation (but 32-bit one is preferred), defined even if JDK is found.

The `JRE_HOME` and `JDK_HOME` variables may point to different installations. For example, if JRE 1.7 and JDK 1.6 but no JDK 1.7 installed — `JRE_HOME` will point to JRE 1.7 and `JDK_HOME` will point to JDK 1.6.

All variables point to the Java Home directories, not to binary files. For example, if you want to execute javac version 1.6, you can use the following path:

<tabs>
<tab title="In TeamCity build configuration">

```Shell
%\env.JDK_1_6%/bin/javac

```

</tab>
<tab title="In Windows bat/cmd file">

```Shell
%\JDK_1_6%\bin\javac

```

</tab>
<tab title="In Unix shell script">

```Shell
$JDK_1_6/bin/javac

```

</tab>
</tabs>

## Predefined Configuration Parameters

[//]: # (Internal note. Do not delete. "Predefined Build Parametersd257e258.txt")

Configuration parameters can be referenced by other parameters (only if defined on the __Parameters__ page), but they are not passed to the build.

To view the complete list of these server parameters, open the [Parameters tab](build-results-page.md#Parameters+Tab) of any build or download the internal _teamcity/properties/build.start.properties.gz_ artifact.




### Dependency Parameters

See the following article to learn more about dependency parameters: [](use-parameters-in-build-chains.md).

[//]: # (Internal note. Do not delete. "Predefined Build Parametersd257e388.txt")

### VCS Parameters

These are the settings of VCS roots attached to a build configuration.

VCS parameters have the following format:

```Plain Text
vcsroot.<VCS_root_ID>.<VCS_root_parameter_name>
```

* `<VCS_root_ID>` — the [VCS root ID](configuring-vcs-roots.md).
* `<VCS_root_parameter_name>` — the name of the VCS root parameter. This parameter is VCS-specific. See how to get the [available list of parameters](#Predefined+Configuration+Parameters).  

If there is only one VCS root in a build configuration, the `<VCS_root_ID>.` part can be omitted.

Parameters marked by the VCS as `secure` (for example, passwords) are not available for [referencing](configuring-build-parameters.md#Parameter+References).

### Build Branch Parameters

When TeamCity starts a build in a build configuration where a [branch specification](working-with-feature-branches.md) is configured, it adds a branch label, or logical name, to each build. This logical name is also available as a configuration parameter:

```Plain Text
teamcity.build.branch
```

To distinguish builds started on a default and a non-default branch, there is an additional boolean configuration parameter which allows differentiating these cases:

```Plain Text
teamcity.build.branch.is_default=true|false

```

For Git and Mercurial, TeamCity provides additional parameters with the names of VCS branches known at the moment of the build start. Note that these may differ from the logical branch name as per branch specification configured. This VCS branch is available from a configuration parameter with the following name:

```Plain Text
teamcity.build.vcs.branch.<VCS_root_ID>

```
where `<VCS_root_ID>` is the [VCS root ID](configuring-vcs-roots.md).

### Other Parameters

<table><tr>

<td width="280">

Parameter

</td>

<td>

Description

</td></tr><tr>

<td>

`teamcity.build.triggeredBy`

</td>

<td>

Human-friendly description of how the build was triggered.

</td></tr><tr>

<td>

`teamcity.build.triggeredBy.username`

</td>

<td>

If the build was triggered by a user, the username of this user is reported. When a build is triggered not by a user, this property is not reported.

</td></tr></table>

 <seealso>
        <category ref="admin-guide">
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
            <a href="using-build-parameters.md">Using Build Parameters</a>
        </category>
</seealso>