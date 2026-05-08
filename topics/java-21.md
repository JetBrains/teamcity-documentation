# Cheatsheet: Updating Java on Server and Agent Machines

This article briefly outlines the Java update procedure for agent and server machines. Refer to these articles for more detailed information.

* [](how-to.md#Install+Non-Bundled+Version+of+Java)
* [](configure-java-for-agent.md)
* [](upgrading-teamcity-server-and-agents.md)


## Migration to Java 21

Starting with version 2026.1, both the TeamCity server and agents require Java 21 to start. TeamCity server supports only Java 21, while TeamCity build agents can run under newer version (for example, Java 25). We plan to officially support the latest Java versions in future releases.


## Upgrade Instructions

The upgrade process boils down to two essential steps:

1. Install Java 21 on the machine.
2. Ensure TeamCity detects and uses the new installation.

The exact procedure depends on your operating system.

### Update Server

<tabs xmlns="">

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

To locate all agents that require an update (including both local and [cloud](teamcity-integration-with-cloud-solutions.md) ones), navigate to **Agents | Overview | Parameters report** and filter agents by the `teamcity.agent.jvm.specification` property value.

<img src="agents-java-report.png" width="706" alt="Outdated agents report"/>

To find all oudated agents using [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html):

```Shell
# Find outdated local agents

curl --location --request GET 'http://$SERVER_URL/app/rest/agents?locator=parameter:(name:teamcity.agent.jvm.specification,value:21,matchType:does-not-match)&fields=count,agent(id,name,connected,authorized,properties($locator(name:teamcity.agent.jvm.specification),property(name,value)))' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $TC_ACCESS_TOKEN'

# Find outdated cloud profiles
# Requires profiles to have at least one running agent

curl --location --request GET 'http://$SERVER_URL/app/rest/cloud/profiles&fields=cloudProfile(id,name,images(cloudImage(id,name))&locator=image:(agent:(parameter:(name:teamcity.agent.jvm.specification,value:21,matchType:does-not-match))) \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer $TC_ACCESS_TOKEN'
```

#### Update Local Agents

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
> * `teamcity.agent.jvm.specification`
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
> * `teamcity.agent.jvm.specification`
>
{style="tip"}

</procedure>

</tab>

</tabs>


#### Update Cloud Agents

Cloud agents can reside on persistent cloud-hosted instances or ephemeral virtual machines spawned from an image. Depending on the VM type and cloud hosting provider, the update instructions may vary. For example, for TeamCity cloud profiles that target [EC2 AMIs](setting-up-teamcity-for-amazon-ec2.md), you need to:

1. Update the base image as described in the [Update Local Agents](#Update+Local+Agents) section and make sure this agent successfully connects to TeamCity.
2. Build a new AMI from this base image.
3. Update your TeamCity cloud profiles and images to target the new AMI.

   
## Unattended Java Upgrades

<include from="configure-java-for-agent.md" element-id="unattended-java-upgrades"/>