# Java 21 Upgrade Procedures

Starting with version 2026.1, neither TeamCity server nor agents will be able to start without Java 21 installed on the machine. If TeamCity 2025.11 runs on an older Java version, it shows a corresponding warning asking you to upgrade Java.

> We plan to support Java 21~25 in version 2026.1. However, version 2025.11 currently supports only Java 21.
> 
{style="note"}


## Upgrade Instructions

The upgrade process boils down to two essential steps:

1. Install Java 21 on the machine.
2. Make sure TeamCity correctly locates your newly installed version.

The specifics vary depending on the machine OS.

<tabs>

<tab title="Windows">

<procedure>

The TeamCity server Windows installer and server Docker images come __bundled with [Amazon Corretto](https://aws.amazon.com/corretto/) 64-bit Java 21__, so you do not need to install Java 21 manually.

Before upgrading an agent machine, we recommend uninstalling the current agent version to avoid potential issues:

1. Uninstall the current agent version. To do this, navigate to the [agent home directory](agent-home-directory.md), invoke `Uninstall.exe`, clear all the "Remove ..." checkboxes, and click **Uninstall**.
2. Open TeamCity UI in a browser and log in.
3. Use the side navigation bar to switch to the **Agents** page.
4. Click **Install agent** and download the .exe agent installer with a bundled JDK.
5. Run this installer on each of your agent machines that requires an update.

> To check which Java the specific agent is using, open the [agent summary](viewing-build-agent-details.md), switch to the **Parameters** tab, and check the following parameter values:
> * `teamcity.agent.jvm.java.home`
> * `teamcity.agent.jvm.version`
> * `teamcity.agent.jvm.vendor`
>
{style="tip"}

</procedure>


</tab>


<tab title="Linux and macOS">

<procedure>

TeamCity server `.tar.gz` archives do not include Java, so you need to install it manually. Please note that the JDK you install matches your platform. For example, [Amazon Corretto 21](https://docs.aws.amazon.com/corretto/latest/corretto-21-ug/downloads-list.html) offers different options for Linux and macOS running on both ARM64 and x86_64 architectures.

Once you install Java 21, assign the corresponding installation path to the `JAVA_HOME` or `TEAMCITY_JRE` environment variable. See [this StackOverflow thread](https://stackoverflow.com/questions/21964709/how-to-set-or-change-the-default-java-jdk-version-on-macos) for the detailed instructions.

* The `JAVA_HOME` is a global variable that specifies the default JDK on your machine. When set, the `java -version` terminal command should point to the corresponding version.
* The `TEAMCITY_JRE` variable is used only by TeamCity and allows you to keep using a different Java version as a default one for other applications.

To update agent machines, follow the same procedure. Alternatively, you can install an [agent archive that already bundles the required JDK](install-teamcity-agent.md#Available+Agent+Distributions). To do so:

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