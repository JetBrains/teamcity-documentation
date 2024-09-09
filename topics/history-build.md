[//]: # (title: History Build)
[//]: # (auxiliary-id: History Build)

A _history build_ is a build that starts after a build with more recent changes. That is, a history build is a build that disrupts a normal build flow according to the [order of the source revisions](revision.md#Revision+order).

<!--[//]: # (Internal note. Do not delete. "History Buildd159e7.txt")-->    

A build may become a history build in the following situations:
* If you initiate a build on particular changes manually using the _[Run Custom Build](running-custom-build.md)_ dialog.
* If you have a [VCS trigger](configuring-vcs-triggers.md) with a quiet period set. During this quiet period, a different user can start a build with more recent changes. In this case, your automatically triggered build will have an older source revision when it starts, and will be marked as a history build.
* If there are several builds of the same configuration in the build queue and they have fixed revisions (for example, they are part of a [build chain](build-chain.md)). If someone manually reorders these builds, the build with fewer changes can be started first.

As the history build does not reflect the current state of the sources, the following restrictions apply to its processing:
* The status of a history build does not affect the project/build configuration status.
* A user does not get notifications about history builds unless they subscribed to notifications on all builds in the build configuration.
* History builds are not shown on the __Projects__ or __Build Configuration Home__ page as the last finished build of a configuration.
* The [Investigation](investigating-and-muting-build-failures.md) option is not available for history builds.

<!--[//]: # (Internal note. Do not delete. "History Buildd159e60.txt")-->    

 <seealso>
        <category ref="concepts">
            <a href="working-with-build-queue.md">Working with Build Queue</a>
        </category>
        <category ref="admin-guide">
            <a href="running-custom-build.md">Triggering a Custom Build</a>
        </category>
</seealso>
