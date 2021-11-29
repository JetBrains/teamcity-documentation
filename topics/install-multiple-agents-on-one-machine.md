[//]: # (title: Install Multiple Agents on One Machine)
[//]: # (auxiliary-id: Install Multiple Agents on One Machine)

If a machine is capable of running several concurrent builds, you can install multiple TeamCity agents on it. However, __we recommend running a single agent per (virtual) machine__ to minimize builds cross-influence and make builds more predictable. 

TeamCity treats all agents equally regardless of whether they are installed on the same or on different machines.

When installing multiple TeamCity build agents on the same machine, consider the following:
* The builds running on such agents should not conflict for any resource (common disk directories, OS processes, OS temp directories).
* Depending on the hardware and the builds' specifics, you may experience degraded building performance. Ensure there are no disk, memory, or CPU bottlenecks when several builds are running at the same time.

After having one agent installed, you can install additional agents by following the regular installation procedure (see an exception for the Windows service below), but make sure that:
* The agents are installed using different OS users so that user-level resources (like Maven/Gradle/NuGet local artifact caches) do not conflict.**
* The agents are installed in separate directories.
* The agents have the distinctive `workDir` and `tempDir` directories in the [`buildAgent.properties`](configure-agent-installation.md) file.
* Values for the `name` and `ownPort` properties of [`buildAgent.properties`](configure-agent-installation.md) are unique.
* No build configurations specify the absolute path to the [checkout directory](build-checkout-directory.md) (or, if necessary, you can enable the "[clean checkout](clean-checkout.md)" option and make sure they do not run in parallel).

Usually, for a new agent installation, you can just copy the directory of an existing agent to a new place except its `temp`, `work`, `logs`, and `system` directories. Then, modify `conf/buildAgent.properties` with the new `name` and `ownPort` values. Clear (delete or remove the value) the `authorizationToken` property and make sure the `workDir` and `tempDir` are relative / do not clash with another agent.

## Configuring Second Build Agent on Windows

If you use the Windows installer and want to run the agent as a service, you will need to perform extra steps as installing second agent as a service on the same machine is not supported by the installer: the existing service is overwritten (see the [related feature request](http://youtrack.jetbrains.net/issue/TW-4962)).

You need to install the second agent [manually](install-teamcity-agent.md#Install+from+ZIP+File) (using the `.zip` agent distribution). You can use the Windows agent installer and do not opt for service installation, but this way you will lose the uninstallation option for the initially installed agent.

After the second agent is installed, register a new service for it as described in [this section](start-teamcity-agent.md#Build+Agent+as+Windows+Service).

>For step-by-step instructions on installing a second Windows agent as a service, see the related [external blog post](https://handcraftsman.wordpress.com/2010/07/20/multiple-teamcity-build-agents-on-one-server/).

## Configuring Second Build Agent on macOS

If you want to start multiple build agents on macOS, repeat the procedure of installing and starting a build agent with the following alterations:
* Install the second build agent in a different directory.
* In `conf/buildAgent.properties`, specify a different agent name.
* Do not run `buildAgent/bin/mac.launchd.sh`; instead
    * Create a copy of the `$HOME/Library/LaunchAgents/jetbrains.teamcity.BuildAgent.plist` file as `$HOME/Library/LaunchAgents/jetbrains.teamcity.BuildAgent2.plist`.
    * Edit the `$HOME/Library/LaunchAgents/jetbrains.teamcity.BuildAgent2.plist` file and set the following parameters:
        * the `Label` parameter to `jetbrains.teamcity.BuildAgent2`
        * the `WorkingDirectory` parameter to the correct path to the second build agent home
    * Start the second agent with the command `launchctl load $HOME/Library/LaunchAgents/jetbrains.teamcity.BuildAgent2.plist`.

To verify that both build agents are running, use the following command:

```Shell
launchctl list | grep BuildAgent 
70599	0	jetbrains.teamcity.BuildAgent2
69722	0	jetbrains.teamcity.BuildAgent

```