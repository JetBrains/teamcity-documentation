[//]: # (title: Dependent Build)
[//]: # (auxiliary-id: Dependent Build)

In TeamCity, one build configuration can depend on one or more configurations. Two types of dependencies can be specified:

* [Snapshot Dependency](#Snapshot+Dependency)
* [Artifact Dependency](#Artifact+Dependency)

An _artifact dependency_ is just a way to get artifacts produced by one build into another. Without a corresponding _snapshot dependency_, it is mainly used when the build configurations are not related in terms of sources. For example, one build provides a reusable component for others.   
A snapshot dependency influences the way builds are processed and implies that the builds are deeply related, one build being a logic part of another.

<anchor name="DependentBuild-SnapshotDependency"/>

## Snapshot Dependency

_Snapshot Dependency_ is a powerful concept that allows expressing source-level dependencies between build configurations in TeamCity. The primary goal is to allow complex build procedures via creating different build configurations linked with snapshot dependencies. This, in particular, allows dividing a single monolith build into a set of interlinked builds ([Build Chain](build-chain.md)) with flexible reuse rules. TeamCity follows the declarative style of defining the build structure on this level (declaring dependencies rather than adding build triggers) as it allows for more flexible and powerful features.

See [Build Dependencies Setup](build-dependencies-setup.md) for a description of typical snapshot dependencies usages and related blog posts: [September 2019](https://blog.jetbrains.com/teamcity/2019/09/build-chains-teamcitys-blend-of-pipelines-part-1-getting-started/), [March 2016](http://blog.jetbrains.com/teamcity/2016/03/teamcity-take-on-build-pipelines/), [April 2012](http://blog.jetbrains.com/teamcity/2012/04/teamcity-build-dependencies-2/).

A snapshot dependency of build configuration A on build configuration B ensures that each build of A has a ["suitable" build](snapshot-dependencies.md#Suitable+Builds) of B before build of A can start. Both builds of A and B use the same sources snapshot (revision of the sources being the same or taken at the same time if the VCS roots are different) when they belong to the same chain and when revisions synchronization is enforced. If revisions synchronization is not enforced, then build A can use up-to-date revisions when promoting a finished build B to A (read more in [Build Chain](build-chain.md#Disabling+Revisions+Synchronization+Between+Chain+Parts)).

The build results page of a build with snapshot dependencies allows reviewing all the dependency builds and their errors, if there are any.

A snapshot dependency alters the builds behavior in the following way:
* when a build is queued, so are the builds from all the build configurations it snapshot-depends on, transitively; TeamCity then determines the revisions to be used by the builds ("checking for changes" process).
* if some of the build configurations already have started builds with matching changes ("suitable builds") and the snapshot dependency has the "_Do not run new build if there is a suitable one_" option ON, TeamCity optimizes the queued builds by using an already finished builds instead of the queued ones. Corresponding queued builds are then silently removed. This procedure can be performed several times, because, while builds of the chain remain in the queue, new builds may start and finish;
* all builds linked via snapshot dependencies are started by TeamCity with explicit specification of the sources revision. The revision is calculated so that it corresponds to the same moment in time (for the same VCS root it is the same revision number). For a queued build chain (all builds linked with a snapshot dependency), the revision to use is determined after adding builds to the queue. At this time, all the VCS roots of the chain are checked for changes and the current revision is fixed in the builds;
* if there is a snapshot dependency and artifact dependency on the __Build from the same chain__ pointing to the same build configuration, TeamCity ensures the download of artifacts from the same-sources build.
* by default, builds that are a part of a build chain are preserved from clean-up, but this can be switched on a per-build configuration basis. Refer to the [Clean-Up](clean-up.md) description for more details.

Depending on the dependencies, topology builds can run sequentially or in parallel.

Behavior on the build chain continuation in case of a build failure is customizable via the snapshot dependency options. For each failed or failed to start dependency you can select one of the four options:
 * __Run build, but add problem__: the dependent build will be run and the problem will be added to it, changing its status to failed (if a problem was not muted earlier)
 * __Run build, but do not add problem__: the dependent build will be run and no problems will be added
 * __Mark build as failed to start__: the dependent build will not run and will be marked as "Failed to start"
 * __Cancel build__: the dependent build will not run and will be marked as "Canceled".

A build of a chain can [reference parameters](predefined-build-parameters.md#Dependencies+Properties) from the preceding builds via `dep.<configurationId>.<parameterName>` syntax.

There is a [special support](predefined-build-parameters.md#Overriding+Dependencies+Properties) for pushing parameters down the chain when a build with snapshot dependencies is triggered. It is done by defining a parameter with `reverse.dep.<configurationId>.<parameterName>` name.

When setting up __triggers__ for the builds in the chain, the recommended approach is: _think about the result_ – the build you want to get at the end of the process, and configure triggers in its corresponding, "top" build configuration. No triggers are necessary in the build configurations this top one depends on, as their builds will be put into the queue automatically when the top one is triggered.   
See also the related "Trigger on changes in snapshot dependencies" [setting](configuring-vcs-triggers.md#Trigger+a+build+on+changes+in+snapshot+dependencies) of a VCS trigger and the "Show changes from snapshot dependencies" [checkbox](build-dependencies-setup.md#show-changes-from-dependencies) in the "Version Control Settings" configuration section.

Let's consider an example to illustrate how snapshot dependencies work.

Let's assume that we have two build configurations, A and B, and configuration A has a snapshot dependency on configuration B and revisions synchronization is enforced.

>If the build configurations connected with a snapshot dependency [share the same set of VCS roots](configuring-vcs-roots.md), all builds will run on the same sources. Otherwise, if the VCS roots are different, changes in the VCS will correspond to the same moment in time.

1. When a build of configuration A is triggered, it automatically triggers a build of configuration B, and both builds will be placed into the build queue. Build B starts first and build A will wait in the queue till build B is finished ([if no other specific options are set](snapshot-dependencies.md)).
2. When builds B and A are added to the queue, TeamCity adjusts the sources to include in these builds. All builds will be run with the sources taken at the moment the builds were added to the queue.   
3. When the build B has finished and if it finished successfully, TeamCity will start to run build A.

>Note that the changes to be included in build A could have become not the latest ones by the moment the build started to run. In this case, build A becomes a [history build](history-build.md).

The example above shows the core basics of snapshot dependencies as a straightforward process without any additional options. For snapshot dependency options, refer to the __[Snapshot Dependencies](snapshot-dependencies.md)__ page.

<anchor name="DependentBuild-ArtifactDependency"/>

## Artifact Dependency
 
>Note that if both a snapshot dependency and an artifact dependency are configured for the same build configuration, in order for it to take artifacts from the build with the same sources, the _Build from the same chain_ option must be selected in the artifact dependency.

Artifact dependencies provide you with a convenient means to use the output ([artifacts](build-artifact.md)) of one build in another build. When an artifact dependency is configured, the necessary artifacts are downloaded to the agent before the build starts. You can then review what artifacts were used in the build or what build used the artifacts of the current build using the __Dependencies__ tab of build results.

To create and configure an artifact dependency, use the __[Dependencies](artifact-dependencies.md)__ build configuration settings page. If you need to download the artifacts inside a build script or locally, you can use the [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html) or [Ivy Ant tasks](artifact-dependencies.md) for that.
 
__Notes on Cleaning Up Artifacts__  
 
Artifacts may not be [cleaned](clean-up.md) if they were downloaded by other builds and these builds are not yet cleaned up. For a build configuration with configured artifact dependencies, you can specify whether the artifacts downloaded by this configuration from other builds can be cleaned or not. This setting is available on the [clean-up policies](clean-up.md) page.

<seealso>
        <category ref="concepts">
            <a href="build-artifact.md">Build Artifact</a>
        </category>
        <category ref="admin-guide">
            <a href="configuring-dependencies.md">Configuring Dependencies</a>
            <a href="build-dependencies-setup.md">Build Dependencies Setup</a>
        </category>
</seealso>
