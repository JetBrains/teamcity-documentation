[//]: # (title: TeamCity Cloud Documentation)
[//]: # (auxiliary-id: TeamCity Cloud Documentation)

Welcome to the documentation for [TeamCity Cloud Beta](https://www.jetbrains.com/teamcity/cloud/).

>This documentation is a work in progress and might contain sections relevant only to TeamCity On-premises. Please refer to the [following section](#Differences+between+TeamCity+Cloud+and+On-premises) to learn about the current differences between TeamCity Cloud and On-premises versions.
>
{type="warning"}

<table>
<tr>
</tr>

<tr>

<td>

### Get Started

* [Getting Started with TeamCity](getting-started-with-teamcity-cloud.md)
* [TeamCity Concepts](concepts.md)
* [Set up Additional Build Agents](setting-up-and-running-additional-build-agents.md)

</td>

<td>

### Administer

* [Configure and Maintain TeamCity server](teamcity-configuration-and-maintenance.md)
* [Work with Projects and Build Configurations](managing-projects-and-build-configurations.md)
* [Manage Users and their Permissions](managing-user-accounts-groups-and-permissions.md)

</td>

</tr>

<tr>

<td>

### Find Answers

* [How To...](how-to.md)
* [Troubleshooting](troubleshooting.md)
* [Common Problems](common-problems.md)
* [Known Issues](known-issues.md)
* [Public Issue Tracker](http://youtrack.jetbrains.net/issues/TW)
* [Community Forum](http://jb.gg/teamcity-forum)
* [Video User Guide](http://blog.jetbrains.com/teamcity/2013/05/teamcity-user-guide-courseware/)

</td>


<td>

### Learn More and Contact Us

* [TeamCity Official Site](http://www.jetbrains.com/teamcity)
* [Official TeamCity Blog](http://blogs.jetbrains.com/teamcity/)
* [Feedback](https://confluence.jetbrains.com/display/TW/Feedback)

</td></tr>
</table>

## TeamCity Cloud Supported Platforms and Environments

The core features of TeamCity are platform-independent. See details at [Supported Platforms and Environments](supported-platforms-and-environments.md).

[//]: # (Internal note. Do not delete. "TeamCity Documentationd313e156.txt")

## Copyright and Trademark Notice

The software described in this documentation is furnished under a software license agreement.  _JetBrains_, _IntelliJ_, _IntelliJ IDEA_, _YouTrack_ and _TeamCity_ are trademarks or registered trademarks of _JetBrains, s.r.o._  _Windows_ is a registered trademark of _Microsoft Corporation_ in the United States and other countries. _Mac,_ _Mac OS, macOS_ are trademarks of _Apple Inc._, registered in the U.S. and other countries. _Linux_ is a registered trademark of _Linus Torvalds_. All other trademarks are the properties of their respective owners.

## Differences between TeamCity Cloud and On-premises

We plan to provide equal CI/CD experience to users of our Cloud and On-premises versions. However, the Cloud version is an automatically configured server and thus does not provide the same server settings as our On-premises solution.

Comparing to On-premises, TeamCity Cloud Beta offers the following new features:
* For better security, you can generate authentication tokens for build agents in advance.
* If you authenticate via GitHub, GitLab, or Bitbucket, the respective [connection](integrating-teamcity-with-vcs-hosting-services.md#Configuring+Connections) will be preconfigured automatically.
* The __Administration | Invitations__ page allows automatically inviting users to the server. __By default, you can add new users only via invitations__. An invited user will be able to register a new user account or authenticate via GitHub, GitLab, or Bitbucket.

All the listed features will be introduced in our On-premises version in the nearest future.

TeamCity Cloud Beta has the following limitations comparing to On-premises:
* Limited server configuration.
* No automatic [server diagnostics](teamcity-monitoring-and-diagnostics.md).
* Data is backed up and cleaned up automatically. The set of available configuration options may differ from the On-premises installations.
* Some settings are unavailable to TeamCity Cloud administrators: for example, cloud profiles' configuration or changing the location for storing external artifacts.
* No plugin management. The following bundled plugins are currently disabled:
    * LDAP support
    * Microsoft Windows Domain authentication
    * VCS Support: CVS and VCS Support: StarTeam
    * RSS feed support
    * Build Agent JVM updater
    * NuGet Support
    * Search

Some listed features might be implemented or receive their Cloud equivalents in terms of the Beta or first released versions of TeamCity Cloud.