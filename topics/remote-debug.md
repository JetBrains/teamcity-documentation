[//]: # (title: Remote Debug)
[//]: # (auxiliary-id: Remote Debug)

_Remote Debug_ is a feature allowing you to remotely debug your tests on the TeamCity agent machine from the IDE on the local developer machine. This feature is of use when the agent environment is unique in some aspect, which causes a test to fail, and it is difficult to reproduce the problem locally.

<note>

With [TeamCity integration for IntelliJ-based IDEs](intellij-platform-plugin.md) enabled, remote debug sessions can be launched right from IntelliJ IDEA for the builds based on the IntelliJ IDEA Project and Ant build steps.

Currently, the following IntelliJ IDEA run configurations are supported:
* Java application run configuration
* JUnit run configuration
* TestNG run configuration
Remote debug for Ant steps requires that the build configuration have the `teamcity.remote-debug.ant.supported=true` [parameter](configuring-build-parameters.md#Defining+Build+Parameters+in+Build+Configuration).

</note>

### Prerequisites
* IntelliJ IDEA run configuration on the local developer machine with the [TeamCity plugin for IntelliJ IDEA](intellij-platform-plugin.md) installed
* Build configuration on the TeamCity Server with the [IntelliJ IDEA Project](intellij-idea-project.md) runner as one of the [build steps](configuring-build-steps.md)
* Remote [TeamCity agent](build-agent.md) to run this build available to the local machine by socket connection

### Debugging Tests Remotely
1. To start remote debugging of a test, select the test and choose the __Debug &lt;Test Name&gt; Remotely on TeamCity Agent__ option from the context menu (the __Remote Debug__ action is also available from the TeamCity plugin menu. The action will require you to select an IntelliJ IDEA run configuration).
2. Once you do this, the TeamCity plugin will ask you to select a build configuration where you want to start the debug session. The process is similar to starting a personal build. For example, if there are personal changes, a personal patch will be created and sent to an agent. Also, since the process is basically the same, when you select a build configuration, you can specify an agent, customize properties, and so on. ![remote_debug_conf_selected.png](remote_debug_conf_selected.png)
3. If the selected configuration contains more than one IntelliJ IDEA Project build step, the plugin will ask you to choose build steps where to start the debug session. ![remote_debug_step_selected.png](remote_debug_step_selected.png)
4. After that, a build is added to the queue and the standard IntelliJ IDEA debug tool window appears: ![remote_debug_tool_window.png](remote_debug_tool_window.png)The debug tool window works in the listening mode, i.e. it waits for the agent to connect to it. Once the agent connects, the Java process on the agent is paused and the Agent Attached notification appears in the IDE: ![agent_attached_popup.png](agent_attached_popup.png)
5. Now we can set some breakpoints and actually start the debug session by clicking __Start__ either in the notification popup or in the debug tool window.   
Once JVM process exits, another notification popup appears in the IDE: ![repeat_debug_notification.png](repeat_debug_notification.png)The debug session is not finished yet, it is possible to either repeat or finish it. Selecting __Repeat__ will rerun the same build step again, which is much faster than starting a new debug session.

__ __