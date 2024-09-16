[//]: # (title: Start TeamCity Agent)
[//]: # (auxiliary-id: Start TeamCity Agent)

>This page is only relevant for [self-hosted build agents](teamcity-cloud-subscription-and-licensing.md#cloud-self-hosted-agents).
>
{type="note" instance="tcc"}

When the newly installed agent connects to the server for the first time, it appears on the __Agents | Unauthorized agents__ page visible to administrators/users with the permissions to authorize it. Agents will not run builds until they are authorized in the TeamCity UI. The agent running on the same computer as the server is authorized by default.
{instance="tc"}

The number of authorized agents is limited by the number of agent licenses on the server. See more under [Licensing Policy](licensing-policy.md).
{instance="tc"}

TeamCity build agents can be started manually or configured to start automatically.

## Manual Start

Run the following script:
* on Windows: `<installation path>\bin\agent.bat start`
* on Linux and macOS: `<installation path>/bin/agent.sh start`

> The agent uses the `serverUrl` setting from `buildAgent.properties` to connect to the TeamCity server (automatically authorizing on the first connection). If the server URL is not accessible from the agent host, you might need to configure the [proxy settings](configuring-proxy-server.md#Use+Proxy+to+Connect+Agents+to+TeamCity+Server).
>
{type="tip" instance="tc"}

## Automatic Start

### Automatic Agent Start Under Windows

To run an agent automatically on a Windows machine launch, you can either set up the agent to run as a Windows service or use another method. Using the Windows service approach is the easiest way, but Windows applies [some constraints](known-issues.md#Agent+running+as+Windows+Service+Limitations) to the processes run this way.   

A TeamCity agent works reliably under a Windows service provided all the [requirements](system-requirements.md#Common+Requirements) are met, but it is often not the case for the build processes configured to be run on the agent. This is why it is recommended running a TeamCity agent as a Windows service only if all your build scripts support this. Otherwise, it is advised to use alternative OS-specific methods to start a TeamCity agent automatically.   
One of them is to configure an [automatic user logon](https://support.microsoft.com/en-us/help/324737/how-to-turn-on-automatic-logon-in-windows) on Windows start and then configure the TeamCity agent start (via `agent.bat start`) on the user logon (for example, via Windows [Task Scheduler](https://msdn.microsoft.com/en-us/library/windows/desktop/aa383614(v=vs.85).aspx)).

### Build Agent as Windows Service

On Windows, you may want to launch a TeamCity agent as a service to allow running it without any user logged in. If you use the Windows agent installer, you have an option to install the service in the installation wizard.

<warning>

To run builds, the build agent must be started under a user with sufficient permissions for performing a build and managing the service. By default, a Windows service is started under the _SYSTEM_ account which is not recommended for production use due to extended permissions this account has. To change it, use the standard Windows Services applet (__Control Panel | Administrative Tools | Services__) and change the user for the _TeamCity Build Agent_ service.
</warning>

>If you start an Amazon EC2 cloud agent as a Windows service, check the [related notes](setting-up-teamcity-for-amazon-ec2.md).
>
{type="tip" instance="tc"}

The following instructions can be used to install a Windows service manually (for example, after `.zip` agent installation). This procedure should also be performed to create Windows services for the [second and following agents](install-multiple-agents-on-one-machine.md) on the same machine.

Install the service:
1. Check if the service with the required name and ID (see Step 4; the service name is _TeamCity Build Agent_ by default) is not present. If installed, remove it.
2. Check that the `wrapper.java.command` property in the `<agent home>\launcher\conf\wrapper.conf` file contains a valid path to the Java executable in the JDK installation directory. You can use `wrapper.java.command=../jre/bin/java` for the agent installed from the Windows distribution file. Make sure to specify the path of the `java.exe` file without any quotes.
3. If you want to run the agent under a user account (recommended) and not "System", add the `wrapper.ntservice.account` and `wrapper.ntservice.password` properties to the `<agent home>\launcher\conf\wrapper.conf` file with appropriate credentials.
4. (for the second and following agents on the same machine) Modify the `<agent>\launcher\conf\wrapper.conf` file so that the `wrapper.console.title`, `wrapper.ntservice.name`, `wrapper.ntservice.displayname`, and `wrapper.ntservice.description` properties have unique values within the OS.
5. Run the `<agent home>\bin\service.install.bat` script under a user with sufficient privileges to register a new agent service. Make sure to start the agent for the first time only after it is configured as described.

Start the service:
* Run `<agent home>/bin/service.start.bat` (or use the standard Windows Services applet).

Stop the service:
* Run `<agent home>/bin/service.stop.bat` (or use the standard Windows Services applet).

You can also use the standard Windows `net.exe` utility to manage the service once it is installed. For example (assuming the default service name):

```Shell
net start TCBuildAgent

```

The `<agent home>\launcher\conf\wrapper.conf` file can also be used to alter the agent JVM parameters.

Note that the user account used to run the build agent service must have enough rights to start/stop the agent service.

### Automatic Agent Start Under Linux

To run an agent automatically on a Linux machine launch, configure a daemon process with the `agent.sh start` command to start it and the `agent.sh stop` command to stop it.

For `systemd`, see the example `teamcityagent.service` configuration file:

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

For `init.d`, refer to this example procedure:

1. Navigate to the services` scripts directory:  
    ```Shell
    cd /etc/init.d/
    
    ```
2. Open the build agent service script:  
    ```Shell
    sudo vim buildAgent
    
    ```
3. Paste the following content into the file:  
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
4. Set the permissions to execute the file:
    ```Shell
    sudo chmod 755 buildAgent
    
    ```
5. Make links to start the agent service on the machine boot and on restarts using the appropriate tool:
   * For Debian/Ubuntu:  
      ```Shell
      sudo update-rc.d buildAgent defaults
      
      ```
   * For Red Hat/CentOS:  
      ```Shell
      sudo chkconfig buildAgent on
      
      ```

### Automatic Agent Start Under macOS

For macOS, TeamCity provides the ability to load a build agent automatically when a build user logs in.

The recommended approach is to use [`launchd`](https://support.apple.com/guide/terminal/script-management-with-launchd-apdc6c1077b-5d5d-4d35-9c19-60f2397b2369/mac) (LaunchAgent):

To configure an automatic build agent startup via `launchd`, follow these steps:

1. Install a build agent on via `buildAgent.zip`.
2. Prepare the `conf/buildAgent.properties` file (set at least the agent name).
3. Make sure that all files under the `buildAgent` directory are owned by `your_build_user` to ensure a proper agent upgrade process.
4. Load the build agent via the command:
    ```Shell
    mkdir buildAgent/logs  # Directory should be created under your_build_user user
    sh buildAgent/bin/mac.launchd.sh load
    
    ```
5. Run these commands under the `your_build_user` account. Wait up to several minutes for the build agent to autoupgrade from the TeamCity server. You can watch the process in the logs:
    ```Shell
    tail -f buildAgent/logs/teamcity-agent.log
    
    ```
6. When the build agent upgrades and successfully connects to the TeamCity server, stop the agent:  
    ```Shell
    sh buildAgent/bin/mac.launchd.sh unload
    
    ```
7. After the build agent upgrades from the TeamCity server, copy the `buildAgent/bin/jetbrains.teamcity.BuildAgent.plist` file to the `$HOME/Library/LaunchAgents/` directory (you might have to create it). If you do not want TeamCity to start under the root permissions, specify the `UserName` key in the `.plist` file, for example:  
    ```XML
    <key>UserName</key>
    <string>your_build_user</string>
    ```
8. Configure your macOS system to __automatically log in__ as `your_build_user`, as described [here](https://support.apple.com/en-us/HT201476).
9. Restart the machine. On the system startup, the build user should automatically log in, and the build agent should start.  
  To quickly check that the build agent is running, use the following command:  
    ```Shell
    launchctl list | grep BuildAgent 
    69722	0	jetbrains.teamcity.BuildAgent
    
    ```

## Stop Build Agent

To stop the agent manually, run the `<Agent home>\agent` script with the `stop` parameter.

Use `stop` to request stopping after the current build finished. Use `stop force` to request an immediate stop (if a build is running on the agent, it will be stopped abruptly (canceled)).   
Under Linux, you can also use `stop kill` to kill the agent process.

If the agent runs with a console attached, you may also press `Ctrl+C` in the console to stop the agent (if a build is running, it will be canceled).

If a build agent has been started as a `LaunchAgent` service on macOS, it can be stopped using the `launchctl` utility:

```Shell
launchctl unload $HOME/Library/LaunchAgents/jetbrains.teamcity.BuildAgent.plist
# or  
launchctl remove jetbrains.teamcity.BuildAgent

```

