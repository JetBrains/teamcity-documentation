[//]: # (title: Ordering Build Queue)
[//]: # (auxiliary-id: Ordering Build Queue)

This page lists the options to manage the queue builds manually. Automatic build queue optimization is detailed in the [separate section](build-queue.md#Build+Queue+Optimization+by+TeamCity).

## Manually Reordering Builds in Queue

To reorder builds in the [Build Queue](build-queue.md), you can drag them to the desired position.

### Moving Builds to Top

* For any queued build, do one of the following: 
   * on the [Build Queue](build-queue.md) page, click the arrow button next to the build sequence number ![moveToTop.png](moveToTop.png) to move the build to the top of the queue.
   * click the build number or build status link anywhere in the UI, and, on the [queued build ](working-with-build-results.md)page, click the __Actions__ menu in the top right. Select the __Move to top__ action. The queue position will change. For a composite build, the whole build chain will be moved to the top of the queue.
 * For a running composite build which has dependencies that have not yet started, click the build number or build status link anywhere in the UI, and, on the [running build](working-with-build-results.md) page, click the __Actions__ menu in the top right. Select the __Move queued dependencies to top__ action. All queued dependencies of this build will be moved to the top of the queue.

## Managing Build Priorities

By default, builds are placed in the build queue in the order they are triggered: most recently triggered build is add to the bottom of the queue. It is possible to change build's priorities so that builds are inserted into the build queue at a position depending on their defined priority and the wait time of the currently queued builds.

In TeamCity you can control build priorities by creating _Priority Classes_. A priority class is a set of build configurations with a specified priority (the higher the number, the higher the priority. For example, `priority=2` is higher than `priority=1`). The higher priority a configuration has, the higher place it gets when added to the Build Queue.   
To access these settings, on the __Build Queue__ tab, click the __Configure Build Priorities__ link in the upper right corner of the page.

<tip>

Note that only users with the __System Administrator__ role can manage build priority classes.
</tip>

By default, there are two predefined priority classes: _Personal_ and _Default_, both with `priority=0`.
* All personal builds ([Remote Run](remote-run.md) or [Pre-tested Commit](pre-tested-delayed-commit.md)) are assigned to the _Personal_ priority class once they are added to the build queue. Note that you can change the priority for personal builds here.
* The _Default_ class includes all the builds not associated with any other class. This allows creating a class with priority lower than default and place some builds to the bottom of the queue.

To create a new priority class:
1. Click __Create new priority class__.
2. Specify its name, priority (in the range `-100..100`) and additional description. Click __Create__.
3. Click the __Add configurations__ link to specify which build configurations should have priority defined in this class.

This setting is taken into account only when a build is added to the queue. To ensure that builds with lower priority always have a chance to run, TeamCity also considers how long each build is staying in the queue. This allows running a long awaiting build with lower priority before the recently added builds with higher priority. For a detailed explanation of this behavior, refer to the [algorithm description](https://confluence.jetbrains.com/display/TW/Build+Queue+Priorities#BuildQueuePriorities-Algorithmdetails).

## Removing Builds From Build Queue

To remove build(s) from the queue, check the __Remove__ box next to the selected build and confirm the deletion. If a build to be removed from the queue is a part of a build chain, TeamCity shows the following message below the comment field: "This build is a part of a build chain". Refer to the [Build Chain](build-chain.md#Stopping%2FRemoving+From+Queue+Builds+from+Build+Chain) description for details.

Additionally, you can:
* Remove all your personal builds from the queue at once from the __Actions__ menu.
* Remove several builds of [paused build configurations](build-configuration.md#Pausing+Several+Build+Configurations+in+Project) from the queue.

## Limiting Maximum Size of Build Queue
{product="tc"}

It is possible to limit the maximum number of builds in the queue. By default, the limit is set to 3000 builds. The default value can be changed using the `teamcity.buildTriggersChecker.queueSizeLimit` [internal property](server-startup-properties.md#TeamCity+Internal+Properties).

When the queue size reaches the limit, TeamCity will pause [Configuring Build Triggers](configuring-build-triggers.md). Automatic build triggering will be reenabled once the queue size gets below limit. While triggering is paused, a warning message is shown to all the users.

 While __automatic__ triggering is paused, it is still possible to add builds to the queue [manually](running-custom-build.md).

 <seealso>
        <category ref="concepts">
            <a href="build-queue.md">Build Queue</a>
        </category>
</seealso>