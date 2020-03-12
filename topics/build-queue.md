[//]: # (title: Build Queue)
[//]: # (auxiliary-id: Build Queue)
The build queue is a list of builds that were [triggered](configuring-build-triggers.md) and are waiting to be started. TeamCity will distribute them to [compatible](agent-requirements.md) build agents as soon as the agents become idle. A queued build is assigned to an agent at the moment when it is started on the agent; no pre\-assignment is made while the build is waiting in the build queue.

When a build is triggered, first it is placed into the build queue, and, when a compatible agent becomes idle, TeamCity will run the build.

## Build Queue Optimization by TeamCity

By default, TeamCity optimizes the build queue as follows:
* if a similar build exists in the queue, a new build (on the same change set and with the same custom properties) will not be added
* if an automatically triggered build chain has more changes than a build chain that is already queued, the latter will be replaced with the automatically triggered build chain, if such replacement will not delay obtaining the build chain results (based on the [estimated duration](#Build+Queue+Tab))
* while a build chain is in the queue, TeamCity tries to replace the queued builds with equivalent started builds
* builds that have been staying in a queue for longer than 15 days are canceled automatically (for example, if there are no compatible agents).

This default behavior can be manually disabled via the corresponding option in the [VCS Build Trigger](configuring-vcs-triggers.md) and [Schedule Build Trigger](configuring-schedule-triggers.md).

## Build Queue Tab

The list of builds waiting to be run can be viewed on the __Build Queue__ tab. 

This tab displays the following information:
* The number of the build in the queue which is a link to the [build results](working-with-build-results.md) page.
* The __branch__ (if available).
* The __build configuration__ name in the following format: __&lt;project name&gt;::&lt;build configuration name&gt;__, where the project and build configuration names are the links to the corresponding overview pages; 
* __Time to start__: the estimated wait duration. Hovering the mouse cursor over the estimated time value shows a tooltip with the following information:
     * the expected start/finish time,
     * the link to the planned agent page.
     * If the current build is a part of a build chain and the builds it depends on are not finished yet, a corresponding note will be displayed. For some builds, like the builds that have never been run before, TeamCity can't estimate possible duration, so the relevant message will be displayed in the tooltip, for example:    

      <img src="unpredictableDuration.png" alt="Unpredictable build duration" width="400"/>
        
* __Triggered by__ \- a brief description of the [event that triggered the build](configuring-build-triggers.md).
* __Can run on__ \- the number of agents compatible with this build configuration. You can click an agent's name link to open the [Agents page](viewing-build-agent-details.md), or use the down arrow to quickly view the list of compatible agents in the pop\-up window.

### Agent Selection for Queued Build

When there are several idle agents that can run a queued build, TeamCity tries to select the fastest one as follows:
1. If no builds have previously run on agents, the [CPU rank](viewing-build-agent-details.md#Agent+Summary) is used to select an agent.
2. If builds have previously run on agents, the estimated build duration for the given build configuration is used to select an agent.  The estimate is made based on the heuristics of the latest builds in the history of the build configuration; for estimating, the execution time of the more recent builds has more weight than that of the earlier builds. [Personal](personal-build.md) and [canceled](build-state.md#Canceled%2FStopped+build) builds are not taken into account, neither are any individual builds whose duration differs significantly from the rest of the builds for this build configuration.

### Ordering Build Queue

You can do the following:
* [reorder](ordering-build-queue.md) the builds in the queue manually.
* [remove](ordering-build-queue.md#Removing+Builds+From+Build+Queue) build configurations or personal builds from the queue.
* If you have System Administrator permissions, you can [assign different priorities to build configurations](ordering-build-queue.md#Managing+Build+Priorities), which will affect their position in the queue.
### Pausing/Resuming Build Queue

The build queue can be paused manually or automatically.

Users with the _Enable/disable agent_ permission (included in the [Agent Manager](role-and-permission.md#Per-Project+Authorization+Mode) role by default) can manually Pause/Resume the Build Queue (since pausing the queue is equivalent to disabling all the agents on the server). 

The build queue can be paused automatically [if the TeamCity Server runs out of disk space](teamcity-disk-space-watcher.md). The queue will be automatically resumed when sufficient space is available.

When the queue is paused, every page in TeamCity will contain a message with information on the reasons for pausing.

 __  __

__See also:__

__Concepts__: [Build Chain](build-chain.md)    
__Administrator's Guide__: [Ordering Build Queue](ordering-build-queue.md)

__ __