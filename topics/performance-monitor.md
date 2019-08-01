[//]: # (title: Performance Monitor)
[//]: # (auxiliary-id: Performance Monitor)
The _Performance Monitor_ build feature allows you to get the statistics on the CPU, disk and memory usage during a build run on a build agent. 

The Performance Monitor supports Windows, Linux, MacOs, Solaris, and, __since TeamCity 2017.1__, FreeBSD operating systems. All the operating systems except Windows require [Perl](https://learn.perl.org/installing/) installed.

Note that the performance monitor reports the load of the whole operating system. It will not report proper results if you have more than one agent running on the same host, or an agent and a server installed on the same machine. 

<note>

If you run your build agent as a Windows [service](setting-up-and-running-additional-build-agents.md#Build+Agent+as+a+Windows+Service), the user starting the agent must be a member of the Performance Monitor Users group to be able to monitor performance metrics. Users can be added to the group via the `lusrmgr.msc` command.
</note>

When enabled, each build has an additional tab called __PerfMon__ on the build results page, where this statistics is presented as a graph. The CPU usage shows the average CPU load during the build, the Disk and Memory usage values are relative to the total disk and memory available on the agent machine. 

Clicking on a point in the graph displays the corresponding value (CPU in  screenshot below) as well as the build log with the highlighted fragment for the selected time:

<img src="performance-monitor.png" width="1433" alt="Performance monitor"/>

