[//]: # (title: List of Predefined Build Parameters)
[//]: # (auxiliary-id: List of Predefined Build Parameters;Predefined Build Parameters)

TeamCity provides dozens of predefined [build parameters](configuring-build-parameters.md) ready to be used in the settings of a build configuration or in build scripts.

All these parameters (except [](#Predefined+Configuration+Parameters)) are **passed to a build process**. Note that the techniques required to access these parameters may vary depending on your build type (for example, see the [Build Properties](gradle.md#Build+properties) section for the information on accessing TeamCity system properties in Gradle builds).

> To check the list of available parameters, click the ![paramsPopupHover.gif](paramsPopupHover.gif) button next to a text field in TeamCity UI, or enter `%`.
>
{style="tip"}



## Predefined Server Build Parameters

These parameters are generated on the server side in the scope of a particular build — for example, a build number.

Server build parameters are typically available as a system property and a corresponding environment variable.

<deflist>
<def title="system.teamcity.version &nbsp; | &nbsp; env.TEAMCITY_VERSION">The version of the TeamCity server. This property can be used to determine if the build runs within TeamCity.</def>
<def title="system.teamcity.projectName &nbsp; | &nbsp; env.TEAMCITY_PROJECT_NAME">The name of the project the current build belongs to.</def>
<def title="system.teamcity.buildConfName &nbsp; | &nbsp; env.TEAMCITY_BUILDCONF_NAME">The name of the build configuration the current build belongs to.</def>
<def title="system.teamcity.buildType.id">The <a href="identifier.md">unique ID</a> used by TeamCity to reference the build configuration the current build belongs to.</def>
<def title="system.teamcity.configuration.properties.file">The full name (including the path) of the file that contains all the build parameters in alphabetical order. This file is written when a build process starts and uses the <a href="https://docs.oracle.com/cd/E23095_01/Platform.93/ATGProgGuide/html/s0204propertiesfileformat01.html">Java Properties File format</a> (for example, special characters are backslash-escaped). See also: <a href="#BuildPropertiesFile">build.properties.file</a></def>
<def title="system.build.is.personal &nbsp; | &nbsp; env.BUILD_IS_PERSONAL">Returns <code>true</code> for <a href="personal-build.md">personal builds</a>. Undefined (does not exist) for regular builds.</def>
<def title="system.build.number &nbsp; | &nbsp; env.BUILD_NUMBER">The build number assigned to the build by TeamCity. The parameter uses the special <a href="configuring-general-settings.md#Build+Number+Format">build number format</a>.</def>
<def title="env.BUILD_URL">The link to the current build.</def>
<def title="system.teamcity.build.id">The internal unique ID used by TeamCity to reference builds.</def>
<def instance="tc" title="teamcity.build.responsibleNode.id">In <a href="multinode-setup.md">multinode setup</a>, this parameter returns the node to which it is attached. Can be assigned to the <code>X-TeamCity-Node-Id-Cookie</code> cookie when sending logging <a href="teamcity-rest-api.md">REST API</a> requests to the <code>/app/rest/builds/id:build_id/log</code> endpoint.</def>
<def title="system.teamcity.auth.userId">A generated username that can be used to <a href="configuring-dependencies.md">download artifacts</a> of other build configurations. Valid only during the build. See this section for more information: <a href="artifact-dependencies.md#Build-level+authentication">Build-level Authentication</a>.</def>
<def title="system.teamcity.auth.password">A generated password that can be used to <a href="configuring-dependencies.md">download artifacts</a> of other build configurations. Valid only during the build. See this section for more information: <a href="artifact-dependencies.md#Build-level+authentication">Build-level Authentication</a>.</def>
<def title="system.build.vcs.number.&lt;VCS_root_ID&gt; &nbsp; | &nbsp; env.BUILD_VCS_NUMBER_&lt;VCS_root_ID&gt;">The latest VCS revision included in the build for the specified root. See the following article for more information about VCS root IDs: <a href="configuring-vcs-roots.md">Configuring VCS Roots</a>.<br/><br/>If a build configuration has only one VCS root, you can use the <code>build.vcs.number</code> parameter without the root ID identifier.<br/><br/>This value is a VCS-specific. For example, a revision number for SVN and a timestamp for CVS.</def>
</deflist>





## Predefined Agent Build Parameters

These <tooltip term="system-property">_system properties_</tooltip> are unique for each build (for example, a path to a file that contains a list of changes). Their values are calculated on the agent side right before the build starts and then passed to the build.


<deflist>
<def title="system.teamcity.build.checkoutDir">The <a href="build-checkout-directory.md">checkout directory</a> used for the build.</def>
<def title="system.teamcity.build.workingDir">The <a href="build-working-directory.md">working directory</a> where the build is started. This is the path where a TeamCity build runner is supposed to start a process. This is a runner-specific property, thus it has a different value for each step.</def>
<def title="system.teamcity.build.tempDir">A full path of the build temp directory generated by TeamCity. The directory is cleaned after the build.</def>
<def title="system.teamcity.build.changedFiles.file">A full path to the file with information about changed files included in the build. You can use this property to <a href="https://plugins.jetbrains.com/docs/teamcity/risk-tests-reordering-in-custom-test-runner.html">support risk test reordering</a> in your custom runner for tests.<br/><br/>This file is not available for <a href="history-build.md">history builds</a> and if there were no changes in this particular build.</def>

<def title="system.teamcity.build.properties.file &nbsp; | &nbsp; env.TEAMCITY_BUILD_PROPERTIES_FILE" id="BuildPropertiesFile">The full name (including the path) of the file containing all build system properties without their <code>system.</code> postfix. This file is written when a build process starts and uses the <a href="https://docs.oracle.com/cd/E23095_01/Platform.93/ATGProgGuide/html/s0204propertiesfileformat01.html">Java Properties File format</a> (for example, special characters are backslash-escaped).</def>
</deflist>






## Predefined Agent Environment Parameters

These agent-specific parameters are defined on each build agent and vary depending on its environment. Aside from standard parameters (for example, `teamcity.agent.jvm.os.name` or `teamcity.agent.jvm.os.arch` provided by the JVM running on an agent), agents can have parameters based on their installed software. TeamCity automatically detects software like .NET Framework, Mono, or Visual Studio and adds the corresponding <tooltip term="system-property">_system properties_</tooltip> and <tooltip term="environment-variable">_environment variables_</tooltip>.

If an agent machine has additional software installed, system administrators can modify the `<Agent Home>/conf/buildAgent.properties` file to override values of corresponding parameters. 

Agent environment parameters can be used to set build configuration options, define [agent requirements](configuring-build-parameters.md#Specify+Agent+Requirements), and [inside build scripts](configuring-build-parameters.md#Pass+Values+to+Builders%27+Configuration+Files).

To check all existing parameters and their current values for a given build agent, open the agent details page and switch to the **Parameters** tab. See this link for more information: [](levels-and-priority-of-build-parameters.md#Checking+Parameter+Values).


<deflist>

<def instance="tc" title="teamcity.agent.name">The name of the agent as specified in the <code>buildAgent.properties</code> <a href="configure-agent-installation.md">agent configuration file</a>. You can use agent names to specify <a href="configuring-build-parameters.md#Specify+Agent+Requirements">agent requirements</a> and limit the number of agents a target configuration can use.</def>
<def instance="tcc" title="teamcity.agent.name">The agent's name. You can use agent names to specify <a href="configuring-build-parameters.md#Specify+Agent+Requirements">agent requirements</a> and limit the number of agents a target configuration can use.</def>

<def title="teamcity.agent.work.dir">The path to the <a href="agent-work-directory.md">agent work directory</a>.</def>
<def title="teamcity.agent.work.dir.freeSpaceMb">Free space available in the <a href="agent-work-directory.md">agent work directory</a>.</def>
<def title="teamcity.agent.home.dir">The path to the <a href="agent-home-directory.md">agent home directory</a></def>

<def instance="tc" title="teamcity.agent.tools.dir">The path to the <a href="installing-agent-tools.md">Tools</a> directory on the agent.</def>

<def title="teamcity.agent.jvm.os.version">The operating system version. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/System.html#getProperties()">corresponding JVM property</a>.</def>
<def title="teamcity.agent.jvm.user.country">The user country. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/util/Locale.html">corresponding JVM property</a>.</def>
<def title="teamcity.agent.jvm.user.home">The user's home directory. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/System.html#getProperties()">corresponding JVM property</a>.</def>
<def title="teamcity.agent.jvm.user.timezone">The user's timezone. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/util/TimeZone.html">corresponding JVM property</a>.</def>
<def title="teamcity.agent.jvm.user.name">The user's account name. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/System.html#getProperties()">corresponding JVM property</a>.</def>
<def title="teamcity.agent.jvm.user.language">The user's primary OS language. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/util/Locale.html">corresponding JVM property</a>.</def>
<def title="teamcity.agent.jvm.user.variant">An arbitrary value used to indicate a variation of a user locale. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/util/Locale.html">corresponding JVM property</a>.</def>
<def title="teamcity.agent.jvm.file.encoding">The name of the default charset, defaults to UTF-8. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/System.html#getProperties()">corresponding JVM property</a>.</def>
<def title="teamcity.agent.jvm.file.separator">The file separator. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/System.html#getProperties()">corresponding JVM property</a>.</def>
<def title="teamcity.agent.jvm.path.separator">The path separator. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/System.html#getProperties()">corresponding JVM property</a>.</def>
<def title="teamcity.agent.jvm.specification">Java Runtime Environment specification version. Return the value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/System.html#getProperties()">java.specification.version JVM property</a>.</def>
<def title="teamcity.agent.jvm.version">Java Virtual Machine implementation version. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/System.html#getProperties()">corresponding JVM property</a>.</def>
<def title="teamcity.agent.jvm.java.home">Java installation directory. See the following section for more information: <a href="#Java-Related+Environment+Variables">Java-Related Environment Variables</a>.</def>
<def title="teamcity.agent.os.arch.bits">The agent's OS bitness.</def>



<snippet id="dotnet-related-properties">

<def title="DotNetCLI">The .NET CLI version.</def>
<def title="DotNetCLI_Path">The path to .NET CLI executable.</def>
<def title="DotNetFramework&lt;version&gt;[_x86|_x64]">Defined only if the corresponding version(s) of .NET Framework runtime is installed.</def>
<def title="DotNetFramework&lt;version&gt;[_x86|_x64]_Path">This parameter's value is set to the corresponding framework runtime version(s) path(s). <br/><br/>Note that this parameter is defined only for the latest installed version per major release. For example, if you installed versions 3.5, 4.5, and 4.8, this parameter will only be defined for 3.5 and 4.8. Version/Parameter 4.5 will be omitted since a newer version of .NET Framework 4 is present. To explicitly define such a version, consider using the <code>DotNetFrameworkTargetingPack&lt;version&gt;_Path</code> parameter instead.</def>
<def title="DotNetFrameworkSDK&lt;version&gt;[_x86|_x64]">Defined if the corresponding version(s) of .NET Framework SDK is installed.</def>
<def title="DotNetFrameworkSDK&lt;version&gt;[_x86|_x64]_Path">The path to the corresponding framework SDK version.</def>
<def title="DotNetFrameworkTargetingPack&lt;version&gt;_Path">The path to the corresponding Reference assemblies (AKA Targeting Pack) location.</def>
<def title="DotNetCoreSDKx.x_Path">The .NET SDK version.</def>
<def title="DotNetWorkloads_&lt;version&gt;">Lists all <a href="https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-workload-install">.NET workloads</a> installed on the agent machine.<br/><br/>The <code>&lt;version&gt;</code> suffix is the version of an installed .NET SDK. For instance, if version 7.0.300 is installed, the agent will report the `DotNetWorkloads_7.0.300` parameter.<br/><br/>In addition to these full SDK versions, agents report workload parameters with shortened <code>major.minor</code> suffixes. For example, if an agent machine has 7.0.100, 7.0.200, and 7.0.300 .NET SDKs installed, the <code>DotNetWorkloads_7.0</code> parameter that refers to the highest 7.0.300 version will be reported.<br/><br/>The parameter value is a string of comma-separated workload names, according to folders in the <b>&lt;dotnet_dir&gt;/metadata/workloads/&lt;sdk_version&gt;/InstalledWorkloads</b> directory. For instance, "android,maui-ios,wasm-tools".</def>
<def title="WindowsSDK&lt;version&gt;">Defined only if the corresponding version of Windows SDK is installed.</def>
<def title="WindowsSDK&lt;version&gt;_Path">The path of the corresponding version of Windows SDK.</def>
<def title="VS&lt;Version&gt;">Defined if the corresponding version(s) of Visual Studio is installed</def>
<def title="VS&lt;Version&gt;_Path">The path to the Visual Studio installation folder (the directory that contains devenv.exe).</def>
<def title="teamcity.dotnet.nunitlauncher&lt;version&gt;">The path to the directory that contains the standalone NUnit test launcher, <code>NUnitLauncher.exe</code>. The version number refers to the version of .NET Framework under which the test will run. The version equals the version of .NET Framework.</def>
<def title="teamcity.dotnet.nunitlauncher.msbuild.task">The path to the directory that contains the MSBuild task <code>dll</code> providing the NUnit task for MSBuild, Visual Studio (sln).</def>
<def title="teamcity.dotnet.msbuild.extensions2.0">The path to the directory that contains MSBuild 2.0 listener and tasks assemblies.</def>
<def title="teamcity.dotnet.msbuild.extensions4.0">The path to the directory that contains MSBuild 4.0 listener and tasks assemblies.</def>


</snippet>

<def title="teamcity.agent.ownPort">The <a href="configure-agent-installation.md#Build+Agent+Port">agent port</a> used by the TeamCity server to connect to the agent.</def>
<def title="teamcity.agent.protocol">The <a href="install-and-start-teamcity-agents.md#Agent-Server+Data+Transfer">protocol</a> used for data transfers between the agent and the server.</def>
<def title="teamcity.agent.cpuBenchmark">The <a href="viewing-build-agent-details.md#Agent+Summary">CPU benchmarking</a> result for the agent.</def>
<def title="teamcity.agent.hardware.cpuCount">The number of cores/threads on the agent machine's CPU.</def>
<def title="teamcity.agent.hostname">The name of the build agent host.</def>


</deflist>



> * Make sure to replace `.` with `_` when using parameters in MSBuild scripts. For example, use `teamcity_dotnet_nunitlauncher_msbuild_task` instead of `teamcity.dotnet.nunitlauncher.msbuild.task`.
> * The `_x86` and `_x64` parameter suffixes are used to designate the specific version of the framework.
> * The `teamcity.dotnet.nunitlauncher` parameters cannot be hidden or disabled.
>
{style="tip"}


<!--[//]: # (Internal note. Do not delete. "Predefined Build Parametersd257e948.txt")-->

### Java-Related Environment Variables

When a build agent starts, it detects the installed JDK and JRE and then defines Java-related environment variables as described [below](#Defining+Java-Related+Environment+Variables). If a started agent already has the Java-related environment variables set, they are not redefined.

These variables can be used in build scripts as usual <tooltip term="environment-variable">_environment variables_</tooltip>.

#### Detecting Java on Agent

An agent searches and launches all Java installations to verify they are valid. It determines the Java version and bitness based on the output.

The following locations are searched (some locations are common for all OSs, some are OS-specific):
* A custom directory on the agent, if defined. See [how to define a custom directory](#Defining+Custom+Directory+to+Search+for+Java).
* The [agent tools](installing-agent-tools.md) directory, `<Agent Home Directory>/tools`, is checked for containing a JRE or JDK. By default, the subdirectories of `/tools` are not scanned. To search the subdirectories, define `teamcity.agent.java.search.path=%\agent.tools.NAME%/INNER_PATH` in the `buildAgent.properties` file.  
  For Unix and macOS, remember to [set the executable bit](https://plugins.jetbrains.com/docs/teamcity/plugins-packaging.html) on the files for TeamCity to be able to launch the discovered Java.
{instance="tc"}  
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

<!--[//]: # (Internal note. Do not delete. "Predefined Build Parametersd257e258.txt")-->

Configuration parameters can be referenced by other parameters (only if defined on the __Parameters__ page), but they are not passed to the build.

To view the complete list of these server parameters, open the [Parameters tab](build-results-page.md#Parameters+Tab) of any build or download the internal _teamcity/properties/build.start.properties.gz_ artifact.




### Dependency Parameters

See the following article to learn more about dependency parameters: [](use-parameters-in-build-chains.md).

<!--[//]: # (Internal note. Do not delete. "Predefined Build Parametersd257e388.txt")-->

### VCS Parameters

These are the settings of VCS roots attached to a build configuration.

VCS parameters have the following format:

```
vcsroot.<VCS_root_ID>.<VCS_root_parameter_name>
```

* `<VCS_root_ID>` — the [VCS root ID](configuring-vcs-roots.md).
* `<VCS_root_parameter_name>` — the name of the VCS root parameter. This parameter is VCS-specific. See how to get the [available list of parameters](#Predefined+Configuration+Parameters).  

If there is only one VCS root in a build configuration, the `<VCS_root_ID>.` part can be omitted.

Parameters marked by the VCS as `secure` (for example, passwords) are not available for [referencing](configuring-build-parameters.md#Parameter+References).

### Build Branch Parameters

When TeamCity starts a build in a build configuration where a [branch specification](working-with-feature-branches.md) is configured, it adds a branch label, or logical name, to each build. This logical name is also available as a configuration parameter:

```
teamcity.build.branch
```

To distinguish builds started on a default and a non-default branch, there is an additional boolean configuration parameter which allows differentiating these cases:

```
teamcity.build.branch.is_default=true|false

```

For Git and Mercurial, TeamCity provides additional parameters with the names of VCS branches known at the moment of the build start. Note that these may differ from the logical branch name as per branch specification configured. This VCS branch is available from a configuration parameter with the following name:

```
teamcity.build.vcs.branch.<VCS_root_ID>

```
where `<VCS_root_ID>` is the [VCS root ID](configuring-vcs-roots.md).

### Other Parameters

<deflist>
  <def title="teamcity.build.triggeredBy">Returns the human-friendly description of how the build was triggered.</def>
  <def title="teamcity.build.triggeredBy.username">If the build was triggered by a user, the username of this user is reported. When a build is triggered not by a user, this property is not reported.</def>
</deflist>




 <seealso>
        <category ref="admin-guide">
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
            <a href="using-build-parameters.md">Using Build Parameters</a>
        </category>
</seealso>
