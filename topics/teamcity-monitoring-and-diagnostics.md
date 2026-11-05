[//]: # (title: TeamCity Monitoring and Diagnostics)
[//]: # (help-id: TeamCity Monitoring and Diagnostics)

<snippet id="intro">

TeamCity provides a variety of diagnostic tools and indicators to monitor and troubleshoot the server, including detailed server logs, health reports, and resource monitors. These tools make it easier to identify and investigate problems and, if needed, [report issues](reporting-issues.md) on your server.

</snippet>

This article describes the diagnostics tools available in __Admin | Diagnostics__. You can also find more details in the nested articles of this section.

> If you need to identify whether the server is up and running, send GET requests to the following endpoints:
> * the `<server_URL>/healthCheck/healthy` endpoint returns "200" if a server is running, even if it is still initializing or in [maintenance mode](teamcity-maintenance-mode.md).
> * the `<server_URL>/healthCheck/ready` endpoint returns "200" if a server is fully initialized and ready to accept user requests. If the server is still initializing or awaits for a data upgrade, the endpoint returns "503".
>
{style="tip"}

## Troubleshooting

This tab provides a number of indicators helping you to detect and address issues with the TeamCity server performance.

### CPU &amp; Memory Usage

This section displays the data provided by the [TeamCity Memory Monitor](teamcity-memory-monitor.md), which regularly checks available memory and submits a warning if the [memory consumption](reporting-issues.md#OutOfMemory+Problems) grows too high. See also information on configuring [memory settings](configure-server-installation.md#Configure+Memory+Settings+for+TeamCity+Server) for the TeamCity server.

Depending on your operating system and Java settings, the list of displayed properties below may vary.

__CPU usage__
* __TeamCity process CPU usage__ shows the CPU time used by the main TeamCity process averaged over a period of time. Other processes consuming CPU resources (databases, Maven server, VCS) are not included.
* __Garbage collection__ shows the time spent on cleaning the server memory averaged over a period of time. High numbers over long periods of time usually indicate insufficient server memory.
* __System load__ displays CPU load information based upon the CPU queue length averaged over a period of time, i.e. the sum of the number of runnable entities queued to the available processors and the number of entities running on the available processors averaged over a period of time (1 minute)
* __Overall CPU usage__ shows the recent CPU usage for the whole system with a value in the \[0.0,1.0\] interval. A value of 0.0 means that all CPUs were idle during the recent period of time, while a value of 1.0 means that all CPUs were actively running 100% of the time during the recent period

__Memory usage__
* __Total heap__  displays the total amount of memory used by TeamCity to store all data, including temporary data about to be collected. Represents all heap memory pools (young and old generations) in terms of java. Garbage collection runs quickly and frequently.
* __Data__ shows the amount of memory used by TeamCity to store persistent data. Represents old generations only in terms of java. Garbage collection runs slowly and infrequently.

### Troubleshooting
{id="troubleshooting-1"}

#### Debug Logging

This allows changing the internal TeamCity server [logging settings](teamcity-server-logs.md#Logging-related+Diagnostics+UI).

#### Hangs and Thread Dumps

The [server thread dump](reporting-issues.md#Server+Thread+Dump) can be viewed in the browser or saved to a file.

If you experience memory problems, this section provides an option to dump a memory snapshot.

#### Server Restart

This section allows restarting the server from the UI.

### Java Configuration

This section informs you on the Java [installed on your server](how-to.md#Install+Non-Bundled+Version+of+Java) and the configured [JVM options](server-startup-properties.md#JVM+Options).

## VCS Status

This tab displays the information on the monitored VCS roots, including their checking for changes status and duration.

You can filter the available VCS roots by the checking for changes duration.

## Metrics

TeamCity provides server load metrics which can be used for monitoring the TeamCity server health.

The __Metrics__ tab displays all supported metrics, their `tag` parameters, and current values.

The `<TeamCity_server_URL>/app/metrics` endpoint provides the metrics in a [Prometheus](https://prometheus.io/) format, ready for importing to monitoring solutions with a Prometheus support (for example, to [Grafana](https://grafana.com/)). Note that server metrics can be obtained only by a user with the "_View usage statistics_" permission.

> Make sure to also read [the blog post](https://blog.jetbrains.com/teamcity/2022/06/monitoring-teamcity-server-health/) about metrics which can be used to monitor TeamCity server health.

The `experimental` tag for metrics is not reported starting with TeamCity 2022.12.
The `?experimental=true` URL parameter for metrics in the Prometheus format still works, and some of the metrics still have the experimental status.
If you find any of the experimental metrics useful and would want them to be graduated to the supported metrics,
let us know via our [support channel](troubleshooting.md).
{instance="tcc"}

The `experimental` tag for metrics is not reported starting with TeamCity 2023.05.
The `?experimental=true` URL parameter for metrics in the Prometheus format still works, and some of the metrics still have the experimental status.
If you find any of the experimental metrics useful and would want them to be graduated to the supported metrics,
let us know via our [support channel](troubleshooting.md).
{instance="tc"}

## Server Logs

This tab allows you to view and download the available [TeamCity server logs](teamcity-server-logs.md), as well as saved thread dumps and memory dumps.

## Internal Properties

This tab displays the TeamCity [server internal properties](server-startup-properties.md#TeamCity+Internal+Properties) and allows modifying them.

## Logging Presets

TeamCity uses the `log4j` library for [logging](teamcity-server-logs.md) internal server activities. In this section you can view and download the available presets, as well as upload new presets, which can then be enabled on the __Diagnostics | Troubleshooting | [Debug Logging](#Debug+Logging)__.

It is also possible to change the logging configuration [manually](teamcity-server-logs.md#Changing+Logging+Configuration).

## Caches

Caches allow TeamCity to quickly get required values without retrieving them from a source or recalculating them every time they are needed. This page allows you to view and dump (reset) all currently used caches.

> TeamCity performs resets its caches automatically during [clean-ups](teamcity-data-clean-up.md). Do not manually reset caches unless you are advised to do so by a JetBrains employee.
> 
{style="warning"}

Depending on their types, TeamCity caches are grouped into the following categories:

<deflist type="medium">

<def title="Cache directory">

Caches that only exist on a file system, in [`<TeamCity Data Directory>`](teamcity-data-directory.md)`/system/caches`. For example:

* `vcsContentCache` — optimizes communications with the VCS server.
* `search` — caches [search results](search.md). Must be reset when enabling [search by build log](search.md#Search+by+Build+Log).

</def>

<def title="Local">

[Node](multinode-setup.md) caches that only exist in the node's memory.

</def>

<def title="Distributed">

[Shared multinode caches](multinode-cache-setup.md) that operate alongside nodes' own local caches.

</def>


<def title="Bypass">

Ignored caches. TeamCity can ignore caches in various scenarios, for instance:

* caches that are explicitly disabled via an [internal property](server-startup-properties.md#TeamCity+Internal+Properties);
* distributed caches without [shared multinode caches](multinode-cache-setup.md) configured.

</def>

<def title="Other">

Combined cache types. For example:

* `git` — contains the bare clone of a remote [Git](git.md) repository.
* `buildsMetadata` — stores build metadata. Must be reset to [reindex the TeamCity NuGet feed](common-problems.md#Problems+with+TeamCity+NuGet+Feed).

</def>

</deflist>

Caches that can report their core data (maximum size or number of records, current utilization, hit rate, and others) do so via TeamCity UI and corresponding [metrics](#Metrics).

## Search

Displays information on the TeamCity data index related to the [search](search.md).

## Browse Data Directory

This tab shows the files in the [TeamCity Data Directory](teamcity-data-directory.md) and allows you to upload new files.
