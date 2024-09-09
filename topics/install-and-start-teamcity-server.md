[//]: # (title: Install and Start TeamCity Server)
[//]: # (auxiliary-id: Install and Start TeamCity Server;Installation;Installing and Configuring the TeamCity Server)

TeamCity Server is a web application responsible for the core functionality of TeamCity. It provides a user interface, distributes the jobs (builds) to TeamCity agents, and aggregates their results. This section contains articles related to installing and starting your own instance of TeamCity Server.
{instance="tc"}

TeamCity Server is a web application responsible for the core functionality of TeamCity. It provides a user interface, distributes the jobs (builds) to TeamCity agents, and aggregates their results. Your TeamCity Cloud server is installed automatically after your register an account: no extra actions are required. After the server is ready, an invitation link will be sent to your email.
{instance="tcc"}

## Preliminaries
{instance="tc"}

Before installing the server, make sure to:
1. Estimate your [system requirements](system-requirements.md).
2. Read about [supported platforms](supported-platforms-and-environments.md).
3. Select a convenient installation package, as described [below](#Select+TeamCity+Installation+Package).

## Select TeamCity Installation Package
{instance="tc"}

TeamCity installation package is identical for both Professional and Enterprise Editions.

The [TeamCity download](https://www.jetbrains.com/teamcity/download/) page on the official JetBrains website provides the following installation options:

<table><tr>

<td>

Target platform

</td>

<td>

Distribution option

</td>

<td>

Description

</td></tr><tr>

<td>

[Windows](install-teamcity-server-on-windows.md)

</td>

<td>

`TeamCity<version_number>.exe`

</td>

<td>

Executable Windows installer bundled with Tomcat and Java.

</td></tr><tr>

<td>

[Windows](install-teamcity-server-on-windows.md) and [Linux/macOS](install-teamcity-server-on-linux-or-macos.md)

</td>

<td>

`TeamCity<version_number>.tar.gz`

</td>

<td>

Archive for manual installation bundled with a Tomcat servlet container.

</td></tr><tr>

<td>

[Docker (Linux, Windows)](https://hub.docker.com/r/jetbrains/teamcity-server/)

</td>

<td>

Docker

</td>

<td>

The official JetBrains TeamCity server Docker image.

</td></tr>

</table>

## Java Installation

TeamCity Server is a JVM web application that runs in a Tomcat application server. It requires a Java SE JRE installation to run. See what Java versions are [bundled with TeamCity](supported-platforms-and-environments.md#Supported+Java+Versions+for+TeamCity+Server) or read [how to install a non-bundled version of Java](how-to.md#Install+Non-Bundled+Version+of+Java).