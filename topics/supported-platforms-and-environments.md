[//]: # (title: Supported Platforms and Environments)
[//]: # (auxiliary-id: Supported Platforms and Environments)

This page covers software-related environments TeamCity works with. For hardware-related notes, see [this section](how-to.md#Estimate+Hardware+Requirements+for+TeamCity).
{product="tc"}

## Platforms (Operating Systems)

### TeamCity Server
{product="tc"}

Core features of TeamCity server are platform-independent. See [considerations](how-to.md#Choose+OS%2FPlatform+for+TeamCity+Server) on choosing server platform.   

The TeamCity server is a web application that runs within a capable J2EE servlet container.

The server requires a Java SE JRE installation to run. See [notes](installing-and-configuring-the-teamcity-server.md#Java+Installation) on how to install Java on the server.

<note>

__Since TeamCity 2019.2__, [Amazon Corretto](https://aws.amazon.com/corretto/) is included in the Windows `.exe` TeamCity distribution (previously, 32-bit Oracle Java and then AdoptOpenJDK were bundled with the TeamCity Windows distribution). Users of the bundled version of JRE are automatically switched to 64-bit Amazon Corretto on upgrading TeamCity to 2019.2 or later.

</note>

Supported Java versions are OpenJDK and Oracle Java 8 (8u16 or later) and 11 (32 or 64 bit). Using 64-bit Java is recommended.

The TeamCity server Windows installer and server Docker images come bundled with 64-bit Java 11.

Generally, __all the recent versions of Windows, Linux, and macOS are supported__. If you find any compatibility issues with any of the operating systems, make sure to [let us know](feedback.md).

The TeamCity server is tested under the following operating systems:
* Linux (Ubuntu, Debian, RedHat, SUSE, and others)
* macOS
* Windows 7/7x64
* Windows Server 2008, 2012, 2016, 2019
* Server Core installation of Windows Server 2016
* Windows 10 under the Tomcat 8.5+ web application server.
   
Reportedly works without known issues on:
* Windows 7+
* Windows Server 2008 R2
* Solaris
* FreeBSD
* IBM z/OS
* HP-UX
   
Note that Windows XP/XP x64 are not supported.

### Build Agents
{product="tc"}

The TeamCity Agent is a standalone Java application.

Build agents require a Java SE JRE installation to run. See [notes](setting-up-and-running-additional-build-agents.md#Configuring+Java) on how to configure Java on agents.

Supported Java versions are OpenJDK and Oracle Java 8 - 11. We recommend using the latest available version of JDK. Support for running agents under Java 1.6 and 1.7 is deprecated.

<note>

__Since TeamCity 2019.2__, 64-bit [Amazon Corretto](https://aws.amazon.com/corretto/) 8 is included in the Windows `.exe` TeamCity distribution (previously, 32-bit Oracle Java and then AdoptOpenJDK were bundled with the TeamCity Windows distribution). Users of the bundled version of JRE are automatically switched to 64-bit Amazon Corretto on upgrading TeamCity to 2019.2 or later.

</note>

The TeamCity agent is tested under the following operating systems:
* Linux
* macOS
* Windows 7/7x64
* Windows 10
* Windows Server 2003/2008, 2012, 2016, 2019
* Server Core installation of Windows Server 2016

Reportedly works on:
* Windows XP/XP x64
* Windows 2000 (interactive mode only)
* Solaris
* FreeBSD
* IBM z/OS
* HP-UX

### Build Agents
{product="tcc"}

The TeamCity Agent is a standalone Java application. TeamCity supports two types of agents:
* Hosted by JetBrains
* Hosted by a customer

You can combine agents of both types in your installation. Read more information on licensing these agents in [Subscription and Licensing](teamcity-cloud-subscription-and-licensing.md).

#### JetBrains-Hosted Agents

These agents are automatically maintained by JetBrains and don't require to be installed or configured by a customer. There are multiple types of these agents:

<table>

<tr>

<td>

Instance Type

</td>

<td>

Hardware

</td>

</tr>

<tr>

<td>

Linux (small)

</td>

<td>

* CPU: 2 vCPU (Intel Xeon (Cascade Lake))
* RAM: 4 Gb ram
* SSD: 20 Gb

</td>

</tr>

<tr>

<td>

Linux (medium)

</td>

<td>

* CPU: 4 vCPU (Intel Xeon (Cascade Lake))
* RAM: 8 Gb ram
* SSD: 20 Gb

</td>

</tr>

<tr>

<td>

Windows (medium)

</td>

<td>

* CPU: 4 vCPU (Intel Xeon (Cascade Lake))
* RAM: 8 Gb ram
* SSD: 20 Gb

</td>

</tr>

</table>

Each JetBrains-hosted agent comes with a set of preinstalled software.

Software preinstalled on Windows agents:

<include src="preinstalled-software-on-teamcity-cloud-windows-agents.md"
include-id="windows-jb-agents"/>

Software preinstalled on Ubuntu agents:

<include src="preinstalled-software-on-teamcity-cloud-ubuntu-agents.md"
include-id="ubuntu-jb-agents"/>

#### Self-Hosted Agents

You can install a build agent locally on your machine, similarly to how you would do it in [TeamCity On-Premises](https://www.jetbrains.com/help/teamcity/setting-up-and-running-additional-build-agents.html), and connect it to the TeamCity Cloud instance. Note that you need to acquire a [concurrent build slot](teamcity-cloud-subscription-and-licensing.md#Using+Build+Credits) for each self-hosted agent.

Build agents require a Java SE JRE installation to run. See [notes](setting-up-and-running-additional-build-agents.md#Configuring+Java) on how to configure Java on agents.

Supported Java versions are OpenJDK and Oracle Java 8 - 11. We recommend using the latest available version of JDK. Support for running agents under Java 1.6 and 1.7 is deprecated.

The TeamCity agent is tested under the following operating systems:
* Linux
* macOS
* Windows 7/7x64
* Windows 10
* Windows Server 2003/2008, 2012, 2016, 2019
* Server Core installation of Windows Server 2016

Reportedly works on:
* Windows XP/XP x64
* Windows 2000 (interactive mode only)
* Solaris
* FreeBSD
* IBM z/OS
* HP-UX
   
### Windows Tray Notifier

Windows 7/7x64/10 with one of the supported versions of Internet Explorer.

## Web Browsers

The TeamCity Web Interface is mostly W3C-compliant, so just about any modern browser should work well with TeamCity. The following browsers have been specifically tested and reported to work correctly in the recent versions of:
   * Google Chrome
   * Mozilla Firefox 
   * Safari under Mac
   * Microsoft Edge
   * Opera 15+
   
## Build Runners

TeamCity supports a wide range of build tools, enabling both Java and .NET software teams to build their projects.

### Supported Java build runners

* [Ant](ant.md) 1.6-1.10. TeamCity comes bundled with Ant 1.9.14.
* [Maven](maven.md) versions 2.0.x, 2.x, 3.x (known at the moment of the TeamCity release). Java 1.5 and higher is supported. TeamCity comes bundled with Maven 2.2.1, 3.0.5, 3.1.1, 3.2.5, 3.3.9, 3.5.4, and 3.6.3.
* [IntelliJ IDEA Project](intellij-idea-project.md) runner (requires Java 8)
* [Gradle](gradle.md) (requires Gradle 0.9-rc-1 or higher)
* [Java Inspections](inspections.md) and [Java Duplicates](duplicates-finder-java.md) based on IntelliJ IDEA (requires Java 8)

### Supported .NET platform build runners

For a unified [.NET](net.md) runner, see a [dedicated section](net.md#Requirements).

Other runners:
* [MSBuild](msbuild.md). Requires .NET Framework or Mono installed on the build agent. Microsoft Build Tools 2013, 2015, 2017, and 2019 are also supported.
* [NAnt](nant.md) versions 0.85 - 0.91 alpha 2. Requires .NET Framework or Mono installed on the build agent.
* [Microsoft Visual Studio Solutions](visual-studio-sln.md) ([2003](visual-studio-2003.md) - 2015, 2017, and 2019). Requires a corresponding version of Microsoft Visual Studio installed on the build agent.
* [FxCop](fxcop.md). Requires FxCop installed on the build agent.
* [Duplicates Finder for C# and VB.NET code](duplicates-finder-resharper.md) based on [ReSharper Command Line Tools](http://www.jetbrains.com/resharper/features/command-line.html). Supported languages are C# up to version 4.0 and VB.NET version 8.0 - 10.0. Requires .NET Framework 4.6.1+ installed on the build agent.
* [Inspections for .NET](inspections-resharper.md) based on [ReSharper Command Line Tools](http://www.jetbrains.com/resharper/features/command-line.html). Requires .NET Framework 4.6.1+ installed on the build agent.
* [.NET Process Runner](net-process-runner.md) for running any .NET application (requires .NET installed on the build agent)
* [NuGet](nuget.md) runners under Windows, Linux, macOS. Require NuGet.exe Command Line tool installed on the agents. Supported NuGet versions under Windows are 1.4+. 
  * Windows: NuGet versions prior to 2.8.6 require .NET Framework 4.0+ installed on the build agent
  * Windows: NuGet 2.8.6 and later requires .NET 4.5
  * Linux and macOS: require [Mono](http://www.mono-project.com/docs/getting-started/install/) 4.4.2+ and NuGet CLI 3.2+ installed on the agent

### Other runners

* [Python](python.md), requires installing Python version 2.0 or later on agents
* [Rake](rake.md)
* [Command Line](command-line.md) for running any build process using a shell script
* [PowerShell](powershell.md), versions 1.0 - 5.0
* [Xcode](xcode-project.md), versions 3-12 (requires [Xcode](xcode-project.md) installed on the build agent)

## Testing Frameworks 

* JUnit 3.8.1+, 4.x
* [NUnit](nunit-support.md) 2.2.10, 2.4.x, 2.5.x, 2.6.x, 3.0.x are supported (dedicated build runner). 
* TestNG 5.3+
* MSTest 8.x-12.x, 14.x, 15.x, 19.x and VSTest are supported by the [.NET](net.md) runner; requires the appropriate Microsoft Visual Studio edition or Visual Studio Test Agent installed on the build agent.
* [MSpec](mspec.md) (requires MSpec installed on the build agent)

## Version Control Systems

* [Git](git.md) (for automatic `git gc` support requires Git client installed on the server in order to perform maintenance of Git clones, latest version is recommended)
* [Subversion](subversion.md) (server versions 1.4-1.9 and higher as long as the protocol is backward-compatible).
* [Perforce](perforce.md) (requires a Perforce client installed on the TeamCity server). Check [compatibility issues](perforce-vcs-compatibility.md).
* [Team Foundation Server](team-foundation-server.md) 2005, 2008, 2010, 2012, 2013, 2015, 2017 are supported. 
* [Mercurial](mercurial.md) (requires the Mercurial "hg" client v1.5.2+ installed on the server)
* [CVS](cvs.md)
{product="tc"}
* [SourceGear Vault](sourcegear-vault.md) 6 and 7 (requires the Vault command line client libraries installed on the TeamCity server), _integrated via an additional plugin_
{product="tc"}
* [Borland StarTeam](starteam.md) 6 and up (the StarTeam client application must be installed on the TeamCity server)
{product="tc"}
* [IBM Rational ClearCase](clearcase.md) Base and UCM modes (requires the ClearCase client installed and configured on the TeamCity server), _integrated via an additional plugin_
{product="tc"}
* [Microsoft Visual SourceSafe](visual-sourcesafe.md) 6 and 2005 (requires a SourceSafe client installed on the TeamCity server, available only on Windows platforms)
{product="tc"}

For support for other VCS please check [external plugins](https://plugins.jetbrains.com/category/93-version-control-systems-support/teamcity) available.

### Checkout on Agent

The requirements noted are for agent environment and are additional to those for the server listed above.

* Git (git client version 1.6.4+ must be installed on the agent, latest version is recommended)
* Subversion (working copies in the Subversion 1.4-1.8 format are supported)
* Perforce (a Perforce client must be installed on the TeamCity agent machine)
* Team Foundation Server 2005-2015, 2017 are supported. 
* Mercurial (the Mercurial "hg" client v1.5.2+ must be installed on the TeamCity agent machine)
* CVS
{product="tc"}
* IBM Rational ClearCase (the ClearCase client must be installed on the TeamCity agent machine)
   
### Labeling Build Sources

* Git
* Subversion
* Perforce
* Team Foundation Server
* Mercurial
* CVS
{product="tc"}
* SourceGear Vault, _integrated via an additional plugin_
{product="tc"}
* Borland StarTeam
{product="tc"}
* ClearCase, _integrated via an additional plugin_
{product="tc"}
   
### Remote Run on Branch

* Git
* Mercurial
   
### Feature Branches

* Git
* Mercurial
   
### VCS Systems Supported via Third Party Plugins

* [AccuRev](https://plugins.jetbrains.com/plugin/8885?pr=teamcity)
* [Bazaar](https://plugins.jetbrains.com/plugin/8886?pr=teamcity)
* [PlasticSCM](https://plugins.jetbrains.com/plugin/8889-plastic-scm?pr=teamcity) (related [details](http://www.plasticscm.com/infocenter/technical-articles/kb-how-to-integrate-plastic-scm-with-teamcity-ci.aspx))


## Cloud Agents Integration
{product="tc"}

* [Amazon EC2](setting-up-teamcity-for-amazon-ec2.md)
* [VMWare vSphere](setting-up-teamcity-for-vmware-vsphere-and-vcenter.md)

See also details on the [cloud integrations](teamcity-integration-with-cloud-solutions.md) and non-bundled and third-party [cloud integration plugins](https://plugins.jetbrains.com/category/102-cloud-support/teamcity).

## VCS Hosting Services Integration

* [GitHub / GitHub Enterprise](integrating-teamcity-with-vcs-hosting-services.md#Connecting+to+GitHub)
* [GitLab](integrating-teamcity-with-vcs-hosting-services.md#Connecting+to+GitLab)
* [Bitbucket Cloud](integrating-teamcity-with-vcs-hosting-services.md#Connecting+to+Bitbucket+Cloud)
* [Azure DevOps Services](integrating-teamcity-with-vcs-hosting-services.md#Connecting+to+Azure+DevOps+Services), or formerly Visual Studio Team Services

## Issue Tracker Integration

* [JetBrains YouTrack](http://jetbrains.com/youtrack) 1.0 and later (tested with the latest version)
* [Atlassian Jira](http://www.atlassian.com/software/jira/) 4.4 and later (all major features also reportedly worked for version 4.2)
* [Bugzilla](http://www.bugzilla.org) 3.0 and later (tested with Bugzilla 5.0.1)
* [GitHub](github.md)
* [Bitbucket](github.md)
* [Azure DevOps Server](team-foundation-work-items.md) (formerly Visual Studio Team Foundation Server — supported version 2012 or later, and Azure DevOps Services are supported)
   
Additional requirements are listed in [Integrating TeamCity with Issue Tracker](integrating-teamcity-with-issue-tracker.md).

Links to issues of any issue tracker can also be recognized in change comments using [Mapping External Links in Comments](mapping-external-links-in-comments.md).
{product="tc"}

## IDE Integration

TeamCity provides productivity plugins for the following IDEs:

* [Eclipse](eclipse-plugin.md): Eclipse versions 3.8 and 4.2-4.6 are supported. Eclipse must be run under JDK 1.5\+
* [IntelliJ Platform Plugin](intellij-platform-plugin.md): compatible with IntelliJ IDEA 15.0.x - 2019.3.x (Ultimate and Community editions); as well as other IDEs based on the same version of the platform, including JetBrains RubyMine 6.3+, JetBrains PyCharm 3.1+, JetBrains PhpStorm/WebStorm 7.1+, AppCode 2.1+. See [more information](intellij-platform-plugin-compatibility.md) on compatibility.
* [Microsoft Visual Studio](visual-studio-addin.md) 2010, 2012, 2013, 2015, 2017, 2019
 is supported by the TeamCity Visual Studio [Add-in shipped as a part of ReSharper Ultimate](visual-studio-addin.md#Installing+Add-in). Installed .NET Framework is required. 

### Remote Run and Pre-tested Commit

[Remote Run](remote-run.md) and [Pre-tested commit](pre-tested-delayed-commit.md) functionality is available for the following IDEs and version control systems:

<table><tr>

<td>

IDE

</td>

<td>

Supported VCS

</td></tr><tr>

<td>

[Eclipse](eclipse-plugin.md)


</td>

<td>


* Subversion 1.7-1.8 via Subclipse and Subversive Eclipse integration plugins or SvnKit.
* Subversion 1.4-1.7 via Subclipse and Subversive Eclipse integration plugins.
* Perforce (P4WSAD 2009.2 - 2010.1, P4Eclipse 2010.1 - 2015.1)
* Git (the EGit 2.0+ Eclipse integration plugin)
* CVS
{product="tc"}
* ClearCase (the client software is required), _integrated via an additional plugin_
* [see also](eclipse-plugin.md)


</td></tr><tr>

<td>

[IntelliJ IDEA Platform](intellij-platform-plugin.md) \*)

</td>

<td>

* Subversion
* Perforce
* Git (remote run only)
* Azure DevOps Server, or formerly Team Foundation Server

</td></tr><tr>

<td>

[Microsoft Visual Studio](visual-studio-addin.md)


</td>

<td>


* Subversion 1.4-1.11 (the command-line client is required); note that 1.10-1.11 is supported __since ReSharper 2018.3__.
* Azure DevOps Server (formerly Team Foundation Server — supported version 2005 or later). Installed Team Explorer is required.
* Perforce 2008.2 and later (the command-line client is required).


</td></tr></table>

\*) Supported only with the VCS integrations bundled with the IDEs by JetBrains

### Code Coverage

<table><tr>

<td>

IDE


</td>

<td>

Supported Coverage Tool


</td></tr><tr>

<td>

[Eclipse](eclipse-plugin.md)


</td>

<td>

[IDEA](intellij-idea.md) and [EMMA](emma.md) code coverage


</td></tr><tr>

<td>

[IntelliJ IDEA Platform](intellij-platform-plugin.md)


</td>

<td>

[IDEA](intellij-idea.md), [EMMA](emma.md) and [JaCoCo](jacoco.md) code coverage


</td></tr><tr>

<td>

[Microsoft Visual Studio](visual-studio-addin.md)


</td>

<td>

JetBrains dotCover coverage. Requires [JetBrains dotCover](http://www.jetbrains.com/dotcover/) installed in Microsoft Visual Studio


</td></tr></table>

## Supported Databases
{product="tc"}

See more at [Setting up an External Database](setting-up-an-external-database.md)

* HSQLDB 2.3.2   
The internal database suits __evaluation purposes only__; we strongly recommend using an external database in a production environment.
* MySQL 5.0.33+, 5.1.49+, 5.5+, 5.6+, 5.7+, 8+ (Note that due to bugs in MySQL, versions 5.0.20, 5.0.22 and 5.1 up to 5.1.48 are not compatible with TeamCity)
* Microsoft SQL Server 2005 or later (including Express editions), SQL Azure; SSL connections support might require [these updates](http://blogs.msdn.com/b/jdbcteam/archive/2012/01/19/patch-available-for-sql-server-and-java-6-update-30.aspx).
* PostgreSQL 8.2 and newer
* Oracle 10g and newer (TeamCity is tested with [driver](http://www.oracle.com/technetwork/database/features/jdbc/index-091264.html) version 12.1.0.1)

## Game Engines

* Unity, by the means of the [Unity Support](https://plugins.jetbrains.com/plugin/11453-unity-support) plugin (bundled in TeamCity Cloud and can be installed on-demand in TeamCity On-Premises)