[//]: # (title: Performance Monitor)
[//]: # (auxiliary-id: Performance Monitor)

The _Performance Monitor_ [build feature](adding-build-features.md) allows you to get the statistics on the CPU, disk I/O, and memory usage during a build run on a build agent. The build feature is enabled by default for [build configurations created from a URL](creating-and-editing-build-configurations.md#Creating+Build+Configuration+from+URL).  Performance Monitor supports Windows, Linux, macOS, Solaris, and FreeBSD operating systems.


Note that Performance Monitor reports the load of the whole operating system. It will not report proper results if you have more than one agent running on the same host, or if an agent and a server are installed on the same machine.

<note>

If you run a build agent [as a Windows service](start-teamcity-agent.md#Build+Agent+as+Windows+Service), the user starting the agent must be a member of the _Performance Monitor Users_ group to be able to monitor performance metrics. Users can be added to the group via the `lusrmgr.msc` command.
</note>

After you enable the Performance Monitor [build feature](adding-build-features.md) for a build configuration, the __Build Results__ page of each newly run build in this configuration will contain the __PerfMon__ tab with a graph of performance statistics. The _CPU_ value reflects the average CPU load during the build. The _Disk I/O_ shows how much of the CPU time is spent on disk input-output operations. The available _Memory_ value is calculated relatively to the physical memory of the agent machine.

Click any point on the graph to view the corresponding value (CPU on a screenshot below) and view the portion of the build log that corresponds to this timeframe.

<img src="performance-monitor.png" width="750" alt="Performance monitor"/>


## External Tools Utilized

On Linux and macOS agents performance monitor obtains statistics from `vmstat`, `/proc/diskstats` and `/proc/meminfo`. Windows agents report performance data using the Windows Management Instrumentation (WMI).

PerfMon requires [Perl](https://learn.perl.org/installing/) to be installed on any used OS except Windows.

If a build agent has [Telegraf](https://www.influxdata.com/time-series-platform/telegraf/) installed, TeamCity automatically detects it and starts using it for collecting performance data. Telegraph allows PerfMon to report the additional "% of free disk space" value.