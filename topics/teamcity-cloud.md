[//]: # (title: TeamCity Cloud)
[//]: # (auxiliary-id: TeamCity Cloud)

<tip>

__TeamCity Cloud Public Beta is open!__

This document describes the current state of the TeamCity Cloud option, its Beta limitations, and target functionality. Please make sure to read it before joining our Public Beta program.

</tip>

## About TeamCity Cloud

TeamCity is a CI/CD server which key features are a powerful toolset and universality. With our Cloud version, we address the user demand in the full-featured CI/CD solution and make it available to you in a couple of minutes, with no need to maintain a server on-premise.

__If you are new to TeamCity__, the Cloud Beta is a great starting point as it automatically resolves the task of installing and configuring the server. After your [cloud TeamCity server is ready](#Starting+TeamCity+Cloud), you can proceed to our [_Configure and run your first build_](configure-and-run-your-first-build.md) guide.

When you register a TeamCity Cloud account, your own TeamCity server is automatically created in Amazon Web Services. The server operates on the newest version of TeamCity and currently provides Windows and Linux cloud agents out of the box (self-hosted agents for Windows, Linux, and macOS are also supported).

We plan to officially release our Cloud version at the end of 2020 but the target date may change depending on the progress of Beta.

__Users of the Beta program will be able to migrate their TeamCity data to the released Cloud version.__

## Starting TeamCity Cloud

To start TeamCity in cloud, __[register a Cloud Beta account](https://www.jetbrains.com/teamcity/cloud/)__. In a matter of seconds, your server will be available under the `beta.teamcity.com` domain.

After the server is ready, an invitation link will be sent to your email. Proceed via this link to get to your administrative account. Everything is ready to build!

## TeamCity Cloud Maintenance

### TeamCity Cloud Server Administration

In case with TeamCity Cloud, the TeamCity team is responsible for maintaining all hardware and infrastructure of cloud servers. Because of that, the Cloud server has a limited administration functionality comparing to the On-premise server.

The __Administration__ pages comprises the following sections:
* Under __Project-related Settings__, you can create projects and monitor server health and disk usage.
* Under __User Management__, you can add users, user groups and roles, and assign permissions to users. __Use the new _Invitations_ page to invite TeamCity users via email__. An invited user will be able to register a new user account or authenticate via GitHub or Bitbucket.
* Under __Server Administration__, you can import projects (for example, exported from your On-premise server), view usage statistics, and control general settings.

### Build Agents in Cloud

In TeamCity Cloud, you can either use self-hosted [build agents](build-agent.md) or request SaaS cloud agents.

<tip>

TeamCity Cloud supports agents hosted on Windows, Linux, and macOS. SaaS agents are available for Windows and Linux from the start, and macOS cloud agents will be introduced later in Beta.

</tip>
 

From the start, you get one Windows and two Linux images available in your default [pool](agent-pools.md). As soon as you run a build, TeamCity will automatically start an instance of the first suitable image. After the build is finished, the instance will be terminated by the idle timeout.

You can get extra agents by clicking __Install Build Agents__ in the upper right corner of the __Agents__ page. In terms of Beta, we provide 10 parallel agents.   
We recommend using tokens to authenticate build agents on the TeamCity server. Click __Use authentication token__ and choose one of the two options: _Generate plain-text token_, or _Download config_ to create a ready-to-use [agent config file](build-agent-configuration.md).

In terms of Beta, cloud agents come with the following characteristics:

### Windows Cloud Agents

Hardware:
* CPU: 4 vCPU (Intel Xeon (Cascade Lake))
* RAM: 8 Gb ram
* SSD: 20 Gb

Software:
* Image: `Windows_Server-2012-R2_RTM-English-64Bit-Base`
* Java (6.45, 7.79, 7.79, 8u181, 8u181, 9.0.4)
* .NET Framework (3.5 (3.0, 2,0 included), 4.5, 4.6.2, 4.7.1)
* .NET Core (1.1.7, 2.1.4, 3.1.102)
* Visual Studio 2019 Build Tools
* Ruby (2.4.3.1, 2.5.1.1, 4.7.2.2913922492)
* Perforce CLI (2020.1)
* NUnit (3.11.1)
* Sysinternals Suite
* Mono (4.2.3)
* Android SDK (3859397 +update)
* Python (2.7, 3.8 + pip)
* Git (2.21.0-64)
* Mercurial (tortoisehg-4.0.2)
* Subversion (1.9.7)
* Gradle (5.5.1)
* NodeJS (12.16.1)
* Yarn (1.22.4)
* CMake (3.14.3)
* 7-Zip (19.0)
* Unity (2019.3.14)
* MSYS2 (base 20200517)
* AWS CLI (1.16.144)

### Linux Cloud Agents

Hardware:
* Medium agent:
    * CPU: 4 vCPU (Intel Xeon (Cascade Lake))
    * RAM: 8 Gb ram
    * SSD: 20 Gb
* Small agent:
    * CPU: 2 vCPU (Intel Xeon (Cascade Lake))
    * RAM: 4 Gb ram
    * SSD: 20 Gb

Software:
* Image: `ubuntu-bionic-18.04-amd64-server`
* Java (java-1.8.0-amazon-corretto-jdk_8.252.09-1_amd64)
* .NET Core (3.1.301, 3.0.103, 2.1.807, 2.2.402, 1.1.13)
* Git (2.26.2)
* Python (3.6.7, 2.7.15; pip 20.1.1)
* Mono (6.8.0.123)
* Perforce (helix-cli latest)
* Mercurial (4.5.3)
* NAnt (latest)
* Ant (latest)
* Android SDK (4333796 +update)
* Gradle (5.5.1)
* RVM (Ruby 2.5.8)
* Go (1.14.1)
* NodeJS (0.33.11) 4, 5, 6, 7, 8.12 (default)
* Docker (19.03.9)
* Docker Compose (1.23.1)
* Ansible (2.9.10)
* Native development tools (automake 1.15.1, autoconf 2.69, gettext, libtool 2.4.6, m4 1.4.18, clang, cmake 3.10.2, libglib2.0-dev, libcairo2-dev, libjpeg-dev, libexif-dev, libgif-dev, libc++-dev)
* Yarn (1.22.4)

<tip>

The software installed on cloud agents will be updated automatically. In case you lack any preinstalled software, consider using our [Docker Wrapper](docker-wrapper.md) extension to run a build step inside a specific Docker image.

Do not hesitate to [contact us](#Troubleshooting+and+Feedback) in case you have any questions about the preinstalled software set.

</tip>

## Differences between TeamCity Cloud and On-premise

We plan to provide equal CI/CD experience to users of our Cloud and On-premise versions. However, the Cloud version is an automatically configured server and thus does not provide the same server settings as our On-premise solution.

Comparing to On-premise, TeamCity Cloud Beta offers the following new features:
* For better security, you can generate authentication tokens for build agents in advance.
* Authorization via GitHub and Bitbucket is available. If you authenticate via any of these services, the respective [connection](integrating-teamcity-with-vcs-hosting-services.md#Configuring+Connections) will be preconfigured automatically.
* The __Administration | Invitations__ page allows automatically inviting users to the server. __By default, you can add new users only via invitations__. An invited user will be able to register a new user account or authenticate via GitHub or Bitbucket.

All the listed features will be introduced in our On-premise version in the nearest future.

TeamCity Cloud Beta has the following limitations comparing to On-premise:
* Limited server configuration.
* No automatic [server diagnostics](teamcity-monitoring-and-diagnostics.md).
* Data is backed up and cleaned up automatically. The set of available configuration options may differ from the On-premise installations.
* Some settings are unavailable to TeamCity Cloud administrators: for example, cloud profiles' configuration or changing the location for storing external artifacts.
* No plugin management. The following bundled plugins are currently disabled:
    * LDAP support
    * Microsoft Windows Domain authentication
    * VCS Support: CVS and VCS Support: StarTeam
    * RSS feed support
    * Build Agent JVM updater
    * NuGet Support
    * Search

Some listed features might be implemented or receive their Cloud equivalents in terms of the Public Beta or first released versions of TeamCity Cloud.

## Troubleshooting and Feedback

To send your feedback or report issues, use our [YouTrack issue tracker](https://youtrack.jetbrains.com/issues/TW). You are welcome to discuss TeamCity Cloud in our [community forum](https://teamcity-support.jetbrains.com/hc/en-us/community/topics/).

Before inquiring, please refer to the [F.A.Q.](https://www.jetbrains.com/teamcity/cloud/) on our website.

### Known Issues

The following issues can be currently observed in the TeamCity Cloud Beta version:

* It is impossible to [restore a just deleted project](how-to.md#Restore+Just+Deleted+Project).
* When running a custom build, you might see an irrelevant warning about no compatible agents available for this build. Please check the build real-time status to see the actual number of available agents.