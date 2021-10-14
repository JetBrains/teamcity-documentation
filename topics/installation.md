[//]: # (title: Installation)
[//]: # (auxiliary-id: Installation)

Your instance of TeamCity Cloud is installed automatically after your register an account: no extra actions are required. After the server is ready, an invitation link will be sent to your email.
{product="tcc"}

>If you are upgrading your existing TeamCity installation, refer to [Upgrade](upgrade.md).
> 
{product="tc"}

## Check System Requirements
{product="tc"}

Before you install TeamCity, please read about the [Supported Platforms and Environments](supported-platforms-and-environments.md). Additionally, read the [hardware requirements for TeamCity](how-to.md#Estimate+Hardware+Requirements+for+TeamCity). Note that these requirements differ significantly depending on the server load and the number of builds run.

## Select TeamCity Installation Package
{product="tc"}

TeamCity installation package is identical for both Professional and Enterprise Editions.

The [TeamCity download](http://www.jetbrains.com/teamcity/download/) page on the official JetBrains site provides the following installation options:

<table><tr>

<td>

Target

</td>

<td>

Option

</td>

<td>

Note

</td></tr><tr>

<td>

[Windows](installing-and-configuring-the-teamcity-server.md#Installing+TeamCity+via+Windows+installation+package)

</td>

<td>

TeamCity&lt;version number&gt;.exe

</td>

<td>

Executable Windows installer bundled with Tomcat and Java 1.8 JRE.

</td></tr><tr>

<td>

[Linux, macOS, Windows](installing-and-configuring-the-teamcity-server.md#Installing+TeamCity+bundled+with+Tomcat+servlet+container+%28Linux%2C+macOS%2C+Windows%29)

</td>

<td>

TeamCity&lt;version number&gt;.tar.gz

</td>

<td>

Archive for manual installation bundled with Tomcat servlet container

</td></tr><tr>

<td>

[Docker (Linux, Windows)](https://hub.docker.com/r/jetbrains/teamcity-server/)

</td>

<td>

Docker

</td>

<td>

Official JetBrains TeamCity server Docker image

</td></tr>

</table>

## Install Additional Build Agents
{product="tc"}

Although the TeamCity server in `.exe` and `.tar.gz` distributions is installed with a default build agent that runs on the same machine as the server, this setup may result in degraded TeamCity web UI performance, and if your builds are CPU-intensive, it is recommended to install build agents on separate machines or ensure that there is enough CPU/memory/disk throughput on the server machine.

To set up additional build agents, follow the [instructions](setting-up-and-running-additional-build-agents.md).

<seealso product="tc">
        <category ref="installation">
            <a href="installing-and-configuring-the-teamcity-server.md">Installing and Configuring the TeamCity Server</a>
            <a href="setting-up-and-running-additional-build-agents.md">Setting up and Running Additional Build Agents</a>
        </category>
</seealso>
