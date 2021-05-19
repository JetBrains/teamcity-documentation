[//]: # (title: Server Health)
[//]: # (auxiliary-id: Server Health)

The __Server Health__ report contains results of the server inspection for any configuration issues which impact or could potentially impact the performance. Such issues, the so-called server _health items_, are collectively reported by TeamCity on the __Server Health__ page in the __Administration__ area.

The Project Administrator [permissions](role-and-permission.md) at least are required to see the report.

## Scope and Severity

The report enables you to define the analysis __scope__: you can select to analyze the global configuration or report on project-related items. The scope available to you depends on the level of the __View Project__ permission granted to you. _Note that the report is not available for archived projects._

The Server Health analysis also employs the __severity__ rating, depending on the issue impact on the configuration of the system on the whole or an individual project.

## Visibility Options

By default, the warning and error level results pertaining to the global configuration will be displayed on the report page as well as at the top of each page in TeamCity.

Besides those, some items will be displayed in-place: depending on the object causing the issue, the server health item will be reported on the Build Configuration, Template or VCS root settings page.

Only active items are displayed on the TeamCity pages. To remove an item from display, use the __hide__ option next to an item on the report page. For global items, this option is available in every server health message.

Hidden items will be removed from the TeamCity pages, and will be displayed on the Server Health page below the active items. Their description will be greyed out.

To return an item to display, use the __Show__ option.

The visibility changes will be listed on the __Audit__ page in the __Administration__ area.

## Issue Categories

Health items cover a wide range of server  functionality to allow administrators to easily monitor the TeamCity overall status.

### Global Configuration Items

TeamCity displays a notification on the availability of the new TeamCity version and a prompt to upgrade.  
A warning is displayed if any of the licenses are incompatible with this new version. The notification is visible to system administrators only and they can use the link in the "Some Licenses are incompatible" message to quickly navigate to the __Licenses__ page, where all incompatible licenses will have a warning icon.

### Agent Configuration

TeamCity displays a notification if agents are not running the recommended Java 8: this report shows all the agents running under Java earlier than version 1.8.

### Multinode Setup Misconfiguration

For a [multinode setup](multinode-setup.md), TeamCity might display health reports in the following cases:

#### Main Node is Inactive

_The main node has been inactive for N minutes_.

If this is a planned inactivity (a restart or upgrade), you can ignore this message — it will disappear once the main node is active again. Otherwise, please log in to the main node server and check the status of the TeamCity process.

If there is no TeamCity process or the server is down and cannot be recovered shortly, you can assign the "[Main TeamCity node](multinode-setup.md#Main+Node+Responsibility)" responsibility to the current or some other node on the __Nodes Configuration__ page.

#### Different Versions of Main and Secondary Servers

_The secondary node version doesn't match the current server version_.

In this case, you need to [update the secondary node](multinode-setup.md#Upgrade%2FDowngrade) to the same version as of the main TeamCity Server.

#### No Proxy Server in Multinode Setup

_Multinode setup doesn't have a proxy server._

To set up a high-availability TeamCity installation, you need to [configure a reverse proxy](multinode-setup.md#Proxy+Configuration).

<anchor name="ServerHealth-WebSocketconnectionissues"/>

### WebSocket connection issues
{product="tc"}

The WebSocket protocol is used to get web UI updated for events, running builds updates and statistics counters.

In case of any problems preventing WebSocket connection from working, a warning will be displayed. TeamCity will automatically switch to the legacy update mode and you will be able to continue using TeamCity in this mode.

However, it is __recommended__ to make the following adjustments to benefit from faster web UI updates as well as reduced unnecessary network traffic and latency:

* Proxy Server Configuration
* BIO Connector Adjustment

<anchor name="ServerHealth-ProxyServerConfiguration"/>

#### Proxy Server Configuration

If a reverse proxy is used in front of the TeamCity server, it needs to be [configured](how-to.md#Set+Up+TeamCity+behind+a+Proxy+Server) to support the WebSocket protocol.

All URLs used by browsers that do not support the WebSocket connection are listed in the corresponding health report.

#### BIO Connector Adjustment

If Tomcat is configured to use the [BIO connector](https://tomcat.apache.org/tomcat-8.5-doc/config/http.html), the WebSocket protocol is [automatically disabled](http://tomcat.apache.org/tomcat-7.0-doc/web-socket-howto.html#Production_usage). It is recommended to change the Tomcat Connector settings [to use the NIO connector](known-issues.md#Slow+download+from+TeamCity+server).

[//]: # (Internal note. Do not delete. "Server Healthd280e163.txt")    

### Critical Errors
{product="tc"}

This category shows the following errors:
* errors in project configuration files - occur if the server detects some inconsistency or a broken configuration while it loads configuration files from the disk
* errors raised by the [disk space watcher](teamcity-disk-space-watcher.md)
* warnings from the TeamCity Server [Memory Monitor](teamcity-memory-monitor.md)

### Database Related Problems
{product="tc"}

TeamCity will warn you if the server currently uses the internal database. [A standalone database is recommended as the storage engine](setting-up-an-external-database.md).

As [TeamCity does not support Sybase as an external database](upgrade-notes.md#no-sybase-support), a warning message will be displayed if you are using Sybase.

### Build Configuration Items

#### Dependency problems

TeamCity detects build configurations dependent on a missing build configuration (for example, on a build configuration deleted after the dependency was configured).

#### Configurations with Large Build Logs

Large [Build Logs](build-log.md) (more than 200 MB by default) can reduce the server performance as they could be too heavy to parse and are hard to view in the browser.

The build script can be tuned to print less output if this inspection fails frequently for some build configuration.

#### Inefficient Artifacts Publishing

TeamCity detects a build publishing many small artifact files and suggests publishing them as a single `.zip` archive.

### VCS Root Related Problems

#### Unused VCS Roots

TeamCity will show you the [VCS roots](vcs-root.md) defined in a project and will offer to delete the unused roots.

#### Similar VCS roots

TeamCity qualifies [VCS roots](vcs-root.md) as identical when their major settings (for example, URLs, branch settings) are the same even if some of their settings (for example, username, password) are different.

The report will show you identical roots and will leave it up to you whether to merge them or not.

#### Similar VCS Root Usages (AKA instances)

You can define values for [VCS root](vcs-root.md) settings or use parameter references to [various parameters defined at different levels](configuring-build-parameters.md).

When the referenced VCS roots parameters are resolved to the same values as the values defined, such cases will be reported as identical VCS root usages.

The general recommendation is to use parameter references for root settings, thus optimizing the amount of VCS roots.

#### Trigger Rules for Unattached VCS roots

TeamCity displays a warning if a rule of a [VCS Trigger or Schedule Trigger](configuring-vcs-triggers.md#vcs-trigger-rules-1) references a VCS root which is not attached to any build configuration.

<anchor name="ServerHealth-RedundantTrigger"/>

#### Redundant Trigger

The report will show cases when a build trigger is redundant, for example:
* There are two build configurations __A__ and __B__
* __A__ snapshot depends on __B__
* Both have VCS triggers, __A__ with the [Trigger on changes in snapshot dependencies](configuring-vcs-triggers.md#Trigger+build+on+changes+in+snapshot+dependencies) option enabled.
In this case, the VCS trigger in __B__ is redundant and causes builds of __A__ to be put into the queue several times.

#### Multiple identical build triggers

The warning is displayed if there are two or more enabled triggers of the same type with identical sets of parameter values. Disabled triggers are not taken into account.

<anchor name="ServerHealth-TooSmallQuietPeriod"/>

#### Effective Quiet Period is Bigger Than Specified

When a [VCS trigger](configuring-vcs-triggers.md) for a build configuration has a quiet period, TeamCity will wait the specified time after the last detected change before triggering the build. During this time, all VCS Roots which affect this build configuration are checked for changes. If other VCS Roots have checking for changes interval bigger than the quiet period, the effective quiet period will be equal to the maximum checking for changes interval of the involved VCS Roots (it could be a VCS Roots from the dependencies).

>When a VCS root uses commit hooks, its check for changes' interval does not affect the quiet period and won't delay build triggering, see [TW-44865](https://youtrack.jetbrains.com/issue/TW-44865).

A possible fix could be one of the following:
* Use commit hooks to trigger checking for changes operations
* Increase quiet period in the VCS trigger so it is larger than checking for changes interval of the related VCS Roots
* Reduce checking for changes interval for the problematic VCS Roots
### VCS checkout 

#### Possible Frequent Clean Checkout 

This section of the report will show possible frequent [clean checkout](clean-checkout.md), which may be caused by the following two reasons:

#### Custom Checkout Directory 

Build configurations having different [VCS settings](configuring-vcs-settings.md) but the same [custom checkout directory](build-checkout-directory.md#Custom+checkout+directory) may lead to frequent clean checkouts, slowing down the performance and hindering the consistency of incremental sources updates.

#### Build Files Cleaner (Swabra) Settings 

Enabling the [Build files cleaner (Swabra)](build-files-cleaner-swabra.md) build feature in several build configurations may cause extra clean checkouts. This may happen if builds from these configurations alternately run on the same agents and have identical Version Control Settings or the same custom checkout directory specified.

_Possible frequent clean checkout (Swabra case)_ server health report shows such incorrectly set up configurations grouped by Swabra settings.


#### Optimal recommended checkout

A report is shown for large server-side patches with a recommendation to switch to the [agent-side checkout](vcs-checkout-mode.md).

#### Default auto-checkout on agent 

If the default agent-side checkout is not possible, TeamCity will display a corresponding health report item and will use the server\-side checkout.

### Integrations-related Items
* If a project or build configuration has secured parameters and is configured to build GitHub pull requests, this report will raise the warning, because malicious code submitted via the pull request can obtain these secured parameters
* If a VCS root points to GitHub or Bitbucket, a suggestion to configure a corresponding issue tracker is displayed.

<anchor name="agentUpgradeFailed"/>

### Agents Health

#### Some agents cannot upgrade
[//]: # (AltHead: agentUpgradeFailed)

The report helps to find agents which failed to upgrade. [Build agent logs](viewing-build-agent-logs.md) should help identify the cause of the issue.

#### Cloud Agents
{product="tc"}

If a user removes an image from a profile, a warning is displayed that the instances already started by TeamCity will not be automatically stopped.

#### Unused Build Agents

The report is displayed for the agents not used for 3 days and more, if
* you have more than 3 agents in your environment
* your agents have been registered for more than 3 days
* if the builds were run on the server during these 3 days

### Incorrect Proxy Server Configuration
{product="tc"}

The report displays detected misconfiguration of the proxy server that is used to access the TeamCity web interface.

See our [recommendations](how-to.md#Set+Up+TeamCity+behind+a+Proxy+Server) how to set up a proxy server with TeamCity.

### Suggested Settings

TeamCity analyzes the current settings of a build configuration and suggests additional options, for example, adding a VCS trigger, a build step, and so on. Besides the server health report, the suggestions for a specific build configuration are shown right on the configuration settings pages.

## Extensibility
{product="tc"}

The default Server Health report provided by TeamCity might cover either too many, or not all the items required by you. Depending on your infrastructure, configuration, performance aspects, and so on. that you need to analyze, a custom Server Health report can be needed. TeamCity enables you to write a [plugin](https://plugins.jetbrains.com/docs/teamcity/custom-server-health-report.html) which will report on specific items.