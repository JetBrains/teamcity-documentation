[//]: # (title: Upgrade Notes)
[//]: # (auxiliary-id: Upgrade Notes)


## Changes from 2024.03.2 to 2024.03.3
{id="2024.03.3"}

No potential breaking changes.

## Changes from 2024.03.1 to 2024.03.2
{id="2024.03.2"}

* The bundled Git was updated to version 2.45.1 in both Server and Agent Docker images.

### Known Issues
{id="known-issues-2024-03-2"}

* If you utilize the [Amazon Elastic Container Service Support](https://plugins.jetbrains.com/plugin/10067-amazon-elastic-container-service-support/versions/stable) plugin, please update it to the latest "SNAPSHOT-20240513140730" version. Older versions may malfunction with the 2024.03.2 version of the TeamCity server.
* AWS-hosted agents of the "Instance" type may fail to authorize to TeamCity server. As a workaround, you can resolve this issue by adding [internal properties](server-startup-properties.md#TeamCity+Internal+Properties) as suggested in the following ticket: [TW-88068](https://youtrack.jetbrains.com/issue/TW-88068#focus=Comments-27-9864960.0-0).
* TeamCity server fails to authorize Azure cloud agents from attached Virtual Machines. Note that this issue does not affect Azure images hosted in managed images. If migrating Azure cloud agents from VMs to images does not suit your requirements, please update [Azure Resource Manager Cloud Support](https://plugins.jetbrains.com/plugin/9260-azure-resource-manager-cloud-support) to the latest "0.12.15" version.

## Changes from 2024.03 to 2024.03.1

No potential breaking changes.

## Changes from 2023.11 to 2024.03

* The [](commit-status-publisher.md) build feature configured for [Perforce Helix Swarm](integrating-with-helix-swarm.md) no longer posts intermediate build statuses (queued, started, canceled) to the Swarm review's **Comments** tab. Instead, the feature announces only the final build status (successful or failed). You can additionally uncheck the **Code Review Comments** option in the build feature's settings dialog to disable these remaining status notifications as well (in this case, the Commit Status Publisher will only update the review's **Tests** tab).
* Starting with version 2023.03, versions of dotCover Command Line Tools that are no longer supported by the JetBrains dotCover team are explicitly marked as "deprecated".
    
    <img src="dk-deprecated-dotnetcli.png" width="706" alt="Deprecated dotNet Cli versions"/>
    
    Although these versions will remain functional, we encourage you to migrate to non-deprecated versions instead. Note that you should also install .NET Framework 4.7.2+ or .NET Core 3.1+ on agent machines for the non-deprecated versions to operate.

* When [installing custom agent tools](installing-agent-tools.md), editing the `teamcity-plugin.xml` file to [set executable bits](https://plugins.jetbrains.com/docs/teamcity/plugins-packaging.html#Making+File+Executable) is no longer required. Instead, make sure that the archived files already contain all required file permissions. In this case, files remain executable when the tool archive is unpacked on the agent machine.

* If your existing configurations continue using the [TeamCity.Node](https://github.com/jonnyzzz/TeamCity.Node) plugin, download the latest version [here](https://teamcity.jetbrains.com/buildConfiguration/bt434?mode=builds). Older plugin versions fail to load with agents reporting the "Failed to initialize spring context" error.


### Maven Tooling Updates

Version 2024.03 introduces a number of changes related to bundled Maven tools. These changes are dictated by certain Maven versions containing known CVEs, and carry on the initiative that aims to reduce the size of TeamCity installers by unbundling certain non-crucial components and tools.

These changes and their potential effects on your existing projects include the following:

* All Maven versions that are not in use by Maven [steps](maven.md) and [triggers](configuring-maven-triggers.md) are removed. Maven versions required by existing configurations will be downloaded and installed when your TeamCity server first starts. If the server cannot establish connection with [https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven](https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven), you will need to manually [install the missing tools](installing-agent-tools.md). If no existing configurations are using Maven, only the latest version 3.9.6 will be installed.

    > You can add the `teamcity.tools.bundled.maven.installOnStartup=false` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) to prevent TeamCity from lazy-loading Maven tools.
    > 
    {style="tip"}

* If there are no existing configurations that utilize the "Default" version of Maven, version 3.9.6 becomes the new "Default". Otherwise, the "Default" option will keep pointing to the same Maven tool as before (for example, 3.6.3).
* If an existing build configuration utilizes manually installed Maven 3.9.6 and [stores its settings in VCS](storing-project-settings-in-version-control.md), editing this configuration generates a [patch](kotlin-dsl.md#Edit+Project+Settings+via+Web+UI) that changes the value of the `mavenVersion` parameter from `custom` to `bundled_3_9_6`.


### Bundled Tools Updates
{id="bundled-tools-updates-2024-03"}

* Maven 3.9.6 was added as one of the standard versions of the tool available in TeamCity. See also the [](#Maven+Tooling+Updates) section for the information about notable changes related to Maven in version 2024.03.
* The bundled Kotlin compiler (used in TeamCity DSL) and Dokka (the documentation engine for Kotlin) was updated to version 1.9.22.
* The [internal HSQLDB database](supported-platforms-and-environments.md#Databases) was updated to version 2.7.2. Note that this database should not be used for actual production purposes.
* The bundled dotCover tool has been updated to version 2023.3.3.
* The [Jersey](https://eclipse-ee4j.github.io/jersey/) library used by [TeamCity REST API](teamcity-rest-api.md) was updated to version 2.41. This change does not affect regular REST API users. However, since version 2.41 exhibits significant changes compared to the previous 1.19 (for example, the modified dependency injection logic), 3rd-party TeamCity plugins that rely on REST API (for example, a highly popular [tcWebHooks](https://github.com/tcplugins/tcWebHooks)) may become incompatible with the updated TeamCity server.
* The bundled Tomcat was updated to version 9.0.87.
* [](agent-docker-images.md) for Linux now ship with updated Docker-related tools.
    * [Docker Engine](https://endoflife.date/docker-engine) (Docker CE and Docker CE CLI) were updated to version 24.0.9. This change also leads to the Docker Compose update to version v2.25.0.
    * [containerd](https://containerd.io) was updated to version 1.6.28.

### Known Issues
{id="known-issues-2024-03"}

* Agents running from the Windows [2024.03-nanoserver-2022](https://hub.docker.com/layers/jetbrains/teamcity-agent/2024.03-nanoserver-2022/images/sha256-ed5c37ef169896e87dd37c250ee3447403f9609dd5f49348b39b75d0a01a8b57?context=explore) Docker image become incompatible with certain runners after they restart. This issue occurs only after a restart, after their initial launch agents operate as expected. View this YouTrack ticket to track this issue: [TW-87124](https://youtrack.jetbrains.com/issue/TW-87124).
* Builds that pull TFS repositories fail with the `java.lang.NoClassDefFoundError` message if the checkout mode is "Always checkout files on agent". To fix this issue, download an updated **VCS support: TFVC** plugin compatible with TeamCity 2024.03 from this YouTrack issue: [TW-82824](https://youtrack.jetbrains.com/issue/TW-82824).
* If you face the "Invalid or corrupt jarfile /data/build/teamcity/buildAgent/plugins/environment-fetcher..." error reported by the bundled [](nodejs.md) or legacy [TeamCity.Node](https://github.com/jonnyzzz/TeamCity.Node) plugins, download the updated **Process Environment Fetcher** plugin from this YouTrack issue: [TW-87170](https://youtrack.jetbrains.com/issue/TW-87170).
* Build agents can unregister due to inactivity after the server restarts. See this issue for more information: [TW-87156](https://youtrack.jetbrains.com/issue/TW-87156/Agent-Disconnected-Unregistered-because-of-inactivity).



## Changes from 2023.11.4 to 2023.11.5

The bundled Git was updated to version 2.45.1 in both Server and Agent Docker images.


## Changes from 2023.11.3 to 2023.11.4

* The bundled Git was updated to version 2.43.2 in both Server and Agent Docker images for Linux and ARM. Windows images keep using version 2.43.0 as this is the latest currently available version of [Git-For-Windows](https://github.com/git-for-windows/git/releases/).

### Known Issues
{id="known-issues-2023-11-4"}

* Installing this bugfix or a stand-alone security patch causes older versions of multiple unbundled plugins to fail with the "403: Access Denied" responses. These issues have been fixed in newer versions of corresponding plugins or TeamCity server.
    * [GitHub Commit Hook](configuring-vcs-post-commit-hooks-for-teamcity.md#GitHub+and+GitHub+Enterprise) plugin — update your plugin to version [2023.11-157452](https://plugins.jetbrains.com/plugin/9179-github-commit-hooks/edit/versions/stable/498931) or newer. More information: [TW-86680](https://youtrack.jetbrains.com/issue/TW-86680/GitHub-Commit-Hooks-fail-in-TeamCity-2023.11.4).
    * [Static UI Extensions](https://plugins.jetbrains.com/plugin/21016-static-ui-extensions) — update your plugin to version 0.x.37 or newer. More information: [TW-86707](https://youtrack.jetbrains.com/issue/TW-86707/Static-UI-extensions-broken-after-patching-2023.11.3-upgrade-to-2023.11.4).
    * [SAML Authentication](https://plugins.jetbrains.com/plugin/12588-saml-authentication) — update your TeamCity server to version 2022.04.5 or newer.

## Changes from 2023.11.2 to 2023.11.3

No potential breaking changes.

See this article for the complete list of fixed issues: [](teamcity-2023-11-3-release-notes.md).

### Known Issues
{id="known-issues-2023-11-3"}

* TeamCity performance is decreased if the server cannot reach the jetbrains.com domain. See this YouTrack ticket for more information: [https://youtrack.jetbrains.com/issue/TW-86288](https://youtrack.jetbrains.com/issue/TW-86288). This issue is [addressed](teamcity-2023-11-4-release-notes.md) in the 2023.11.4 bug-fix date.
* Generating coverage reports using JaCoCo may fail with the `ClassNotFoundError`. To resolve this issue, upgrade to TeamCity 2023.11.4. Depending on whether your current JaCoCo coverage tool was installed pre- or post- 2023.11.x server update, you may also need to reinstall this tool and restart your build agents. See this YouTrack ticket for more information: [TW-86574](https://youtrack.jetbrains.com/issue/TW-86574/jacocoReport.xml-is-created-empty-due-to-ClassNotFoundError-if-non-bundled-Jacoco-is-used#post-fix-notes).


## Changes from 2023.11.1 to 2023.11.2

No potential breaking changes.

See this article for the complete list of fixed issues: [](teamcity-2023-11-2-release-notes.md).

### Known Issues
{id="known-issues-2023-11-2"}

* Generating coverage reports using JaCoCo may fail with the `ClassNotFoundError`. To resolve this issue, upgrade to TeamCity 2023.11.4. Depending on whether your current JaCoCo coverage tool was installed pre- or post- 2023.11.x server update, you may also need to reinstall this tool and restart your build agents. See this YouTrack ticket for more information: [TW-86574](https://youtrack.jetbrains.com/issue/TW-86574/jacocoReport.xml-is-created-empty-due-to-ClassNotFoundError-if-non-bundled-Jacoco-is-used#post-fix-notes).


## Changes from 2023.11 to 2023.11.1

* Previously, [reporting test metadata](reporting-test-metadata.md) with the `##teamcity[testMetadata testName='...' name='...' type='number' value='...']` service messages resulted in TeamCity showing a graph with milliseconds as the Y-axis units. This behavior was unexpected for users who passed non-DateTime values. In version 2023.11.1, the `type='number'` parameter formats the Y-axis of the graph as plain numbers. To continue viewing values as milliseconds, change the type to the new `ms` value. Other available values introduced in this version are `bytes` and `percent`.

### Bundled Tools Updates
{id="bundled-tools-updates-2023-11-1"}

* The bundled Tomcat was updated to version 9.0.83.

### Known Issues
{id="known-issues-2023-11-1"}

* Generating coverage reports using JaCoCo may fail with the `ClassNotFoundError`. To resolve this issue, upgrade to TeamCity 2023.11.4. Depending on whether your current JaCoCo coverage tool was installed pre- or post- 2023.11.x server update, you may also need to reinstall this tool and restart your build agents. See this YouTrack ticket for more information: [TW-86574](https://youtrack.jetbrains.com/issue/TW-86574/jacocoReport.xml-is-created-empty-due-to-ClassNotFoundError-if-non-bundled-Jacoco-is-used#post-fix-notes).

## Changes from 2023.05 to 2023.11

### Bundled Tools Updates
{id="bundled-tools-updates-2023-11"}

* .NET Core 3.1 is no longer bundled with the TeamCity Agent Docker images.
* The version of .NET SDK bundled with the TeamCity Agent Docker images has been updated from 5.0 to 6.0 (the current [Microsoft LTS](https://dotnet.microsoft.com/en-us/platform/support/policy/dotnet-core) version).
* Starting with this release, the .NET SDK version bundled with the TeamCity Agent Docker images is aligned with the Microsoft LTS version current at the time of the TeamCity release.
  > If you need an earlier version of .NET SDK (for example, .NET Core 3.1 or .NET SDK 5.0) or a later version (for example, .NET SDK 7.0), we recommend that you build your own Docker image using the provided TeamCity Minimal Agent Docker base image (`jetbrains/teamcity-minimal-agent`).
  > See the [README](https://github.com/JetBrains/teamcity-docker-images/tree/master/custom#readme) for custom agent images in the [`teamcity-docker-images`](https://github.com/JetBrains/teamcity-docker-images) repository for more details.
  > 
  {style="note"}

* The bundled Tomcat was updated to version 9.0.80.
* The bundled Git was updated to version 2.43 in both Server and Agent Docker images.
* Following the [initial end-of-life announcement](https://learn.microsoft.com/en-us/lifecycle/announcements/windows-10-version-2004-end-of-servicing) by Microsoft, TeamCity server and agent Docker images will no longer use Windows 10 version 2004. Starting with the 2023.11 release, we will publish images based on Windows Server 2022 images instead. Older Docker images that were already published (for example, `jetbrains/teamcity-minimal-agent:2023.05.4-nanoserver-2004`) will be neither upgraded nor removed.
* The bundled dotCover tool has been updated to version 2023.2.2.
* To reduce the size of TeamCity distributions, the largest TeamCity build tool, IntelliJ IDEA, no longer ships with TeamCity installers. Instead, TeamCity will download and install this tool on the first server startup. To check the download/install progress (or to manually install [the required version of IntelliJ IDEA](https://www.jetbrains.com/intellij-repository/releases/com/jetbrains/intellij/idea/ideaIU/2022.1.3/ideaIU-2022.1.3.zip) on server instances that failed to do so automatically), navigate to the **Administration | Tools** page and scroll to the **IntelliJ Inspections and Duplicates Engine** section.
* The bundled Kotlin compiler (used in TeamCity DSL) and Dokka (the documentation engine for Kotlin) was updated to version 1.8.22.
* The bundled ReSharper CLT was updated to version 2023.1.1. This version does not include the dupFinder Command-Line Tool, which deprecates the TeamCity [](duplicates-finder-resharper.md) runner (see the [initial deprecation announcement](#Upcoming+DupFinder+Runner+Deprecation)). To continue using this runner, install [JetBrains ReSharper Command Line Tools 2021.2.3](https://www.jetbrains.com/resharper/download/other.html) or older and specify a path to this tool in the runner's advanced settings (the **R# CLT Home Directory** field).

### S3 Plugin Updates
{id="2023-11-s3-update"}

Due to the [S3 Plugin overhaul](storing-build-artifacts-in-amazon-s3.md), the following settings are no longer available:

* The **Use pre-signed URLs** feature is available by default and cannot be disabled.
* The **Access Key ID**, **Secret Access Key**, **IAM Role** and **Default provider chain** options are no longer available for native AWS S3 storages. Instead, use settings of an [AWS Connection](configuring-connections.md#AmazonWebServices) these storages utilize to edit corresponding options. When you view or edit an existing S3 bucket that employed any of these settings, TeamCity shows the **Convert to AWS Connection** link that allows you to transfer them to a new AWS Connection. We recommend that you do so to keep all connection-related options outside storage settings.
* The **AWS Region** is now automatically retrieved from the selected storage.
* The **Open IAM Console** link is hidden.
* Existing storages with custom endpoints and enabled **Default Credential Provider Chain** option are now explicitly converted to the "Custom S3" type.

### EC2 Plugin Updates

The [Amazon EC2 plugin](setting-up-teamcity-for-amazon-ec2.md) was significantly reworked in version 2023.11. As a part of this overhaul, it is no longer possible to [push TeamCity agents](install-teamcity-agent.md#Install+via+Agent+Push) to EC2 instances spawned from AWS Cloud Image. As an alternative, use EC2 images that already include TeamCity agents.

In version 2023.11.2, we expect to rollback this change for cloud profiles configured before the 2023.11 update. This will allow you to continue using the agent push functionality for the existing cloud agents. However, we encourage you to update your setup and bake TeamCity agents into your AMIs instead of installing them via the agent push. The latter option will be completely disabled in one of the future releases.

**2023.11.4 Update:** Agent pushes are temporarily re-enabled for all cloud profiles, both those configured prior to version 2023.11 and new ones.

### TeamCity Plugin for IntelliJ Platform Updates

* From TeamCity 2023.11 onwards, when two-factor authentication (2FA) is enabled on the TeamCity server, it is no longer possible to log in to TeamCity from the IntelliJ Platform using username/password credentials. You can log in with username/access token credentials instead.

### HTTP / SSO Authentication Modules Updates

The following updates have been made to the Azure DevOps OAuth 2.0, Bitbucket Cloud, GitHub App, GitHub Enterprise, GitHub.com, GitLab CE/EE, and GitLab.com [authentication modules](https://www.jetbrains.com/help/teamcity/configuring-authentication-settings.html#HTTP+%2F+SSO+Authentication+Modules):

* The **Add Module** dialog has a new **Allow any &lt;IdentityProvider> user to log in** checkbox. If you want to configure a module that does not restrict users to a particular domain, organization, workspace, or group, you must check this box instead of leaving the **Restrict authentication** field empty.
* If an existing authentication module in version 2023.05.04 is configured with an empty **Restrict authentication** field, after migrating to version 2023.11, TeamCity displays the warning notification `All users should be allowed to login, or at least one <IdentityProvider> organization must be specified.` on the **Administration | Authentication** page.

### Known Issues
{id="known-issues-2023-11"}

* At TeamCity, we are fully dedicated to bolstering the comprehensive security of our platform, and we consistently enhance our product to realize this commitment.

    In version 2023.11, we have fixed certain issues related to the [artifacts' domain isolation](teamcity-configuration-and-maintenance.md#artifacts-domain-isolation) feature. As a side effect of these changes, some users can experience infinite redirect loops (`ERR_TOO_MANY_REDIRECTS`) when attempting to access build artifacts. To fix this issue, make sure your proxy server provides valid `X-Forwarded-Host` headers (see the [](configuring-proxy-server.md) article for the configuration examples).

    You can also roll back these changes by adding the `teamcity.internal.domainIsolation.serveArtifactsOnlyFromArtifactsUrl=false` [internal property](server-startup-properties.md#TeamCity+Internal+Properties). Be advised that the internal property disables the aforementioned security update, thus lowers the TeamCity server security.

* If your TeamCity username includes encoded special symbols (for example, emoji), you may be unable to log in to TeamCity via the [](intellij-platform-plugin.md). See the following ticket for more information: [TW-85284](https://youtrack.jetbrains.com/issue/TW-85284/Unable-to-log-in-from-the-IntelliJ-IDEA-TeamCity-plugin).

* *(fixed in the 2023.11.1 bugfix update)* TeamCity may be unable to run new builds for Subversion repositories accessed over the secure SSH protocol (SVN+SSH). See this issue for more information: [TW-85310](https://youtrack.jetbrains.com/issue/TW-85310).

* *(fixed in the 2023.11.1 bugfix update)* LDAP synchronization currently fetches first 1000 users. As a workaround, set the `teamcity.ldap.search.pageSize` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) to a larger value. See this YouTrack ticket for the resolution progress: [TW-85444](https://youtrack.jetbrains.com/issue/TW-85444/LDAP-sync-retrieves-only-1000-users).

* *(fixed in the 2023.11.1 bugfix update)* [](commit-status-publisher.md) cannot publish build statuses to Bitbucket Cloud if the parent TeamCity build configuration has an ID longer than 40 characters. See this issue for more information: [TW-85393](https://youtrack.jetbrains.com/issue/TW-85393). 




## Changes from 2023.05.4 to 2023.05.6

Tooling updates in Server and Agent Docker imaages:

* Git and Git for Windows were updated to version 2.45.1.
* Perforce was updated to version 2022.2-2531894.

### Known Issues
{id="known-issues-2023-05-5"}

* Requests sent by the [GitHub Commit Hook](configuring-vcs-post-commit-hooks-for-teamcity.md) plugin fail with the "403: Access Denied" error. Update your plugin to [version 2022.04-109057](https://plugins.jetbrains.com/plugin/9179-github-commit-hooks/versions/stable/544827) to resolve this issue. See this YouTrack ticket for more information: [TW-86680](https://youtrack.jetbrains.com/issue/TW-86680).
* The **Administration** page is not accessible from TeamCity UI if the plain-text [LDAP connection](ldap-integration.md) URL (`ldap://...`) is used. To resolve this issue, switch to a secure LDAPS connection (recommended) or add an internal property as suggested in the following ticket: [TW-88069](https://youtrack.jetbrains.com/issue/TW-88069).

## Changes from 2023.05.3 to 2023.05.4

No potential breaking changes.

See this article for the complete list of fixed issues: [](teamcity-2023-05-4-release-notes.md).


## Changes from 2023.05.2 to 2023.05.3

No potential breaking changes.

See this article for the complete list of fixed issues: [](teamcity-2023-05-3-release-notes.md).

### Bundled Tools Updates
{id="bundled-tools-updates-2023-5-3"}

* The bundled Git was updated to version 2.42 in both Server and Agent Docker images.


## Changes from 2023.05.1 to 2023.05.2

See this article for the complete list of fixed issues: [](teamcity-2023-05-2-release-notes.md).

### Upcoming DupFinder Runner Deprecation

Starting with the next TeamCity version, the [](duplicates-finder-resharper.md) runner will be unable to operate since it relies on a tool that is no longer shipped with ReSharper Command Line Tools. See the corresponding [server health report](server-health.md#Duplicates+Finder+Runner) for more information.

### Known Issues

* Builds that pull TFS repositories fail with the `java.lang.NoClassDefFoundError` message if the checkout mode is "Always checkout files on agent". See this YouTrack issue for more information: [TW-82824](https://youtrack.jetbrains.com/issue/TW-82824).

## Changes from 2023.05 to 2023.05.1

### Publishing Artifacts of Batch Builds in Parent Configuration Builds

With this bugfix update, automatically created [batch builds](parallel-tests.md) aggregate their artifacts under the **Artifacts** tab of parent configuration builds. See this article for more information: [](parallel-tests.md#Publish+Artifacts+Produced+By+Batch+Builds).

### Bundled Tools Updates
{id="bundled-tools-updates-2023-5-1"}

* The bundled Git was updated to version 2.41 in both Server and Agent Docker images.

### Miscellaneous Changes

* The *"Open an interactive session to the agent"* [permission](managing-roles-and-permissions.md) was renamed to *"Invoke interactive agent terminals"*. The new name highlights the recent behavior change: this permission now specifies whether users can open [agent terminal tabs](install-and-start-teamcity-agents.md#Debug+Agents+Remotely) introduced in version 2023.05 and is no longer related to deprecated SSM terminals.
* You can now decorate artifact publishing rules with the `#teamcity:symbolicLinks=...` attribute to choose whether symlinks present in published directories should be included as is, or TeamCity should include files and folders referenced by these symlinks in the published archive. See this article for more information: [](configuring-general-settings.md#Publishing+Symlinks).





## Changes from 2022.10 to 2023.05

### Planned deprecation of Java 8 in TeamCity Server

TeamCity 2023.05 supports Java versions 8, 11 and 17, but __Java 8 support will be discontinued in one the next TeamCity versions__. If you use a non-bundled version of Java 8, we highly recommend that you migrate your server to Java 11 or 17.


### Bundled Tools Updates
{id="bundled-tools-updates-2023-5"}

* The bundled Kotlin compiler (used in [TeamCity DSL](kotlin-dsl.md)) and Dokka (the documentation engine for Kotlin) were updated to version 1.7.10.
* The bundled Tomcat was updated to version 9.0.75.
* Amazon Corretto Java bundled with TeamCity Windows installer and TeamCity Docker images was updated to version 17.0.7.7.1.

### REST API Update

The Web Application Description Language (WADL) generator is now removed. See the [initial announcement](#Upcoming+REST+API+Updates) for more information.

### Multinode Setup Updates

* The "Processing user requests to modify data" responsibility was renamed to "Handling UI actions and load balancing user requests".
* The `[data_directory](teamcity-data-directory.md)/config/nodes-config.xml` file listed only "MAIN_NODE" responsibility for main nodes. In version 2023.05, this configuration file lists all responsibilities enabled on a main node.

### Podman Support

Due to the implementation of Podman support, the following changes were made:

* The "Docker Wrapper" extension was renamed to [](container-wrapper.md).
* The "Docker Info" tab on the [](build-results-page.md) was renamed to "Container Info".
* Adding the [](container-wrapper.md) build feature to a build configuration no longer applies the `docker.server.version exists` agent requirement. Instead, TeamCity now defines the `docker.server.osType exists` condition. This property is synchronized with `podman.osType` so that agents with Podman installed instead of Docker are compatible with this new requirement.


### TeamCity Metrics Updates

Starting with version 2023.05, [TeamCity metrics](teamcity-monitoring-and-diagnostics.md#Metrics) accessible via the `<TeamCity_server_URL>/app/metrics` endpoint comply with the [OpenMetrics](https://github.com/OpenObservability/OpenMetrics/blob/main/specification/OpenMetrics.md) specification. Due to this enhancement, the following changes were implemented:

* "summary" metrics changed their suffixes from `_total` to `_sum`. For example, TeamCity now reports `build_queue_optimization_time_milliseconds_sum` instead of `build_queue_optimization_time_milliseconds_total`.
* metrics that previously had the `_number` suffix no longer have it. For example, the `agents_connected_authorized_number` metric is now called `agents_connected_authorized`.

To preserve previously collected metrics and use them along with updated data, do one of the following in your metric monitoring solution (such as [Grafana](https://grafana.com)):

* (recommended) Use the `or` operator in graph settings to merge metrics with new and old names. For instance:
  ```
  sum(increase(vcs_changes_checking_milliseconds_sum{type="COLLECT_CHANGES"}[1m])) or 
  sum(increase(vcs_changes_checking_milliseconds_total{type="COLLECT_CHANGES"}[1m])) 
  ```
* Use the Prometheus [relabeling configuration](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#relabel_config) to rename metrics back to their old names before they are written to a Prometheus database.

In addition to these changes, TeamCity no longer reports the "experimental" tag for metrics. Note that some metrics are still considered experimental and accessible via the `<TeamCity_server_URL>/app/metrics?experimental=true` endpoint.

### Miscellaneous Updates

* Users with the "Project Developer" [role](managing-roles-and-permissions.md) can now download and view the `.teamcity/settings/buildSettings.xml` [hidden artifact](build-artifact.md#Hidden+Artifacts). Previously, this action required the "Edit project" permission that is enabled for "Project Administrator" and higher roles.
* Agent pages no longer display the **Open SSM Terminal** action link. This functionality was deprecated in favor of more generic **Open Terminal** button. See [](install-and-start-teamcity-agents.md#Debug+Agents+Remotely) for more details.
* Configurations with [agent-side checkout](vcs-checkout-mode.md) mode do not support postfixes in checkout directory paths (for instance, `+:src/main => src/main/postfixDirectory`). If you specified a postfix in checkout rules, previous TeamCity versions silently swallowed this error and ran builds that ignored your postfixes. Starting with version 2023.05, TeamCity shows the corresponding error message and does not allow new builds to start. See this section for more information: [](git.md#Limitations).


### Known Issues
{id="known-issues-2023-5"}

* TeamCity shows the "Docker rate limit warning" for build agents that use rootful Podman (that is, containers run as root on agent machines) to [pull containers](container-wrapper.md), even if the Podman client successfully authorizes before pulling an image. For agents using Docker, this warning is showed only if agents pull images without authentication, which may cause reaching the [download rate limit](integrating-teamcity-with-container-managers.md#Conforming+with+Docker+Download+Rate+Limits).
* If you have multiple projects with [GitHub App connections](configuring-connections.md#GitHub) to the same GitHub App, a [webhook](configuring-vcs-post-commit-hooks-for-teamcity.md) for only the first connection detected by TeamCity is functional. Projects with other connections keep polling their corresponding repositories for changes.
* Uploading artifacts to [S3 buckets](storing-build-artifacts-in-amazon-s3.md) may fail for larger files. We expect to fix this issue in the next bugfix update (2023.05.1). In the meantime, download and [install](installing-additional-plugins.md#Installing+Plugin+Manually) a custom build of the S3 Plugin from this YouTrack issue: [TW-81866](https://youtrack.jetbrains.com/issue/TW-81866/Failed-to-publish-artifacts-in-AWS-s3-after-updating-TeamCity-to-v2023.05).
* Settings of EC2-based [cloud profiles](setting-up-teamcity-for-amazon-ec2.md) may not show the checkbox that allows you to utilize locally stored IAM Roles, leaving authorization by Access ID/Secret Key as the only option. We expect to fix this issue in the next 2023.05.1 bugfix update.
* If a directory published as a build artifacts contains symbolic links, files and folderes referenced by these symlinks are no longer included in the produced artifact archive. This issue will be resolved in the 2023.05.1 bugfix update, see this article for more information: [](configuring-general-settings.md#Publishing+Symlinks).
* Some TeamCity pages are missing their `html` and `body` tags. See this ticket for more information: [TW-82749](https://youtrack.jetbrains.com/issue/TW-82749).
* Agents spawned from AWS machine images that utilize the first version of Amazon Metadata (IMDSv1) fail to retrieve property values from metadata and pass automatic [authorization](configure-agent-installation.md). See this ticket for more information: [TW-82176](https://youtrack.jetbrains.com/issue/TW-82176).


## Changes from 2022.10.4 to 2022.10.6

Tooling updates in Server and Agent Docker imaages:

* Git and Git for Windows were updated to version 2.45.1.
* Perforce was updated to version 2022.2-2531894.

### Known Issues
{id="known-issues-2022-10-6"}

* Requests sent by the [GitHub Commit Hook](configuring-vcs-post-commit-hooks-for-teamcity.md) plugin fail with the "403: Access Denied" error. Update your plugin to [version 2022.04-109057](https://plugins.jetbrains.com/plugin/9179-github-commit-hooks/versions/stable/544827) to resolve this issue. See this YouTrack ticket for more information: [TW-86680](https://youtrack.jetbrains.com/issue/TW-86680).
* The **Administration** page is not accessible from TeamCity UI if the plain-text [LDAP connection](ldap-integration.md) URL (`ldap://...`) is used. To resolve this issue, switch to a secure LDAPS connection (recommended) or add an internal property as suggested in the following ticket: [TW-88069](https://youtrack.jetbrains.com/issue/TW-88069).

## Changes from 2022.10.3 to 2022.10.4

No potential breaking changes.

## Changes from 2022.10.2 to 2022.10.3

### Bundled Tools Updates
{id="bundled-tools-updates-2022-10-3"}

* The bundled Git was updated to version 2.40 in both Server and Agent Docker images.
* The bundled Tomcat was updated to version 9.0.71.
* The Perforce Helix Core client (p4) was updated to version 2022.2-2407422 in Agent and Server Docker images.

### Known Issues
{id="known-issues-2022-10-3"}

Using the "Default Credentials Provider" as a principal AWS connection may cause the "Default Credentials Provider Chain: The security token included in the request is expired" error when the session expires. This issue is already fixed in the latest AWS Core plugin that will be bundled with TeamCity in the upcoming 2022.10.4 bugfix version. To manually install this plugin version, download the corresponding attachment from the [TW-80253](https://youtrack.jetbrains.com/issue/TW-80253/) ticket.

## Changes from 2022.10.1 to 2022.10.2

### Bundled Tools Updates
{id="bundled-tools-updates-2022-10-2"}

* The bundled Git was updated to version 2.39.1 in both Server and Agent Docker images.
* The Perforce Helix Core client (p4) was updated to version 2022.2-2369846 in Agent Docker images.
* The bundled Apache Tomcat was updated to version 8.5.84.


### Upcoming REST API Updates

The Web Application Description Language (WADL) generator will be removed in version 2023.05 since we now utilize Swagger to generate documentation [REST API](teamcity-rest-api.md) and client code.

If you rely on this generator tool, [contact us](feedback.md) to share your business requirements.

## Changes from 2022.10 to 2022.10.1

### AWS Connection

#### Default Provider Chain credentials type is disabled by default

The **[Default Provider Chain](configuring-connections.md#AmazonWebServices)** credentials type in AWS connections is now disabled by default to prevent [associated security risks](upgrade-notes.md#known-issues-202210).
To enable this option, set [the internal property](server-startup-properties.md#TeamCity+Internal+Properties) `teamcity.internal.aws.connection.defaultCredentialsProviderEnabled=true` (The default value is `false`.)
No server restart is required after the property is set.

#### Custom STS endpoint is disabled by default

Only [the global or regional AWS STS endpoints](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_enable-regions.html)
can be used as STS Endpoints in [the AWS connection configuration](configuring-connections.md#AmazonWebServices).
To use a custom endpoint for Amazon alternatives like [MinIO](https://min.io/), [contact the TeamCity support team](https://teamcity-support.jetbrains.com/hc/en-us/requests/new?).



<!--<include from="upgrade-notes-older-versions.md" element-id="older-upgrade-notes" />-->


## Changes from 2022.04 to 2022.10


### Planned deprecation of Java 8 in TeamCity Server 2023.04

TeamCity 2022.10 Server supports Java versions 8 and 11, but __Java 8 support will be discontinued in the next version, TeamCity 2023.04__, to be released in April 2023. If you use a non-bundled version of Java 8, we highly recommend that you migrate your server to Java 11 before TeamCity 2023.04.

Note that TeamCity is not compatible with Java 17, which makes Java 11 the only version planned for support in TeamCity Server 2023.04.

### Bundled tools updates
{id="bundled-tools-updates-2022-10"}

* The bundled Amazon Corretto Java has been updated to version 11.0.16.9.1.
* The bundled Tomcat has been updated to version 8.5.82.
* The bundled Kotlin compiler in the [Kotlin Script runner](kotlin-script.md) has been updated to version 1.7.10.


> Kotlin compiler 1.7.10 changes its behavior in respect to the`@file:Repository` annotations supported by the `.main.kts` extension
(used in build steps with custom scripts by default). In Kotlin 1.5, when such annotations were used in the script
and a dependency was not found in any of the mentioned repositories, the dependency resolver would also look in the Maven Central
even if it was not mentioned explicitly.
In Kotlin 1.7 this behavior changes. Maven Central is only checked if no `@file:Repository` annotations are used in the script.
If you use such annotations in your code, you’ll need to mention Maven Central explicitly
if you want the dependency resolver to look in it.
>
{style="note"}
* Maven 3.8.6 has been added as one of the bundled versions of the tool.
* The embedded Maven library has been updated to version 3.8.6.
* JDBC drivers for external databases suggested on the fresh TeamCity installation have been updated to the following versions:
    * MySQL to 8.0.30
    * PSQL to 42.5.0
    * MSSQL to 9.4.1


### Other updates
{id="other-updates-2022-10"}

#### Permission to connect to an agent's EC2 instance via AWS SSM

TeamCity system administrators are now granted the new role, _Open an interactive session to the agent_,
which lets them use an interactive browser-based shell on an EC2 agent from the TeamCity UI without providing Amazon credentials.
It is possible to connect to agents if they are configured as described [here](setting-up-teamcity-for-amazon-ec2.md).


#### Free disk space for artifacts is calculated automatically

The **Free disk space** build feature keeps track of the size of artifacts and automatically calculates the disk space required for resolving artifact dependencies.
You do not have to take into account the size of the artifacts downloaded during the build when specifying the required disk space.

#### Backward compatibility for Bitbucket Server pull request branches

TeamCity provides backward compatibility with Bitbucket Server pull request branches
that are [not officially supported by Atlassian](https://community.atlassian.com/t5/Bitbucket-questions/Current-Atlassian-position-regarding-refs-pull-requests-from/qaq-p/1376356#M54578).
The [Pull Requests](pull-requests.md) build feature has the **Use pull request branches** option that enables detection of such branches `(pull-requests/*)` instead of source branches.
After the upgrade, this option will be enabled for existing build configurations using such branches. We do not recommend using this option.

#### Performance Monitor

The [Performance Monitor](performance-monitor.md) build feature is now enabled by default for [build configurations created from a URL](creating-and-editing-build-configurations.md#Creating+Build+Configuration+from+URL).

### Known issues
{id="known-issues-202210"}

#### Security risks of AWS connection

If your TeamCity server is hosted on an AWS instance that has an associated IAM role granting access to sensitive resources,
using an [Amazon Web Services (AWS) connection](configuring-connections.md#AmazonWebServices)
with the **Default Provider Chain** credentials may present a security risk.
In this case TeamCity project administrators who configured this type of connection can access all AWS resources permitted by the role.

To work around this security issue, we **strongly recommend that TeamCity server administrators disable the use of the
Default Provider Chain** credentials type in AWS connections
by setting the [internal property](server-startup-properties.md#TeamCity+Internal+Properties)`teamcity.internal.aws.connection.defaultCredentialsProviderEnabled=false` (The default value is `true`.)
No server restart is required after the property is set.

We will disable this type of credentials by default in the next bugfix update.

#### Kotlin DSL plugin may fail to resolve dependencies

The Kotlin DSL plugin may fail [to resolve DSL dependencies](https://youtrack.jetbrains.com/issue/TW-78351/Kotlin-DSL-fails-to-generateget-DSL-dependencies-since-upgrading-to-202210) after upgrading to 2022.10,
if a project's Kotlin DSL settings use third-party libraries.

If you face this problem, upgrade to the bug-fix version 2022.10.1 that ships with an updated version of the DSL plugin.

## Changes from 2022.04.5 to 2022.04.7

Tooling updates in Server and Agent Docker imaages:

* Git and Git for Windows were updated to version 2.45.1.
* Perforce was updated to version 2022.2-2531894.

### Known Issues
{id="known-issues-2022-04-6"}

* Requests sent by the [GitHub Commit Hook](configuring-vcs-post-commit-hooks-for-teamcity.md) plugin fail with the "403: Access Denied" error. Update your plugin to [version 2022.04-109057](https://plugins.jetbrains.com/plugin/9179-github-commit-hooks/versions/stable/544827) to resolve this issue. See this YouTrack ticket for more information: [TW-86680](https://youtrack.jetbrains.com/issue/TW-86680).
* The **Administration** page is not accessible from TeamCity UI if the plain-text [LDAP connection](ldap-integration.md) URL (`ldap://...`) is used. To resolve this issue, switch to a secure LDAPS connection (recommended) or add an internal property as suggested in the following ticket: [TW-88069](https://youtrack.jetbrains.com/issue/TW-88069).

## Changes from 2022.04.4 to 2022.04.5

No potential breaking changes.

## Changes from 2022.04.3 to 2022.04.4

* The `ReservedCodeCacheSize=640m` attribute is set by default for new server installations.
  If the attribute was specified in an earlier TeamCity version, you'll have to update it manually after upgrading.
  See the [TW-76238](https://youtrack.jetbrains.com/issue/TW-76238/High-CPU-usage-if-code-cache-is-filled-in) issue.
* SVNKit has been updated to 1.10.8.

## Changes from 2022.04.2 to 2022.04.3

* SVNKit was updated to 1.10.7, which resulted in a problem with svn+ssh roots.
  It did not close connections and generated many threads. The problem was resolved in version 2022.04.4. To fix the issue in TeamCity 2022.04.3, download a plugin from the [TW-77134](https://youtrack.jetbrains.com/issue/TW-77134/svnssh-connections-on-the-TeamCity-server-may-generate-threads-overtime) issue.

## Changes from 2022.04.1 to 2022.04.2

No potential breaking changes.

## Changes from 2022.04 to 2022.04.1

No potential breaking changes.

### Known issues
{id="known-issues-2022041"}

* There is [a new performance problem](https://youtrack.jetbrains.com/issue/TW-76397) in the Git plugin which slows down checking for changes operation for some of the Git repositories. For the issue to reproduce, a VCS root should have "Enable to use tags in the branch specification" option enabled and `+:refs/tags/*` should be specified in the VCS root branch specification. The repository should also have many thousands of tags. Please see [TW-76397](https://youtrack.jetbrains.com/issue/TW-76397) for a workaround.

## Changes from 2021.2 to 2022.04

* To comply with the common identifier format of .NET tests, TeamCity now uses a different format of names for .NET assemblies (omitting a file extension).
  On updating to 2022.04, this format will be applied within all the tests launched via the `test` or `vstest` command of the [.NET](net.md) runner.
  As a result of this change, the investigations, mutes, and history of these tests may be reset.
* TeamCity stops supporting the [Microsoft Edge Legacy](https://techcommunity.microsoft.com/t5/microsoft-365-blog/new-microsoft-edge-to-replace-microsoft-edge-legacy-with-april-s/ba-p/2114224) web browsers.
* [Triggering builds via REST API](https://www.jetbrains.com/help/teamcity/rest/start-and-cancel-builds.html#Advanced+Build+Run) will be disabled
  when the [queue limit](https://www.jetbrains.com/help/teamcity/2021.12/ordering-build-queue.html#Limiting+Maximum+Size+of+Build+Queue) is reached on the server.
* TeamCity reporting of Ant's tasks will be disabled if Ant is started by a Java version below 1.8.
* Windows docker images based on 2004 will not be published for 2022.04 version.
* Replaced log4j v1.2 with log4j v2.17 (see [TW-47084](https://youtrack.jetbrains.com/issue/TW-47084)).

### External plugins updates

Some popular external plugins are not compatible with TeamCity 2022.04 and have to be updated before the upgrade.

Download newer versions of these plugins from JetBrains Marketplace:
* [GitHub Commit Hooks](https://plugins.jetbrains.com/plugin/9179-github-commit-hooks)
* [Hashicorp Vault Support](https://plugins.jetbrains.com/plugin/10011-hashicorp-vault-support)
* [Gradle Build Scan](https://plugins.jetbrains.com/plugin/9326-integration-for-gradle-and-maven-build-scans)


### Bundled tools updates
{id="bundled-tools-updates-2022-04"}

* Versions 2017.1 and 2017.2 of [TeamCity REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html) have been unbundled.
  If you have been using any of these versions in your scripts, consider switching to the latest protocol version as described [here](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html#REST+API+Versions). If switching is not an option and this is a breaking change for your setup, please contact us via any convenient [feedback channel](feedback.md).
* Updates in TeamCity Agent Docker images:
    * The bundled .NET Core SDK has been updated to 6.0.100.
    * The two bundled versions of .NET Core Runtime are 3.1.21 and 5.0.12.
    * Bundled Git has been updated to version 2.36.0
    * The bundled Java was updated to version 11.0.15.9.1
* Bundled IntelliJ IDEA has been updated to version 2021.2.3. Note that this version requires Java 11.x. Previously added IntelliJ Inspections/Duplicates steps with the bundled version will become incompatible with the agents running Java below version 11.
* The bundled Kotlin compiler used in [TeamCity DSL](kotlin-dsl.md) has been updated to version 1.6.21
* The [SBT](https://www.scala-sbt.org/) launcher, used in the [Simple Build Tool (Scala)](simple-build-tool-scala.md) plugin, has been updated to version 1.5.5.
* Freemarker, used by TeamCity [notification templates](customizing-notification-templates.md), has been updated to version 2.3.31.
* The [Qodana plugin](https://www.jetbrains.com/help/qodana/qodana-teamcity-plugin.html) has been bundled with TeamCity. If you previously installed the Qodana plugin and used DSL, you'll need to update your DSL settings. We're providing a special version of the plugin that contains both [old and new Kotlin DSL settings](https://plugins.jetbrains.com/plugin/15498-qodana/versions/stable/169313).
  All deprecated settings are marked and alternatives are provided. After the migration, you can delete this plugin and use the version bundled with TeamCity.

* The [CVS plugin](cvs.md) has been unbundled from TeamCity. If you want to continue using it on your server, please [download it from JetBrains Marketplace](https://plugins.jetbrains.com/plugin/18552-vcs-support-cvs) and install it as described [here](installing-additional-plugins.md).
* The Eclipse plugin has been unbundled from TeamCity. [Contact our support](https://teamcity-support.jetbrains.com/hc/en-us) if you need the plugin.

## Changes from 2021.2.2 to 2021.2.3

* To avoid false positive reports from some security scanners, TeamCity now uses an instance of the Log4j 1.2 library without vulnerable classes. To achieve this, we've created [our own fork of Log4j 1.2](https://github.com/JetBrains/teamcity-log4j) on GitHub, removed vulnerable packages unused by TeamCity (`net`, `chainsaw`, `jdbc`, and `jmx`), and built the library.

### Known Issues
{id="known-issues-202123"}

* After upgrading to 2021.2.3, builds might fail to check out sources from git repositories on Azure DevOps (both Server and Services) via SSH. See the [related issue](https://youtrack.jetbrains.com/issue/TW-75102) for details.  
  If you face this problem, please apply a workaround from the [issue](https://youtrack.jetbrains.com/issue/TW-73759#focus=Comments-27-5347961.0-0).
* Builds might fail to publish artifacts or a build number via commands of an MSBuild script. This problem can be worked around by changing the build log verbosity level to _normal_: to do this, set the `msbuild.logger.params` [configuration parameter](configuring-build-parameters.md) to `verbosity=normal`. Alternatively, you can download the fixed .NET plugin [here](https://youtrack.jetbrains.com/issue/TW-75259#focus=Comments-27-5837867.0-0) and [install it manually](installing-additional-plugins.md) on your server.

## Changes from 2021.2.1 to 2021.2.2

* __Changed format for .NET assembly names__  
  To comply with the common identifier format of .NET tests, TeamCity now uses a different format of names for .NET assemblies (omitting a file extension). On updating to 2021.2.2, this format will be applied within all the tests launched via the `test` or `vstest` command of the [.NET](net.md) runner, but the investigations and history of these tests might be reset. The details on the changes to the common identifier format in .NET can be found in the [Microsoft Documentation](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.assemblyname.name?view=net-6.0).

### Bundled tools updates
{id="bundled-tools-updates-202122"}

* Updates in TeamCity Agent Docker images:
    * The bundled version of .NET Core SDK has been updated to 6.0.100.
    * Bundled two versions of .NET Core Runtime: 3.1.21 and 5.0.12.

## Changes from 2021.2 to 2021.2.1

* To comply with the common identifier format of .NET tests, TeamCity now uses a different format of names for .NET assemblies (omitting a file extension). After updating to 2021.2.1, this format will be applied within all the tests launched via the `test` or `vstest` command of the [.NET](net.md) runner, but the investigations and history of these tests might be reset.
* If parametrized .NET tests are launched with the `test` command of the [.NET](net.md) runner, TeamCity will show them as a single test with multiple runs, while previously it counted them separately and displayed the parameters' values per test in the __Tests__ tab.  
  To revert to the previous behavior, please download the [fixed version of our .NET plugin](https://youtrack.jetbrains.com/issue/TW-74176#focus=Comments-27-5585620.0-0) and install it as described [here](installing-additional-plugins.md).

### Bundled tools updates
{id="bundled-tools-updates-202121"}

* The Perforce Helix Core client (p4) is updated to version 2021.2/2201121.

## Changes from 2021.1 to 2021.2

### No data converters in 2021.2
{instance="tc"}

TeamCity 2021.2 does not introduce any new data formats compared to version 2021.1 and does not contain data converters. This simplifies and thus speeds up the upgrade/downgrade between these versions.

### 2021.2 Known Issues

* __Can't use remote run from ReSharper and Eclipse with enabled 2FA__  
  Users who have configured [two-factor authentication](configuring-your-user-profile.md#Configuring+Two-Factor+Authentication) for their TeamCity accounts, temporarily cannot [run and debug TeamCity builds remotely](remote-run.md) from ReSharper and Eclipse. If using the remote run from these tools is crucial to your pipelines, make sure 2FA is set to _Optional_ on your servers (default option), so users can disable it for their own accounts anytime.
* __.NET builds may fail due to enabled deterministic source paths__  
  Builds with [.NET steps](net.md) may fail with the "_SourceRoot items must include at least one top-level (not nested) item when DeterministicSourcePaths is true_" error. This error is caused by the conflict between the expected paths' format ([deterministic](https://devblogs.microsoft.com/dotnet/producing-packages-with-source-link/#deterministic-builds)) and the approach used in your project (likely, absolute paths to sources).    
  As a workaround, consider adding the `/p:ContinuousIntegrationBuild=false` command-line argument to disable deterministic source paths, or download and install the fixed .NET runner as described [here](https://youtrack.jetbrains.com/issue/TW-73746#focus=Comments-27-5341660.0-0).
* __.NET builds fail if .NET version is \< 6.0 and a path to a build agent contains whitespaces__  
  If you run a build with a [.NET step](net.md) on a build agent that has whitespaces in its OS path __and__ the used .NET version is earlier than 6.0, the build will fail with the "_Only one project can be specified_" error.  
  As a workaround, consider switching to .NET 6.0, or download and install the fixed .NET runner as described [here](https://youtrack.jetbrains.com/issue/TW-73745#focus=Comments-27-5341673.0-0).
* __Invalid property errors when passing .NET parameters containing special characters__  
  If running .NET commands via the [.NET](net.md) runner results in the "_Property is not valid_" error, you may need to adjust the system parameters being passed to it. Since version 2021.2, the .NET runner no longer escapes special characters in parameters. This new approach allows passing lists of parameters, but might cause errors if your existing parameters contain such special characters.  
  To resolve this issue, please revise and adjust the parameters being passed — make sure to [escape](https://docs.microsoft.com/en-us/visualstudio/msbuild/how-to-escape-special-characters-in-msbuild?view=vs-2022#to-use-an-msbuild-special-character-as-a-literal-character) all the occurring [special characters](https://docs.microsoft.com/en-us/visualstudio/msbuild/msbuild-special-characters) in them.
* __Microsoft Azure agents fail to automatically start/stop__  
  Build agents running in the Microsoft Azure cloud may fail to automatically start/stop and, after a timeout, freeze in the "Scheduled to Stop" state.  
  To work around this issue in the Azure plugin, please set the `teamcity.kotlinCoroutinesPool.configurator.enabled=false` [internal property](server-startup-properties.md#TeamCity+Internal+Properties). This issue will be automatically resolved on upgrading to the next bugfix update.
* __Builds using Ruby Environment Configurator have no compatible agents__  
  Enabling the [Ruby Environment Configurator](ruby-environment-configurator.md) feature in a build configuration will add the `env.AAAA` [agent requirement](agent-requirements.md) to it. Thus, the build agents that don't have this environment variable will be marked as incompatible, and TeamCity won't be able to run this build on them.  
  To work around this issue, please update the Ruby plugin to the fixed version as described [here](https://youtrack.jetbrains.com/issue/TW-73814#focus=Comments-27-5368673.0-0). This issue will be automatically resolved on upgrading to the next bugfix update.
* __DSA/DSS SSH keys can't be used in TeamCity 2021.1.4 and 2021.2__  
  After upgrading to any of these versions, the [SSH keys](ssh-keys-management.md) of the DSA/DSS format are rejected with the "_ssh-dss cannot be used as public key type for identity_" error.  
  To continue using them in TeamCity, please follow [this workaround](https://youtrack.jetbrains.com/issue/TW-73759#focus=Comments-27-5347961.0-0).
* __UnknownHostException when fetching files from Git LFS 3.0.0__  
  Users might get the `java.net.UnknownHostException` error when fetching files to build agents from Git LFS 3.0.0. This version [switched to the pure SSH protocol](https://github.com/git-lfs/git-lfs/blob/main/CHANGELOG.md#300-24-sep-2021). According to this protocol, it adds `—` before the host name. The JSch library used by TeamCity for SSH connections doesn't support them and treats these symbols as a part of the host name.  
  To work around this issue, set the `teamcity.git.use.native.ssh=true` [build configuration parameter](configuring-build-parameters.md). Alternatively, you can temporarily downgrade Git LFS to a version earlier than 3.0.0. This issue will be fixed in TeamCity 2021.2.1.
* __C# Script builds may fail if .NET profiling is enabled__  
  Running a build with [C# Script](c-script.md) steps on .NET with enabled [profiling](https://docs.microsoft.com/en-us/dotnet/core/run-time-config/debugging-profiling) may result in the "_Failed to create CoreCLR, HRESULT: 0x80004005_" error. This issue can occur only if .NET is launched inside a Docker container or in Windows Subsystem for Linux (WSL).  
  To work around it, add an [environment variable](configuring-build-parameters.md#Environment+Variables) `COMPlus_EnableDiagnostics=0` to your build configuration.
* __TeamCity fails to initialize a cloud client when creating an Amazon EC2 spot fleet profile__  
  When creating a [cloud profile](agent-cloud-profile.md) for an Amazon EC2 spot fleet, users might get the "_Failed to initialize cloud client 'amazon'. An exception occurred while parsing config._" error. This error only occurs if the "_Specify instance attributes that match your compute requirements_" option is enabled in the current fleet's _[instance type requirements](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-fleet-attribute-based-instance-type-selection.html#abs-create-spot-fleet)_.  
  To work around this issue, please remove the `InstanceRequirements` block from the fleet's JSON configuration file before uploading it to TeamCity. This issue will be fixed in TeamCity 2022.1.
* __Builds might fail to publish artifacts to Amazon S3 if AWS KMS is used__  
  After updating to 2021.2, builds might start failing on an attempt to publish artifacts to an Amazon S3 bucket encrypted with an [AWS KMS](https://docs.aws.amazon.com/kms/index.html) key. This issue is caused by the recently added integrity check for build artifacts. To temporarily disable it in a project and workaround the issue, set the `teamcity.internal.storage.s3.upload.enableConsistencyCheck=false` property on a [project level](levels-and-priority-of-build-parameters.md).  
  This problem will be fixed in TeamCity 2021.2.3.

### Canceled bidirectional agent-server communication protocol

The support for the bidirectional agent-server communication protocol has been stopped. Since version 2021.2, agents will connect to the server exclusively via the [unidirectional protocol](install-and-start-teamcity-agents.md#Agent-Server+Data+Transfer).

To upgrade TeamCity from versions earlier than 9.1, where the unidirectional support was first introduced, to 2021.2, use one of the following methods:
* Upgrade the server to version 2021.1, wait until all the agents upgrade as well, and then upgrade the server to 2021.2.
* Upgrade the server to version 2021.2, uninstall the old agents manually, and then [install the new agents](install-and-start-teamcity-agents.md).

### Perforce automatic labels become default

If you use [VCS labeling](vcs-labeling.md) for a [Perforce](perforce.md) root, note that TeamCity now creates [automatic labels](https://www.perforce.com/manuals/p4guide/Content/P4Guide/labels.alias.html) by default. If you want to continue using static labels, you can revert to the previous behavior by adding the `teamcity.perforce.useStaticLabels=true` [internal property](server-startup-properties.md#TeamCity+Internal+Properties).

### Fixed inconsistency in build chains clean-up

Previously, builds in an artifact dependency configuration were never [cleaned up](teamcity-data-clean-up.md) if its dependent configuration had a snapshot dependency on another build configuration which was set to be preserved. For example, if __C__ artifact-depends on __B__ and snapshot-depends on __A__ and __A__ is set to be preserved, __B__ was not cleaned up even if the "_Keep artifact dependencies_" option was enabled in __C__. Now, builds in the artifact dependency configuration (__C__) will be cleaned up properly, in full accordance with the clean-up rules.

This fix restores the intended behavior, but we recommend that you review your clean-up settings to ensure no builds will be cleaned up unexpectedly after the upgrade.

<anchor name="Planned+deprecation+of+Java+8+in+TeamCity+Server+2022.1"/>

### Planned deprecation of Java 8 in TeamCity Server 2022.04

TeamCity 2021.2 Server supports Java versions 8 and 11, but __Java 8 support will be discontinued in TeamCity 2022.04__, in April 2022. If you use a non-bundled version of Java 8, we highly recommend that you migrate your server to Java 11 until the 2022.04 release.

Note that TeamCity is not compatible with Java 17, which makes Java 11 the only version planned for support in TeamCity Server 2022.04.

### Bundled tools updates
{id="bundled-tools-updates-20212"}

* Bundled Amazon Corretto Java has been updated to version 11.0.12.7.1 in the TeamCity server and agent Docker images for Windows and Linux.
* Bundled Tomcat has been updated to version 8.5.72.
* Bundled JaCoCo has been updated to version 0.8.7.
* Bundled Ant has been updated to version 1.10.11.
* The Bundled Kotlin compiler, used in [TeamCity DSL](kotlin-dsl.md), has been updated to version 1.5.31.
* The bundled dotCover tool has been updated to version 2021.2.2.
* The following notifications plugins are no longer actively used and thus unbundled from TeamCity:
    * [Jabber/XMPP](https://plugins.jetbrains.com/plugin/17722-notifier-jabber-xmpp)
    * [RSS feed support](https://plugins.jetbrains.com/plugin/17723-rss-feed-support)  
      To proceed using their functionality in TeamCity 2021.2, you need to download the required plugin via the link above and install it as described [here](installing-additional-plugins.md).

## Changes from 2021.1.3 to 2021.1.4

* Updates in [TeamCity Agent Docker images](https://hub.docker.com/r/jetbrains/teamcity-agent/) for Linux:
    * Git is updated to version 2.25.1.
    * The Perforce Helix Core client (p4) is updated to version 2021.1/2179737.

## Changes from 2021.1.2 to 2021.1.3

No potential breaking changes.

## Changes from 2021.1.1 to 2021.1.2

* If you run a [personal build](personal-build.md) that is a part of a [build chain](build-chain.md), all its dependency builds will now be run as personal builds as well.  
  However, if you enable the [reuse of suitable builds](snapshot-dependencies.md#Suitable+Builds) in the dependency settings, TeamCity will try to optimize the chain whenever possible. If running a personal dependency build does not bring any value or contradicts the checkout rules, TeamCity will use a finished non-personal build instead.

## Changes from 2021.1 to 2021.1.1

No potential breaking changes.

### Known Issues
{id="known-issues-2021211"}

* Builds that use Git [submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) might fail with the "_Failed to perform checkout on agent_" error. To prevent it, you need to set a branch for each used submodule in `.gitmodules`.  
  This problem will be fixed in version 2021.1.2, but you can already download and install our Git plugin with the fix [here](https://youtrack.jetbrains.com/issue/TW-71933#focus=Comments-27-4975853.0-0).

### Other Updates
{id="other-updates-202111"}

* If you have added the `teamcity.nuget.feed.async.request.enabled` internal property to work around [this issue](#ki-202121) in 2021.1, remember to remove it on upgrading to 2021.1.1.
* VCS roots of archived subprojects are now hidden by default on the __Project Settings | VCS Roots__ page. You can display them by enabling the _including archived_ filter option.

## Changes from 2020.2.x to 2021.1

No potential breaking changes.

<anchor name="ki-202121"/>

### Known Issues
{id="known-issues-202121"}

* When trying to load a NuGet package which name contains the `.` (dot) character, users get the "Could not find acceptable representation" exception in the build log. This is caused by the issue in the new performance optimization algorithm: it truncates the filename to the part preceding the first dot. To work around this issue, please download the fixed NuGet Support plugin [here](https://youtrack.jetbrains.com/issue/TW-71659#focus=Comments-27-4916989.0-0) and upload it in __Administration | Plugins__. Alternatively, you can temporarily disable the new optimization mode by setting the `teamcity.nuget.feed.async.request.enabled` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) to `false` — note that this property has to be removed after upgrading to TeamCity 2021.1.1.

### Git Use Mirrors is deprecated in favor of Checkout Policy

[Git](git.md) VCS roots now receive the new _[Checkout Policy](git.md#git-checkout-policy)_ option that replaces the _Use Mirrors_ checkbox and provides more flexibility. On upgrading, the roots' settings will keep their selected states. However, Git roots' settings in [Kotlin DSL](kotlin-dsl.md) specifications need to be updated.

The `useMirrors` parameter in the Kotlin DSL of Git VCS roots is deprecated and replaced by the `checkoutPolicy` parameter that supports the following values: `AUTO` (default), `USE_MIRRORS`, `NO_MIRRORS`, `SHALLOW_CLONE`.

### Deprecated non-portable Kotlin DSL

Non-portable Kotlin DSL format is deprecated. It is no longer possible to create new projects in this format. For compatibility, projects that are already stored in this format will continue working.

### Changed default port for Windows installers

The default port in the TeamCity installer for Windows has been changed to 8111. Now, both `tar.gz` and `exe` installers use the same port.

### OR as default operator for Lucene search

TeamCity [Lucene-based search](search.md) now uses the `OR` operator by default instead of `AND`. This corresponds to the default Lucene syntax and helps optimize the search behavior and reduce its index size.

### SVG build status icon by default

The build status icon, available via the default `http://<TeamCity Server host>:<port>/app/rest/builds/<buildLocator>/statusIcon` [REST API endpoint](https://www.jetbrains.com/help/teamcity/rest/get-build-status-icon.html), is now provided in the SVG format instead of PNG. The `statusIcon.svg` endpoint is still supported for compatibility with existing scripts.  
To get a PNG icon, use the `statusIcon.png` endpoint.

### Unbundled old versions of REST API

The following old versions of [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html) have been unbundled: 6.0, 7.0, 8.1, 9.0, 9.1. If this change causes any problems for your setup, please contact us via any [feedback channel](feedback.md).

### Bundled Tools Updates
{id="bundled-tools-updates-20211"}

* Bundled Amazon Corretto Java has been updated to version 11.0.11.9.1 in the TeamCity server Docker images and Windows installers.
* Bundled [Ant](ant.md) has been updated to version 1.10.10. Note that this version requires Java 8 or later.
* Bundled dotCover and ReSharper CLT have been updated to version 2021.1.2.
* Bundled JaCoCo has been updated to version 0.8.6.
* The Bundled Kotlin compiler, used in [TeamCity DSL](kotlin-dsl.md), has been updated to version 1.4.32.
* Bundled Kotlin, used in the [Kotlin Script](kotlin-script.md) build runner, has been updated to version 1.5.0.
* JGit version, used in the [Git](git.md) plugin, has been updated to 5.10.0.202012080955-r.
* SVNKit, used in [Subversion](subversion.md) VCS roots, has been updated to version 1.10.3.


### Other Updates
{id="other-updates-20211"}

* The __My Settings & Tools__ page has been renamed to __Your Profile__.

## Changes from 2020.2.3 to 2020.2.4

### Verifying NuGet used in NuGet dependency trigger

To ensure that the integration with NuGet is secure, TeamCity now checks if a trusted NuGet installer is used when starting builds on Windows with a [NuGet dependency trigger](nuget-dependency-trigger.md). If your triggers use a NuGet version installed via __[Administration | Tools](installing-agent-tools.md)__, this verification will go smoothly, with no user actions required. Otherwise, you might get the "_Problem with NuGet Dependency Trigger_" error. The recommended solution is to switch all affected triggers to any NuGet version that has been regularly [installed via the TeamCity UI](installing-agent-tools.md). Such versions automatically appear in the _NuGet.exe_ drop-down menu in the trigger's settings. Alternatively, if you need to use a custom NuGet executable and absolutely trust it, you can add it to the whitelist by specifying the following [internal property](server-startup-properties.md#TeamCity+Internal+Properties):

```
teamcity.nuget.server.cli.path.whitelist=<disk>:\\<path-to-executable>\\NuGet.exe
```

### SSH runners: change in authentication with default key

In this version, we've changed the behavior of SSH authentication with the default private key in the [SSH Exec](ssh-exec.md) and [SSH Upload](ssh-upload.md) build runners. Previously, when authenticating via SSH during a build, a build agent would use the username configured in the agent's SSH config or, if it's missing, the name of the OS user who runs this agent. The value of the optional _Username_ field in the build step's settings would always be ignored. Now, if this value is specified, the agent will use it for SSH authentication instead of the one in the SSH config.  
If you have previously configured an SSH username directly inside build steps and the _Default private key_ authentication method is selected in these steps, make sure that this username is still relevant and allows for successful authentication.

## Changes from 2020.2.2 to 2020.2.3

### Bundled Tools Updates
{id="bundled-tools-updates-202023"}

* In the [TeamCity agent Docker image](https://hub.docker.com/r/jetbrains/teamcity-agent/), Docker has been updated to version 19.03.14 and Docker Compose has been updated to version 1.28.5.
* [SBT](https://www.scala-sbt.org/), used in the [Simple Build Tool (Scala)](simple-build-tool-scala.md) plugin, has been updated to version 1.4.7.

## Changes from 2020.2.1 to 2020.2.2

### IntelliJ Platform Compatibility
{id="intellij-202022"}

IntelliJ IDEA version 2019.2 and earlier, as well as other IntelliJ-based products released prior to IntelliJ platform version 193, is no longer supported by the [IntelliJ Platform plugin](intellij-platform-plugin.md). See the [version compatibility table](intellij-platform-plugin-compatibility.md) for details.

### Bundled Tools Updates
{id="bundled-tools-updates-202022"}

* __Kotlin__, used in [TeamCity DSL](kotlin-dsl.md), has been updated to version __1.4.21__.

### Other Updates
{id="other-202022"}

* The `teamcity.tests.recentlyFailedTests.file` [system property](configuring-build-parameters.md) contains the full path to a file with the recently failed tests and can be used to [reorder risk tests](https://plugins.jetbrains.com/docs/teamcity/risk-tests-reordering-in-custom-test-runner.html) in custom build runners. Previously, it was always generated by default. Now, it is only provided if the used build runner is set to reorder test runs _or_ if the `teamcity.internal.tests.provide.recentlyFailedTests` parameter is set to `true`.

## Changes from 2020.2 to 2020.2.1

No potential breaking changes.

### Known Issues
{id="known-issues-202021"}

* Windows Docker images of the TeamCity server don't allow restarting the server from the UI. See how to [stop and start the server](start-teamcity-server.md) via the command line.

### Breaking change for Linux Docker images of TeamCity Server: non-root user by default

Following the security practices we've applied to our [Agent Docker images](#Agent+Docker+images+run+under+non-root+user), the __TeamCity Server Docker images for Linux now run under a non-root user by default__.

When trying to run a Linux image under a default user, you will get the "_Permission denied_" error. To prevent it, you need to change the ownership over the host data directories: run `chown -R 1000:1000` applied to the required volumes. __For large directories, this operation may take a long time and slow down the disk performance__.   
Alternatively, you can start the container under the root user by passing the `-u 0` parameter to the `docker run` command. This is a quick workaround which allows saving time on changing the directories' ownership. However, we suggest that you stick to the `chown` approach in the long term.

### No auto prefix for dotnet run command line parameters

Since this version, the [.NET](net.md) build runner __does not apply `--` before the `dotnet run` parameters__. Previously, the runner added this prefix automatically which made it impossible to pass the custom options to the `run` command. To fix this, we've disabled the previous behavior.  
Unfortunately, the affected .NET build steps cannot be converted automatically on upgrading. If any of your steps pass arguments to the running .NET application, please make sure to alter these steps and prepend the respective parameters with `--`.

### Bundled Tools Updates
{id="bundled-tools-updates-202021"}

* Bundled Tomcat has been updated to version 8.5.61.

## Changes from 2020.1.x to 2020.2

* For TeamCity servers using a PostgreSQL database, the JDBC driver must be upgraded to 42.x version. Otherwise, a critical performance degradation may occur.

### Known Issues
{id="known-issues-20202"}

* If upgrade fails with an error in OptimizeAndCleanupIdsGroupsTableConverter, please apply the workaround described in [this issue](https://youtrack.jetbrains.com/issue/TW-68938#focus=Comments-27-4538713.0-0).
* TeamCity agent Docker images with the `latest` tag do not bundle Docker.  
  To be able to run Docker in a TeamCity 2020.2 agent, download the `teamcity-agent` image with the `{TEAMCITY_VERSION}-linux-sudo` tag instead. More information is available in our [Docker Hub documentation](https://hub.docker.com/r/jetbrains/teamcity-agent/).

### New Header by Default

The new header is enabled in both the classic and Sakura UIs. Some plugins developed for the previous header might not work in the new one. With [our new API](https://blog.jetbrains.com/teamcity/2020/09/teamcity-2020-2-updated-plugin-development/), you can make your custom plugins compatible with the new header or write new ones using modern web technologies.

If you have troubles displaying valuable information or actions in this header and updating plugins is not a convenient option, you can set the `teamcity.ui.useClassicHeader=true` internal property — this will switch your TeamCity header to the previous view. Please note that this is not a recommended solution as we might disable the obsolete header in the future versions.

### Reindexing build search

Lucene version in [TeamCity search](search.md) has been updated to 8.5.1. On upgrading, TeamCity will reindex all builds on the server which might take some time and load the CPU. During reindexing, some builds might not be present in the search results.

### Bundled Python runner

The [external Python build runner](https://plugins.jetbrains.com/plugin/9042-python-runner) is no longer supported. All existing build steps will continue to work normally, but we recommend switching existing Python steps to the new [bundled](python.md) runner.

### Gradle updates

* Previously, the _Build file_ field of the [Gradle](gradle.md) runner was set to `build.gradle` by default. We have removed this default value as some users rely on custom names of build files and prefer to let Gradle decide what file to choose.   
  If `build.gradle` was selected as a build file in your Gradle steps, this setting will remain. In projects whose settings are stored in VCS, TeamCity will prompt to commit the respective change so the `build.gradle` property is explicitly specified in the versioned project settings.
* The [Gradle](gradle.md) runner now displays test names based on the `displayName` properties assigned to the respective test methods. If your Gradle tests are annotated with a custom `displayName` property (for example, a JUnit 5 test with the `@DisplayName` annotation), their names will change in TeamCity on upgrading. This might break test and investigation history in respective builds. To prevent this, consider switching the behavior back with the `teamcity.internal.gradle.testNameFormat=name` internal property.

### New responsibility for secondary nodes

Since version 2019.2, a secondary node allows user actions if at least one responsibility is assigned to it. In 2020.2, we have added a new responsibility — "Processing user requests to modify data" (version 2023.05 update — this responsibility was renamed to "Handling UI actions and load balancing user requests"). Nodes with this responsibility can process all currently supported user actions and allow changing project settings. Without it, a node will provide a read-only interface.   
On upgrading, this responsibility will be automatically enabled on all your secondary nodes that have at least one other responsibility. This will ensure no current functionality of these nodes is affected. To allow user actions on new secondary nodes, you have to manually enable the new responsibility in __Administration | Server Configuration__.

### Bundled tools updates
{id="bundled-tools-updates-202020"}

* .NET in TeamCity server and agent Docker images (both Windows and Linux) have been updated to version 3.1.403.
* Java in TeamCity Docker images has been updated:
    * On Windows server images: to Amazon Corretto x64 v.11.0.9.11.2
    * On Linux server images: to Amazon Corretto x64 v.11.0.9.11.
    * On Windows and Linux agent images: to Amazon Corretto x64 v.8.272.10.3
* Java bundled in the TeamCity server and agent Windows installers has been updated to version 11.0.9.11.2.
* Bundled Tomcat has been updated to version 8.5.57.
* The SAC Windows image in the TeamCity server Docker containers has been updated to version 2004. Windows LTS version is 1809, as in TeamCity 2020.1.
* The Linux image in TeamCity server Docker containers has been updated to version 20.04 (LTS).
* Bundled dotCover and ReSharper CLT have been upgraded to version 2020.2.4.
* The deprecated Visual Studio 2003 build runner is disabled in TeamCity. We recommend using the [.NET](net.md) runner instead.   
  If you were actively using the VS 2003 runner and cannot easily migrate to the .NET runner, please let us know about it via any of our [feedback channels](https://teamcity-support.jetbrains.com/hc/en-us){nullable="true"}.
* JDBC drivers for external databases, suggested on the fresh TeamCity installation, have been updated to the following versions:
    * MySQL - 8.0.22
    * MSSQL - 8.4.1
    * PostgreSQL - 42.2.18

### Other updates

* When detecting [GitHub issues](github.md), TeamCity now filters out pull requests that have no issues assigned to them. If you need to display such independent pull requests in the issue log, you can disable this filter by setting the `teamcity.issues.github.filter.pull.requests=false` [internal property](server-startup-properties.md#TeamCity+Internal+Properties).
* [Email Notifier](notifications.md#Email+Notifier) now uses the same versions of the TLS protocol as supported by the current TeamCity server's JVM.

## Changes from 2020.1.4 to 2020.1.5

No potential breaking changes.

## Changes from 2020.1.3 to 2020.1.4

### Bundled Tools Updates
{id="bundled-tools-updates-2020-1-4"}

* Bundled Amazon Corretto Java has been updated to version 11.0.8 in the TeamCity server installers and Docker images.
* Mercurial, dropped in [version 2020.1.2](#Changes+from+2020.1.1+to+2020.1.2), is again supported in the Windows Server Core agent Docker images.

## Changes from 2020.1.2 to 2020.1.3

* The [.NET](net.md) build runner now supports earlier versions of Visual Studio and MSBuild. The currently supported versions are: Visual Studio 2010 or later, MSBuild 4 / 12 or later.

### Known Issues
{id="known-issues-202013"}

* If you try to re-run a build that has an artifact dependency but not snapshot dependency on another build, the _Re-run build_ dialog will not load.   
  This issue will be fixed in TeamCity 2020.1.4. To work around it in version 2020.1.3, follow [this instruction](https://youtrack.jetbrains.com/issue/TW-67351#focus=Comments-27-4355962.0-0).

## Changes from 2020.1.1 to 2020.1.2

* Mercurial support has been dropped for our Windows Server Core agent Docker images. If you need to use Mercurial on Windows Server Core agents, consider pulling the previous version of the agent Docker image — 2020.1.1.

## Changes from 2020.1 to 2020.1.1

* The .NET runner introduces the [custom command](net.md#Custom+Commands) option. Note that if you downgrade TeamCity to the previous version after configuring the custom .NET command, the respective build steps will be ignored during the build.
* The Linux version used in the TeamCity server and agent Docker images has been updated to 4.19.76-linuxkit.

### Known Issues
{id="known-issues-202011"}

* In the TeamCity classic UI, the _Projects_ link in the header is missing the expand button.  
  To work around this issue, please follow the instruction described [here](https://youtrack.jetbrains.com/issue/TW-66577#focus=streamItem-27-4216533.0-0).

## Changes from 2019.2.x to 2020.1

No potential breaking changes.

### Known Issues
{id="known-issues-20201"}

#### Jira Cloud Integration build feature requires specific VCS URL

The Jira Cloud API, used by the new [Jira Cloud integration](jira-cloud-integration.md) build feature, requires sending a server URL in a specific format. Because of that, the build feature does not support VCSs like Perforce, TFS, and SVN out of the box.

To address this issue, we have updated the responsible plugin, and you can find it attached to the related [issue](https://youtrack.jetbrains.com/issue/TW-66118) in our tracker. Please download the fixed plugin and install it as described [here](installing-additional-plugins.md).   
The bundled Jira Cloud plugin will be automatically updated with this fix in our next release.

The feature might also fail to resolve some Git paths that do not correspond to the format expected by the Jira Cloud API. In this case, you can either change the URL manually (for example, from `git@<vcs_address>:<workspace_ID>/<repo_name>.git` to `ssh://git@<vcs_address>/<workspace_ID>/<repo_name>.git`) or download the fixed plugin as described above.

#### Jira Cloud Integration feature does not support legacy Jira Cloud domain

Currently, the new [Jira Cloud integration](jira-cloud-integration.md) build feature supports only `atlassian.net` domains. We have added the support of the legacy `jira.com` domain in the fixed version of the responsible plugin. If your Jira Cloud server resides on the `jira.com` domain, you can download the plugin, attached to the [related issue](https://youtrack.jetbrains.com/issue/TW-66103#focus=streamItem-27-4154698.0-0), and install it as described [here](installing-additional-plugins.md).   
The bundled Jira Cloud plugin will be automatically updated with this fix in our next release.

#### Bad Redirect URI error when authenticating in Slack

To be able to sign in to Slack from TeamCity, you need to specify all the possible URIs of the TeamCity server as _Redirect URLs_ in the [Slack app's](configuring-connections.md#Slack) settings.   
If you use nginx to set up TeamCity behind a proxy server, you might still get the `bad_redirect_uri` error when trying to establish a connection with Slack. This error is caused by the mismatch between the nginx and Tomcat configuration.

To work around this issue, download the fixed plugin, attached to the [related issue](https://youtrack.jetbrains.com/issue/TW-66113), and install it as described [here](installing-additional-plugins.md). Alternatively, you can try [updating the Tomcat settings](configuring-proxy-server.md#TeamCity+Tomcat+Configuration).   
The bundled Slack plugin will be automatically updated with this fix in our next release.

#### Problems with built-in authentication in upgraded 2020.1 EAP1 installations

If you had installed the 2020.1 EAP1 build in terms of our Early Access Program, you might experience problems with signing in to TeamCity via the [built-in authentication](configuring-authentication-settings.md#Built-in+Authentication). This issue might occur after upgrading from any 2020.1 EAP version (EAP1 or any later version to which it was upgraded) to the release 2020.1 build.

To work around this problem, please send the following query to the TeamCity database:

```Console

UPDATE users SET algorithm = 'BCRYPT' WHERE password like '$2a$07$%' and algorithm = 'MD5';

```

### Changes in Java support on server and agents

* Java 11 has been bundled with the TeamCity server Windows installer and server Docker images instead of Java 8.
* TeamCity agents stop supporting Java versions earlier than 8. If any of your agents run on earlier versions of Java, make sure to upgrade their JRE so you can continue running builds on these agents.

#### New format of env.JDK_ environment variables

To better align with the current and future Java versions we've introduced a new format of `env.JDK_` environment variables.   
Starting with 2020.1, the format is as follows: `env.JDK_<major>_<minor>[_x64]`. For example: `env.JDK_1_6`, `env.JDK_1_7`, `env.JDK_1_8`, `env.JDK_11_0_x64`.   
This way, if you are using rather old Java 1.4, the proper variable is `env.JDK_1_4`, while `env.JDK_14_0` will be used for Java 14.0.

For backward compatibility, previous environment variables, such as `env.JDK_16` or `env.JDK_18`, will be generated too, but these variables will no longer be shown in the TeamCity autocompletion pop-up menus.  
If you are using these environment variables in your build scripts, we encourage you to migrate to the new format.

See the [related issue](https://youtrack.jetbrains.com/issue/TW-64998).

### Need for plugin recompilation

Plugins which implement some build runners might need to be recompiled/upgraded.

The corresponding error might look like `java.lang.NoSuchMethodError: jetbrains.buildServer.controllers.admin.projects.BuildRunnerBean.getPropertiesBean` when a new build step for the corresponding custom build runner is created or updated.

See the related issues about the [Checkmarx plugin](https://youtrack.jetbrains.com/issue/TW-66311) and the
[SonarQube Runner plugin](https://youtrack.jetbrains.com/issue/TW-66106).

### Agent Docker images run under non-root user

To comply with recommended security practices, the TeamCity agent Docker images now run under a non-root user.

This change might affect the following use cases:

* To create/update a custom image based on the standard TeamCity agent image, you might need to switch to the `root` user first (see the [related issue](https://github.com/JetBrains/teamcity-docker-agent/issues/65) for details).
* If you mount a host directory to a container, you might get the "_Permission denied_" error. To prevent this issue, try any of the following workarounds:
    * Set the UID of the directories' owner to `1000` with the `chown` command.
    * Send the `--user` argument with the `docker run` command to set the same UID for the Docker user as that of the host machine. For example, use `docker run -it --user $(id -u) ...`.
    * Note that TeamCity automatically creates volumes for writable directories, and there is usually no need to map them explicitly. Consider omitting any explicit references to prevent the permission issues.

### Deprecated Windows Tray Notifier

TeamCity Windows Tray Notifier has been deprecated in favor of the new [Browser Notifier extension](browser-notifier.md).

Windows Tray Notifier will continue working with the new version of TeamCity, but we recommend you trying the new extension instead. Note that since version 2020.1, the __My Settings & Tools | Notification Rules | Windows Tray Notifier__ tab in TeamCity has been renamed to __Browser Notifier__.

### Bundled Kubernetes Support plugin does not contain Helm runner

The [Kubernetes Support plugin](https://plugins.jetbrains.com/plugin/9818-kubernetes-support) is now bundled with TeamCity. On upgrade, it will replace the external plugin if it is installed on your TeamCity server. Note that the bundled plugin does not contain the Helm build runner. To continue using this runner in your build configuration, please install the [new external plugin](https://plugins.jetbrains.com/plugin/14315-helm-support).

### Limitation of CORS support for writing operations

TeamCity improves the security of REST API integration mechanisms by introducing CSRF tokens. This change will not affect the behavior of custom integration scripts unless they rely on Cross-Origin Resource Sharing (CORS) in writing operations and the `rest.cors.origins` internal property is [enabled in TeamCity](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html#CORS-support) (it is disabled by default).

Previously, CSRF protection was presented in TeamCity with the verification of `Origin/Referer` headers of HTTP requests. To improve TeamCity CSRF protection, this method has been disabled in favor of a more secure one — CSRF tokens. Since this release, TeamCity stops supporting the CORS mechanism for `POST/PUT/DELETE` REST API requests. Cross-origin GET requests' headers are processed as before and still require [CORS configuration](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html#CORS-support).

If necessary, you can enforce verification of `Origin/Referer` headers for writing CORS operations by setting the `teamcity.csrf.paranoid=false` internal property. Note that this is a transitory and less secure solution: we strongly recommend refactoring your existing requests so they comply with the new security policy and provide a token within a CSRF header or parameter. A CSRF token can be obtained via the `GET https://your-server/authenticationTest.html?csrf` request and provided via the `X-TC-CSRF-Token` HTTP header to the write CORS requests.

### Bundled Tools Updates

* Bundled __IntelliJ IDEA__ has been updated to version __2020.1.1__.
* Bundled __Ant__ has been updated to version __1.9.14__.
* Bundled __Tomcat__ has been updated to version __8.5.54__.
* Bundled __Maven__ has been updated to version __3.6.3__.
* __Kotlin__, used in TeamCity DSL, has been updated to version __1.3.70__.
* JDBC drivers for external databases, suggested on the fresh TeamCity installation, have been updated to the following versions:
    * MySQL - 8.0.20
    * MSSQL - 8.2.2
    * PostgreSQL - 42.2.12

### REST API changes

Filtering test occurrences by a branch (`.../app/rest/testOccurrences?locator=branch(XXX)` request) has been changed. It used to support only branch names with case-sensitive matching. Now, the `XXX` value supports branch locators (the same as when filtering builds): it is case-insensitive by default and matches the `<default>` branch display name.

### Other changes

* Now, a user cannot assign a role to another user, if this role has more permissions than the role of the current user.   
  If you experience troubles with certain roles being unavailable to users after upgrading to 2020.1, make sure these roles don't comprise any permissions that exceed the rights of the respective project administrators.
* TeamCity has dropped support for Internet Explorer. Please use Microsoft Edge instead.
* Since this version, the new .NET runner, introduced in TeamCity 2019.2.3, is not compatible with the obsolete external [.NET CLI Support](https://plugins.jetbrains.com/plugin/9190--net-cli-support) plugin (used in versions 2017.1 and earlier). If you have previously installed this plugin, please [uninstall](installing-additional-plugins.md#Uninstalling+Plugin+via+Web+UI) it from your server to be able to use the new .NET runner.

## Changes from 2019.2.3 to 2019.2.4

No potential breaking changes.

## Changes from 2019.2.2 to 2019.2.3

### Reworked .NET build runner

The .NET CLI (dotnet) build runner has been refactored and renamed to __.NET__ thus emphasizing that now it supports all .NET-related operations previously implemented in TeamCity as multiple build steps.

All existing .NET CLI (dotnet) steps will continue working as usual under the new _.NET_ name, with no additional tuning required.

We stop providing active support for the [MSBuild](msbuild.md), [Visual Studio (sln)](visual-studio-sln.md), [Visual Studio 2003](visual-studio-2003.md), and [Visual Studio Tests](visual-studio-tests.md) runners. These steps are left for compatibility of existing build configurations with new versions of TeamCity. We recommend switching all your affected build steps to the .NET runner to receive new features and support in our following versions.

See the [.NET description](net.md) for more information about the new .NET step and migration notes.


> Since the reworked .NET runner introduces new options and features, you might not be able to use them if downgrading to the earlier versions of TeamCity. In such case, you will have to return to using the obsolete runners after downgrading. To prevent any issues, you can [back up your TeamCity data](creating-backup-from-teamcity-web-ui.md) before upgrading to version 2019.2.3.
>
{style="note"}

If you face any problems with migration to the .NET runner or encounter other related issues, do not hesitate to contact us via any convenient [feedback channel](feedback.md).

### Known Issues
{id="known-issues-201923"}

#### Handshake failure on establishing SSL connection

Some users might get the "_Received fatal alert: handshake_failure_" error when the TeamCity server attempts to establish an SSL connection. The problem is caused by the broken `sunec.dll` in JRE bundled with TeamCity 2019.2.3.

To check if this problem has affected your installation, open the `<TeamCity_installation_directory>/jre/bin/sunec.dll` file. If there is a JSON code in this file, your server is affected. To work around the problem, please follow the steps described in the [related issue](https://youtrack.jetbrains.com/issue/TW-65566#focus=streamItem-27-4119621.0-0).

#### NuGet feed credentials for external repositories do not work with .NET runner

The [.NET build runner](net.md) currently does not support using [NuGet feed credentials](nuget-feed-credentials.md) for authentication in external repositories.

To work around this issue in TeamCity 2019.2.3, download the [patched .NET Packages Support plugin](https://youtrack.jetbrains.com/issue/TW-65581#focus=streamItem-27-4110405.0-0) and install it as any other [additional plugin](installing-additional-plugins.md). The bundled .NET Packages Support plugin will be automatically updated with the fix in our next release.

#### AWS region us-east-1 cannot be set in S3 artifact storage settings

If the `us-east-1` region is selected in S3 artifact storage settings, it will be automatically reset to another available region on saving the settings. This is caused by the incorrect bucket location returned for `us-east-1` from AWS.

To work around this issue in TeamCity 2019.2.3, download the [patched TeamCity S3 Storage plugin](https://youtrack.jetbrains.com/issue/TW-64670#focus=streamItem-27-4108345.0-0) and install it as any other [additional plugin](installing-additional-plugins.md). The bundled S3 Storage plugin will be automatically updated with the fix in our next release.

### Bundled Java for Windows installers is updated

The bundled version of Java in Windows installers of TeamCity Server and Agent as well as in the Docker images is updated to [Amazon Corretto 8.252.09.1](https://github.com/corretto/corretto-8/blob/release-8.252.09.1/CHANGELOG.md).

## Changes from 2019.2.1 to 2019.2.2

### Caching Git submodules

To improve performance on agent checkout, TeamCity caches regular Git repositories on agents. Since this version, it also caches Git submodules.   
If your custom scripts or settings depend on the main alternates source for submodules, and it causes Git to operate with errors, consider one of the following workarounds:
* Disable the new mirroring mechanism by setting the [build parameter](configuring-build-parameters.md) `teamcity.internal.git.agent.submodules.useMirrors` to `false`.
* Modify your custom settings to point at the parent `git` directory instead of the exact source directory.

### Bundled Tools Updates
{id="bundled-tools-updates-1"}

* TeamCity Visual Studio Add-in Web installer has been updated to ReSharper version 2019.3.2.

## Changes from 2019.2 to 2019.2.1

> __Java 8 update 242+ incompatibility__
>
> For versions 2019.2.1 and earlier, please __[do not use](known-issues.md#jdk8_240) Java 8 newer than [update 232](https://github.com/corretto/corretto-8/releases/tag/8.232.09.1)__ for the TeamCity server.
>
{style="tip"}

No potential breaking changes.

## Changes from 2019.1.x to 2019.2

No potential breaking changes.

### Known Issues
{id="known-issues-20192"}

#### Potential issues with restoring NuGet packages in .NET projects

TeamCity might fail to restore NuGet packages if a build comprises at least one .NET CLI (dotnet) step __and__ an [MSBuild](msbuild.md) or [Visual Studio (sln)](visual-studio-sln.md) build step (or both).

This issue is caused by the difference in paths to cache directories between these build runners.   
The MSBuild and Visual Studio (sln) runners use the default path to the NuGet global cache while the .NET CLI (dotnet) runner redefines this path (for example, when run inside a Docker container).

The recommended workaround is to use the [NuGet Installer](nuget-installer.md) build runner instead of the .NET CLI (dotnet) runner for restoring packages.


### Switch to 64-bit Bundled Java in Windows installer and Docker images

The bundled version of Java in Windows installers of TeamCity Server and Agent as well as in the Docker images is now 64-bit [Amazon Corretto](https://aws.amazon.com/corretto/) 8 (previous TeamCity versions bundled 32-bit Java, and TeamCity 2019.1 bundled AdoptOpenJDK).

If you were using the default bundled Java on Windows, make sure the following conditions are satisfied:
* The TeamCity server and agents operate on the 64-bit Windows OS. If you need to use a 32-bit OS, you need to install and use 32-bit Java to run TeamCity;
* If the TeamCity server has manually configured [memory settings](configure-server-installation.md#Configure+Memory+Settings+for+TeamCity+Server) (the `TEAMCITY_SERVER_MEM_OPTS` environment variable is defined), the value of the `-Xmx` parameter should be increased (recommended to twice the previous value). Before increasing the value, make sure the machine has enough physical memory for that.
* If you use Microsoft SQL Server as the TeamCity database with the integrated MS SQL authentication, the 64-bit `sqljdbc_auth.dll` native library has to be [present in the due location](setting-up-teamcity-with-ms-sql-server.md#integratedSecurityAuth).
* If there was any custom logic executing native tools on the server, check that it still works with the new process bitness.

<anchor name="running-builds-node-discontinued"/>

<anchor name="upgrade-notes-running-builds-node-discontinued"/>

<anchor name="UpgradeNotes-running-builds-node-discontinued"/>

### Discontinued Running Builds Node

The [Running Builds Node](https://confluence.jetbrains.com/display/TCD18/Configuring+Running+Builds+Node) is discontinued. In a multinode setup, you can instead [configure a secondary node](multinode-setup.md) with the "_Processing data produced by running builds_" responsibility.

### Automatic management of git fetch memory

TeamCity now can automatically manage the amount of memory used by the `git fetch` process.   
If you have previously used the `teamcity.git.fetch.process.max.memory` internal property to set the memory amount available for fetching in each VCS root, you can now disable it to delegate the detection of memory consumption to the TeamCity server. To control the limit of available memory, use the `teamcity.git.fetch.process.max.memory.limit` property.

### Bundled Tools Updates
{id="bundled-tools-updates-2"}

* Bundled IntelliJ IDEA has been updated to version 2019.3.
* Kotlin, used in TeamCity DSL, has been upgraded to version 1.3.60.
* The Docker client in the TeamCity Linux agent image has been upgraded to version 19.03.3 to prevent the problems with unexpected Docker stop (see the related [Docker issue](https://github.com/docker/for-linux/issues/517)).
* Docker Compose has been updated to version 1.24.1.
* Bundled dotCover and ReSharper CLT have been upgraded to version 2019.2.3.

#### Unbundled VCS Support plugins for ClearCase and SourceGear Vault

The VCS Support plugins for [ClearCase](https://plugins.jetbrains.com/plugin/13210-vcs-support-clearcase) and [SourceGear Vault](https://plugins.jetbrains.com/plugin/8892-vcs-support-sourcegear-vault) have been unbundled. To be able to use any of these VCS types in TeamCity, download and install the required plugin as described [here](installing-agent-tools.md).

## Changes from 2019.1.4 to 2019.1.5

* In the [TeamCity agent Docker image](https://hub.docker.com/r/jetbrains/teamcity-agent/), Docker has been updated to version 19.0.3 and Docker Compose has been updated to version 1.24.1.

## Changes from 2019.1.3 to 2019.1.4

* The bundled Java was updated to OpenJDK 8u222 (except for the Docker Windows TeamCity images).

### Known Issues
{id="known-issues-201914"}

#### Unavailable Default Credential Provider Chain option for Amazon ECR

_This issue has been fixed in TeamCity 2019.1.5._

Due to recent changes in our Docker Support plugin, the "[Default credential provider chain](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/credentials.html#credentials-default)" option becomes unavailable in the Amazon ECR connection settings.

If this option was previously enabled in some ECR connection and you make any changes to this connection, the state of this option will be automatically set to `false`. When any build will try to use this connection, it will fail to start with the "_Access key cannot be null_" error.

To work around this problem without upgrading to 2019.1.5, download the fixed Docker Support plugin from the [related issue](https://youtrack.jetbrains.com/issue/TW-62595#focus=streamItem-27-3749459.0-0) and upload it on the __Server Administration | Plugins List__ page.

#### Missing packages in NuGet feed

_This issue has been fixed in TeamCity 2019.1.5._

In certain cases, when a build is supposed to create and publish several NuGet packages to a NuGet feed, and the package indexing is enabled, some packages might not be published to the feed. This problem is caused by recent changes in [NuGet Packages Indexer](nuget-packages-indexer.md).

To work around this problem without upgrading to 2019.1.5, download the fixed NuGet Support plugin from the [related issue](https://youtrack.jetbrains.com/issue/TW-62545#focus=streamItem-27-3754398.0-0) and upload it on the __Server Administration | Plugins List__ page.

## Changes from 2019.1.2 to 2019.1.3

### Known issues
{id="known-issues-21"}

* When using versioned settings, build history can be lost on importing settings from VCS. [Details](https://youtrack.jetbrains.com/issue/TW-62106).

### Bundled Tools Updates
{id="bundled-tools-updates-3"}

* The bundled ReSharper Command Line Tools (Inspections and Duplicates Finder) have been upgraded to version 2019.2.1.

## Changes from 2019.1.1 to 2019.1.2

<anchor name="running-builds-node-deprecated"/>

* The [Running Builds Node](https://confluence.jetbrains.com/display/TCD18/Configuring+Running+Builds+Node) is deprecated and will be discontinued in TeamCity 2019.2. In a [multinode setup](multinode-setup.md), you can instead configure a secondary node with the _"Processing data produced by running builds"_ responsibility.

### Known issues

* When using versioned settings, build history can be lost on importing settings from VCS. [Details](https://youtrack.jetbrains.com/issue/TW-62106).
* If you use the .NET Core ("dotnet") steps on Windows agents, you can get the _".NET SDK was not found"_ error if you have .NET Core runtime (not SDK) installed on the agent in the location like `C:\Program Files (x86)\dotnet`. As a workaround, set the `env.DOTNET_HOME` parameter to the location of .NET Core SDK.   
  See the related [issue](https://youtrack.jetbrains.com/issue/TW-61413) for details.

## Changes from 2019.1 to 2019.1.1

### Bundled Tools Updates
{id="bundled-tools-updates-4"}

* The bundled IntelliJ IDEA has been updated to 2019.1.3.
* The bundled ReSharper tools (Inspections and Duplicates Finder) have been upgraded to 2019.1.0-eap08d version.

## Changes from 2018.2.x to 2019.1

### Mandatory tags for Amazon instances

Tags are now __mandatory__ for all Amazon instances run by TeamCity, which helps identify them. If you use integration with Amazon EC2, make sure to add the [`ec2:CreateTags`](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-supported-iam-actions-resources.html#supported-iam-actions-tagging) resource-level permission in Amazon.

### Changed behavior of reversed dependencies properties

Starting with 2019.1, the behavior of [`reverse.dep`](use-parameters-in-build-chains.md#Override+Parameters+of+Preceding+Configurations) parameters has been changed, and this change can affect your existing builds. In versions prior to 2019.1, when a build chain is triggered, TeamCity only took into account the `reverse.dep` parameters specified in the top-most build of the chain, i.e. in the build which depends on all other builds. If some intermediate builds of the chain had `reverse.dep` parameters, they were ignored.   
After [this fix](https://youtrack.jetbrains.com/issue/TW-41341) this is no longer the case. Now, when a build chain is triggered, all `reverse.dep` parameters specified in all nodes of the build chain will be processed.

### Lazy agent tool loading

[Agent tools](installing-agent-tools.md) (located under the `<agent_installation>/tools` directory on agents) are now transferred to an agent not on the agent upgrade, but right before the first build that uses the respective tool. You might need to update the build configuration settings so TeamCity knows which tools are required by the builds.   
Before starting a build on an agent, TeamCity checks for the references to `teamcity.tool.<tool_ID>` configuration parameters to collect the set of tools used by the build. If some tool is referenced via this parameter, TeamCity will make sure this tool is present on the agent before the build logic starts executing.   
If some of your builds use tools on agent assuming their locations under the `<agent installation>/tools` directory, such references should be changed to a TeamCity-provided parameter reference. Paths like `<agent_installation>/tools/<tool_ID>` used in TeamCity settings should be changed to the `%teamcity.tool.<tool_ID>%` parameter reference. For example, `../tools/maven3.4.5/bin/mvn` should be replaced with `%\teamcity.tool.maven3.4.5%/bin/mvn`.

### Changed .NET build requirements in ReSharper tools

The requirements for the .NET Framework version used by ReSharper tools have changed. Now, if you use ReSharper tools (dotCover and ReSharper Inspections) of version 2018.2 or newer (including the version bundled with TeamCity 2019.1) in your build configuration, the requirements to build agents will change to .NET Framework 4.6.1 or newer. Make sure to update .NET Framework on agents.

### Token-based authentication enabled by default

On upgrading to 2019.1, the Token-Based Authentication module will be enabled by default, so you can generate [access tokens](configuring-your-user-profile.md#Managing+Access+Tokens) and start using them right away.

### New CSP header value

Now TeamCity web UI uses more restrictive value for the [`Content-Security-Policy`](https://content-security-policy.com/) HTTP header. This provides extra security at the expense of prohibiting usage of the web resources not hosted on the TeamCity server.   
If you rely on external resources (for example, in the build report tabs content or by using not yet updated plugins), you can specify new header value in the `teamcity.web.header.Content-Security-Policy.protectedValue=<full_header_value>` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) (and `teamcity.web.header.Content-Security-Policy.adminUI.protectedValue` property for the web pages in Administration area). Plugins can use [`ContentSecurityPolicyConfig`](https://javadoc.jetbrains.net/teamcity/openapi/current/jetbrains/buildServer/web/ContentSecurityPolicyConfig.html) open API interface to add to the value configured.

### Change in dotCover artifacts

The `dotCover.dcvr` hidden artifact is no longer published by default. It is now created in the build temporary folder and removed when the build finishes.   
If you use dotCover and rely on this artifact, specify the path to the `%\system.teamcity.build.tempDir%\..\agentTmp\dotNetCoverageResults\dotCover.dcvr` file explicitly in the [Artifact paths](configuring-general-settings.md#Artifact+Paths).


### Bundled Tools Updates
{id="bundled-tools-updates-5"}

* The latest JaCoCo version (0.8.4) has been added to TeamCity.
* The bundled .NET tools (dotCover and ReSharper CLT) have been upgraded to the latest released version, 2019.1.1.
* JDBC drivers for external databases suggested on the fresh TeamCity installation have been updated to the following versions:
    * MySQL - 8.0.16
    * MSSQL - 7.2.2
    * PostgreSQL - 42.2.5

### Note on beta versions of PowerShell

If you have been using beta versions of PowerShell, make sure to remove all beta versions earlier than `v6.0.0-beta.9` before installing any new released PowerShell version. Due to updates in the PowerShell detector, if the old beta version is installed, TeamCity will use it instead of the new released one.

### Note on using Docker and Windows Server with process isolation

If you use Docker images and Windows Server 2019 with process isolation, build agents may fail to start (read more in [Known Issues](known-issues.md#%22Access+is+denied%22+or+%22Access+to+the+path+is+denied%22+problem+on+container+start)). To work around the issue, grant the "Full control" access to the "Authenticated Users" group.

## Changes from 2018.2.3 to 2018.2.4
There is a regression in TeamCity 2018.2.4 related to the data displayed on the unauthorized agents' page: the data on agent authorization, enabling/disabling comments, and the latest activity time has been removed from the page. This data is displayed on the agent details page.

When the performance is improved, we will return the data to the unauthorized agents' page.

## Changes from 2018.2.2 to 2018.2.3
TeamCity now ships Windows Docker images for 1803/1809 platforms.

### Known issues
{id="known-issues-1"}

Running builds are not shown on the build configuration page if there are no finished builds.  To work around the issue, stop the TeamCity server, replace the `TEAMCITY_DIRECTORY/webapps/ROOT/js/ring/bundle.js` with the `bundle.js` file  attached to [this issue](https://youtrack.jetbrains.com/issue/TW-59529#focus=streamItem-27-3327362.0-0) and start the server.

## Changes from 2018.2.1 to 2018.2.2
* The bundled Tomcat has been updated to version 8.5.3
* The TeamCity agent Docker image supports .NET Core 2.2

## Changes from 2018.2 to 2018.2.1

No potential breaking changes.

## Changes from 2018.1.x to 2018.2

### Known issues
{id="known-issues-2"}

* If upgrade fails with an error in MoveCustomDataStorageToDatabaseConverter or MoveRepositoryStateToCustomDataStorageConverter, apply the workaround from [the issue](https://youtrack.jetbrains.com/issue/TW-58289#focus=streamItem-27-3207962-0-0).
* If you're using Subversion externals from the same repository, you may face an issue with incorrect revision detection. A workaround for the problem is described in [this issue](https://youtrack.jetbrains.com/issue/TW-58336).
* If you see OutOfMemoryError during TeamCity startup with `org.jetbrains.dokka` in stack trace, set the internal property `teamcity.kotlinConfigsDsl.docsGenerationXmx=768m` (as described in [this issue](https://youtrack.jetbrains.com/issue/TW-56408))

### Bundled Tools Updates
{id="bundled-tools-updates-6"}

* The bundled .NET Tools (dotCover and ReSharper CLT) have been upgraded to the latest released version, 2018.1.4
* TeamCity 2018.2 comes bundled with IntelliJ IDEA 2018.3.1. The IntelliJ IDEA Project Runner uses JPS 2018.3.1
* OpenJDK is bundled in the Windows `.exe` TeamCity distribution instead of Oracle Java.

### NuGet feed

The parameters that were previously used to reference the global NuGet feed are removed. After upgrading, all of them will be converted to [the parameters referencing the default NuGet feed in Root project](using-teamcity-as-nuget-feed.md).

If you're still using deprecated NuGet feed references in project parameters please update them as following:

<table><tr>

<td>

Global feed name

</td>

<td>

Project feed name

</td></tr><tr>

<td>

teamcity.nuget.feed.server

</td>

<td>

teamcity.nuget.feed.guestAuth.\_Root.default.v2

</td></tr><tr>

<td>

teamcity.nuget.feed.auth.server

</td>

<td>

teamcity.nuget.feed.httpAuth.\_Root.default.v2

</td></tr><tr>

<td>

system.teamcity.nuget.feed.auth.serverRootUrlBased.server

</td>

<td>

teamcity.nuget.feed.httpAuth.\_Root.default.v2

</td></tr></table>

## Changes from 2018.1.4 to 2018.1.5

No potential breaking changes.

## Changes from 2018.1.3 to 2018.1.4

### Known issues
{id="known-issues-3"}

* Builds which are running while the server upgrades to 2018.1.4 can get their build log and status truncated, not duly reporting build failure. The build log gets warning with "\_\_tc\_longResponseMarker" text ([details](https://youtrack.jetbrains.com/issue/TW-58266)). It is recommended to wait till there are not running builds when upgrading to this version.
### Misc

Support for Microsoft Internet Explorer 10 is discontinued in TeamCity 2018.1.4.

## Changes from 2018.1.2 to 2018.1.3

### Bundled Tools Updates
{id="bundled-tools-updates-7"}

The latest JaCoCo version (0.8.2) has been added to TeamCity.

### Known issues
{id="known-issues-4"}

If you use JaCoCo coverage and you decide to downgrade from TeamCity 2018.1.3\+ to TeamCity versions 2018.1 \- 2018.1.2,  you may experience issues requiring manual fixes to make the affected configurations work again.

## Changes from 2018.1.1 to 2018.1.2

### Known issues
{id="known-issues-5"}

If you are using [tcWebHooks](https://plugins.jetbrains.com/plugin/8948-web-hooks-tcwebhooks-) third\-party TeamCity plugin, update it to the latest version before upgrading ([details](https://youtrack.jetbrains.com/issue/TW-56626)).

We increased accuracy for recognizing similar TeamCity exit code build problems. The existing investigations and mutes for these build problems will be reset.

### Bundled Tools Updates
{id="bundled-tools-updates-8"}

The bundled Tomcat has been updated to version 8.5.32.

## Changes from 2018.1 to 2018.1.1

### Known issues
{id="known-issues-6"}

If you're upgrading from 2018.1 to 2018.1.1 and you want to see the NuGet packages missing due to issues [TW-55703](https://youtrack.jetbrains.com/issue/TW-55703) and [TW-55833](https://youtrack.jetbrains.com/issue/TW-55833), do the following:
* Clean-up `.teamcity/nuget/packages.json` files in the build artifacts. Consider using this PowerShell script:


```Shell

> cd "%\TeamCityDataDir%\system\artifacts\"
> Get-ChildItem -Recurse | Where-Object { $_.FullName -match '\.teamcity\\nuget\\packages\.json' } | Remove-Item
```

* On the __Administration | Diagnostics | Caches__ page, reset the "buildsMetadata" cache and wait while re\-indexing is finished. To temporary increase indexing speed, see the [following tip](https://confluence.jetbrains.com/display/TCD18/Known+Issues#KnownIssues-PackagesindexingisslowinTeamCityNuGetfeed).

### Docker Images

Since 2018.1.1 TeamCity has multiplatform Docker images marked by the `latest` and version number tags published in Docker Hub, for example, `jetbrains/teamcity-server` or `jetbrains/teamcity-server:2018.1.1`. This allows using the same Docker image reference for Linux and Windows docker containers: see [TW-55061](https://youtrack.jetbrains.com/issue/TW-55061) for details.

## Changes from 2017.2.x to 2018.1

### Known issues
{id="known-issues-7"}

While publishing NuGet packages into the TeamCity NuGet feed in multiple build steps, only the packages published by the first build step will be visible. See [TW-55703](https://youtrack.jetbrains.com/issue/TW-55703) for details. If you experience problems with download of NuGet packages published within archives see [TW-55833](https://youtrack.jetbrains.com/issue/TW-55833).

### Stricter rules for parameter names used in parameter references

Names of the Build Configuration parameters are now validated in more strict manner. While already existing parameters should continue to work, it is highly recommended to review the names and use Latin letters and no special symbols. [Details](configuring-build-parameters.md)

### User self-registration

If you have Built\-in authentication enabled with the "Allow user registration from the login page" setting on, the setting will be disabled on upgrade. If you need the registration, make sure the server is not open to unauthorized users access (e.g. not accessible from Internet) and enable the setting via the health item displayed at the top of the administration pages or in the "Administration | Authentication" under the "Built\-in" module settings.

### Bundled Tools Updates
{id="bundled-tools-updates-9"}

* The IntelliJ IDEA Project Runner uses JPS 2017.3.4 requiring Java 1.8 as the minimal version.
* The bundled ReSharper CLT and dotCover have been updated to version 2018.1.2

### NuGet feed
{id="nuget-feed-1"}

* Configuration of the NuGet feed was moved from the server level to the project level: now each project can have its own feed. The "NuGet packages indexer" build feature can be added to build configurations whose artifacts should be indexed.
* The following NuGet feed\-related build parameters are deprecated:
    * `teamcity.nuget.feed.auth.server`
    * `teamcity.nuget.feed.server`
    * `system.teamcity.nuget.feed.auth.serverRootUrlBased.server`

  You now need to explicitly specify the URL from the [NuGet Feed](using-teamcity-as-nuget-feed.md) page in the __Project Settings__.

* The enabled default NuGet feed with all published packages accessed by URL `/app/nuget/v1/FeedService.svc/` is now moved to the Root project feed `/app/nuget/feed/_Root/default/v2/`. It is recommended to switch to new URL in your projects.
* .nupkg files are now indexed on the agent side instead of the server which could slightly increase the time of builds for projects with the NuGet Feed feature and the automatic package indexing enabled or for builds with NuGet Packages Indexer build feature.

### REST API

REST API uses version 2018.1. The previous versions of the API are still available under /app/rest/2017.2, /app/rest/2017.1 (/app/rest/10.0), app/rest/9.1, /app/rest/9.0, /app/rest/8.1, /app/rest/7.0, /app/rest/6.0 URLs. It is recommended to stop using previous APIs URLs as we are going to remove them in the following releases.

#### Filtering builds by agent names

When agent name contains the parentheses symbols, instead of using agentName:&lt;name&gt;, use "agentName:(value:&lt;value&gt;)

#### Locators with "value:&lt;text&gt;"

Requests which used "value:&lt;text&gt;" locators (e.g. for matching properties) and no "matchType" dimension specification will start to use "equals" matching by default. Add "matchType:contains" to preserve the old behavior. [Details](https://youtrack.jetbrains.com/issue/TW-55350)

### VSS plugin is unbundled

The Visual SourceSafe plugin is no longer bundled with TeamCity but is available as a [separate download](https://plugins.jetbrains.com/plugin/10902-vcs-support-vss). Please contact our [support](feedback.md), if you still use this VSS for your builds.

### Other

* Commit Status Publisher supports Gerrit 2.6\+ versions. For support for older Gerrit versions, please turn to our [support](feedback.md).
* When upgrading from TeamCity versions before 9.1, if TeamCity 2018.1 starts and agents are upgraded, but then you decide to roll back the server to the previous TeamCity version, the agents will not be able to connect back to the old server and will need to be reinstalled manually.
* Make sure that no HTTP requests from the agents to the server are blocked (e.g. requests to .../app/agents/... URLs)
* Since 2018.1, TeamCity uses the full project name with "/" instead of "::" as the separator for Project \- Subproject wherever the full project name is present.

## Changes from 2017.2.3 to 2017.2.4

The Inspections (.NET) and Duplicates Finder (.NET) build steps were renamed to Inspections (ReSharper) and Duplicates finder (ReSharper).

## Changes from 2017.2.2 to 2017.2.3

### Build revisions

In all the build configurations, the builds run before the upgrade will not be reused in the chains and will run anew (only the last build on the default branch is affected if branches are used). This may also result in a clean checkout in the first run build for VCSs like Perforce. The behavior is similar to that after VCS root editing.

__Security__

When upgrading to 2017.2.x versions (please ignore when upgrading to 2018.1 and further versions): It is recommended to add "`teamcity.artifacts.restrictRequestsWithArtifactReferer=true`" [internal property](server-startup-properties.md) to enhance security of the server.

## Changes from 2017.2.1 to 2017.2.2

<anchor name="knownIssues_2017_2_2"/>

### Known issues
{id="known-issues-8"}

(Fixed 2017.2.3) If you use the Artifactory plugin and get the "Invalid RSA public key" browser message on opening build step settings, please apply the [workaround](https://youtrack.jetbrains.com/issue/TW-53562#comment=27-2698895).

Under Windows, when TeamCity server is started as a service, "logs\teamcity\-winservice.log" file is not created and server startup errors are nowhere to be seen. [Details](https://youtrack.jetbrains.com/issue/TW-54483)

### IDE Plugins

It is highly recommended to update the IDE plugins for all users to the latest version and then add the `teamcity.uploadPersonalPatch.requireAuthorization=true` [internal property](server-startup-properties.md) to enhance security of the server.

### Perforce VCS Root executable paths

__Since TeamCity 2017.2.2__, the field which specifies the path to p4 works only on the agent side, for agent\-side checkout.

For the server, the p4 binary should be present in the PATH of the TeamCity server (or can be specified via the `teamcity.perforce.customP4Path ` [internal property](server-startup-properties.md)). The `teamcity.perforce.p4PathOnServerWhitelist` [internal property](server-startup-properties.md) can be used to specify a  semi\-colon\-separated list of allowed p4 paths. The paths from this list can be set in VCS Root p4 path parameter for the server side (to restore old behavior).

### Mercurial VCS Root properties

__Since TeamCity 2017.2.2__, a number of Mercurial VCS root properties change their behavior for security reasons.
* the "HG command path" is used on the TeamCity server only if included into the [whitelist](https://confluence.jetbrains.com/display/TCD10/Mercurial#Mercurial-Pathtohgexecutabledetection)
* the "Clone repository to" property is hidden if the VCS root doesn't have it already and is ignored by default. To make TeamCity display the property in all VCS roots, add the `teamcity.hg.showCustomClonePath=true` [internal property](server-startup-properties.md). The value of the VCS root property is respected only if it is included into the whitelist specified by the `teamcity.hg.customClonePathWhitelist` [internal property](server-startup-properties.md), which is a semi\-colon\-separated list of directories where a clone is allowed. Use `/path/to/dir/*` to allow clones to the child directories of the `/path/to/dir`.
* the "Mercurial config" is ignored on the server. If you need to enable some Mercurial plugins, please do that in the global .`hgrc` on the TeamCity server machine.

## Changes from 2017.2 to 2017.2.1

### Kotlin DSL changes

The versions in pom.xml were updated: `kotlin.version` is updated to `1.2.0`, `teamcity.dsl.version` is updated to `2017.2.1`. The dependency on `kotlin-stdlib` is replaced with the dependency on `kotlin-stdlib-jdk8` in order to provide access to additional functionality available in jdk8 (e.g. named groups in regexps). The dependency on the deprecated `kotlin-runtime` and the redundant dependency on `kotlin-compiler-embeddable` were dropped.

Now TeamCity provides a parent maven project for Kotlin DSL which defines the `teamcity.dsl.version` and `kotlin.version` properties. With such a parent project, you will not have to update your pom.xml after eachTeamCity upgrade.

The easiest way to apply these changes is to run the 'Download settings in Kotlin format' action in project admin area and use the pom.xml from the zip produced by  theTeamCity server.

### Bundled Java used in Docker images

The bundled Java used in Docker images has been updated to 8u151.

## Changes from 2017.1.x to 2017.2

<anchor name="knownIssues_2017_2"/>

### Known issues
{id="known-issues-9"}

(Fixed 2017.2.1) TFS in Java working mode (when Team Explorer is not installed on the machine) report "TFS subsystem was destroyed" errors. See [TW-52685](https://youtrack.jetbrains.com/issue/TW-52685) for details.

Upgrading using Windows installer can take significant time if your TeamCity installation directory contains lots of nested directories (e.g. TeamCity Data Directory is under it). The long stage can occur after "Extract: Uninstall.exe..." progress message. In you encounter this long step, please wait for the completion of the operation (the installer runs icacls.exe utility as a nested process). To prevent the issue it is recommendd to move the [Data Directory](teamcity-data-directory.md) out of TeamCity server installation home.

### Perforce branch specification change

There is a breaking change which requires your action if you use Perforce streams with enabled feature branches and you're using a non\-default branch filter.

Starting from TeamCity 2017.2, Perforce VCS Roots use the same format for Perforce streams and TeamCity feature branches specification.

In the VCS Root branch specification for Perforce, `+:stream_name` must now be replaced with `+://stream_depot/stream_name`. Also, for better presentation of stream names in the UI, you may want to replace the default branch specifications like `+:*` with `+://your_stream_depot/*`.

This change was made in the scope of fixing [TW-48038 ](https://youtrack.jetbrains.com/issue/TW-48038).

### Server process restart

Now if the server process is stopped unexpectedly or killed, the process will automatically restart. The server should be stopped using the `teamcity-server.bat/sh stop` command which performs a graceful stop.

### Bundled plugins

#### Docker integration plugin

The Docker integration plugin is bundled since TeamCity 2017.2.x. If you installed the plugin for the previous version manually, please [remove it](https://confluence.jetbrains.com/display/TCD10/Installing+Additional+Plugins).

#### .NET CLI plugin

The .NET CLI (.NET Core) plugin is bundled since TeamCity 2017.2.x. If you installed the plugin for the previous version manually, please [remove it](https://confluence.jetbrains.com/display/TCD10/Installing+Additional+Plugins).

During upgrade all existing .NET Core build steps will be converted into .NET CLI steps and existing .NET Core plugin will be disabled.

__Note__: The `DotNetCore` and `DotNetCore_Path` agent configuration parameters will be changed to  `DotNetCLI` and `DotNetCLI_Path`; please consider updating your agent requirements which depend on these parameters.

### REST API
{id="rest-api-1"}

REST API uses version 2017.2. The previous versions of the API are still available under /app/rest/2017.1 (/app/rest/10.0), app/rest/9.1, /app/rest/9.0, /app/rest/8.1, /app/rest/7.0, /app/rest/6.0 URLs.

__buildType entity__

has "templates" sub\-element now instead of "template" to support multiple templates.

__build entity__ No longer expose boolean "running" attribute, textual "state" attribute with value "running" is used instead.

### Windows Versions Support

Windows XP and Vista are no longer the supported versions of Windows for the TeamCity Server and Agent. While the server and agent will still most probably work on these old versions, we do not target the versions during our development. [Let us know](feedback.md) if the support for the versions is important for your TeamCity usage or you find any issues with the systems support.

### J2EE Servlet 2.5 container is no longer supported

J2EE Servlet container version 2.5 is not supported since TeamCity 2017.2. TeamCity does not guarantee support for Tomcat 6.x and Jetty 7.x implementing Servlet 2.5. For .war distribution (not recommended, .tar.gz distribution is recommended), TeamCity supports Apache Tomcat 7\+, J2EE Servlet 3.0\+ and JSP 2.2\+.

### Other
{id="other-1"}

* The bundled Tomcat 8.5. restricted usage of special characters in the URL including curly bracket symbols (\{ \}). [Details](https://youtrack.jetbrains.com/issue/TW-51052).
* TeamCity integration with Intellij\-based IDEs no longer supports StarTeam and Visual Source Safe version controls.
## Changes from 2017.1.4 to 2017.1.5

The bundled JetBrains dotCover has been updated to version 2017.2

The SSH Agent build feature started to report a build problem if it fails to start an SSH agent with the specified SSH key (in the scope of [TW-42707](https://youtrack.jetbrains.com/issue/TW-42707)). Previously errors were only logged, but not reported as build problems. As a result, builds with invalid SSH Agent settings would start to fail after the upgrade.

## Changes from 2017.1.3 to 2017.1.4

### Known issues
{id="known-issues-10"}

TFS Personal support lists all build configurations for TFVC VCS root. See [TW-51497](https://youtrack.jetbrains.com/issue/TW-51497) for details.

## Changes from 2017.1.2 to 2017.1.3

[TW-50148](https://youtrack.jetbrains.com/issue/TW-50148) was fixed and the DSL API documentation was improved. If you need these changes for local development, please update the [maven dependency version](upgrading-dsl.md) to 2017.1.3.

Now TeamCity server runs 'git gc' automatically to improve performance of git operations. This requires a git client to be installed on the server and be accessible the server via the PATH environment variable. If a native git client cannot be found, then the corresponding health report is shown. For TeamCity to find the git client, the client needs to be installed on the server machine and added to `$PATH` (the server restart is required afterwards). Instead of modifying PATH, the path to the git client can be specified via the `teamcity.server.git.executable.path` [internal property](server-startup-properties.md).

## Changes from 2017.1.1 to 2017.1.2

No potential breaking changes.

## Changes from 2017.1 to 2017.1.1

The bundled IntelliJ IDEA has been updated to 2017.1.2

## Changes from 10.0.x to 2017.1

### Known issues
{id="known-issues-11"}

Editing cloud profile cancels all builds on profile agents. See [TW-49616](https://youtrack.jetbrains.com/issue/TW-49616) for details.

### TeamCity Versioning Changes

Since 2017, TeamCity adopts the common JetBrains versioning scheme that identifies versions by year following the pattern: &lt;year&gt;.&lt;number of the feature release within the year&gt;.&lt;bugfix update number&gt;. The current version is TeamCity 2017.1 formerly known as TeamCity 10.1.

### Update settings in Kotlin DSL

In this version TeamCity settings format has been changed and if your settings are stored in Kotlin DSL, you might need to [update Kotlin DSL scripts](upgrading-dsl.md) before continuing to use them. Check any related server health reports after the server upgrade.

<anchor name="2017.1-CSRF"/>

### CSRF Protection: Modifying GET requests and Proper Proxy configurations

TeamCity now implements [CSRF protection](csrf-protection.md) to improve web UI security and this introduces several changes in behavior which might affect your installation. In particular:
* If you use a __reverse proxy__ before TeamCity, the proxy should not change the original "Host" request header (this typically requires configuring the proxy to set the Host header to the original request value). Also, it should not modify the "Origin" and "Referer" headers present in the original request. While this has been the recommended setup for a long time, now it becomes critical for the TeamCity web UI functioning;
* if you use non\-bundled clients which perform HTTP __GET requests__ to TeamCity, some of the GET requests (those which change the state of the server, like http://server/action.html?add2Queue=XXX) stop working in 2017.1, please change the requests to use POST instead of GET;
* the non\-browser clients which reuse authentication by supplying the __TCSESSIONID__ cookie with the request, need to be updated to supply the "Origin" HTTP header with the value the same as the host the request is being sent to.
  If the check fails, you get the HTTP __403 response__ with the details of the failed check. The details are also logged into teamcity\-auth.log.

### Old IPR Runner

The old (TeamCity 6.0) IPR runner has been removed from the TeamCity. It was deprecated and has not been offered as an option since TeamCity 6.0, and now it is gone completely (the corresponding build configurations will not run anymore).

### Log4j configuration

It is recommended to overwrite the server's `conf\teamcity-server-log4j.xml` file with the content of the `conf\teamcity-server-log4j.xml.dist` file which represents the default logging configuration which has changed in this release. If you do need a custom logging configuration, consider using [logging presets](teamcity-server-logs.md) instead of modifying `conf\teamcity-server-log4j.xml`.

The same overwriting from the sibling .dist file is recommended for the TeamCity agents.The `conf\teamcity-*-log4j.xml.dist` file is created after the first start of the upgraded TeamCity version.

### Builds metadata storage (NuGet feed)

Builds metadata storage will be re\-created and builds will be reindexed right after the upgrade. As a result, immediately after the upgrade, the TeamCity internal NuGet feed will not contain all of the packages.During the builds reindexing, the following messages may appear in the `teamcity-server.log`:


```Shell

INFO - .index.BuildIndexer (metadata) - Enqueued next 100 builds for indexing, builds left: 2000, last build id: 3813
INFO - .index.BuildIndexer (metadata) - Enqueued next 100 builds for indexing, builds left: 1900, last build id: 3713
```



"builds left:" indicates the number of builds left to process. Note that TeamCity starts reindexing from the most recent builds, so all fresh builds should appear in the TeamCity NuGet feed in a relatively short time.

To increase the metadata indexing speed you could use the [following tip](known-issues.md).

### REST API
{id="rest-api-2"}

REST API has only minor changes, so the same API is exposed under the `app/rest/10.0` and `/app/rest/2017.1` URLs. API version has been updated to 2017.1 though to reflect the changes.The build's node "triggeredBy" now has more correct values of "type" attribute for the builds started after 2017.1 upgrade. In particular, the "buildType" value is not used anymore, the"finishBuild", "snapshot", etc. values are used instead.

### Visual Studio Add-in fails to install from TeamCity UI

As a workaround, [ReSharper web installer](https://www.jetbrains.com/resharper/download/#section=web-installer) could be used. See [TW-51680](https://youtrack.jetbrains.com/issue/TW-51680) for details.

## Changes from 10.0.4 to 10.0.5

If you are using TFS with agent\-side checkout, note that due to the fix of  [TW-48555](https://youtrack.jetbrains.com/issue/TW-48555)  TeamCity will have to re\-create TFS workspaces, which may result in a clean checkout on the agents after the upgrade.

## Changes from 10.0.3 to 10.0.4

### Precedence of dep.ID.NAME parameter references

When `%\dep.ID.NAME%` parameter reference is used and there are several dependency paths to the same build configuration with id "ID" so that different builds are accessible via (direct or indirect) artifact dependencies, the result of the reference resolution could have used any of the builds without any guaranteed precedence.

Since 10.0.4 `dep.` parameter resolution works as follows:
1. if there is a snapshot dependency, the build from the same chain wins.
2. if there is no snapshot dependency and several builds are accessible via an artifact dependency, the build with a greater `buildId` wins. If there are several artifact dependencies from a single build configuration, only the first one is considered.

### Updates

AWS SDK has been updated to 1.11.66 to support new instance types (r4.4xlarge, f1.16xlarge, t2.2xlarge, t2.xlarge, r4.2xlarge, r4.xlarge, r4.large, r4.16xlarge, r4.8xlarge, f1.2xlarge).

## Changes from 10.0.2 to 10.0.3

### Amazon EBS-Optimizied Instances

The behavior of [EBS optimization](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSOptimized.html), enabled by default since TeamCity 10.0, is changed similarly to what EC2 console offers:

1. EBS optimization is turned on by default for `c4.*`, `m4.*`, and `d2.*` (non-configurable).
2. EBS optimization is turned off by default for any other instance types.
3. EBS optimization can be turned on for instances that support it (such as `c3.xlarge`) by checking the appropriate box when configuring the image of the Amazon cloud profile.

### Bundled Tools Updates
{id="bundled-tools-updates-10"}

The bundled dotCover has been updated to version 2016.2.2

## Changes from 10.0.1 to 10.0.2
* The known issue mentioned for 10.0.1 is fixed.
* The bundled JetBrains dotCover has been updated to version 2016.2.
* Jabber integration is now [more restrictive](https://youtrack.jetbrains.com/issue/TW-47046) with regard to SSL connection checking. If you are using jabber.org for sending notifications, and face a problem regarding SSL certificate, you should either enable "Use legacy SSL" option or (better) change JVM version for running TeamCity to at least 1.8.0\_101.
* Precedence of %\dep.ID.NAME% parameters resolution was changed unintentionally in case of several different builds used as dependency. See [TW-47518](https://youtrack.jetbrains.com/issue/TW-47518) for details.

## Changes from 10.0 to 10.0.1

All known issues mentioned for 10.0 are fixed.

### Known Issues
{id="known-issues-12"}

(fixed in 10.0.2) TeamCity server temp folder can fill up if an [agent tool](installing-agent-tools.md) is installed as a directory in \<[TeamCity Data Directory](teamcity-data-directory.md)\>\/plugins\/.tools. [Details and workaround](https://youtrack.jetbrains.com/issue/TW-46648).

## Changes from 9.1.x to 10.0

### Known Issues
{id="known-issues-13"}

(These known issues are fixed in 10.0.1)

#### Failed to collect TFS changes - From version &lt;x&gt; is greater then current version &lt;y&gt;

If you use TFS version control and get "Error collecting changes for VCS repository ... Failed to collect TFS changes \- From version x is greater then current version y" error, either commit a new change so that it appears in the affected build configuration, ot install an [updated plugin](https://youtrack.jetbrains.com/issue/TW-46336#comment=27-1534797) (related [issue](https://youtrack.jetbrains.com/issue/TW-46336)).

#### Project administrator may not be able to re-define parameters inherited from the parent project

See request [TW-46372](https://youtrack.jetbrains.com/issue/TW-46372) for details and possible workaround.

#### Upgrade form TeamCity version before 8.1 fails with "Can't take exclusive lock when db lock is not held"

See request [TW-46385](https://youtrack.jetbrains.com/issue/TW-46385) for details and possible workaround.

#### Subversion VCS roots with svn+ssh:// protocol can report "Host key (xxx) can not be verified."

See request [TW-46385](https://youtrack.jetbrains.com/issue/TW-46489)  for details and for the plugin with the fix


<!--[//]: # (Internal note. Do not delete. "Upgrade Notesd333e995.txt")-->


### Changes in agent properties reporting .NET 4.x runtime

TeamCity agents before version 10 used to report DotNetFramework4.0\_\* properties whenever any of 4.x versions of .NET framework were installed on the agent. Since TeamCity 10, DotNetFramework4.0\_\* properties are reported only when 4.0 runtime (without updates) is installed. For 4.5.\*, 4.6.\* updates corresponding DotNetFramework4.N\_\* properties are reported. This change in behavior allows for more precise requirements definition.

If after the upgrade you get incompatible agents with __Unmet requirement: Exists=&gt;DotNetFramework4.0\_x86(/x64) exists__ message, review the explicit requirements in your build configuration. If your build is compatible with any .NET 4.x runtime (it is a most common case) please use __Exists=&gt;DotNetFramework4.\*\_x86(/x64)__ requirement. If you would like to run on .NET 4.5\+ agents \- use  __Exists=&gt;DotNetFramework4.(5|6).\*__ requirement.

Some third\-party plugins are known to be affected. On the upgrade, make sure to upgrade __xUnit__ plugin to version [1.1.2+](https://github.com/carlpett/xUnit-TeamCity/releases/tag/1.1.2)

### Ignored tests table optimization

During upgrade TeamCity will optimize data in the ignored\_tests table (we do that in order to speedup the TeamCity built\-in backup / restore process). In some rare cases, when this table contains millions of rows, the process of table optimization may take significant time \- possibly a few hours. Among other data, the table contains the reasons why a particular test in a build was marked as ignored. If this information for old builds is not very important for you, you can start TeamCity server with the additional [JVM option](server-startup-properties.md#JVM+Options): `-Dteamcity.truncateIgnoreReasonConverter.copyReasons=false`

In this case TeamCity will not copy the ignore reasons into the new, optimized table, and this particular step of the upgrade process will run much faster.

### Java 8

Starting from TeamCity 10, the TeamCity server __[requires Java 8 ](supported-platforms-and-environments.md)__ JRE/JDK (included in the Windows `.exe` distribution).

__TeamCity agents__ currently require Java 1.6\+, but starting from the next TeamCity version, the minimum requirement for [the Java on the agent](supported-platforms-and-environments.md) will be Java 8 (included in agent's Windows `.exe` distribution). It is recommended that you now consider upgrading the agents Java.

__Java memory options change__

It is recommended to remove the " `-XX:MaxPermSize=..."`  [JVM option](server-startup-properties.md) from `TEAMCITY_SERVER_MEM_OPTS` environment variable, if previously configured. (This is due to the fact that Java 8 [does not use](https://javaeesupportpatterns.blogspot.com/2013/02/java-8-from-permgen-to-metaspace.html) permanent generation (PermGen) anymore)

### Agent requirements and artifact dependencies disabling

Agent requirements and artifact dependencies can be disabled now. TeamCity plugins and REST API\- based code using a version of API prior to TeamCity 10 is likely to ignore the disabled status of these settings.

### TFS

TeamCity comes with cross\-platform TFS integration: to work with TFS, you no longer need to install a TeamCity server on a Windows machine.

### Visual Studio Online Work Items plugin

Visual Studio Online Work Items plugin is __obsolete since TeamCity 10.0__ and can be safely removed. TeamCity 10.0 has a built\-in integration with [Team Foundation Work Items](https://confluence.jetbrains.com/display/TCD10/Team+Foundation+Work+Items) which supports TFS 2010\+ and Visual Studio Team Services. After upgrade, TeamCity will detect the existing issue tracker connections of this plugin and convert them into TFS Work Items.

### Default Checkout mode for newly created build configurations

The default setting for the VCS checkout mode on creating new build configurations has changed: now TeamCity will check out the sources on the agent before the build. If the agent\-side checkout is not possible, TeamCity  will use the server\-side checkout. Explicit server\-side or agent\-side checkout is still in place. The new default applies only to newly created build configurations; all the existing ones will work as configured before.

### Project-based Agent Management Permissions

New TeamCity installations now have different agent management permissions assignments: Project Administrator role does not include (global) Agent Manager role. Instead, Project administrator role has [agent-project permissions](managing-roles-and-permissions.md#Project-level+Agent+Management+Permissions) which allow managing agents from the agent pools with only projects where user has Project Administrator role.

Existing installations are not affected by this change in order not to change the user permissions. However, it is recommended to review the Project Administrator role and consider excluding "Agent Manager" role and adding the following permissions:
* Enable / disable agents associated with project
* Start / Stop cloud agent for project
* Change agent run configuration policy for project
* Administer project agent machines (for example, reboot, view agent logs)
* Remove project agent
* Authorize project agent
### UI Changes

#### Server Administration UI

The new __Administration | Tools__ page allows setting up tools to be used by appropriate plugins. Tools are automatically distributed to all build agents and can be used in related runners.

#### New Create project / Create build configuration buttons

The new __Create subproject__ and __Create build configuration__ buttons have a drop\-down now allowing you to select whether you want to create a project from scratch (manually), from URL, or using the popular version control systems GitHub.com and Bitbucket.

#### NuGet-related UI

NuGet settings page is removed. NuGet.exe can be installed using the new __Tools__ page;  to set up TeamCity as a NuGet Server, go to the  __Administration__  |  __NuGet Feed__  page.

#### Tests-related UI

The Problematic Tests tab is no longer available and the View all tests failed within the last 120 hours link is removed from the the  Current Problems tab.

TeamCity now detects [Flaky tests](viewing-tests-and-configuration-problems.md) displayed on the dedicated tab for a given project.

### Visual Studio Add-in

The legacy version of the TeamCity Visual Studio Add\-in is no longer supported. Visual Studio 2005 and 2008 are not supported.

TeamCity Visual Studio Add\-in is shipped [as a part of ReSharper Ultimate](visual-studio-addin.md). After installation, the TeamCity Add\-in will be available under the RESHARPER menu in Visual studio.

Note that the installer will remove the pre\-bundle products versions: TeamCity and ReSharper versions prior to 9.0, dotCover prior to 3.0, dotTrace prior to 6.0.ReSharper Ultimate does not support the Visual Studio versions 2005 and 2008.

### IntelliJ IDEA Compatibility

IntelliJ IDEA 12.1 and older, as well as other IntelliJ-based products released prior to year 2013, is no longer supported by [IntelliJ Platform Plugin](intellij-platform-plugin.md).

### Snapshot dependencies builds rebuilding

After the server upgrade, the builds used as snapshot dependencies may be rebuilt once even if the snapshot dependency has the "Do not run new build if there is a suitable one" option set to ON. This is done to fix the [issue](https://youtrack.jetbrains.com/issue/TW-43226).

### Perforce

Clean checkout will be enforced in builds with Stream-based and Client-based Perforce VCS Roots.

### Subversion

Starting from TeamCity 10, TeamCity does not accept by default connections to SVN servers accessed by https:// protocol with non-trusted server SSL certificate. To enable access with such certificates, you should either import the certificate to server JVM keychain, or enable VCS Root option "__Accept non-trusted SSL certificate__" (Enable non-trusted SSL certificate in 10.0) ([issue](https://youtrack.jetbrains.com/issue/TW-45592)).

### Bundled Tools Updates
{id="bundled-tools-updates-11"}

Ant runner: the bundled Ant distribution has been upgraded from 1.9.6 to 1.9.7

.NET dotCover coverage: the bundled dotCover is updated to 2016.1

ReSharper command line tools: the bundled R# CLT is updated to 2016.1

Java inspections and duplicates: the bundled IntelliJ IDEA is updated to 2016.2

### GitHub Issue Tracker

If you were using the TeamCity-GitHub [third-party plugin](https://github.com/milgner/TeamCityGithub)  prior to TeamCity 10.0, you can safely [remove](installing-additional-plugins.md) it: the built-in TeamCity integration will detect the existing connection to GitHub issue tracker and pick up your settings automatically.

### NuGet Support

Configuration parameters `teamcity.tool.NuGet.CommandLine.%\NUGET_VERSION%.nupkg` are not reported anymore. The `teamcity.tool.NuGet.CommandLine.%\NUGET_VERSION%` parameters should be referenced instead.

For example, instead of using `%\teamcity.tool.NuGet.CommandLine.DEFAULT.nupkg%` parameter reference `%\teamcity.tool.NuGet.CommandLine.DEFAULT%` should be used.

### Build Statistics

Several statistic values (metrics) has been reworked and renamed:
* `BuildCheckoutTime` into `buildStageDuration:sourcesUpdate`
* `BuildArtifactsPublishingTime` into `buildStageDuration:artifactsPublishing`
* `ArtifactsResolvingTime` into `buildStageDuration:dependenciesResolving`

Old keys are still supported in charts definitions.

### REST API
{id="rest-api-3"}

REST API uses version 10.0.  The previous versions of the API are still available under `/app/rest/9.1`,` /app/rest/9.0`,` /app/rest/8.1`, `/app/rest/7.0`,` /app/rest/6.0` URLs.

Requests for a set of items with the locator addressing a single item which resulted in 404 responses previously will now return an empty set as a more consistent approach. For example, `.../app/rest/builds?locator=id:<non-existent build id>`. REST debug logging might have diagnostics message with more details as to the case.

Requests for set of items __may return not all/incomplete results__ (with zero or more items included) and provide `nextHref` sub-element with the link to retrieve the next "page" of items. The search result is complete when no `nextHref` sub-element is provided.

Some requests for set of items (e.g. ...`/app/rest/vcs-roots` and .../`app/rest/vcs-root-instances`) will use paged results by default when queried without a locator (they used to list all the items). Add the "`count:NNN"` locator dimension to set page size.

__Finding builds__ (`.../app/rest/builds/...` URL)   
When performing builds scan to find those matched by the locator specified, by default for performance reasons TeamCity will return partial result limited by scanning only 5000 most recent builds. To process a larger portion of the history, check the `nextHref` attribute returned or set the `lookupLimit` locator dimension to a larger value.   
Previously, until specifically requested the builds from non-default branch as well as canceled, personal and failed to start builds were not returned. Now these filtered out builds are returned by default for running and queued builds queries as well as when filtering by agent or user. Use `defaultFIlter:true/false` locator dimension to manage the default filtering explicitly.   
Also, `number:NNN` locator now adheres to the same default logic: only "usual" finished builds from default branch are searched by number and several builds can be returned if found.

__Finding VCS roots__ (`.../app/rest/vcsRoots/... URL`)(minor)   
A VCS root locator with `project` and `buildType` specified used `project` as the context for finding `buildType`. This is no longer the case,the  `buildType` locator should be full one to find the build configuration.

__Build Configuration's Artifact Dependencies__ (entities returned by` .../app/rest/buildTypes/...` URL)   
The `artifact-dependencies` sub\-element of the `buildType` element now uses textual generated ids instead of numeric ones which depended on the order previously. This also affects requests for artifact dependencies modification.   
The `agent-requirements` sub\-element of the `buildType` element now uses generated ids instead of the parameter name as id. This also affects requests for agent requirements modification.

__Editing agent requirements__ (`.../app/rest/buildTypes/.../artifact-requirements/...` URL)   
Previously, on adding  a new agent requirement for the same parameter, the existing one was overridden by the new one; now a new one is added. Previously, on adding  a new agent requirement, the parameter name was derived from the `id` attribute of the `agent-requirement` node. Since TeamCity 10, the parameter name is derived from the "`property-name`" property.

__Test and problem occurrences__ (`.../app/rest/testOccurrences`,` .../app/rest/problemOccurrences` URL)   
The sorting of the returned results is changed for some of the queries compared to the previous versions. For example, the "`../app/rest/testOccurrences?locator=build:(xxx)`" request now returns the tests in the order they were run in the build.   
Previously, test occurrences were sorted by the new status and then by name. Problem occurrences were sorted by problem id.   
Also, the `build` dimension in the test/problem\-related locators now supports multiple builds so for the requests which matched several builds via the "`build`" dimension, all the builds will be processed; previously only the first matching build was processed.

__Entities__   
The `property` entity used to have its "own" Boolean attribute indicating whether the parameter is redefined in the build configuration as opposed to inherited from a template/project. Now the attribute is renamed to '`inherited`' and its value is inverted.   
The `vcs-root` and `vcs-root-instance` used to have `status` and `lastChecked` attributes. Now VCS root instance has `status` element (with `current` node which has `status` and and `timestamp` attributes) and VCS root does not have the data as it produced undefined results in case of several VCS root instances.   
The `test` entity `id` became Sring instead of Long (due to [inability](https://youtrack.jetbrains.com/issue/TW-42028) to represent some Java Long value in JavaScript)

__Build type and template__   
The `settings` node used to contain all the supported settings. Now only those defined in the build configuration or template are present. The same is true for `.../app/rest/buldTypes/XXX/settings/*` requests: only the values changed from defaults are present.

__Compatible agents__   
When querying for compatible agents, only the agents which can actually run the builds are now returned. By default, unauthorized, disconnected and disabled agents are not listed. This behavior differs from that in previous versions which had a number of discrepancies. Affected requests and entities: `.../app/rest/agents?locator=compatible:(...)`; `../app/rest/agents/.../compatibleBuildTypes` and `incompatibleBuildTypes`; nested nodes `Agent.compatibleBuildTypes`, `QueuedBuild.compatibleAgents`, `BuildType.compatibleAgents`.

## Changes from 9.1.6 to 9.1.7

No potential breaking changes.

## Changes from 9.1.5 to 9.1.6

#### Known Issues
{id="known-issues-14"}

There is a [known issue](https://youtrack.jetbrains.com/issue/TW-43731) in the bundled dotCover run on Windows XP and Vista. You can use the [hotfix](https://youtrack.jetbrains.com/issue/TW-43731#comment=27-1286417) or the [workaround ](https://youtrack.jetbrains.com/issue/TW-43731#comment=27-1286522)provided. The issue will be fixed in the next dotCover release.

There is a [known issue](https://youtrack.jetbrains.com/issue/TW-44479) with [NuGet Credentials](https://confluence.jetbrains.com/display/TCD9/NuGet+Feed+Credentials) not working for the Authenticated Feed URL of the internal TeamCity NuCet server on the local agents. As a workaround, instead of using `%\teamcity.nuget.feed.auth.server%`, please specify the external server URL in the build step and the build feature.

#### NuGet

NuGet versions prior to 2.8.6 require .Net Framework 4.0\+ installed on the build agent, NuGet 2.8.6 and later requires .NET 4.5.

#### NUnit

Since version 9.1.6, TeamCity does not support NUnit 3 beta versions (released before NUnit 3.0.0).

The "Run a process per assembly" option of the [NUnit runner](nunit.md) has been removed from NUnit 3 settings. Configure the desired behavior using the required [command line options](https://github.com/nunit/nunit/wiki/Console-Command-Line) in the [corresponding field](nunit.md).

#### Bundled Tools Updates
{id="bundled-tools-updates-12"}

Bundled IntelliJ IDEA updated to version # 143.1945 (roughly equivalent to 15.0.3 with a few additional fixes).

Bundled version of Maven 3.2.x updated to 3.2.5.

#### Performance Monitor
{id="performance-monitor"}

Note on permissions: to [monitor performance](performance-monitor.md) of a build agent run as a Windows [service](https://confluence.jetbrains.com/display/TCD9/Setting+up+and+Running+Additional+Build+Agents#SettingupandRunningAdditionalBuildAgents-BuildAgentasaWindowsService), make sure the user starting the agent is member of the Performance Monitor Users group.

## Changes from 9.1.4 to 9.1.5

#### Known Issues
{id="known-issues-15"}

There is a [known issue](https://youtrack.jetbrains.com/issue/TW-43731) in the bundled dotCover run on Windows XP and Vista. You can use the [hotfix](https://youtrack.jetbrains.com/issue/TW-43731#comment=27-1286417) or the [workaround](https://youtrack.jetbrains.com/issue/TW-43731#comment=27-1286522) provided. The issue will be fixed in the next dotCover release.

#### Product icons

JetBrains product icons are updated in accordance with the [new JetBrains branding](https://blog.jetbrains.com/blog/2015/12/10/the-drive-to-develop/).

#### Git

Since TeamCity 9.1.5, `git sparse-checkout`  [is disabled](https://youtrack.jetbrains.com/issue/TW-43330#comment=27-1229510) by default. To enable it in a TeamCity project, add the `teamcity.git.useSparseCheckout=true` [parameter](configuring-build-parameters.md) to this project.

#### Gradle: Breaking change compared to 9.1.2

Gradle runner `system.*` [properties](gradle.md), introduced in __TeamCity 9.1.2__, defined for the build as JVM's system properties of the Gradle process, will not work __since 9.1.5__. Now the TeamCity system properties can be accessed in Gradle scripts as Gradle properties (similarly to the ones defined in the `gradle.properties `file) and are to be referenced as follows:

a) name allowed as a Groovy identifier (the property name does not contain dots): `customUserProperty`   
b) name not allowed as a Groovy identifier (the property name contains dots, e.g. `build.vcs.number.1`): `project.ext["build.vcs.number.1"]`

#### Bundled Tools Updates
{id="bundled-tools-updates-13"}

The bundled JetBrains IntelliJ IDEA (IDEA inspections and duplicates) has been updated to version 15.0.2

#### .NET tools updates

JetBrains ReSharper command line tools (.NET inspection and duplicates) have been updated to match ReSharper 10.0.2 releaseTeamCity Visual Studio Add-in Web installer updated to ReSharper 10.0.2 releaseBundled JetBrains dotCover updated to version 10.0.2

## Changes from 9.1.3 to 9.1.4

#### Known Issues
{id="known-issues-16"}

Certain roles/permissions configurations can result in error loading roles and no ability for regular users to view projects. In such cases the  "__Circular reference is detected between roles__" critical server error is displayed on the server administration pages for those logged in as server administrator. Please check the [workaround](https://youtrack.jetbrains.com/issue/TW-43186#comment=27-1211556) for the issue.

__Git agent\-side checkout__ may malfunction (details at [TW-43202](https://youtrack.jetbrains.com/issue/TW-43202) ) when the `teamcity.git.use.native.ssh=true` parameter is specified in a build configuration or in the agent config. To fix that, install the [#snapshot-34](https://teamcity.jetbrains.com/viewType.html?buildTypeId=TeamCityPluginsByJetBrains_Git_JetBrainsGitPluginTeamCity91x) build of the Git\-plugin.

__Git agent\-side checkout__ works incorrectly with git client versions 1.7.0\-1.7.4: the checkout directory contains files only, all directories are missing (details at [TW-43330](https://youtrack.jetbrains.com/issue/TW-43330)).  To work around the problem, add the `teamcity.git.useSparseCheckout=false` parameter in the Root TeamCity project.

#### TeamCity Windows binaries signatures

Since 9.1.4  the TeamCity Windows binaries are signed with SHA\-2 code\-signing certificate following the [Microsoft SHA-2 policies](http://social.technet.microsoft.com/wiki/contents/articles/32288.windows-enforcement-of-authenticode-code-signing-and-timestamping.aspx). This means that on systems prior to Windows XP SP3, the executables will not pass code signing verification; newer Windows systems require the corresponding security update from Microsoft.

#### Bundled Tools Updates
{id="bundled-tools-updates-14"}

Bundled Oracle JRE (in both Server and Agent.exe installers) has been updated to version  1.8.0\_66 (32\-bit)

#### .NET tools updates
{id="net-tools-updates-1"}

JetBrains ReSharper command line tools (.NET inspection and duplicates) have been updated to match ReSharper 10.0 release

TeamCity Visual Studio Add-in Web installer updated to ReSharper 10.0 release

Bundled JetBrains dotCover updated to version 10.0

## Changes from 9.1.2 to 9.1.3

#### Known Issues
{id="known-issues-17"}

There is a [known issue](https://youtrack.jetbrains.com/issue/DCVR-7708) in the bundled dotCover 3.2 which could cause a build's failure with the following exception: "System.Security.VerificationException: System.Security.VerificationException: Operation could destabilize the runtime". The issue is fixed in dotCover 10.0 bundled with TeamCity 9.1.4 release.

Bundled JVM (Server Windows installer and Agent Windows installer) is updated which results in disabled SSL RC4 chiper suite for outgoing HTTPS connections. For example this makes connections to CloudForge SVN servers non\-functional with "SSLHandshakeException: Received fatal alert: handshake\_failure" error. ([details](https://youtrack.jetbrains.com/issue/TW-42577))

Changes from 9.1.1 to 9.1.2

#### Known issues
{id="known-issues-18"}

The Command line runner can fail to execute a custom script if it has a non\-default hashbang specified at the beginning of the script: [TW-42498](https://youtrack.jetbrains.com/issue/TW-42498)

When using Amazon EC2 cloud integration, an AMI\-image, containing build agent versions 9.1.2\-9.1.5 will appear twice with name EC2\-i\-abcdefgh and EC2\-i\-abcdefgh\-1 consuming 2 licenses. To fix the issue please use an AMI\-image with agent 9.1.6\+. Just updating server to 9.1.6 won't help, if AMI remains the same. ([details](https://youtrack.jetbrains.com/issue/TW-43992))

#### Build status icons

Build status icons updated to a more "standard" look and are of a bit larger now.

#### Bundled Tools Updates
{id="bundled-tools-updates-15"}

JetBrains ReSharper command line tools (.NET inspection and duplicates) have been updated to match ReSharper 9.2 release

TeamCity Visual Studio Add-in Web installer updated to ReSharper 9.2 release

Bundled JetBrains dotCover updated to version 3.2

Bundled Oracle JRE (in both Server and Agent .exe installers) updated to version 1.8.0_60 (32-bit)

## Changes from 9.1 to 9.1.1

Bundled Jacoco coverage library updated to version 0.7.5

Perforce VCS Roots with disabled ticket authentication won't run 'p4 login' operation anymore if password authentication is disabled on the Perforce server.   
I.e. if password authentication is disabled, "Use ticked\-based authentication" option must be enabled on the VCS Root. [TW-42818](https://youtrack.jetbrains.com/issue/TW-42818)

<anchor name="UpgradeNotes-Changesfrom9.0.xto9.1"/>

## Changes from 9.0.x to 9.1

#### Bundled Ant

Bundled Ant distribution has been upgraded form 1.8.4 to 1.9.6. Note that Ant build steps using bundled Ant will use another version of Ant after the server upgrade.  Ant 1.9.6 requires Java 1.5 at least, so builds using Ant and running under Java 1.4 will stop working.

#### MSTest runner converted into Visual Studio Tests runner

MSTest runner is merged with [VSTest console runner](https://confluence.jetbrains.com/display/TW/VSTest.Console+Runner) (previously provided as a separate plugin) into the [Visual Studio Tests](visual-studio-tests.md) runner. Note that after upgrade to TeamCity 9.1, MSTest build steps are automatically converted to the Visual Studio Tests runner steps, while VSTest steps remain unchanged.


> If you have used [VSTest.Console runner plugin](https://confluence.jetbrains.com/display/TW/VSTest.Console+Runner), make sure that you have latest version (build __32407__) installed. The plugin version can be viewed on __Administration | Plugins List__ page. Earlier versions of this plugin are __not compatible__ with TeamCity 9.1 and may cause malfunction of .NET related build runners which can manifest with `java.lang.NoSuchMethodError: jetbrains.buildServer.runner.NUnit.NUnitVersion.parse(Ljava/lang/String;)` build errors. The plugin can be downloaded from [its page](https://confluence.jetbrains.com/display/TW/VSTest.Console+Runner).
Consider migrating your vstest.console execution steps to the bundled Visual Studio Tests runner.
>
{style="note"}

#### MSTest installation agent properties

TeamCity agent automatically detects the installed MSTest and used to expose the locations in `system.MSTest.N.N` system properties.

__Since TeamCity 9.1__, the locations are exposed via `teamcity.dotnet.mstest.N.N` configuration parameters. Check [TW-41845](https://youtrack.jetbrains.com/issue/TW-41845) for a workaround if you cannot easily change the properties usage.

#### Nested test reporting

Previously TeamCity supported a case when one test could have been reported from within another test using [service messages](service-messages.md). Now, after the fix of [TW-40319](https://youtrack.jetbrains.com/issue/TW-40319), starting another test finishes the currently started test in the same "flow". To still report tests from within other tests, you will need to specify another [flowId](build-script-interaction-with-teamcity.md) in the nested test service messages.

#### REST API
{id="rest-api-4"}

REST API uses version 9.1. Previous versions of API are still available under `/app/rest/9.0`, `/app/rest/8.1`, `/app/rest/7.0`, `/app/rest/6.0` URLs.

__Finding builds__   
Summary (tl;dr): Some build filtering rules has subtle changes. Most importantly, a queued build can now be returned instead of 404 when searching by build id and meaning of the "project" locator dimension has changed to be not recursive. Also, failed to start builds are now not included until "failedToStart:any" locator dimension is specified.


<!--[//]: # (Internal note. Do not delete. "Upgrade Notesd333e2056.txt")-->


Details:   
Affected requests: /app/builds/&lt;locator&gt;..., /app/builds?locator=&lt;locator&gt;, /app/buildTypes/&lt;btLocator&gt;/builds and others with build locator

locator: id:&lt;number&gt; or taskId:&lt;number&gt;
* previously, if the matching build was a queued one, 404 (Not Found) was returned
* now the queued build is returned

locator: project:&lt;id&gt;...
* previously, all the builds belonging to build configurations of the project and all it's subprojects (recursively) were found
* now only the builds belonging to build configurations of the project specified are found. For finding the builds recursively, use "affectedProject:&lt;id&gt;." dimension. This makes the usage consistent with build type locators.

locator: tag:&lt;text&gt;
* previously, when "&lt;text&gt;" used ":" character, that used to treat the entire "&lt;text&gt;" as tag name
* now the "&lt;text&gt;" is parsed as a nested locator. For searhing tags with ":" character, locator "tag:(name:(&lt;tag&gt;))" should be used

locator: &lt;text&gt;
* previously, if &lt;text&gt; is not a number, response was 400 (Bad Request) with "LocatorProcessException: Invalid single value: '&lt;text&gt;'. Should be a number." message.
* now search by build number across all builds on the server is performed (this is not recommended to be used on production servers). For not found builds 404 (Not found) response is returned

locator: id:&lt;number&gt;,xxx:yyyy
* previously, build was found by id "&lt;number&gt;, other dimensions were ignored
* now if the build found by id does not match other dimensions, response is 404 (Not Found)

locator: agent:&lt;agentLocator&gt;
* previously &lt;agentLocator&gt; was used as is, without applying any defaults (unauthorized agents were included until specifically excluded)
* now &lt;agentLocator&gt; has the same behavior as in /app/rest/agents request: unauthorized agents are excluded by default

__Finding projects__   
Searching project by name used to return 404 error is several projects were matched. Now will return the first project found.

__Build's artifacts__   
There are several bugs fixed in listing build artifacts via /app/rest/builds/&lt;locator&gt;/artifacts/\* requests which can cause subtle changes in the results for the request. Check the new behavior if you relied on the response.The most important changes are:
* the initial path specified via URL part is searched for without current locator value, it will not generate 404 responses until there is no such artifact on disk.
* archives are not treated as directories (do not have children elements) by default. Specify "browseArchives:true" to treat archives as directories (in "recursive:true" mode only one level of archives is treated as directories)

__Agents__   
Agents which are not known to the system (which were deleted) used to have id "\-1". Now "id", "pool" and some other entries are no longer included for such agents.

#### Xcode 7 Support

[Experimental support](xcode-project.md) for Xcode 7 has been added.

#### Issue trackers integration

Due to API changes, third party issue trackers integration plugins might not be compatible with TeamCity version 9.1. Old plugins will not work and will report "java.lang.NoSuchMethodError: jetbrains.buildServer.issueTracker.AbstractIssueProviderFactory.&lt;init&gt;(Ljetbrains/buildServer/issueTracker/IssueFetcher;Ljava/lang/String;)V" error in `teamcity-server.log` log (more details in the [issue](https://youtrack.jetbrains.com/issue/TW-41508)). If you observe such errors, please contact the [plugin authors](https://plugins.jetbrains.com/teamcity). If you are the author of affected plugin, please refer to the related notes in [Open API Changes](https://confluence.jetbrains.com/display/TCD18/Open+API+Changes).

## Changes from 9.0.4 to 9.0.5

No potential breaking changes.

## Changes from 9.0.3 to 9.0.4

No potential breaking changes.

## Changes from 9.0.2 to 9.0.3

No potential breaking changes.

## Changes from 9.0.1 to 9.0.2

No potential breaking changes.

## Changes from 9.0 to 9.0.1

#### Known Issues
{id="known-issues-19"}

If you have enabled versioned settings for projects which use meta\-runners in TeamCity 9.0, on upgrade and following commit into the settings VCS root, the meta runners will be deleted from the server. Workaround is to commit the meta\-runners definitions into the settings repository manually. Related issue: [TW-39519](https://youtrack.jetbrains.com/issue/TW-39519).

#### Oracle 10.x JDBC driver is not supported anymore

Due to missing support for national character sets (nvarchar) in Oracle 10.x JDBC drivers, TeamCity 9.0.1 will ask to upgrade Oracle JDBC drivers to the latest version. The minimal supported version of Oracle JDBC driver is 11.1.

## Changes from 8.1.x to 9.0

#### Known Issues
{id="known-issues-20"}

* If you have custom artifact clean-up rules configured which mention ".teamcity" directory, build logs can be deleted by the clean-up procedure. Make sure you have build logs backup before upgrade and remove all the custom artifacts cleanup rules with ".teamcity". Related issue: [TW-40042](https://youtrack.jetbrains.com/issue/TW-40042). This issue is fixed in 9.0.3 release.
* If you use Microsoft SQL Server database with TeamCity, after the scheduled clean-up background run, TeamCity UI pages can lock until the server restart. See [TW-39549](https://youtrack.jetbrains.com/issue/TW-39549) for details. This issue is fixed in 9.0.2 release.
* If you use LDAP authentication on the server and there are lots of login attempts on the server (e.g. there is an active REST\-using script), OutOfMemory errors can occur and require server restart. Consider installing an LDAP plugin with a fix from the [issue](https://youtrack.jetbrains.com/issue/TW-39316). This issue is fixed in 9.0.1 release.
* If you have large Maven projects, you can see builds failing with OutOfMemoryError. This is caused by update of back\-end embedded Maven to 3.2.3 which has bigger memory footprint. Consider increasing Build Agent [memory limits](https://confluence.jetbrains.com/display/TCD9/Configuring+Build+Agent+Startup+Properties) Related issue: [TW-41052](https://youtrack.jetbrains.com/issue/TW-41052)

#### UUID in XML settings files

__Since TeamCity 9.0__, disk\-stored XML settings definitions of projects, build configurations and VCS roots have unique non\-human\-readable id (uuid) stored. These ids are automatically generated and are assumed to globally unique. On settings files copying, you need to change/make unique not only id (file name) and name (across siblings) of the entity , but also remove it's uuid from the file. TeamCity will generate new uuid automatically.

#### Build logs storage

The location of the build logs in the internal format stored under [TeamCity Data Directory](teamcity-data-directory.md) has changed. The build log files in internal format are now stored under hidden build artifacts.Namely, the location has changed from `system/messages/CHyy/xxyy.*` to `system/artifacts/<PROJECT EXTERNAL ID>/<BUILD CONFIGURATION NAME>/xxyy/.teamcity/logs/buildLog.*`.

Old build logs are migrated to the new location on TeamCity server startup ([TW-37362](https://youtrack.jetbrains.com/issue/TW-37362)). To avoid this migration, `teamcity.skip.logs.migration` internal property should be set __before__ server startup.


<!--[//]: # (Internal note. Do not delete. "Upgrade Notesd333e2251.txt")-->


#### Builds re-indexing after upgrade

On the first server start after upgrade from a version prior to 9.0, the server will reindex all builds for the purpose of builds search functionality and NuGet feeds. During the indexing time, some builds will not be available in the search results and in NuGet feeds. The server can also behave in less performant manner way during the indexing. `teamcity-server.log` has corresponding logging. On indexing finishing, there are "BuildIndexer (search) \- Finished re\-indexing builds" and "BuildIndexer (metadata) \- Finished re\-indexing builds" lines in the log.

#### Integration with issue trackers

__Since TeamCity 9.0__, the issue trackers are configured on the project level instead of the global server\-wide configuration.On the server upgrade, all existing issue tracker integrations are moved to the Root project, which makes them still accessible to all the projects on the server.

#### WebSocket connections and proxy servers

__Since 9.0__, TeamCity tries to establish WebSocket connections between the browser and the server for the UI updates. If you have a proxy server (like nginx) in front of the TeamCity web UI, make sure that the proxy is [configured](how-to.md#Set+Up+TeamCity+behind+a+Proxy+Server) properly to support WebSocket connections.

If the proxy is misconfigured or does not support the WebSocket protocol, a server health item will be shown for TeamCity system administrators. In this case TeamCity will use plain old polling for the UI updates as before.

#### REST API
{id="rest-api-5"}

REST API uses version 9.0. Previous versions of API are still available under `/app/rest/8.1`, `/app/rest/7.0`, `/app/rest/6.0` URLs.
* Change bean: the `webLink` attribute is renamed to `webUrl` to match other beans ([TW-34398](https://youtrack.jetbrains.com/issue/TW-34398)).
* Sub\-elements representing empty collections in some of the beans are no longer included into responses (used to be included as an empty tag in XML).
* the builds `changes` element does not include the "count" attribute by default (for performance reasons), count can still be included by providing fields parameter like: "fields=$long,changes(count,href)"
* The `/app/rest/agents` request now returns all the authorized agents by default (used to include unauthorized connected agents as well)
* queued builds now have the `id` attribute instead of the `taskId` attribute (they are the same for new builds since TeamCity 9.0)

__Build tags\-related changes__   
The `/app/rest/builds/<buildLocator>/tags` build request now returns a different XML: `<tags count="1"><tag name="TAG"/></tags>` instead of `<tags><tag>TAG</tag></tags>`.   
The same applies to the `/app/rest/buildTypes/<buildTypeLocator>/<buildTypeLocator>/buildTags` request.   
The same change in the structure also applies to the build's entity nested "tags" element.

To create a tag, there is an old way to post a plain\-text tag name to the `app/rest/builds/<buildLocator>/tags` URL.   
When sending POST or PUT XML or JSON requests to the URL, the new XML format is to be used (`<tag name="TAG"/></tags>` instead of `<tag>TAG</tag>`).

#### Handling tests with the same name within a build

In TeamCity 9.0, multiple tests with the same name within the same build are considered a single test with an invocation count. If any of these test runs fail, the whole test is considered failed in the build. The related issue is [TW-24212](https://youtrack.jetbrains.com/issue/TW-24212).

This change results in drop of test number counters in builds which have multiple runs for the same test. If you have a build failure condition which relies on test number in the build, this change may affect you.

If you need the tests to be treated as separate ones, consider running them in the test suites with different names or otherwise changing the test/running logic to change the full test name displayed in TeamCity.

#### Database-related changes

The national character sets (nchar, nvarchar, nclob types) for text fields are now supported in MS SQL databases used by TeamCity. It is recommended to use the Microsoft native JDBC driver, as jTDS JDBC driver does not support the nchar and nvarchar characters. If you still use jTDS, please [migrate](set-up-external-database.md).

Upon upgrade and entering the normal working status, TeamCity starts a background process to move the entries from the vcs\_changes database table to vcs\_change table. This process is transparent and you can continue working with the server as usual. It has negligible impact on the server performance and the only affected logic is the projects import feature (it is recommended to be used only with backups taken after the process is completed). The progress of the process can be seen on the Backup section in the server administration, along with "TeamCity is currently optimizing VCS\-related data in the database for better backup/restore performance" message.   
The other important thing is that the data copying increases the size of the raw database storage.   
If this is an issue for your case (e.g. it might be with Microsoft SQL Server database with set database size limit), it is recommended to ensure the database size limit is twice the current size before the upgrade. It is possible to perform database\-specific procedures to shrink the storage to match the actually stored data after the VCS changes migration process finishes.

#### VCS Root-related changes

The Git and Mercurial VCS roots no longer provide the ability to specify a custom clone path on the server for new VCS roots. If you need this ability, set the following internal properties to `true` for git and mercurial respectively: `teamcity.git.showCustomClonePath`, `teamcity.hg.showCustomClonePath`.

#### Visual Studio Addin

The TeamCity Add\-in installed as a part of [ReSharper Ultimate ](https://www.jetbrains.com/dotnet/) will remove the pre\-bundle products versions: TeamCity and ReSharper versions prior to 9.0, dotCover prior to 3.0, dotTrace prior to 6.0.

Besides, it will not use the settings provided by the 8.1 version. The traditional add\-in downloaded from TeamCity server can still use settings from previous version.

#### Other
{id="other-2"}

TeamCity agent installation via the Java web start installation package is no longer available.

## Changes from 8.1.1 to 8.1.4

No potential breaking changes.

## Changes from 8.1 to 8.1.1

#### Command Line Runner

The change in behavior introduced in 8.1 (see [below](#Known+issue+with+Command+Line+Runner)) has been fixed. Command line runners using "Executable with parameters" option which were created/changed with TeamCity 8.1 can expose a change in behavior with the upgrade. The recommended approach is to switch to "Custom script" option instead of "Executable with parameters" in command line runner.

#### Separate download for VSTest.Console runner

VSTest console runner is no longer bundled with TeamCity and is available as a separate plugin. For download details, see the [plugin page](https://confluence.jetbrains.com/display/TW/VSTest.Console+Runner)

## Changes from 8.0.6 to 8.1

#### Known issue with creating MS SQL database with integrated security

When installing TeamCity anew and creating an MS SQL database with integrated security using the database setup UI, you may receive an error. We are planning to resolve it in the next bugfix, meanwhile use the following workaround:
1. After receiving the error, stop the TeamCity server.
2. Start the TeamCity server.
3. On the database configuration screen, fill out the required information. __Do not use the Refresh button.__ Make sure the information specified is correct.
4. Continue with the configuration steps.

If for some reason the workaround above does not resolve the problem, do the following:

1. Start the TeamCity server, approve a new database creation, configure the MS SQL access with a login and password, without [integrated security](set-up-external-database.md).
2. Ensure that TeamCity works properly.
3. Stop the TeamCity server.
4. Modify the [Setting up an External Database](set-up-external-database.md) file: configure MS SQL connection string to use integrated security, and remove the login and password.
5. Start the TeamCity server again.

#### Known issue with VSTest.Console runner

A new "VSTest.Console" runner which first appeared in TeamCity 8.1 is in experimental state and is not recommended for production use at this time. It will not be present in TeamCity 8.1.x by default (will be available as a separate download).

#### Known issue with PowerShell runner

PowerShell runner plugin is broken in 8.1. Fix is available, please follow instructions in [issue comment](https://youtrack.jetbrains.com/issue/TW-21554#comment=27-676676).

#### Known issue with Command Line Runner

Command line runner using "Executable with parameters" option can process quotes (") and percentage signs (%\) in a bit different way then in previous TeamCity versions (see details in the [issue](https://youtrack.jetbrains.com/issue/TW-35087)). To switch back to the previous (8.0) behavior you may specify command.line.run.as.script=false configuration parameter in a build configuration or in a project. The issue is fixed in 8.1.1.   
The recommended approach is to switch to "Custom script" option instead of "Executable with parameters" in command line runner.

#### Memory Settings

If you have not [switched to 64 bit JVM](install-and-start-teamcity-server.md) yet and use `-Xmx1300` memory setting for the server and the server is running on Windows, consider decreasing the setting to `-Xmx1200` as otherwise you might encounter "Native memory allocation (malloc) failed" JVM crash. See the [recommended memory settings](install-and-start-teamcity-server.md) for details.

#### Actions menu

Some actions has moved under the "Actions" button available at the top\-right of the page, near Run button.   
These include:"Label this build sources" on Changes tab of a build,   
"Pause", "Copy", "Move", "Delete", "Associate with Template", "Extract Template", "Extract Meta\-Runner" on build configuration settings administration page,   
"Copy", "Move", "Delete", "Archive", "Bulk edit IDs" on the __Project Settings__ administration page.

#### Create Maven build configuration is not available by default

Action "Create Maven build configuration" is no longer available. Most of its functionality is covered by create project from URL and create VCS root from URL pages.

#### triggeredBy parameter from GroovyPlug plugin

The `build.triggeredBy` and `build.triggeredBy.username` configuration parameters provided by the [plugin](https://confluence.jetbrains.com/display/TW/Groovy+plug) added by the plugin are now [available](predefined-build-parameters.md) without the plugin under teamcity.build.triggeredBy and teamcity.build.triggeredBy.username names respectively. Consider migrating to the latter set of parameters in your settings if you used the plugin's ones.

#### Shared Resources build feature

If the build takes lock on all values of a resource with custom values, these values are provided as lock values in build parameters. Corresponding issue: [TW-29779](https://youtrack.jetbrains.com/issue/TW-29779)

#### TeamCity Disk Space Watcher

The following [internal properties](server-startup-properties.md) define free disk space thresholds on the TeamCity server machine:
* `teamcity.diskSpaceWatcher.threshold` set to 500 Mb by default displays a warning on all the pages of the TeamCity web UI.
* `teamcity.pauseBuildQueue.diskSpace.threshold` set to 50 Mb by default pauses the build queue.
  The `teamcity.diskSpaceWatcher.softThreshold` property is removed.

#### PowerShell

The PowerShell plugin now uses the version that was specified in the UI as the `-Version` command line argument when executing scripts. Corresponding issue: [TW-33472](https://youtrack.jetbrains.com/issue/TW-33472)

#### REST API
{id="rest-api-6"}

The latest version of the API has not changed, it is still "8.0" while there are changes in the API detailed below. If you find this inconvenient for your REST API usages, please comment in the corresponding [issue](https://youtrack.jetbrains.com/issue/TW-35227).

Entities returned in the response of REST API requests might now exclude attributes/elements with empty/default values. This is relevant for boolean fields with "false" value and empty collections. The recommended approach is to make sure the client code assumes "false" as a value for not present boolean attributes/elements.

"projectName" of buildType node now contains full project name (with the names of the parent projects) instead of the short name of the project.

In the lists of builds, "startDate" attribute is not longer included in the "build" node. It has become an element instead of attribute to match the full build data representation. If your REST API usage is affected, check [a way](https://youtrack.jetbrains.com/issue/TW-35032#comment=27-704059) to get that element in a request for the list of builds.

Requests /app/rest/buildTypes/XXX/parameters/YYY and /app/rest/projects/XXX/parameters/YYY now support "text/plain" and "application/xml" responses. To get plain text response (which was the only supported way before 8.1) you will need to supply "Accept: text/plain" header to the request.

Password properties of the VCS roots are now included into the responses, just without values.

CCTray\-format XML (`app/rest/cctray/projects.xml`) does not include paused build configurations now.

Response to the experimental request `/app/rest/buildTypes/XXX/investigations` has changed the format and got additional fields to cover tests and problem investigations. There is an internal property `rest.beans.buildTypeInvestigationCompatibility` to include removed sub\-items. Please let us know via [support email](feedback.md) if you need to use the internal property.

## Changes from 8.0.5 to 8.0.6

No potential breaking changes.

## Changes from 8.0.4 to 8.0.5

No potential breaking changes.

## Changes from 8.0.3 to 8.0.4

#### First Clean-up

First Clean-up after server upgrade might take a bit more time then regularly if there are many builds on the server. Following clean-ups will then run a bit faster then in previous versions.

## Changes from 8.0 to 8.0.3

No potential breaking changes.

## Changes from 7.1.x to 8.0

#### Project and Build Configuration IDs

This version introduces user\-assignable IDs for projects and build configurations. This new ID is now used instead of internal id (projectN and btNNN) in at least:
* URLs of the web pages and artifact downloads
* in REST API
* project IDs are also used in directory names on the server under `<TeamCity Data Directory>\system\artifacts` instead of project names used prior to TeamCity 8.0

If you used any of the above, please, verify if you are affected by the change.   
Learn more about IDs at [Identifier](identifier.md).

On upgrade, all the projects get automatically generated IDs based on their names.   
Build configuration IDs are set to be equal to internal (btNNN) ids and can be later changed from the Administration UI via the __Regenerate ID__ or __Bulk Edit IDs__ actions.

Please note that the names of the projects and build configurations are no longer unique server\-wide (are only unique within the direct parent project) and can contain any symbols which might be relevant if you used these in directory or file names.

#### Project settings format on disk

The format of the project settings storage on the disk under [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/config` has been changed.   
If you used any tools to read or update `project-config.xml` files, you will need to update the tools. It is recommended to use REST API or TeamCity open API (Java) to make changes so that the tools are not hugely affected by the format change.

#### Build Configuration templates

In version 8.0 build configuration templates support project hierarchy and TeamCity uses new rules:
* The TeamCity administration UI limits the use of templates only to those from the current project and its parents. On copying a project or a build configuration, the templates which do not belong to the target project or one of its parents are automatically copied.
* TeamCity no longer allows attaching a build configuration to a template if the template does not belong to the current project or one of its parents.
* Before version 8.0 it was possible to extract templates from a build configuration of one project to an unrelated project or to associate a build configuration in one project with a template in another. After upgrade to TC 8.0, such templates will become inaccessible in the current project. To reuse build configuration templates from an unrelated project, it is recommended to manually move them into the common parent project (or the Root project if you want them to be globally available).
#### JVM-originated agent parameters (os.arch and others)

The agent no longer reports system properties which come from the agent JVM: `system.os.arch, system.os.name, system.os.version, system.user.home, system.user.name, system.user.timezone, system.user.language, system.user.country, system.user.variant, system.path.separator, system.file.encoding, system.file.separator`).All the aforementioned parameters are now reported as configuration parameters with the `teamcity.agent.jvm.` prefix instead.If you used any of the parameters, make sure you update them to the new values.

#### IntelliJ IDEA project runner

IntelliJ IDEA project runner now uses IntelliJ IDEA's external make tool to build projects. Since this tool requires Java 1.6 to work, IntelliJ IDEA project runner now requires Java 1.6 (at least) too.

#### Clean-up for build configurations with feature branches

Build configurations with feature branches now process clean\-up rules per\-branch which can result in more builds preserved during clean\-up than in previous versions. See [details](teamcity-data-clean-up.md).

#### Team Foundation Server integration

TFS now prefers Team Explorer 2012 to Team Explorer 2010 (if both are installed) for TFS operations


<!--[//]: # (Internal note. Do not delete. "Upgrade Notesd333e2777.txt")-->




#### Compatibility with YouTrack

If you use JetBrains YouTrack and use its TeamCity integration features, please note that only YouTrack version 4.2.4 and later are compatible with TeamCity 8.0.   
If you need earlier YouTrack versions to work with TeamCity 8.0, please [let us know](feedback.md).


<!--[//]: # (Internal note. Do not delete. "Upgrade Notesd333e2793.txt")-->


#### REST API
{id="rest-api-7"}

__External ids__ There are changes in the API related to the new external ids for project/build types/templates as well as other changes.   
The old API compatible with TeamCity 7.1 is still provided under "/app/rest/7.0" URL.

If you used URLs with locators having "id" for projects, build configuration or templates (like `.../app/rest/projects/id:XXX` or `.../app/rest/buildTypes/id:XXX`), please, update the locators to one of the following:
* (recommended) "id:EXTERNAL\_ID" (you can get the external ID in web UI URLs or via a request to `.../app/rest/projects/internalId:OLD\_ID/id`
* just "ANY\_ID" to find the entity either by its internal, external id or name (use with caution: you can find more than you expect)
* "internalId:INTERNAL\_ID" to find the entity by the internal idYou can also use the "/app/rest/7.0/" URL prefix instead of "/app/rest/" to work with 7.0\-version of REST API which still uses internal IDs except for finish build trigger properties.

Also, it is possible to set the internal property `rest.compatibility.allowExternalIdAsInternal=true` to turn on the compatibility mode so that id:xxx locators will search also by the internal id. Note that this will be dropped in the future versions of TeamCity and is not recommended for use.

__Other Changes__   
Requests for builds ".../builds/&lt;locator&gt;/..." and ".../builds?locator=&lt;locator&gt;" no longer return personal and canceled builds by default. To include those, make sure you add ",personal:any,canceled:any" to the locators.

The "relatedIssues" element of the build entity no longer contains a full list of related issues. It has only the "href" attribute whose value can be used to get the related issues via a separate request.   
There is also an internal property "rest.beans.build.inlineRelatedIssues" which can be set to `true` to return the "relatedIssues" node back for compatibility. See [TW-20025](https://youtrack.jetbrains.com/issue/TW-20025) for details. Also, the ".../builds/xxx/related\-issues" URL is renamed to ".../builds/xxx/relatedIssues".

The "source\_buildTypeId" property is dropped from snapshot and artifact dependency nodes. Instead, the "source\-buildType" sub\-element is added with a reference to the build type.   
Creating dependencies is still supported with the "source\_buildTypeId" property, but is deprecated. There is an internal property "rest.compatibility.includeSourceBuildTypeInDependencyProperties" which can be set to `true` to include the "source\_buildTypeId" property back.

In version 8.0 VCS roots support project hierarchy:
* When creating a VCS root, the `project` element should always be provided now. The element supports the `locator` attribute to specify the project.
* the `shared` attribute is dropped from the VCS root: after upgrade, such VCS roots are attached to the root project (with the "\_Root" ID) and become globally available.
* when copying projects and build configurations, the `shareVCSRoots` attribute is no longer present. To make the VCS root available to projects and build configurations, move it to the parent/root project and then proceed with the copying.

It is recommended to create projects hierarchy which corresponds to organizational/settings sharing structure and move the VCS roots to most nested umbrella projects. Users then can be granted "Create / delete VCS root" role in the project to be able to edit VCS roots. Please note that users can edit a VCS root only if it is used in the projects they have "Edit project" permission for.

The "template" attribute in a build configuration template node is renamed to "templateFlag".

PUT for /users/&lt;locator&gt;/roles and /userGroups/&lt;locator&gt;/roles now accepts list of roles as it should and replaces existing roles instead of accepting single riles and adding it.

Many of PUT and POST requests which used to return nothing now return the entities created.

#### Open API changes

See [details](https://confluence.jetbrains.com/display/TCD18/Open+API+Changes)

#### Shared Resources plugin

If you used the [Shared Resources plugin](https://confluence.jetbrains.com/display/TW/TeamCity+Shared+Resources) with TeamCity 7.1.x, make sure to remove it as it is now bundled. See the [upgrade instructions](https://confluence.jetbrains.com/display/TW/TeamCity+Shared+Resources).

#### Queue Manager plugin

If you used the [QueueManager plugin](https://confluence.jetbrains.com/display/TW/TeamCity+Queue+Manager), make sure to remove it as it is now bundled. See the [upgrade instructions](https://confluence.jetbrains.com/display/TW/TeamCity+Queue+Manager)

#### Bundled Maven

Maven bundled with TeamCity upgraded to version 3.0.5.

#### HTTPS connections from agents to server

If your agents connect to the TeamCity server by HTTPS protocol, and after upgrade agents fail to connect with error messages like:javax.net.ssl.SSLHandshakeException: Received fatal alert: handshake\_failure

then you should change Tomcat SSL connector configuration, i.e. add the following attribute to SSL connector and restart TeamCity server:sslEnabledProtocols="TLSv1,SSLv3,SSLv2Hello"

The issue only manifests when the server runs under Java 1.7.

See also:
* [http://mail-archives.apache.org/mod_mbox/tomcat-users/201302.mbox/%3C512559F7.4080001@gmail.com%3E](http://mail-archives.apache.org/mod_mbox/tomcat-users/201302.mbox/%3C512559F7.4080001@gmail.com%3E)
* [https://youtrack.jetbrains.com/issue/TW-30221#comment=27-561843](https://youtrack.jetbrains.com/issue/TW-30221#comment=27-561843)

## Changes from 7.1.4 to 7.1.5

__teamcity.build.branch__ parameter semantics has changed, see [https://youtrack.jetbrains.com/issue/TW-23699#comment=27-448002](https://youtrack.jetbrains.com/issue/TW-23699#comment=27-448002)

## Changes from 7.1.3 to 7.1.4

No potential breaking changes.


<!--[//]: # (Internal note. Do not delete. "Upgrade Notesd333e2962.txt")-->

## Changes from 7.1.2 to 7.1.3

No potential breaking changes.

Please check up\-to\-date list of [known regressions](https://youtrack.jetbrains.com/issues/TW?q=Affected+versions%3A+%7BFaradi+7.1.3+%2824266%29%7D+tag%3Aregression) for the version in our issue tracker.

## Changes from 7.1.1 to 7.1.2

__Possible issues with hg server\-side checkout__   
There is a known issue with 7.1.2 release: [TW-24405](https://youtrack.jetbrains.com/issue/TW-24405) which can reproduce when server\-side checkout, labeling or file content viewing are used for Mercurial repository.If you experience the error with message "abort: destination 'hg1' is not empty", please install the patch attached to the issue.

__Other known issues__   
Please also check a list of [known regressions](https://youtrack.jetbrains.com/issues/TW?q=Affected+versions%3A+%7BFaradi+7.1.2+%2824170%29%7D+tag%3Aregression) for the version in our issue tracker.

## Changes from 7.1 to 7.1.1

No potential breaking changes.

## Changes from 7.0.x to 7.1

__Windows service configuration__   
Since version 7.1, TeamCity uses its own service wrapping solution for the TeamCity server as opposed to that of default Tomcat one in previous versions.This changes the way TeamCity service is configured (Data Directory and server startup options including memory settings) and makes it unified between service and console startup.   
Please refer to the updated [section](server-startup-properties.md) on configuring the server startup properties.

Agent windows service started to use OS\-provided environment variables. Once Agent server (and JVM) are x86 processes, agent will report x86 environment variables. The change may affect your CPU bitness checks. See [MSDN Blog](http://blogs.msdn.com/b/david.wang/archive/2006/03/26/howto-detect-process-bitness.aspx) on how to check if machine supports x64 by reported environment variables

__Default location for TeamCity Data Directory when installed with Windows installer__   
This is only relevant for fresh TeamCity installations with Windows installer. Existing settings are preserved if you upgrade an existing installation.   
Windows installer now uses `%\ALLUSERSPROFILE%\JetBrains\TeamCity` location as default one for [TeamCity Data Directory](teamcity-data-directory.md). In TeamCity 7.0 and previous versions that used to be `%\USERPROFILE%\.BuildServer`.

__Windows domain login module__   
When TeamCity server runs under Windows and Windows domain user authentication is used, TeamCity now uses another library (Waffle) to talk to the Windows domain. Under Linux the behavior is unchanged: jCIFS library is used as it were.

Unless you specified specific settings for jCIFS library in ntlm\-config.properties file, your installation should not be affected.   
If you experience any issues with login into TeamCity with your Windows username/password after upgrade, please provide details [to us](feedback.md). In the mean time you can switch to using old jCIFS library. For this, add `teamcity.ntlm.use.jcifs=true` line into [internal properties file](server-startup-properties.md).   
Please note that jCIFS library approach can be depricated in future versions of TeamCity, so the property specification is not recommended if you can go without it.

__Checkout directory change for Git and Mercurial__   
Build configurations that have either Git or Mercurial VCS roots and use default checkout directory will perform clean checkout upon upgrade. The clean checkout will be triggered by changed default checkout directory name. Further builds will reuse the checkout directory more aggressively (all builds using different branches but using the same VCS root will use the same directory). This affects agent\- and server\-side checkouts.

__Perforce agent checkout workspace names change__   
Build configurations using Perforce agent\-side checkout will perform clean checkout once after server upgrade. This is related to changed names for automatically generated Perforce workspaces.

__SVN revision format__   
For changes, detected in external repositories, SVN revision got format `NNN_MMM:EXTUUID_CHANGEDATE`, where `NNN` \- revision of the main repository, `MMM` \- revision of externals repository, `EXTUUID` \- UUID of externals repository, `CHANGEDATE` \- change timestamp. This change may affect plugins/REST api clients which use revision of the last build change somehow.

__Default schema when Microsoft SQL Server is used as an external database__   
Starting with version 7.1 TeamCity works only with a single database schema unlike previous versions when it could work with tables in any schemas of the database server.

TeamCity\-related tables should now be located in the database schema which is set as default one for the database user used by TeamCity to connect to the database.

This change may require reconfiguration of the database to set default schema for the user used by TeamCity server to connect to the database.

Please check that all TeamCity\-related tables are located in the default user's schema before performing the upgrade.

If the default user's schema is not set right, TeamCity can report "TeamCity database is empty or doesn't exist. If you proceed, a new database will be created." message on the first start of newer TeamCity.

To change user's default schema, use the '[alter user](https://msdn.microsoft.com/en-us/library/ms176060.aspx)' SQL command.

For the default schema description, see the "Default Schemas" section in the [corresponding documentation ](https://msdn.microsoft.com/en-us/library/ms190387.aspx).

__Open API changes__   
See [details](https://confluence.jetbrains.com/display/TCD18/Open+API+Changes)

## Changes from 7.0.1 to 7.0.4

No potential breaking changes.

## Changes from 7.0 to 7.0.1

__HTML report tabs URLs Change__   
If you use direct links for build\-level or project\-level [report tabs](including-third-party-reports-in-the-build-results.md), please update the links as they will [change](https://youtrack.jetbrains.com/issue/TW-20462#comment=27-302397) after upgrade. The change is necessary to make the feature more reliable.

## Changes from 6.5.x to 7.0

__(Known issue) Build can hang or produce memory error for NUnit and other .Net test runners__   
Affected are: .Net test runners (NUnit, MSTest, MSpec) as well as TeamCity NUnit console launcher.   
Reproduces when path to test assemblies has several deep paths without wildcards ("\*").

Visible outcome: build hangs or fails with OutOfMemoryException error after "Starting ...JetBrains.BuildServer.NUnitLauncher.exe" link in the build log.

The issue ([TW-20482](https://youtrack.jetbrains.com/issue/TW-20482)) is fixed and the fix will be included in the next release.   
Patch with a fix is [available](https://youtrack.jetbrains.com/issue/TW-20482#comment=27-302393).

__Minimum Supported Project JDK for Ant Runner__   
Starting with this version Ant runner requires minimum of JDK 1.4 in __runtime__ build part (was 1.3 previously). This means that you will not be able to use TeamCity Ant runner if your project uses JDK 1.3 for compilation or tests running.For projects that require JDK 1.3 you can use command\-line runner instead and configure "XML report processing" build feature to parse test reports.

__Supported Java for Server and Agent__   
Starting with this version the following requirements
* TeamCity __server__ should be run with JRE 1.6 or above (was 1.5 previously). TeamCity .exe distribution is already bundled with appropriate Java. For .tar.gz or .war TeamCity distributions you might need to install and configure server [manualy](install-and-start-teamcity-server.md).
* TeamCity __agent__ should be run with JRE 1.6 or above (was 1.5 previously). Agent .exe distribution is already bundled with appropriate Java. If you used .zip agent distribution or installed the TeamCity agent with TeamCity version 5.0 or earlier, you might need [manual steps](install-and-start-teamcity-agents.md). If you run TeamCity 6.5.x, please check "Agents" page of your existing TeamCity server: the page will have a yellow warning in case any of the connected agents are running JDK less than 1.6.


> If any of your agents are running under JDK version less than 1.6, the agents will fail to upgrade and will stop running on the server upgrade. You will need to recover them manually by installing JDK 1.6 and making sure the agents will [use it](install-and-start-teamcity-agents.md).
>
{style="note"}

__Project/Template parameters override__   
In TeamCity 7.0 project parameters have higher priority than parameters defined in template, i.e. if there is a parameter with some name and value in the project and there is parameter with the same name and different value in template of the same project, value from the project will be used. This was not so in TeamCity 6.5 and was [changed](https://youtrack.jetbrains.com/issue/TW-17247) to be more flexible when template belongs to anohter project.Build configuration parameters have the highest priority, as usual.

<anchor name="no-sybase-support"/>

__Support for Sybase is discontinued__   
From this version support for Sybase as external database is shifted back into "experimental" state.   
The reason for this decision is that it does not seem like the database is actively used with TeamCity, and supporting it requires a significant effort from TeamCity team which otherwise can be directed to improving more important areas of the product.   
While it should be still [possible](https://confluence.jetbrains.com/display/TCD65/Setting+up+an+External+Database), we do not recommend using Sybase as an external database and we are not planning to provide support for the Sybase\-related issues.   
Please consider using one of the other [databases supported](supported-platforms-and-environments.md). If you use Sybase, please migrate to another database before upgrading TeamCity.

__REST API Changes__
* Several objects got additional attributes and sub\-elements (e.g. BuildType, VcsRoot). Please check that your parsing code still works. _/buildTypes/_ path: BuildType object dropped runParameters field (as well as _/&lt;locator&gt;/runParameters_ path is dropped) in favor of _steps_ collection and _/&lt;locator&gt;/steps/_ path.
* A [bug](https://youtrack.jetbrains.com/issue/TW-17478) fixed which resulted in non\-array JSON representation of single element arrays for some resources. Please check if your code is affected.
* in build object, "dependency\-build" element is renamed to "snapshot\-dependencies", revisions/revision/vcs\-root is renamed to revisions/revision/vcs\-root\-intance (and it points to resolved VCS root instance now), revisions/revision/display\-version is renamed to "version".
* in buildType object, "vcs\-root" element is renamed to "vcs\-root\-entries"

Old version of the REST API is available via `/app/rest/6.0/...` URL in TeamCity 7.0. Please update your REST\-using code as future versions of TeamCity might drop support for 6.0 protocol.

__Minimum version of supported Tomcat__   
If you use TeamCity .war distribution, please note that Tomcat 5.5 is no longer supported. Please update Tomcat to version 6.0.27 or above (Tomcat 7 is recommended).

__Swabra__

swabra.handle.exe.path and handle.exe.path configuration parameters are no longer supported for providing path to Sysinternals handle.exe on agents. See the [plugin page](build-files-cleaner-swabra.md) for the details.

__Open API Changes__

Classes from __jetbrains.buildServer.messages.serviceMessages__ package like __jetbrains.buildServer.messages.serviceMessages.BuildStatus__ no longer depend on __jetbrains.buildServer.messages.Status__ class. To make your code compatible with TeamCity 6.0 \- 7.0 you can use __jetbrains.buildServer.messages.serviceMessages.ServiceMessage#asString__ methods, for example:


```Shell

ServiceMessage.asString("buildStatus", new HashMap<String, String>() {{
  put("text", "Errors found");
  put("status", "FAILURE");
}});

```



See also [Open API Changes](https://confluence.jetbrains.com/display/TCD18/Open+API+Changes)

## Changes from 6.5.4 to 6.5.6

No potential breaking changes.

## Changes from 6.5.4 to 6.5.5

(Known issue infex in 6.5.6) .NET Duplicates finder may stop working, the patch is available, please see this comment: [https://youtrack.jetbrains.com/issue/TW-18784#comment=27-261174](https://youtrack.jetbrains.com/issue/TW-18784#comment=27-261174)

## Changes from 6.5.3 to 6.5.4

No potential breaking changes.

## Changes from 6.5.2 to 6.5.3

No potential breaking changes.

## Changes from 6.5.1 to 6.5.2

__Maven runner__   
Working with MAVEN\_OPTS has changed again. Hopefully for the last time within the 6.5.x iteration. (see [https://youtrack.jetbrains.com/issue/TW-17393](https://youtrack.jetbrains.com/issue/TW-17393))   
Now TeamCity acts as follows:
1. If MAVEN\_OPTS is set TeamCity takes JVM arguments from MAVEN\_OPTS
2. If "JVM command line parameters" are provided in the runner settings, they are taken instead of MAVEN\_OPTS and __MAVEN\_OPTS is overwritten with this value to propagate it to nested Maven executions__.

Those who after upgrading to 6.5 had problems of not using MAVEN\_OPTS and who had to copy its value to the "JVM command line parameters" to make their builds work, now don't need to change anything in their configuration. Builds will work the same way they do in 6.5 or 6.5.1.

## Changes from 6.5 to 6.5.1

__(Fixed known issue) Long upgrade time and slow clean-up under Oracle__

## Changes from 6.0.x to 6.5

__(Known issue) Long upgrade time and slow clean-up under Oracle__   
On first upgraded server start the database structures are converted and this can take a long time (hours on a large database) if you use Oracle external database ([TW-17094](https://youtrack.jetbrains.com/issue/TW-17094)). This is already fixed in 6.5.1.

__Agent JVM upgrade__   
With this version of TeamCity we added semi\-automatic upgrade of JVM used by the agents. If there is a Java 1.6 installed on the agent, and the agent itself is still running under the Java 1.5, TeamCity will ask to switch agent to Java 1.6. All you need is to review that detected path to Java is correct and confirm this switch, the rest should be done automatically. The operation is per\-agent, you'll have to make it for each agent separately. Note that we recommend to switch to Java 1.6, as at some point TeamCity will not be compatible with Java 1.5. Make sure newly selected java process has same firewall rules (i.e. port 9090 is opened to accept connections from server)

__IntelliJ IDEA Coverage data__   
Coverage data produced by IntelliJ IDEA coverage engine bundled with TeamCity 6.5 can only be loaded in IntelliJ IDEA 10.5\+. Due to coverage data format changes older versions of IntelliJ IDEA won't be able to load coverage from the server.

__IntelliJ IDEA 8 is not supported__   
Plugin for IntelliJ IDEA no longer supports IntelliJ IDEA 8.

__Unsupported MySQL versions__   
Due to [bugs](https://youtrack.jetbrains.com/issue/TW-16508#comment=27-219601) in MySQL 5.1.x TeamCity no longer supports MySQL versions in range 5.1 \- 5.1.48. TeamCity won't start with appropriate message if unsupported MySQL version is detected. Please upgrade your MySQL server to version 5.1.49 or later.

__Finish build properties are displayed__   
Finished builds now display all their properties used in the build on "Parameters" tab. This can potentially expose names and values of parameters from other builds (those that the given build uses as artifact or snapshot dependency). Please make sure this is acceptable in your environment. You can also manage users who see the tab with "View build runtime parameters and data" permissions which is assigned "Project Developers" role by default.

__PowerShell runner is bundled__   
If you installed [PowerShell plugin](https://confluence.jetbrains.com/display/TW/PowerShell) manually, please remove it form `.BuildServer/plugins` as a fresh version is now bundled with TeamCity.

__Changed settings location__
* XML test reporting settings are moved from runner settings into a dedicated [build feature](adding-build-features.md).
* "Last finished build" artifact dependency on a build which has snapshot dependency is automatically converted into dedicated "Build from the same chain" source build setting.

__Responsibility is renamed to Investigation__   
A responsibility assigned for a failing build configuration or a test is now called investigation. This is just a terminology change to make the action more neutral.   
If you have any email processing rules for TeamCity investigation assignment activity, please check is they need updating to use new text patterns.

__REST API Changes__   
Several objects got additional attributes and sub\-elements (e.g. "startDate" in reference to a build, "personal" in a change). Please check that your parsing code still works.

__Cleaning Non\-default Checkout Directories__   
In previous releases, if you have specified build [checkout directory](build-checkout-directory.md) explicitly using absolute path, TeamCity would not clean the content of the directory to free space on the disk.   
This is no longer the case.   
So if you have absolute path specified for the checkout directory and you need the directory to be present on agent for other build or for the machine environment, please set `system.teamcity.build.checkoutDir.expireHours` [property](build-checkout-directory.md) to "never" and re\-run the build. Please take into account that using [custom checkout directory](build-checkout-directory.md) is not recommended.

If you are using one of [system.teamcity.build.checkoutDir.expireHours](build-checkout-directory.md) properties and it is set to "never" to prevent the checkout directory from automatic deletion, the directory might be deleted once after TeamCity upgrade. Running the build in the build configuration once after the upgrade (and within 8 days from the previous build) will ensure that the directory preserves the "protected" behavior and will not be automatically removed by TeamCity.

__Free disk space__   
This release exposes [Free disk space feature](free-disk-space.md) in UI that was earlier only available via setting build configuration properties.   
While the old properties still work and take precedence, it is highly recommended to remove them and specify the value via "Disk Space" build feature instead. Future TeamCity versions might stop to consider the properties specified manually.

__Command line runner__   
`@echo off` which turns off command\-echoing is added to scripts provided by "Custom script" runner parameter. To enable command\-echoing add `@echo on` to the script.

__Windows Tray Notifier__   
You will need to upgrade windows tray notifier by uninstalling it and installing it again. Unfortunately, auto\-upgrade will not work due to issues in old version of Windows Tray Notifier.

__Maven runner__
* In earlier TeamCity versions Maven was executed by invoking the 'mvn' shell script. You could specify some parameters in MAVEN\_OPTS and some in UI. Maven build runner created its own MAVEN\_OPT by concatenating these two (`%MAVEN\_OPTS%\+jvmArgs`). In this case, if some parameter was specified twice \- in MAVEN\_OPTS and in UI, only the one specified in MAVEN\_OPTS was effective. Starting with TeamCity 6.5 Maven runner forms direct java command. While this approach solves many different problems, it also means that MAVEN\_OPTS isn't effective anymore and all JVM command line parameters should be specified in build runner settings instead of MAVEN\_OPTS.
* Those who had to manually setup surefire XML reporting for Maven release builds in TeamCity 6.0.x because otherwise tests weren't reported, now can forget about that. Since TeamCity 6.5 surefire tests run by release:prepare or release:perform goals are automatically detected. So don't forget to switch surefire XML reporting off in the build configuration settings to avoid double\-reporting!

__Email sending settings__   
Please check email sending settings are working correctly after upgrade (via Test connection on Administration &gt; Server Configuration &gt; EMail Notifier). If no authentication is needed, make sure login and password fields are blank. Non\-blank fields may cause email sending errors if SMTP server is not expecting authentication requests.

__XML Report Processing__   
Tests from Ant JUnit XML reports can be reported twice (see [TW-19058](https://youtrack.jetbrains.com/issue/TW-19058)), as we no longer automatically ignore TESTS\-xxx.xml report.   
To work around this, avoid using \*.xml mask and specify more concrete rules like TEST\-\*.xml or alike that will not match report with name starting with "TESTS\-"

__Open API Changes__   
Several return types have changes in TeamCity open API, so plugins might need recompilation against new TeamCity version to continue working.  
Also, some API was deprecated and will be discontinued in later releases. It is recommended to update plugins not to use deprecated API.   
See also [Open API Changes](https://confluence.jetbrains.com/display/TCD18/Open+API+Changes)

## Changes from 6.0.2 to 6.0.3

No potential breaking changes.

## Changes from 6.0.1 to 6.0.2

__Maven and XML Test Reporting Load CPU on Agent__   
If you use Maven or XML test reporter and your build is CPU\-intensive, you might find important the [known issue](https://youtrack.jetbrains.com/issue/TW-15335). Patch is available, fixed in the following updates.

## Changes from 6.0 to 6.0.1

No potential breaking changes.

## Changes from 5.1.x to 6.0

__Visual Studio Add\-in and Perforce__   
There is critical bug in TeamCity 6.0 VS Add\-in when Perforce is enabled. This can cause Visual Studio hangs and crashes. The fixed add\-in version is [available](ftp://ftp.intellij.net/pub/.teamcity/TW-14765/vsAddinInstaller.zip). (related [issue](https://youtrack.jetbrains.com/issue/TW-14765)). The issue is fixed in TeamCity 6.0.1.

__TFS checkout on agent__   
TFS checkout on agent might refuse to work with errors. Patch is available, see the [comment](https://youtrack.jetbrains.com/issue/TW-14804#comment=27-185725). Related [issue](https://youtrack.jetbrains.com/issue/TW-14804). The issue is fixed in TeamCity 6.0.1.

__Error Changing Priority class__   
You may encounter a browser error while changing priority number of a priority class. A patch is available in a related [issue](https://youtrack.jetbrains.com/issue/TW-14816). The issue is fixed in TeamCity 6.0.1.



__IntelliJ IDEA Compatibility__   
IntelliJ IDEA 6 and 7 are no longer supported in TeamCity plugin for IntelliJ IDEA.

Also, if you plan to upgrade to IntelliJ IDEA X (or other JetBrains IDE) please review this [known issue](known-issues.md).

__Build Failure Notifications__   
TeamCity 6.0 differentiates between build failure occurred while running a build script and one occurred while preparing for the build. The errors occurring in the latter case are called "failed to start" errors and are hidden by default from web UI (see "Show canceled and failed to start builds" option on Build Configuration page).

Since TeamCity 6.0, there is a separate notification rule "The build fails to start" which applies for "failed to start" builds. All the rest build failure notifications relate to build script\-related failures.

Please note that on upgrade, all users who had "The build fails" notification on, will automatically get "The build fails to start" option to preserve old behavior.

__Properties Changes__   
`teamcity.build.workingDir` property should no longer be used in non\-runner settings. For backward compatibility, the property is supported in non\-runner settings and is resolved to the working directory of the first defined build step.

__Swabra and Build Queue Priorities Plugins are Bundled__   
If you have installed the plugins previously, please remove them (typically form `.BuildServer/plugins`) before starting upgraded TeamCity version.

__Maven runner__   
Java older than 1.5 is no longer supported by the agent part of Maven runner. Please make sure you specify 1.6\+ JVM in Maven runner settings or ensure JAVA\_HOME points to such JVM.

__NUnit and MSTest Tests__   
If you had NUnit or MSTest tests configured in TeamCity UI (sln and MSBuild runners), the settings are extracted form the runners and converted to a new runner of corresponding type.

Please note that implementation of tests launching has changed and this affected relative paths usage: in TeamCity 6.0 the working directory and all the UI\-specified wildcards are resolved based on the build's [checkout directory](build-checkout-directory.md), while they used to be based on the directory containing .sln file. Simple settings are converted on TeamCity upgrade, but you might need to verify the runners contain appropriate settings.

__"%\" Escaping in the Build Configuration Properties__

Now, two percentage signs (%\%\) in values defined in Build Configuration settings are treated as escape for a single percentage sign. Your existing settings are converted on upgrade to preserve functioning like in previous versions. However, you might need to review the settings for unexpected "%\" sign\-related issues.

__.Net Framework Properties are Reported as Configuration Parameters__

In previous TeamCity versions, installed .Net Frameworks, Visual Studios and Mono were reported as System Properties of the build agents.   
This made the properties available in the build script.In order to reduce number of TeamCity\-specific properties pushed into the build scripts, the values are now reported via Configuration Parameters (that is, without "system." prefix) and are not available in the build script by default. They still be used in the Build Configuration settings via %\-references by their previous names, just without "system." prefix.

__Ipr runner is deprecated in favor of IntelliJ IDEA Project runner__   
Runner for IntelliJ IDEA projects was completely rewritten. It is not named "IntelliJ IDEA Project" runner. Previously available Ipr runner is also preserved but is marked as deprecated and will be removed in one of the further major releases of TeamCity. It is highly recommended to migrate your existing build configurations to the new runner.   
Please note that the new runner uses different approach to run tests: you need to have a shared Run Configuration created in IntelliJ IDEA and reference it in the runner settings.

__Clean-up for Inspection and Duplicates data__    
Starting from 6.0 Inspection and Duplicates reports for the builds are cleaned when build is cleaned from history, not when build's artifacts are cleaned as it used to be.

__Inspection and Duplicates runners require Java 1.6__   
"Inspections" and "Duplicates (Java)" runners now require Java JDK 1.6. Please ensure Java 1.6 is installed on relevant agents and check it is specified in the "JDK home path" setting of the runners.

__XML Report Validation__   
If you had invalid settings of "XML Report Processing" section of the build runners, you might find the Build Configurations reporting "Report paths must be specified" messages upon upgrade. In this case, please go to the runner settings and correct the configuration. (related [issue](https://youtrack.jetbrains.com/issue/TW-14821))

__Open API Changes__   
See [Open API Changes](https://confluence.jetbrains.com/display/TCD18/Open+API+Changes) Several jars in `devPackage` were reordered, some moved under `runtime` subdirectory. Please update your plugin projects to accommodate for these changes.

__REST API Changes__   
Several objects got additional attributes and sub\-elements. Please check that your parsing code still works.

__Perforce Clean Checkout__   
All builds using Perforce checkout will do a clean checkout after server upgrade. Please note that this can impose a high load on the server in the first hours after upgrade and server can be unresponsive while many builds are in "transferring sources" stage.



## Changes from 5.1.2 to 5.1.3

__Path to executable in Command line runner__   
The [bug](https://youtrack.jetbrains.com/issue/TW-11840) was fully fixed. The behavior is the same as in pre\-5.1 builds.

## Changes from 5.1.1 to 5.1.2

Jabber notification sending errors are displayed in web UI for administrators again (these messages were disabled in 5.1.1). If you do not use Jabber notifications, please pause the Jabber notifier on the Jabber settings server settings page.

## Changes from 5.1 to 5.1.1

__Path to executable in Command line runner__   
The [bug](https://youtrack.jetbrains.com/issue/TW-11840) was partly fixed. The behavior is the same as in pre\-5.1 builds except for the case when you have the working directory specified and have the script in both checkout and working directory. The script from the working directory is used.

__Path to script file in Solution runner and MSBuild runner__   
The [bug](https://youtrack.jetbrains.com/issue/TW-11854) was fixed. The behavior is the same as in pre\-5.1 builds.

## Changes from 5.0.3 to 5.1



> If you plan to upgrade from version 3.1.x to 5.1, you will need to modify some dtd files in `<TeamCity Data Directory>/config` before upgrade, read more in the issue: [TW-11813](https://youtrack.jetbrains.com/issue/TW-11813#comment=27-148589)
>
{style="tip"}


> NCover 3 support may not work. See [TW-11680](https://youtrack.jetbrains.com/issue/TW-11680#comment=27-148573)
>
{style="tip"}

__Notification templates change__   
Since 5.1, TeamCity uses [new template engine](customizing-notification-templates.md) (Freemarker) to generate notification messages. New default templates are supplied and customizations to the templates made prior to upgrading are no longer effective.

If you customized notification templates prior to this upgrade, please review the new notification templates and make changes to them if necessary. Old notification templates are copied into `<TeamCity Data Directory>/config/_trash/_notifications` directory. Hope, you will enjoy the new templates and new extended customization capabilities.

__External database drivers location__   
JDBC drivers can now be placed into `<TeamCity Data Directory>/lib/jdbc` directory instead of `WEB-INF/lib`. It is recommended to use the new location. See details at [Setting up an External Database](set-up-external-database.md).

PostgresSQL jdbc driver is no more bundled with TeamCity installation package, you will need to [install](set-up-external-database.md) it yourself upon upgrade.

__Database connection properties__   
Database connection properties template files have changed their names and are placed into `database.<database-type>.properties.dist` files under `<TeamCity Data Directory>/config` directory. They follow [.dist files convention](teamcity-data-directory.md).

It is recommended to review your `database.properties` file by comparing it with the new template file for your database and remove any options that you did not customize specifically.

__Default memory options change__   
We changed the default [memory option](install-and-start-teamcity-server.md) for PermGen memory space and if you had `-Xmx` JVM option changed to about 1.3G and are running on 32-bit JVM, the server may fail to start with a message like: "Error occurred during initialization of VM Could not reserve enough space for object heap Could not create the Java virtual machine".

On this occasion, please consider either:
* [switching to 64-bit JVM](configure-server-installation.md#Configure+Memory+Settings+for+TeamCity+Server).
* reducing PermGen memory via `-XX:MaxPermSize` [JVM option](server-startup-properties.md) (to previous default 120m)
* reducing heap memory via `-Xmx` [JVM option](server-startup-properties.md)

__Vault Plugin is bundled__   
In this version we bundled [SourceGear Vault VCS plugin](https://confluence.jetbrains.com/display/TW/Vault) (with experimental status). Please make sure to uninstall the plugin from `.BuildServer/plugins` (just delete the plugin's ZIP) if you installed it previously.

__Path to executable in Command line runner__ A [bug](https://youtrack.jetbrains.com/issue/TW-11840) was introduced that requires changing the path to executable if working directory is specified in the runner.The bug is partly fixed in 5.1.1 and fully fixed in 5.1.3.

__Path to script file in Solution runner and MSBuild runner__   
A [bug](https://youtrack.jetbrains.com/issue/TW-11854) was introduced that requires changing the path to script if working directory is specified in the runner. The bug is fixed in 5.1.1.

__Open API Changes__   
See [Open API Changes](https://confluence.jetbrains.com/display/TCD18/Open+API+Changes)

## Changes from 5.0.2 to 5.0.3

No potential breaking changes.


> There is a known issue with .NET duplicates finder: [TW-11320](https://youtrack.jetbrains.com/issue/TW-11320)   
Please use the patch attached to the issue.
>
{style="tip"}

## Changes from 5.0.1 to 5.0.2

__External change viewers__   
The `relativePath` variable is now replaced with relative path of a file _without_ checkout rules. The previous value can be accessed via `relativeAgentPath`. More information at [TW-10801](https://youtrack.jetbrains.com/issue/TW-10801).

## Changes from 5.0 to 5.0.1

No potential breaking changes.

## Changes from 4.5.6 to 5.0

__Pre\-5.0 Enterprise Server Licenses and Agent Licenses need upgrade__   
With the version 5.0, we announce changes to the upgrade policy: Upgrade to 5.0 is not free. Every license (server and agent) bought since 5.0 will work with any TeamCity version released within one year since the license purchase. Please review the detailed information at [Licensing and Upgrade](https://www.jetbrains.com/teamcity/buy/index.jsp) section of the official site.

__Bundled plugins__   
If you used standalone plugins that are now bundled in 5.0, do not forget to remove the plugins from the `.BuildServer/plugins` directory.The newly bundled plugins are:
* Mercurial
* Git (JetBrains)
* REST API (was provided with YouTrack previously)

__Other plugins__   
If you use any plugins that are not bundled with TeamCity, please make sure you are using the latest version and it is compatible with the 5.0 release. e.g. You will need the latest version of [Groovy plug](https://confluence.jetbrains.com/display/TW/Groovy+plug) and other properties\-providing extensions.Pre\-5.0 notifier plugins may lack support for per\-test and assignment responsibility notifications.

__Obsolete Properties__   
The system property "build.number.format" and environment variable "BUILD\_NUMBER\_FORMAT" are removed. If you need to use build number format in your build (let us know why), you can define build number format as `%system.<property name>%` and define &lt;property name&gt; system property in the build configuration (it will be passed to the build then).

__Oracle database__   
If you use TeamCity with Oracle database, you should add an addition privilege to the TeamCity Oracle user. In order to do it, log in to Oracle as user SYS and perform

```Shell

grant execute on dbms_lock to <TeamCity_User>;

```



__PostgreSQL database__   
TeamCity 5.0 supports PostrgeSQL version 8.3\+.   
So if the version of your PostgreSQL server is less than 8.3 then it needs to be upgraded.

__Open API Changes__ See [Open API Changes](https://confluence.jetbrains.com/display/TCD18/Open+API+Changes)

## Changes from 4.5.2 to 4.5.6

No potential breaking changes.

## Changes from 4.5.1 to 4.5.2

Here is a critical issue with Rake runner in 4.5.2 release. Please see [TW-8485](https://youtrack.jetbrains.com/issue/TW-8485) for details and a fixing patch.

## Changes from 4.5.0 to 4.5.1

No potential breaking changes.

## Changes from 4.0.2 to 4.5

__Default User Roles__   
The roles assigned as default for new users will be moved to "All Users" groups and will be effectively granted to all users already registered in TeamCity.

__Running builds during server restart__   
Please ensure there are no running builds during server upgrade.If there are builds that run during server restart and these builds have test, the builds will be canceled and re\-added to build queue ([TW-7476](https://youtrack.jetbrains.com/issue/TW-7476)).

__LDAP settings rename__   
If you had LDAP integration configured, several settings will be automatically converted on first start of the new server. The renamed settings are:
* `formatDN` — is renamed to `teamcity.auth.formatDN`
* `loginFilter` — is renamed to `teamcity.auth.loginFilter`

## Changes from 4.0.1 to 4.0.2

__Increased first clean-up time__   
The first server clean-up after the update can take significantly more time. Further clean-ups should return to usual times. During this first clean-up the data associated with deleted build configuration is cleaned. It was not cleaned earlier because of a bug in TeamCity versions 4.0 and 4.0.1.

## Changes from 4.0 to 4.0.1

__"importData" service message arguments id__ argument renamed to __type__ and __file__ to __path__. This change is backward\-compatible. See [Using Service Messages](fxcop.md) section for examples of new syntax.

## Changes from 3.1.2 to 4.0

__Initial startup time__   
On the very first start of the new version of TeamCity, the database structure will be upgraded. This process can increase the time of the server startup. The first startup can take up to 20 minutes more than regular one. This time depends on the size of your builds history, average number of tests in a build and the server hardware.

__Users re\-login will be forced after upgrade__   
Upon upgrade, all users will be automatically logged off and will need to re\-login in their browsers to TeamCity web UI. After the first login since upgrade, __Remember me__ functionality will work as usual.

__Previous IntelliJ IDEA versions support__   
IntelliJ IDEA plugin in this release is no longer compatible with IntelliJ IDEA 6.x versions. Supported IDEA versions are 7.0.3 and 8.0.

__Using VCS revisions in the build__   
`build.vcs.number.N` system properties are replaced with `build.vcs.number.<escaped VCS root name>` properties (or just `build.vcs.number` if there is only one root). If you used the properties in the build script you should update the usages manually or switch compatibility mode on. References to the properties in the build configuration settings are updated automatically. Corresponding environment variable has been affected too. [Read more](configuring-build-parameters.md).

__Test suite__   
Due to the fact that TeamCity started to handle tests suites, the tests with suite name defined will be treated as new tests (thus, test history can start from scratch for these tests.)

__Artifact dependency pattern__   
Artifact dependencies patterns now support [Ant-like wildcards](https://confluence.jetbrains.com/display/TCD4/Wildcards).   
If you relied on `""` pattern to match directory names, please adjust your pattern to use `"/"` instead of single `"*"`.   
If you relied on the `""` pattern to download only the files without extension, please update your pattern to use `"."` for that.

__Downloading of artifacts with help of Ivy__   
If you downloaded artifacts from the build scripts (like Ant build.xml) with help of Ivy tasks you should modify your ivyconf.xml file and remove all statuses from there except "integration". You can take the ivyconf.xml file from the following page as reference: [https://www.jetbrains.com/help/teamcity/4.0/configuring-dependencies.html](https://www.jetbrains.com/help/teamcity/4.0/configuring-dependencies.html)

__Browser caches (IE)__   
To force Internet Explorer to use updated icons (i.e. for the Run button) you may need to force page reload (Ctrl\+Shift\+R) or delete "Temporary Internet Files".

## Changes from 3.1.1 to 3.1.2

No potential breaking changes.

## Changes from 3.1 to 3.1.1

No potential breaking changes.

## Changes from 3.0.1 to 3.1

__Guest User and Agent Details__

Starting from version 3.1, the Guest user does not have access to the agent details page. This has been done to reduce exposing potentially sensitive information regarding the agents' environment. In the Enterprise Edition, the Guest user's roles can be edited at the __Users and Groups__ page to provide the needed level of permission.

__StarTeam Support__

__Working Folders in Use__

Since version 3.1 when checking out files from a StarTeam repository TeamCity builds directory structure on the base of the working folder names, not just folder names as it was in earlier versions. So if you are satisfied with the way TeamCity worked with StarTeam folders in version 3.0, ensure the working folders' names are equal to the corresponding folder names (which is so by default).

Also note that although StarTeam allows using absolute paths as working folders, TeamCity supports relative paths only and doesn't detect absolute paths' presence. So be careful and review your configuration.

__StarTeam URL Parser Fixed__

In version 3.0 a user must have followed a wrong URL scheme. It was like _starteam://server:49201/project/view/rootFolder/subfolder/..._ and didn't work when user tried to refer a non\-default view. In version 3.1 the native StarTeam URL parser is utilized. This means you now don't have to specify the root folder in the URL, and the previous example should look like _starteam://server:49201/project/view/subfolder/..._

## Changes from 3.0 to 3.0.1

__Linux Agent Upgrade__
* Due to an issue with Agent upgrade under Linux, Agent auxiliary Launcher processes may have been left running after agent upgrades. Versions 3.0.1 and up fix the issue. To get rid of the stale running processes, after automatic agent upgrade, please stop the agent (via `agent.sh kill` command) and kill any running `java jetbrains.buildServer.agent.Launcher` processes and start the agent again.

## Changes from 2.x to 3.0

__Incompatible changes__

Please note that TeamCity 3.0 introduces several changes incompatible with TeamCity 2.x:
* __build.working.dir__ system property is renamed to __teamcity.build.checkoutDir__. If you use the property in you build scripts, please update the scripts.
* `runAll.bat` script now accepts a required parameter: __start__ to start server and agent, __stop__ to stop server and agent.
* Under Windows, `agent.bat` script now accepts a required parameter: __start__ to start agent, __stop__ to stop agent. Note that in this case agent will be stopped only after it becomes idle (no builds are run). To force immediate agent stopping, use `agent.bat stop force` command that is available under both Windows and Linux (`agent.sh stop force`). Under Linux you can also use `agent.sh stop kill` command to stop agents not responding to `agent.sh stop force`.

__Build working directory__

Since TeamCity 3.0 introduces ability to configure VCS roots on per\-Build Configuration basis, rather then per\-Project, the default directory in which build configuration sources are checked out on agent now has generated name. If you need to know the directory used by a build configuration, you can refer to `<agent home>/work/directory.map` file which lists build configurations with the directory used by them. See also [Build Checkout Directory](build-checkout-directory.md)

__User Roles when upgrading from TeamCity 1.x/2.x/3.x Professional to 3.x Enterprise__

When upgrading from TeamCity 1.x/2.x/3.x Professional to 3.x Enterprise for the first time TeamCity's accounts will be assigned the following [roles](managing-roles-and-permissions.md) by default:
* _Administrators_ become System Administrators
* _Users_ become Project Developers for all of the projects
* The _Guest_ account is able to view all of the projects
* _Default user_ roles are set to Project Developer for all of the projects

## Changes from 1.x to 2.0

__Database Settings Move__   
Move your database settings from the `<TeamCity installation folder>/ROOT/WEB-INF/buildServerSpring.xml` file to the `database.properties` file located in the TeamCity configuration Data Directory (`<TeamCity Data Directory>/config`).



.NET inspection and duplicates
inherited




> If you were using the TeamCity\-GitHub [third-party plugin](https://github.com/milgner/TeamCityGithub) prior to TeamCity 10.0, you can safely [remove](installing-additional-plugins.md) it: the built\-in TeamCity integration will detect the existing connection to GitHub issue tracker and pick up your settings automatically.
>
{type=tip}


`teamcity.TruncateIgnoreReasonConverter.copyReasons`:

```Shell

 Error
   collecting changes 
  for
   VCS repository 
  ...
  

  Failed
   to collect TFS changes 
  -
   
  From
   version x 
  is
   greater 
  then
   current version y 
```


The [bug with temp tool folders](https://youtrack.jetbrains.com/issue/TW-46648) introduced in the previous version has been fixed.





<seealso>
        <category ref="admin-guide">
            <a href="licensing-policy.md">Licensing Policy</a>
        </category>
</seealso>



