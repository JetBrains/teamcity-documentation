# Cheatsheet: Updating Java on Server and Agent Machines

This article briefly outlines the Java update procedure for agent and server machines. Refer to these articles for more detailed information.

* [](how-to.md#Install+Non-Bundled+Version+of+Java)
* [](configure-java-for-agent.md)
* [](upgrading-teamcity-server-and-agents.md)


## Migration to Java 21

Starting with version 2026.1, both the TeamCity server and agents will require Java 21 to start. Some features (for example, [](ai-assistant.md)) are already unavailable in TeamCity 2025.11 when running on older Java versions. For this reason, we recommend upgrading to Java 21 ahead of the 2026.1 release.

> We plan to support Java 21~25 in version 2026.1. However, version 2025.11 currently supports only Java 21.
> 
{style="note"}

## Upgrade Instructions

The upgrade process boils down to two essential steps:

1. Install Java 21 on the machine.
2. Ensure TeamCity detects and uses the new installation.

The exact procedure depends on your operating system.

### Update Server

<tabs>

<tab title="Windows">

<procedure>

The TeamCity Windows installer and server Docker images include [Amazon Corretto](https://aws.amazon.com/corretto/) 64-bit Java 21, so you do not need to install it manually. Simply run the TeamCity 2025.11 installer and it will provide the required JDK.

</procedure>


</tab>


<tab title="Linux and macOS">

<procedure>

TeamCity server `.tar.gz` archives do not include Java, so you need to install it manually. Please note that the JDK you install matches your platform. For example, [Amazon Corretto 21](https://docs.aws.amazon.com/corretto/latest/corretto-21-ug/downloads-list.html) offers different options for Linux and macOS running on both ARM64 and x86_64 architectures.

Once you install Java 21, assign the corresponding installation path to the `JAVA_HOME` or `TEAMCITY_JRE` environment variable. See [this StackOverflow thread](https://stackoverflow.com/questions/21964709/how-to-set-or-change-the-default-java-jdk-version-on-macos) for the detailed instructions.

* The `JAVA_HOME` is a global variable that specifies the default JDK on your machine. When set, the `java -version` terminal command should point to the corresponding version.
* The `TEAMCITY_JRE` variable is used only by TeamCity and allows you to keep using a different Java version as a default one for other applications.


</procedure>

</tab>

</tabs>

### Update Agents

<tabs>

<tab title="Windows">

<procedure>

Before upgrading an agent machine, we recommend uninstalling the existing agent to avoid potential issues:

1. Go to the [agent home directory](agent-home-directory.md), run `Uninstall.exe`, leave all "Remove ..." checkboxes unchecked, and complete the uninstall.
2. Open the TeamCity UI in your browser and log in.
3. In the side navigation bar, open **Agents**.
4. Click **Install agent** and download the .exe agent installer bundled with a JDK.
5. Run this installer on every agent machine that needs an update.

> To verify which Java version an agent is using, open the [agent summary](viewing-build-agent-details.md), switch to the **Parameters** tab, and check the following parameter values:
> * `teamcity.agent.jvm.java.home`
> * `teamcity.agent.jvm.version`
> * `teamcity.agent.jvm.vendor`
>
{style="tip"}

If a build agent runs as a service, make sure the `wrapper.java.command` property in the `<agent_home>/launcher/conf/wrapper.conf` file points to the required Java version. See the following article for the details: [](upgrading-teamcity-server-and-agents.md#Upgrading+the+Build+Agent+Windows+Service+Wrapper).

</procedure>


</tab>


<tab title="Linux and macOS">

<procedure>


To update agent machines, follow the same procedure as you do for the server. Alternatively, you can install an [agent archive that already bundles the required JDK](install-teamcity-agent.md#Available+Agent+Distributions). To do so:

1. Navigate to **Admin | Agent JDKs** in TeamCity UI.
2. Click **Add JDK** to upload a required Java version.
3. Once TeamCity downloads the target JDK, a corresponding option will appear under **Agents | Install agent | Agent Distributions with JDK**. Full agent installations include the `/jre` directory. When launched, the agent prioritizes Java from this directory over versions returned by `JAVA_HOME` and `TEAMCITY_JRE` environment variables.

> To check which Java the specific agent is using, open the [agent summary](viewing-build-agent-details.md), switch to the **Parameters** tab, and check the following parameter values:
> * `teamcity.agent.jvm.java.home`
> * `teamcity.agent.jvm.version`
> * `teamcity.agent.jvm.vendor`
>
{style="tip"}

</procedure>

</tab>

</tabs>

