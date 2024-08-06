[//]: # (title: List of Predefined Build Parameters)
[//]: # (auxiliary-id: List of Predefined Build Parameters;Predefined Build Parameters)

TeamCity provides dozens of predefined [build parameters](configuring-build-parameters.md) ready to be used in the settings of a build configuration or in build scripts.

All these parameters (except [](#Predefined+Configuration+Parameters)) are **passed to a build process**. Note that the techniques required to access these parameters may vary depending on your build type (for example, see the [Build Properties](gradle.md#Build+properties) section for the information on accessing TeamCity system properties in Gradle builds).

> To check the list of available parameters, click the ![paramsPopupHover.gif](paramsPopupHover.gif) button next to a text field in TeamCity UI, or enter `%`.
>
{type="tip"}



## Predefined Server Build Parameters

These parameters are generated on the server side in the scope of a particular build — for example, a build number.

Server build parameters are typically available as a system property and a corresponding environment variable.

<dl>


<dt>system.teamcity.version &nbsp; | &nbsp; env.TEAMCITY_VERSION</dt>
<dd>The version of the TeamCity server. This property can be used to determine if the build runs within TeamCity.</dd>


<dt>system.teamcity.projectName &nbsp; | &nbsp; env.TEAMCITY_PROJECT_NAME</dt>
<dd>The name of the project the current build belongs to.</dd>

<dt>system.teamcity.buildConfName &nbsp; | &nbsp; env.TEAMCITY_BUILDCONF_NAME</dt>
<dd>The name of the build configuration the current build belongs to.</dd>

<dt>system.teamcity.buildType.id</dt>
<dd>The <a href="identifier.md">unique ID</a> used by TeamCity to reference the build configuration the current build belongs to.</dd>



<dt>system.teamcity.configuration.properties.file</dt>
<dd>The full name (including the path) of the file that contains all the build parameters in alphabetical order. This file is written when a build process starts and uses the <a href="https://docs.oracle.com/cd/E23095_01/Platform.93/ATGProgGuide/html/s0204propertiesfileformat01.html">Java Properties File format</a> (for example, special characters are backslash-escaped). See also: <a href="#BuildPropertiesFile">build.properties.file</a></dd>



<dt>system.build.is.personal &nbsp; | &nbsp; env.BUILD_IS_PERSONAL</dt>
<dd>Returns <code>true</code> for <a href="personal-build.md">personal builds</a>. Undefined (does not exist) for regular builds.</dd>


<dt>system.build.number &nbsp; | &nbsp; env.BUILD_NUMBER</dt>
<dd>The build number assigned to the build by TeamCity. The parameter uses the special <a href="configuring-general-settings.md#Build+Number+Format">build number format</a>.</dd>


<dt>env.BUILD_URL</dt>
<dd>The link to the current build.</dd>

<dt>system.teamcity.build.id</dt>
<dd>The internal unique ID used by TeamCity to reference builds.</dd>

<dt>teamcity.build.responsibleNode.id</dt>
<dd>In <a href="multinode-setup.md">multinode setup</a>, this parameter returns the node to which it is attached. Can be assigned to the <code>X-TeamCity-Node-Id-Cookie</code> cookie when sending logging <a href="teamcity-rest-api.md">REST API</a> requests to the <code>/app/rest/builds/id:build_id/log</code> endpoint.</dd>

<dt>system.teamcity.auth.userId</dt>
<dd>A generated username that can be used to <a href="configuring-dependencies.md">download artifacts</a> of other build configurations. Valid only during the build. See this section for more information: <a href="artifact-dependencies.md#Build-level+authentication">Build-level Authentication</a>.</dd>

<dt>system.teamcity.auth.password</dt>
<dd>A generated password that can be used to <a href="configuring-dependencies.md">download artifacts</a> of other build configurations. Valid only during the build. See this section for more information: <a href="artifact-dependencies.md#Build-level+authentication">Build-level Authentication</a>.</dd>

<dt>system.build.vcs.number.&lt;VCS_root_ID&gt; &nbsp; | &nbsp; env.BUILD_VCS_NUMBER_&lt;VCS_root_ID&gt;</dt>
<dd>The latest VCS revision included in the build for the specified root. See the following article for more information about VCS root IDs: <a href="configuring-vcs-roots.md">Configuring VCS Roots</a>.<br/><br/>If a build configuration has only one VCS root, you can use the <code>build.vcs.number</code> parameter without the root ID identifier.<br/><br/>This value is a VCS-specific. For example, a revision number for SVN and a timestamp for CVS.</dd>

</dl>





## Predefined Agent Build Parameters

These <emphasis tooltip="system-property">system properties</emphasis> are unique for each build (for example, a path to a file that contains a list of changes). Their values are calculated on the agent side right before the build starts and then passed to the build.


<dl>

<dt>system.teamcity.build.checkoutDir</dt>
<dd>The <a href="build-checkout-directory.md">checkout directory</a> used for the build.</dd>


<dt>system.teamcity.build.workingDir</dt>
<dd>The <a href="build-working-directory.md">working directory</a> where the build is started. This is the path where a TeamCity build runner is supposed to start a process. This is a runner-specific property, thus it has a different value for each step.</dd>


<dt>system.teamcity.build.tempDir</dt>
<dd>A full path of the build temp directory generated by TeamCity. The directory is cleaned after the build.</dd>

<dt>system.teamcity.build.changedFiles.file</dt>
<dd>A full path to the file with information about changed files included in the build. You can use this property to <a href="https://plugins.jetbrains.com/docs/teamcity/risk-tests-reordering-in-custom-test-runner.html">support risk test reordering</a> in your custom runner for tests.<br/><br/>This file is not available for <a href="history-build.md">history builds</a> and if there were no changes in this particular build.</dd>

<anchor name="BuildPropertiesFile"/>
<dt>system.teamcity.build.properties.file &nbsp; | &nbsp; env.TEAMCITY_BUILD_PROPERTIES_FILE</dt>
<dd>The full name (including the path) of the file containing all build system properties without their <code>system.</code> postfix. This file is written when a build process starts and uses the <a href="https://docs.oracle.com/cd/E23095_01/Platform.93/ATGProgGuide/html/s0204propertiesfileformat01.html">Java Properties File format</a> (for example, special characters are backslash-escaped).</dd>

</dl>






## Predefined Agent Environment Parameters

These agent-specific parameters are defined on each build agent and vary depending on its environment. Aside from standard parameters (for example, `teamcity.agent.jvm.os.name` or `teamcity.agent.jvm.os.arch` provided by the JVM running on an agent), agents can have parameters based on their installed software. TeamCity automatically detects software like .NET Framework, Mono, or Visual Studio and adds the corresponding <emphasis tooltip="system-property">system properties</emphasis> and <emphasis tooltip="environment-variable">environment variables</emphasis>.

If an agent machine has additional software installed, system administrators can modify the `<Agent Home>/conf/buildAgent.properties` file to override values of corresponding parameters. 

Agent environment parameters can be used to set build configuration options, define [agent requirements](configuring-build-parameters.md#Specify+Agent+Requirements), and [inside build scripts](configuring-build-parameters.md#Pass+Values+to+Builders%27+Configuration+Files).

To check all existing parameters and their current values for a given build agent, open the agent details page and switch to the **Parameters** tab. See this link for more information: [](levels-and-priority-of-build-parameters.md#Checking+Parameter+Values).


<dl>

<dt product="tc">teamcity.agent.name</dt>
<dd product="tc">The name of the agent as specified in the <code>buildAgent.properties</code> <a href="configure-agent-installation.md">agent configuration file</a>. You can use agent names to specify <a href="configuring-build-parameters.md#Specify+Agent+Requirements">agent requirements</a> and limit the number of agents a target configuration can use.</dd>

<dt product="tcc">teamcity.agent.name</dt>
<dd product="tcc">The agent's name. You can use agent names to specify <a href="configuring-build-parameters.md#Specify+Agent+Requirements">agent requirements</a> and limit the number of agents a target configuration can use.</dd>


<dt>teamcity.agent.work.dir</dt>
<dd>The path to the <a href="agent-work-directory.md">agent work directory</a>.</dd>

<dt>teamcity.agent.work.dir.freeSpaceMb</dt>
<dd>Free space available in the <a href="agent-work-directory.md">agent work directory</a>.</dd>

<dt>teamcity.agent.home.dir</dt>
<dd>The path to the <a href="agent-home-directory.md">agent home directory</a></dd>

<dt product="tc">teamcity.agent.tools.dir</dt>
<dd product="tc">The path to the <a href="installing-agent-tools.md">Tools</a> directory on the agent.</dd>

<dt>teamcity.agent.jvm.os.version</dt>
<dd>The operating system version. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/System.html#getProperties()">corresponding JVM property</a>.</dd>

<dt>teamcity.agent.jvm.user.country</dt>
<dd>The user country. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/util/Locale.html">corresponding JVM property</a>.</dd>

<dt>teamcity.agent.jvm.user.home</dt>
<dd>The user's home directory. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/System.html#getProperties()">corresponding JVM property</a>.</dd>

<dt>teamcity.agent.jvm.user.timezone</dt>
<dd>The user's timezone. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/util/TimeZone.html">corresponding JVM property</a>.</dd>

<dt>teamcity.agent.jvm.user.name</dt>
<dd>The user's account name. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/System.html#getProperties()">corresponding JVM property</a>.</dd>

<dt>teamcity.agent.jvm.user.language</dt>
<dd>The user's primary OS language. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/util/Locale.html">corresponding JVM property</a>.</dd>

<dt>teamcity.agent.jvm.user.variant</dt>
<dd>An arbitrary value used to indicate a variation of a user locale. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/util/Locale.html">corresponding JVM property</a>.</dd>

<dt>teamcity.agent.jvm.file.encoding</dt>
<dd>The name of the default charset, defaults to UTF-8. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/System.html#getProperties()">corresponding JVM property</a>.</dd>

<dt>teamcity.agent.jvm.file.separator</dt>
<dd>The file separator. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/System.html#getProperties()">corresponding JVM property</a>.</dd>

<dt>teamcity.agent.jvm.path.separator</dt>
<dd>The path separator. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/System.html#getProperties()">corresponding JVM property</a>.</dd>

<dt>teamcity.agent.jvm.specification</dt>
<dd>Java Runtime Environment specification version. Return the value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/System.html#getProperties()">java.specification.version JVM property</a>.</dd>

<dt>teamcity.agent.jvm.version</dt>
<dd>Java Virtual Machine implementation version. Reports a value of the <a href="https://docs.oracle.com/en/java/javase/20/docs/api/java.base/java/lang/System.html#getProperties()">corresponding JVM property</a>.</dd>

<dt>teamcity.agent.jvm.java.home</dt>
<dd>Java installation directory. See the following section for more information: <a href="#Java-Related+Environment+Variables">Java-Related Environment Variables</a>.</dd>

<dt>teamcity.agent.os.arch.bits</dt>
<dd>The agent's OS bitness.</dd>


<chunk include-id="dotnet-related-properties">

<dt>DotNetCLI</dt>
<dd>The .NET CLI version.</dd>


<dt>DotNetCLI_Path</dt>
<dd>The path to .NET CLI executable.</dd>


<dt>DotNetFramework&lt;version&gt;[_x86|_x64]</dt>
<dd>Defined only if the corresponding version(s) of .NET Framework runtime is installed.</dd>


<dt>DotNetFramework&lt;version&gt;[_x86|_x64]_Path</dt>
<dd>This parameter's value is set to the corresponding framework runtime version(s) path(s). <br/><br/>Note that this parameter is defined only for the latest installed version per major release. For example, if you installed versions 3.5, 4.5, and 4.8, this parameter will only be defined for 3.5 and 4.8. Version/Parameter 4.5 will be omitted since a newer version of .NET Framework 4 is present. To explicitly define such a version, consider using the <code>DotNetFrameworkTargetingPack&lt;version&gt;_Path</code> parameter instead.</dd>

<dt>DotNetFrameworkSDK&lt;version&gt;[_x86|_x64]</dt>
<dd>Defined if the corresponding version(s) of .NET Framework SDK is installed.</dd>

<dt>DotNetFrameworkSDK&lt;version&gt;[_x86|_x64]_Path</dt>
<dd>The path to the corresponding framework SDK version.</dd>

<dt>DotNetFrameworkTargetingPack&lt;version&gt;_Path</dt>
<dd>The path to the corresponding Reference assemblies (AKA Targeting Pack) location.</dd>

<dt>DotNetCoreSDKx.x_Path</dt>
<dd>The .NET SDK version.</dd>

<dt>DotNetWorkloads_&lt;version&gt;</dt>
<dd>Lists all <a href="https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-workload-install">.NET workloads</a> installed on the agent machine.<br/><br/>The <code>&lt;version&gt;</code> suffix is the version of an installed .NET SDK. For instance, if version 7.0.300 is installed, the agent will report the `DotNetWorkloads_7.0.300` parameter.<br/><br/>In addition to these full SDK versions, agents report workload parameters with shortened <code>major.minor</code> suffixes. For example, if an agent machine has 7.0.100, 7.0.200, and 7.0.300 .NET SDKs installed, the <code>DotNetWorkloads_7.0</code> parameter that refers to the highest 7.0.300 version will be reported.<br/><br/>The parameter value is a string of comma-separated workload names, according to folders in the <b>&lt;dotnet_dir&gt;/metadata/workloads/&lt;sdk_version&gt;/InstalledWorkloads</b> directory. For instance, "android,maui-ios,wasm-tools".</dd>

<dt>WindowsSDK&lt;version&gt;</dt>
<dd>Defined only if the corresponding version of Windows SDK is installed.</dd>

<dt>WindowsSDK&lt;version&gt;_Path</dt>
<dd>The path of the corresponding version of Windows SDK.</dd>

<dt>VS&lt;Version&gt;</dt>
<dd>Defined if the corresponding version(s) of Visual Studio is installed</dd>

<dt>VS&lt;Version&gt;_Path</dt>
<dd>The path to the Visual Studio installation folder (the directory that contains devenv.exe).</dd>

<dt>teamcity.dotnet.nunitlauncher&lt;version&gt;</dt>
<dd>The path to the directory that contains the standalone NUnit test launcher, <code>NUnitLauncher.exe</code>. The version number refers to the version of .NET Framework under which the test will run. The version equals the version of .NET Framework.</dd>

<dt>teamcity.dotnet.nunitlauncher.msbuild.task</dt>
<dd>The path to the directory that contains the MSBuild task <code>dll</code> providing the NUnit task for MSBuild, Visual Studio (sln).</dd>

<dt>teamcity.dotnet.msbuild.extensions2.0</dt>
<dd>The path to the directory that contains MSBuild 2.0 listener and tasks assemblies.</dd>

<dt>teamcity.dotnet.msbuild.extensions4.0</dt>
<dd>The path to the directory that contains MSBuild 4.0 listener and tasks assemblies.</dd>

</chunk>

<dt>teamcity.agent.ownPort</dt>
<dd>The <a href="configure-agent-installation.md#Build+Agent+Port">agent port</a> used by the TeamCity server to connect to the agent.</dd>


<dt>teamcity.agent.protocol</dt>
<dd>The <a href="install-and-start-teamcity-agents.md#Agent-Server+Data+Transfer">protocol</a> used for data transfers between the agent and the server.</dd>


<dt>teamcity.agent.cpuBenchmark</dt>
<dd>The <a href="viewing-build-agent-details.md#Agent+Summary">CPU benchmarking</a> result for the agent.</dd>


<dt>teamcity.agent.hardware.cpuCount</dt>
<dd>The number of cores/threads on the agent machine's CPU.</dd>

<dt>teamcity.agent.hostname</dt>
<dd>The name of the build agent host.</dd>

</dl>



> * Make sure to replace `.` with `_` when using parameters in MSBuild scripts. For example, use `teamcity_dotnet_nunitlauncher_msbuild_task` instead of `teamcity.dotnet.nunitlauncher.msbuild.task`.
> * The `_x86` and `_x64` parameter suffixes are used to designate the specific version of the framework.
> * The `teamcity.dotnet.nunitlauncher` parameters cannot be hidden or disabled.
>
{type="tip"}


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

<dl>

<dt>teamcity.build.triggeredBy</dt>
<dd>Returns the human-friendly description of how the build was triggered.</dd>

<dt>teamcity.build.triggeredBy.username</dt>
<dd>If the build was triggered by a user, the username of this user is reported. When a build is triggered not by a user, this property is not reported.</dd>


</dl>




 <seealso>
        <category ref="admin-guide">
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
            <a href="using-build-parameters.md">Using Build Parameters</a>
        </category>
</seealso>
