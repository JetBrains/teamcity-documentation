[//]: # (title: Setting up and Running Additional Build Agents)
[//]: # (auxiliary-id: Setting up and Running Additional Build Agents)

>This page is about starting up and running [self-hosted build agents](teamcity-cloud-subscription-and-licensing.md#cloud-self-hosted-agents). [JetBrains-hosted build agents](teamcity-cloud-subscription-and-licensing.md#cloud-jb-hosted-agents) are maintained by the TeamCity Cloud team and require no actions from users.
> 
{type="note" product="tcc"}

Before you can start customizing projects and creating build configurations, you need to configure [build agents](build-agent.md). Review the [agent-server communication](#Agent-Server+Data+Transfers) and [Prerequisites](#Prerequisites) sections before proceeding with agent installation. Make sure to also read our [security notes](security-notes.md#Build+Agents) on maintaining build agents.
{product="tc"}

Before you can start customizing projects and creating build configurations, you need to add and configure [build agents](build-agent.md). Review the [agent-server communication](#Agent-Server+Data+Transfers) and [Prerequisites](#Prerequisites) sections before proceeding with agent installation. Make sure to also read our [security notes](security-notes.md#Build+Agents) on maintaining build agents and [licensing policy](teamcity-cloud-subscription-and-licensing.md) on adding new agents.
{product="tcc"}

<tip product="tc">

__Tips__

* If you install TeamCity bundled with a Tomcat servlet container, or use the TeamCity installer for Windows, both the server and one build agent are installed on the same machine. This is not a recommended setup for [production purposes](installing-and-configuring-the-teamcity-server.md#Configuring+Server+for+Production+Use) because of [security concerns](security-notes.md) and since the build procedure can slow down the responsiveness of the web UI and overall TeamCity server functioning. If you need more build agents, perform the procedure described below.
* If you need the agent to run an operating system different from the TeamCity server, perform the procedure described below.
* For production installations, it is recommended to adjust the [Agent's JVM parameters](configuring-build-agent-startup-properties.md) to include the `-server` option.
</tip>

## Prerequisites

### Necessary OS and environment permissions

Before the installation, review the [Conflicting Software](known-issues.md#Conflicting+Software) section. In case of any issues, make sure no conflicting software is used.

Note that to run a TeamCity build agent, the environment and user account used to run the Agent need to comply with the following requirements:

<anchor name="Network"/>

#### Common

The agent process (Java) must:
* be able to open outbound HTTP connections to the server URL configured via the `serverUrl` property in the [`buildAgent.properties`](build-agent-configuration.md) file (typically the same address you use in the browser to view the TeamCity UI). Sending requests to the paths under the configured URL should not be limited. See also the recommended [reverse proxy settings](how-to.md#Set+Up+TeamCity+behind+a+Proxy+Server). Ensure that any firewalls installed on the agent or server machines, network configuration and proxies (if any) comply with these requirements.
{product="tc"}
* be able to open outbound HTTP connections to the server URL configured via the `serverUrl` property in the [`buildAgent.properties`](build-agent-configuration.md) file (typically the same address you use in the browser to view the TeamCity UI). Sending requests to the paths under the configured URL should not be limited. Ensure that any firewalls installed on the agent, network configuration, and proxies (if any) comply with these requirements.
{product="tcc"}
* have full permissions (read/write/delete) to the following directories recursively: [`<agent home>`](agent-home-directory.md) (necessary for automatic agent upgrade and agent tools support), [`<agent work>`](agent-work-directory.md), [`<agent temp>`](agent-home-directory.md#Agent+Directories), and agent system directory (set by `workDir`, `tempDir`, and `systemDir` parameters in the `buildAgent.properties` file).
* be able to launch processes (to run builds).
* be able to launch nested processes with the following parent process exit (this is used during agent upgrade).

<anchor name="Windows"/>

#### Windows
* Log on as a service (to run as Windows service).
* Start/Stop service (to run as Windows service, necessary for the agent upgrade to work, see also [Microsoft KB article](https://support.microsoft.com/en-us/help/325349/how-to-grant-users-rights-to-manage-services-in-windows-server-2003)).
* Debug programs (required for take process dump functionality).
* Reboot the machine (required for agent reboot functionality) .
* To be able to [monitor performance](performance-monitor.md) of a build agent run as a Windows [service](#Build+Agent+as+a+Windows+Service), the user starting the agent must be a member of the Performance Monitor Users group.

<note>


For granting necessary permissions for unprivileged users, see [Microsoft documentation](https://support.microsoft.com/en-us/kb/325349).   
You can assign rights to manage services with Microsoft [SubInACL](http://www.microsoft.com/downloads/details.aspx?FamilyID=e8ba3e56-d8fe-4a91-93cf-ed6985e3927b&displaylang=en) utility, a command-line tool enabling administrators to directly edit security information. The tool uses the following syntax:

```Shell
SUBINACL /SERVICE \\MachineName\ServiceName /GRANT=[DomainName]UserName[=Access]

```

For example, to grant Start/Stop rights, you might need to execute the command:

```Shell
subinacl.exe /service TCBuildAgent /grant=<user login name>=PTO

```

</note>

#### Linux

* The user must be able to run the `shutdown` command (for the agent machine reboot functionality and the machine shutdown functionality when running in a cloud environment).
* If you are using `systemd`, it should not kill the processes on the main process exit (use [`RemainAfterExit=yes`](https://serverfault.com/questions/660063/teamcity-build-agent-gets-killed-by-systemd-when-upgrading)). See also [how to set up automatic agent start under Linux](#Automatic+Agent+Start+under+Linux).

#### Build-related Permissions

The build process is launched by a TeamCity agent and thus shares the environment and is executed under the OS user used by the TeamCity agent. Ensure that the TeamCity agent is configured accordingly. See [Known Issues](known-issues.md) for related Windows Service Limitations.

<anchor name="SettingupandRunningAdditionalBuildAgents-ServerDataTransfers"/>
<anchor name="SettingupandRunningAdditionalBuildAgents-Agent-ServerDataTransfers"/>

### Agent-Server Data Transfers

[//]: # (AltHead: Server-Agent Data Transfers)

A TeamCity agent connects to the TeamCity server via the URL configured as the `serverUrl` agent property. This is called [unidirectional](#Unidirectional+Agent-to-Server+Communication) agent-to-server connection.

If specifically configured, TeamCity agent can use legacy [bidirectional communication](#Bidirectional+Communication) which also requires establishing a connection from the server to the agents. To view whether the agent-server communication is unidirectional or bidirectional for a particular agent, navigate to __Agents | &lt;Agent Name&gt; | Agent Summary__ tab, the __Details__ section, __Communication Protocol__.
{product="tc"}

#### Unidirectional Agent-to-Server Communication

Agents use unidirectional agent-to-server connection via the polling protocol: the agent establishes an HTTP(S) connection to the TeamCity Server, and polls the server periodically for server commands.

It is recommended to use __HTTPS__ for agent-to-server communications (check related [server configuration notes](how-to.md#Configure+HTTPS+for+TeamCity+Web+UI)). If the agents and the server are deployed into a secure environment, agents can be configured to use plain HTTP URL for connections to the server as this reduces transfer overhead. Note that the data travelling through the connection established from an agent to the server includes build settings, repository access credentials and keys, repository sources, build artifacts, build progress messages and build log. In case of using the HTTP protocol that data can be compromised via the "man in the middle" attack.
{product="tc"}

#### Bidirectional Communication
{product="tc"}

The bidirectional communication is a legacy connection between the agent and the server and it needs to be specifically enabled (see the example below). When enabled, it requires the agent to connect to the server via HTTP (or HTTPS) and the server to connect to the agent via HTTP.

The data that is transferred via the connections established by the server to agents is passed via an unsecured HTTP channel and thus is potentially exposed to any third party that may listen to the traffic between the server and the agents. Moreover, since the agent and server can send "commands" to each other, an attacker that can send HTTP requests and capture responses may in theory trick the agent into executing an arbitrary command and perform other actions with a security impact.

The communication protocol used by TeamCity agents is determined by the value of the `teamcity.agent.communicationProtocols` server [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties). The property accepts a comma-separated ordered list of protocols (`polling`  and `xml-rpc` are supported protocol names) and is set to `teamcity.agent.communicationProtocols=polling` by default. If several protocols are specified, the agent tries to connect using the first protocol from this list and if it fails, it will try to connect via the second protocol in the list. `polling` means unidirectional protocol, `xml-rpc` - older, bidirectional communication.

#### Changing Communication Protocol
{product="tc"}

* To change the communication protocol __for all agents__, set the TeamCity server `teamcity.agent.communicationProtocols` server [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties). The new setting will be used by all agents which will connect to the server after the change. To change the protocol for the existing connections, restart the TeamCity server.
* By default, the agent's property is not configured; when the agent first connects to the server, it receives it from the TeamCity server. To change the protocol __for an individual agent__ after the initial agent configuration, change the value of the `teamcity.agent.communicationProtocols` property in the [agent's properties](build-agent-configuration.md). The agent's property overrides the server property. After the change the agent will restart automatically upon finishing a running build, if any.

[//]: # (Internal note. Do not delete. "Setting up and Running Additional Build Agentsd283e376.txt")

## Generating Authentication Token
{product="tcc"}

The recommended approach to connecting a self-hosted agent to a TeamCity Cloud instance is to generate a unique authentication token for this agent. To do this, go to __Agents__, open the __Install Build Agents__ menu in the upper right corner of the screen, and click _Use authentication token_. There are two options:

* _Generate plain-text token_: you need to copy the generated token and enter it in the [build agent configuration](build-agent-configuration.md) file. On Windows, you will be prompted to enter it right in the _Configure Build Agent Properties_ installation dialog.
* _Download config_: enter an agent name (`name` attribute in the [build agent config](build-agent-configuration.md)) and download the entire config file. Place it as the `buildAgent.properties` file in the build agent directory.

Please generate own token or configuration file per each self-hosted agent.

<anchor name="SettingupandRunningAdditionalBuildAgents-InstallingAdditionalBuildAgents"/>

## Installing Additional Build Agents

1. Install a build agent using any of the following options:
  * [using Windows installer](#Installing+via+Windows+installer) (Windows only)
  * [by downloading a ZIP file and installing manually](#Installing+via+ZIP+File) (any platform): minimal and full-packed ZIP archives ara available
  * via the [Docker Agent Image](#Installing+via+Docker+Agent+Image) option to prepare a Docker container based on the official [TeamCity Agent image](https://hub.docker.com/r/jetbrains/teamcity-agent/)
2. After installation, configure the agent specifying its name and the address of the TeamCity server in the [`conf/buildAgent.properties`](build-agent-configuration.md) file.
{product="tc"}
2. After installation, configure the agent specifying its name, the address of the TeamCity server, and the [authentication token](#Generating+Authentication+Token) in the [`conf/buildAgent.properties`](build-agent-configuration.md) file.
{product="tcc"}
3. [Start](#Starting+the+Build+Agent) the agent. If the agent does not seem to run correctly, check the [agent logs](viewing-build-agent-logs.md).

When the newly installed agent connects to the server for the first time, it appears on the `Agents` page, `Unauthorized agents` tab visible to administrators/users with the permissions to authorize it. Agents will not run builds until they are authorized in the TeamCity web UI. The agent running on the same computer as the server is authorized by default.
{product="tc"}

The number of authorized agents is limited by the number of agents licenses on the server. See more under [Licensing Policy](licensing-policy.md).
{product="tc"}

### Installing via Windows installer
1. In the TeamCity web UI, navigate to the __Agents__ tab.
2. Click the __Install Build Agents__ link and select __Windows Installer__ to download the installer.
3. On the agent, run the `agentInstaller.exe` Windows Installer and follow the installation instructions.

<note>

Ensure that the user account used to run the agent service has appropriate [permissions](#Necessary+OS+and+environment+permissions)
</note>

### Installing via Docker Agent Image
1. In the TeamCity UI, navigate to the __Agents__ tab.
2. Click the __Install Build Agents__ link and select _Docker Agent Image_.
3. Follow the instructions on the opened [page](https://hub.docker.com/r/jetbrains/teamcity-agent/).

### Installing via ZIP File
1. Make sure a JDK (JRE) 1.8.0_161 or later (Java 8-11 are supported, but 1.8.0_161+ is recommended) is properly installed on the agent computer.
2. On the agent computer, make sure the `JRE_HOME` or `JAVA_HOME` environment variables are set (pointing to the installed JRE or JDK directory respectively).
3. In the TeamCity web UI, navigate to the __Agents__ tab.
4. Click the __Install Build Agents__ link and select one of the two options to download the archive:
   * __Minimal ZIP file distribution__: a regular build agent with no plugins
   * (since TeamCity 2020.1) __Full ZIP file distribution*__: a full build agent prepacked with all plugins currently enabled on the server
5. Extract the downloaded file into the desired directory.
6. Navigate to the `<installation path>\conf` directory, locate the file called `buildAgent.dist.properties` and rename it to `buildAgent.properties`.
7. Edit the `buildAgent.properties` file to specify the TeamCity server URL (HTTPS is recommended, see the [notes](#Agent-Server+Data+Transfers)), the name of the agent, and the [authentication token](#Generating+Authentication+Token). Refer to the [Build Agent Configuration](build-agent-configuration.md) page for details on agent configuration.
{product="tcc"}
7. Edit the `buildAgent.properties` file to specify the TeamCity server URL and the name of the agent. Refer to the [Build Agent Configuration](build-agent-configuration.md) page for details on agent configuration.
{product="tc"}
8. Under Linux, you may need to give execution permissions to the `bin/agent.sh` shell script.

On Windows, you may also want to install the [build agent Windows service](#Build+Agent+as+a+Windows+Service) instead of using the manual agent startup.

<tip>

__\*__ A __minimal TeamCity agent__ distribution does not contain plugins: the agent downloads them on the first start. The __full agent__ contains all enabled plugins and automatically stays relevant with the current TeamCity server state. This makes its distribution archive larger but significantly reduces the time spent on the first agent run.

The full agent is the most convenient if you use scripts for creating agent images (for example, [in cloud](agent-cloud-profile.md)). All instances will be synchronized with the server from the start and can instantly run a build.
{product="tc"}

The full agent is the most convenient if you use scripts for creating agent images. All instances will be synchronized with the server from the start and can instantly run a build.
{product="tcc"}

Note that after starting, the full agent behaves like a regular agent. If you modify the state of plugins on the TeamCity server, all active agents will need to restart to sync with the server.

</tip>

### Installing via Agent Push
{product="tc"}

TeamCity provides the Agent Push functionality that allows installing a build agent to a remote host. Currently, supported combinations of the server host platform and targets for build agents are:
* from the Unix-based TeamCity server, build agents can be installed to Unix hosts only (via SSH)
* from the Windows-based TeamCity server, build agents can be installed to Unix (via SSH) or Windows (via psexec) hosts

<note>

__SSH note__

Make sure the "Password" or "Public key" authentication is enabled on the target host according to a preferred authentication method.
</note>

### Installing via Agent Push
{product="tcc"}

TeamCity provides the Agent Push functionality that allows installing a build agent to a remote Unix host.

<note>

__SSH note__

Make sure the "Password" or "Public key" authentication is enabled on the target host according to a preferred authentication method.
</note>

#### Remote Host Requirements

There are several requirements for the remote host:

<table><tr>

<td width="150">

Platform


</td>

<td>

Prerequisites


</td></tr><tr>

<td>

Linux


</td>

<td>

* Installed JDK (JRE) 8-11 (__1.8.0_161 or later is recommended__). The JVM should be reachable via the `JAVA_HOME` (`JRE_HOME`) global environment variable or be in the global path (instead of being specified in, for example, user's `.bashrc` file)

* The `unzip` utility.

* Either `wget` or `curl`.


</td></tr><tr>

<td>

Windows


</td>

<td>

* Installed JDK/JRE 8-11 (__1.8.0_161 or later is recommended__).

[//]: # (Internal note. Do not delete. "Setting up and Running Additional Build Agentsd283e644.txt")

* `Sysinternals psexec.exe` has to be installed on the TeamCity server and accessible in paths. You can install it on the __Administration__ | __Tools__ page.   
Note that [PsExec](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec) applies additional requirements to a remote Windows host. Make sure the following preconditions are satisfied:
  * [Administrative share](https://en.wikipedia.org/wiki/Administrative_share) on a remote host is enabled and accessible.
  * Remote services work (MMC snap-in can connect to the machine).
  * Remote registry works (`regedit` can connect to the machine via `services.msc`).
  * Server and workstation services are running (check via `services.msc`).
  * [Classic network authentication](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/network-access-sharing-and-security-model-for-local-accounts) is enabled.   
 
 You can test the connection with the following commands:   
  ```Console
    net use \\target\Admin$ /user:Administrator 
    dir \\target\Admin$ 
   ```


</td></tr></table>

#### Installation
1. In the TeamCity Server web UI navigate to the __Agents__ | __Agent Push__ tab, and click __Install Agent__.   
If you are going to use same settings for several target hosts, you can __create a preset__ with these settings and use it each time when installing an agent to another remote host.
2. In the _Install agent_ dialog, either select a saved preset or choose "_Use custom settings_", specify the target host platform, and configure corresponding settings. Agent Push to a Linux system via SSH supports custom ports (the default is 22) specified as the __SSH port__ parameter. The port specified in a preset can be overridden in the host name (for example, `hostname.domain:2222`), during the actual agent installation.
3. You may need to download `Sysinternals psexec.exe`, in which case you will see the corresponding warning and a link to the __Administration__ | __Tools__ page where you can download it.

<tip product="tc">

You can use Agent Push presets in [Agent Cloud profile](agent-cloud-profile.md) settings to automatically install a build agent to a started cloud instance.

</tip>

## Starting the Build Agent

TeamCity build agents can be started manually or configured to start automatically.

### Manual Start

__To start the agent manually__, run the following script:
* for Windows: `<installation path>\bin\agent.bat start`
* for Linux and macOS: `<installation path>\bin\agent.sh start`

### Automatic Start

To configure the agent to be __started automatically__, see the corresponding sections:
* [Windows](#Automatic+Agent+Start+under+Windows)
* [Linux](#Automatic+Agent+Start+under+Linux): configure a daemon process with the `agent.sh start` command to start it and the `agent.sh stop` command to stop it.
* [macOS](#Automatic+Agent+Start+under+macOS)

#### Automatic Agent Start under Windows

To run an agent automatically on the machine boot under Windows, you can either set up the agent to be run as a Windows service or use another way of the automatic process start.   
Using the Windows service approach is the easiest way, but Windows applies [some constraints](known-issues.md#Agent+running+as+Windows+Service+Limitations) to the processes run this way.   
A TeamCity agent works reliably under Windows service provided all the [requirements](#Necessary+OS+and+environment+permissions) are met, but is often not the case for the build processes configured to be run on the agent.

That is why it is recommended to run a TeamCity agent as a Windows service only if all the build scripts support this. Otherwise, it is advised to use alternative OS-specific ways to start a TeamCity agent automatically.   
One of the ways is to configure an [automatic user logon](https://support.microsoft.com/en-us/help/324737/how-to-turn-on-automatic-logon-in-windows) on Windows start and then configure the TeamCity agent start (via `agent.bat start`) on the user logon (for example, via Windows [Task Scheduler](https://msdn.microsoft.com/en-us/library/windows/desktop/aa383614(v=vs.85).aspx)).

#### Build Agent as a Windows Service

In Windows, you may want to run TeamCity agent as a Windows service to allow it running without any user logged on. If you use the Windows agent installer, you have an option to install the service in the installation wizard.

<warning>

To run builds, the build agent must be started under a user with sufficient permissions for performing a build and [managing](#Windows) the service. By default, a Windows service is started under the SYSTEM account which is not recommended for production use due to extended permissions the account uses. To change it, use the standard Windows Services applet (__Control Panel | Administrative Tools | Services__) and change the user for the _TeamCity Build Agent_ service.
</warning>

>If you start an Amazon EC2 cloud agent as a Windows service, check the [related notes](setting-up-teamcity-for-amazon-ec2.md#Preparing+Image+with+Installed+TeamCity+Agent).
> 
{type="tip" product="tc"}

The following instructions can be used to install the Windows service manually (for example, after `.zip` agent installation). This procedure should also be performed to create Windows services for the [second and following agents](#Installing+Several+Build+Agents+on+the+Same+Machine) on the same machine.

__To install the service:__
1. Check if the service with the required name and id (see #4 below, service name is `TeamCity Build Agent` by default) is not present; if installed, remove it.
2. Check that the `wrapper.java.command` property in the `<agent home>\launcher\conf\wrapper.conf` file contains a valid path to the Java executable in the JDK installation directory. You can use `wrapper.java.command=../jre/bin/java` for the agent installed with the Windows distribution. Make sure to specify the path of the java.exe file without any quotes.
3. If you want to run the agent under user account (recommended) and not "System", add the `wrapper.ntservice.account` and `wrapper.ntservice.password` properties to the `<agent home>\launcher\conf\wrapper.conf` file with appropriate credentials
4. (for second and following installations) Modify the `<agent>\launcher\conf\wrapper.conf` file so that the `wrapper.console.title`, `wrapper.ntservice.name`, `wrapper.ntservice.displayname`, and `wrapper.ntservice.description` properties have unique values within the computer.
5. Run the `<agent home>\bin\service.install.bat` script under a user with sufficient privileges to register the new agent service. Make sure to start the agent for the first time only after it is configured as described.

__To start the service:__
* Run `<agent home>/bin/service.start.bat` (or use standard Windows Services applet)

__To stop the service:__
* Run `<agent home>/bin/service.stop.bat` (or use standard Windows Services applet)   

You can also use the standard Windows `net.exe` utility to manage the service once it is installed.   
For example (assuming the default service name):


```Shell
net start TCBuildAgent

```


The `<agent home>\launcher\conf\wrapper.conf` file can also be used to alter the agent JVM parameters.

The user account used to run the build agent service must have enough rights to start/stop the agent service, as described [above](#Windows).

#### Automatic Agent Start under Linux

To run an agent automatically on the machine boot under Linux, configure a daemon process with the `agent.sh start` command to start it and the `agent.sh stop` command to stop it.

For systemd, see the example `teamcityagent.service` configuration file:

```Shell
[Unit]
Description=TeamCity Build Agent
After=network.target

[Service]
Type=oneshot

User=teamcityagent
Group=teamcityagent
ExecStart=/home/teamcityagent/agent/bin/agent.sh start
ExecStop=-/home/teamcityagent/agent/bin/agent.sh stop

# Support agent upgrade as the main process starts a child and exits then
RemainAfterExit=yes
# Support agent upgrade as the main process gets SIGTERM during upgrade and that maps to exit code 143
SuccessExitStatus=0 143

[Install]
WantedBy=default.target

```
 

For `init.d`, refer to an example procedure:

1\. Navigate to the services start/stop services scripts directory:

```Shell
cd /etc/init.d/

```

2\. Open the build agent service script:


```Shell
sudo vim buildAgent

```

3\. Paste the following into the file :


```bash
#!/bin/sh
### BEGIN INIT INFO
# Provides:          TeamCity Build Agent
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start build agent daemon at boot time
# Description:       Enable service provided by daemon.
### END INIT INFO
#Provide the correct user name:
USER="agentuser"
 
case "$1" in
start)
 su - $USER -c "cd BuildAgent/bin ; ./agent.sh start"
;;
stop)
 su - $USER -c "cd BuildAgent/bin ; ./agent.sh stop"
;;
*)
  echo "usage start/stop"
  exit 1
 ;;
 
esac
 
exit 0

```


4\. Set the permissions to execute the file:


```Shell
sudo chmod 755 buildAgent

```


5\. Make links to start the agent service on the machine boot and on restarts using the appropriate tool:

* For Debian/Ubuntu:

```Shell
sudo update-rc.d buildAgent defaults

```


* For Red Hat/CentOS:

```Shell
sudo chkconfig buildAgent on

```

#### Automatic Agent Start under macOS

For macOS, TeamCity provides the ability to load a build agent automatically when a build user logs in.

The `LaunchAgent` approach:

To configure an automatic build agent startup via `LaunchAgent`, follow these steps:

1\. Install a build agent on a Mac via `buildAgent.zip`.

2\. Prepare the `conf/buildAgent.properties` file (set agent name there, at least).

3\. Make sure that all files under the `buildAgent` directory are owned by `your_build_user` to ensure a proper agent upgrade process.

4\. Load the build agent via the command:


```Shell
mkdir buildAgent/logs  # Directory should be created under your_build_user user
sh buildAgent/bin/mac.launchd.sh load

```

Run these commands under the `your_build_user` account.

__Wait several minutes__ for the build agent to auto-upgrade from the TeamCity server. You can watch the process in the logs:


```Shell
tail -f buildAgent/logs/teamcity-agent.log

```

5\. When the build agent upgrades and successfully connects to TeamCity server, stop the agent:


```Shell
sh buildAgent/bin/mac.launchd.sh unload

```

6\. After the build agent upgrades from the TeamCity server, copy the `buildAgent/bin/jetbrains.teamcity.BuildAgent.plist` file to the `$HOME/Library/LaunchAgents/` directory. If you don't want TeamCity to start under the root permissions, specify the __UserName__ key in the `.plist` file, for example:
```XML
<key>UserName</key>
<string>teamcity_user</string>
```

7\. Configure your Mac system to __automatically login__ as a build user, as described [here](https://support.apple.com/en-us/HT201476).

8\. Reboot.

On the system startup, the build user should automatically log in, and the build agent should start.

To quickly check that the build agent is running, use the following command:

```Shell
launchctl list | grep BuildAgent 
69722	0	jetbrains.teamcity.BuildAgent

```

## Stopping the Build Agent

__To stop the agent manually__, run the `<Agent home>\agent` script with a `stop` parameter.

Use `stop` to request stopping after the current build finished.   
Use `stop force` to request an immediate stop (if a build is running on the agent, it will be stopped abruptly (canceled)).   
Under Linux, you have one more option top use `stop kill` to kill the agent process.

If the agent runs with a console attached, you may also press __Ctrl\+C__ in the console to stop the agent (if a build is running, it will be canceled).

### Stopping the Agent Service on macOS

If a build agent has been started as a `LaunchAgent` service, it can be stopped using the `launchctl` utility:

```Shell
launchctl unload $HOME/Library/LaunchAgents/jetbrains.teamcity.BuildAgent.plist
# or  
launchctl remove jetbrains.teamcity.BuildAgent

```
    

## Configuring Java

A TeamCity build agent is a Java application ([supported Java versions](supported-platforms-and-environments.md#Build+Agents)).

A build agent contains two processes:
* Agent Launcher — a Java process that launches the agent process
* Agent — the main process for a Build Agent; runs as a child process for the agent launcher

The (Windows) `.exe` TeamCity distribution comes bundled with 64-bit Amazon Corretto 8.   
If you run a previous version of the TeamCity agent, you will need to repeat the agent installation to update the JVM.

Using 64-bit JDK (not JRE) is recommended. JDK is required for some build runners like [IntelliJ IDEA Project](intellij-idea-project.md), Java [Inspections](inspections.md), and [Duplicates](duplicates-finder-java.md). If you do not have Java builds, you can install JRE instead of JDK.   
Using of x64 bit Java is possible, but you might need to double the `-Xmx` memory value for the main agent process (see [Configuring Build Agent Startup Properties](configuring-build-agent-startup-properties.md) and alike [section](installing-and-configuring-the-teamcity-server.md#Setting+Up+Memory+settings+for+TeamCity+Server) for the server).
{product="tc"}

Using 64-bit JDK (not JRE) is recommended. JDK is required for some build runners like [IntelliJ IDEA Project](intellij-idea-project.md), Java [Inspections](inspections.md), and [Duplicates](duplicates-finder-java.md). If you do not have Java builds, you can install JRE instead of JDK.   
Using of x64 bit Java is possible, but you might need to double the `-Xmx` memory value for the main agent process (see [Configuring Build Agent Startup Properties](configuring-build-agent-startup-properties.md)).
{product="tcc"}

<anchor name="java-paths"/>

For the `.zip` agent installation you need to install the appropriate Java version. Make it available via `PATH` or available in one of the following places:
* the `<Agent home>/jre` directory
* in the directory pointed to by the `TEAMCITY_JRE`, `JAVA_HOME` or `JRE_HOME` environment variables (check that you only have one of the variables defined).
* if you plan to run the agent via Windows service, make sure to set the `wrapper.java.command` property in the `<agent home>\launcher\conf\wrapper.conf` file to a valid path to the Java executable

<anchor name="SettingupandRunningAdditionalBuildAgents-UpgradingJavaonAgents"/>

### Upgrading Java on Agents

If you are trying to launch an agent, and it is not able to find the required Java version (currently Java 8) in any of the [default locations](setting-up-and-running-additional-build-agents.md#java-paths), the agent will report an error on starting, the process will not launch, and the agent will be shown as disconnected in the TeamCity UI.

If a build agent uses a Java version older than Java 8, you will see the corresponding warning on the agent's page and a [health item](server-health.md) in the web UI.

<note>

__Support for Java prior to version 8 on agents has been dropped in TeamCity 2019.2__. Consider upgrading Java on the agent if you see the warning.   
An agent machine can have multiple Java versions installed, and the agent can use one Java version while the build tasks use other Java versions.

Please let us know using any of our [support channels](feedback.md) if your setup depends on the older version of Java and if you will not be able to upgrade to version 8 for some reason.
</note>

It is recommended to use latest Java 8, 64-bit version.
OpenJDK 8 (for example, bundled [Amazon Corretto](https://aws.amazon.com/corretto/)) 1.8.0_161 or later. [Oracle Java 8](http://www.oracle.com/technetwork/java/javase/downloads/) is also supported.

To update Java on agents, do one of the following:
* If the agent details page in the TeamCity UI displays a Java version note with the corresponding action, you can switch to using newer Java: if the appropriate Java version of the same bitness as the current one is detected on the agent, the agent page provides an action to switch to using that Java automatically. Upon the action invocation, the agent process is restarted (once the agent becomes idle, i.e. finishes the current build if there is one) using the new Java.
* (Windows) Since the build agent Windows installer comes bundled with the required Java, you can just manually reinstall the agent using the Windows installer (`.exe`) obtained from the TeamCity server __Agents__ page. See [installation instructions](setting-up-and-running-additional-build-agents.md#Installing+via+Windows+installer). It is important to uninstall the previous version of the agent before installing the updated agent: invoke `Uninstall.exe` in the agent home directory, clear all the "_Remove_" checkboxes, and click __Uninstall__.
* Install a required Java on the agent into one of the standard locations, and restart the agent — the agent should then detect it and provide an action to use a newer Java in the web UI (see above).
* Install a required Java on the agent and [configure the agent](#Configuring+Java) to use it.
 

<note>

In a rare case of updating the Java for the process that launches the TeamCity agent, use one of the options for the agent Java upgrade. Another way for Build Agent started as a __Windows service__, is to stop the service, change the `wrapper.java.command` variable in `buildAgent\launcher\conf\wrapper.conf` to point to the new `java.exe` binary, and restart the service.
</note>

 

## Installing Several Build Agents on the Same Machine

You can install several TeamCity agents on the same machine if the machine is capable of running several builds at the same time. However, __we recommend running a single agent per (virtual) machine__ to minimize builds cross-influence and making builds more predictable. When installing several agents, it is recommended to install them under different OS users so that user-level resources (like Maven/Gradle/NuGet local artifact caches) do not conflict.

TeamCity treats all agents equally regardless of whether they are installed on the same or on different machines.

When installing several TeamCity build agents on the same machine, consider the following:
* The builds running on such agents should not conflict by any resource (common disk directories, OS processes, OS temp directories).
* Depending on the hardware and the builds' specifics, you may experience degraded building performance. Ensure there are no disk, memory, or CPU bottlenecks when several builds are run at the same time.
* Preferably, agents should run under different OS users.

After having one agent installed, you can install additional agents by following the regular installation procedure (see an exception for the Windows service below), but make sure that:
* The agents are installed in separate directories.
* The agents have the distinctive `workDir` and `tempDir` directories in the [`buildAgent.properties`](build-agent-configuration.md) file.
* Values for the `name` and `ownPort` properties of [`buildAgent.properties`](build-agent-configuration.md) are unique.
* No build configurations specify the absolute path to the [checkout directory](build-checkout-directory.md) (or, if necessary, you can enable the "[clean checkout](clean-checkout.md)" option and make sure they do not run in parallel).

Usually, for a new agent installation you can just copy the directory of the existing agent to a new place except its `temp`, `work`, `logs`, and `system` directories. Then, modify `conf/buildAgent.properties` with the new `name` and `ownPort` values. Clear (delete or remove the value) the `authorizationToken` property and make sure the `workDir` and `tempDir` are relative/do not clash with another agent.

### Configuring second build agent on macOS

If you want to start several build agents on macOS, repeat the procedure of installing and starting build agent with the following changes:
* Install the second build agent in a different directory.
* In `conf/buildAgent.properties`, specify a different agent name.
* Do not run `buildAgent/bin/mac.launchd.sh`; instead
  * Create a copy of the `$HOME/Library/LaunchAgents/jetbrains.teamcity.BuildAgent.plist` file as `$HOME/Library/LaunchAgents/jetbrains.teamcity.BuildAgent2.plist`.
  * Edit the `$HOME/Library/LaunchAgents/jetbrains.teamcity.BuildAgent2.plist` file and set the following parameters:
    * the `Label` parameter to `jetbrains.teamcity.BuildAgent2`
    * the `WorkingDirectory` parameter to the correct path to the second build agent home
  * Start the second agent with the command `launchctl load $HOME/Library/LaunchAgents/jetbrains.teamcity.BuildAgent2.plist`.

To check that both build agents are running, use the following command:

```Shell
launchctl list | grep BuildAgent 
70599	0	jetbrains.teamcity.BuildAgent2
69722	0	jetbrains.teamcity.BuildAgent

```

### Configuring second build agent on Windows

If you use Windows installer to install additional agents and want to run the agent as a service, you will need to perform manual steps as installing second agent as a service on the same machine is not supported by the installer: the existing service is overwritten (see also a [feature request](http://youtrack.jetbrains.net/issue/TW-4962)).

In order to install the second agent, it is recommended to install the second agent [manually](#Installing+via+ZIP+File) (using the `.zip` agent distribution). You can use the Windows agent installer and do not opt for service installation, but you will lose uninstall option for the initially installed agent this way.

After the second agent is installed, register a new service for it as mentioned in the [section above](#Build+Agent+as+a+Windows+Service).

>For step-by-step instructions on installing a second Windows agent as a service, see the related [external blog post](https://handcraftsman.wordpress.com/2010/07/20/multiple-teamcity-build-agents-on-one-server/).

<seealso>
        <category ref="concepts">
            <a href="build-agent.md">Build Agent</a>
        </category>
</seealso>