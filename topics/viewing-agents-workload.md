[//]: # (title: Viewing Agents Workload)
[//]: # (auxiliary-id: Viewing Agents Workload)
TeamCity provides handy ways to estimate build agents efficiency and help you manage your system:

## Load Statistics Matrix

The Matrix available at the __Matrix__ tab on the __Agents__ page provides you with a bird's eye view of the overall Build Agents workload for all finished builds during the time range you selected. By taking a look at the build configurations compatible with a particular agent, you can [assign the build configuration to particular Build Agents](assigning-build-configurations-to-specific-build-agents.md) and significantly lower the idle time. This helps you adjust the hardware resources usage more effectively and fill the discovered productivity gaps.

<img src="agent-matrix.png" width="750" alt="Agents matrix"/>

agent-matrix.png

1. Specified time range. Click __Update__ to reload the matrix.
2. [Agent pool](agent-pools.md). Clicking a link opens the pool's page.
3. Individual agents. Clicking a link opens the [agent's details](viewing-build-agent-details.md) page.
4. Total build duration during the specified time.
5. Total build duration for a particular build configuration during the specified time range.
6. Total duration for a particular build configurationof builds that were run on the particular build agent during the specified time period.
7. The build agent is compatible with the build configuration, but no builds were run during the specified time range.
8. The build agent is incompatible with the configuration, or disconnected.

## Build Agents' Workload Statistics

TeamCity provides a number of visual metrics, namely, statistics of Build Agents' activity and their usage during a particular time period. This information is available at the __Statistics__ tab on the __Agents__ page.

<img src="agents-statistics.png" width="750" alt="Agents statistics"/>

You will find this feature helpful in:
* your daily administration activities aimed at lowering your company's network workload
* locating and eliminating the gap between the most frequently used computers and those which are often idle
* reducing the cost of your hardware resources ownership

__  __

__See also:__

__Concepts__: [Build Agent](build-agent.md)   
__Installation and Upgrade__: [Installing and Running Additional Build Agents](installation.md)

__ __