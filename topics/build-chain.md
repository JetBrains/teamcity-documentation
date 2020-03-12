[//]: # (title: Build Chain)
[//]: # (auxiliary-id: Build Chain)

A _build chain_ is a sequence of builds interconnected by [snapshot dependencies](dependent-build.md#Snapshot+Dependency). Sometimes the build chain is called a "pipeline". Parts of a build chain linked with snapshot dependencies with enabled revisions synchronization use the same snapshot of the sources.

## Common Use Case

The most common use case for specifying a build chain is running the same test suite of your project on different platforms. For example, before a _release build_ you want to make sure the tests are run correctly under different platforms and environments. For this purpose, you can instruct TeamCity to run tests, then an integration build first, and a release build after that.

Let's see how the build chain mechanism works in details. On triggering a dependent build of the _Release_ build configuration, TeamCity does the following:
1. Resolves a chain of all build configurations that the _Release_ build configuration snapshot depends on.
2. Checks for changes for all dependent build configurations and synchronizes them when the first build in the build chain enters a build queue.
3. Adds all the builds that need building with specific revisions to the build queue.

## Configuring Build Chains

__To specify dependencies in your build configuration__:
1. On the __Build Configuration Settings__ page, select __Dependencies__.
2. On the __Dependencies__ page, click the __Add new snapshot dependency__ link.

See also [Build Dependencies Setup](build-dependencies-setup.md) for details and an example.

## Stopping/Removing From Queue Builds from Build Chain

If a build being stopped or removed from the build queue is a part of a build chain, there is a message below the comment field: "_This build is a part of a build chain_".

If there are other running or queued parts of the build chain (i.e. other running builds or queued builds, which are connected with the build under the action), these builds will be listed below under the label "_Stop other parts_".

If a user has access rights to stop a build in the list, there is a checkbox near it. The checkbox is selected by default if stopping the current build will definitely cause the build in the list to fail (for instance, if the listed build depends on the original build being stopped).

If user has no access right to stop a build from the list, the checkbox is not visible.

Selecting the checkbox marks the selected build for a stop/removal from queue.

If a user has no access right to view a build which is a part of the build chain, this build is not visible to the user at all. If there is at least one such build, there is a warning displayed: "_You don't have access rights to all its parts_". The stripe is shown right under the message "_This build is a part of a build chain_".

In case when all other parts of the build chain cannot be viewed by the current user, we show a yellow stripe with warning: "_You don't have access rights to see its other parts_".

If there are no running or queued builds for the build chain (i.e. all other parts of the build chain have finished), no additional information is shown.

## Disabling Revisions Synchronization Between Chain Parts

You can [disable revisions synchronization](snapshot-dependencies.md#enforce-rev-sync) for a snapshot dependency of a build configuration when promoting a build.   
This option works if you promote a build from chain part 1 to chain part 2, and the first build configuration of part 2 has this option disabled. In this case, TeamCity can use different sources revisions for builds in part 1 and part 2. See the build setup example in [Build Dependencies Setup](build-dependencies-setup.md#Turned+off+Enforced+Revisions+Synchronization).

This is useful when you need to run a dependent build without synchronizing its code revision with its dependencies (preceding builds in a chain).   
For example, you can promote an older build to a [deployment build configuration](deployment-build-configuration.md), and this build will be run using the latest deployment scripts.
In load/acceptance testing, when you store tests in a version control system and often change them to test your system, you do not need to rebuild your application entirely; instead, you can pick up the chain directly from the testing phase.


## Build Chains Visual Representation

Basically, each build chain is a [directed acyclic graph](http://en.wikipedia.org/wiki/Directed_acyclic_graph), i.e. it cannot have cycles.

Build Chains are visible in various places in the TeamCity web UI:

* [Dependencies page of build configuration settings](#Dependencies+page+of+build+configuration+settings)
* [Build Chains tab of the Project Home and Build Configuration Home pages](#Build+Chains+tab+of+Project+Home+and+Build+Configuration+Home+pages)

### Dependencies page of build configuration settings

If a build configuration is a part of a build chain, the corresponding information is displayed in the __Build Configuration settings | Dependencies | Snapshot dependencies__. Clicking the build chain link opens the preview of the build chain and its configuration in a separate window. The preview shows builds of the chain; the builds with automatic triggering configured are marked with ths icon: ![v.png](v.png).

<img src="snapshotDepPreview.png" alt="Preview of snapshot dependencies"/>

<note>

If build configurations in a chain use feature branches, the [logical names](working-with-feature-branches.md#Logical+branch+name) of the branches are also represented as build labels on the chain graph. Note that these names [may differ](working-with-feature-branches.md#Dependencies) from the VCS roots actually used for the builds: if a build uses a default branch but depends on a build that uses a non-default branch, the label of the dependency build will reflect this. For example, if build A uses the default branch "master" but depends on build B that uses branch "v1.1", both builds will be labeled with "v1.1".

</note>

### Build Chains tab of Project Home and Build Configuration Home pages

You can review build chains on both project and build configuration pages: each of those pages has a __Build Chains__ tab appearing when snapshot dependencies are configured. 

The tab displays the list of build chains that contain builds of this project or this build configuration with the ability to filter them. Note that build chains are sorted so that the build chain with the last finished build appears at the top of the list.

The pie\-chart icon displays the ratio of the statuses for builds that are parts of the chains. On hovering over the pie chart, the details are displayed:

<img src="buildChainsCollapsed.png" alt="Collapsed build chains"/>

When a chain is expanded, the following information is also available:

   * all builds this build chain is comprised of
   * status of these builds: not triggered, in queue, running or finished and its details
   * the chain displays builds in order of actual execution, i.e. builds that start first are on the left.

Clicking a build in a chain highlights the selected build and all its direct dependencies. This page:

   * provides a compact representation of chains: if several top builds triggered the same chain of dependent builds, TeamCity displays one build chain with several top builds.
   * has additional display options: Group by projects and Hide details.
   * transitively highlights all the downstream/upstream builds when a build is selected in a build chain.


From this page you can also:
   * Continue a chain, if there are yet "not triggered" builds. Click the __Run__ button and a new build will be started on the chain revisions and associated with builds from this chain.
   * Click ![custom_build(1).png](custom_build_1.png)  to open the [custom build dialog](triggering-a-custom-build.md) with build chain revisions preselected. This action can be used if you want to re\-run some build in the chain.

### Dependencies tab of build results page

If dependencies are configured, you can view their details on the build results page, the __Dependencies__ tab. This tab also displays indirect dependencies, for example, if a build A depends on a build B which depends on builds C and D, then these builds C and D are indirect dependencies for build A.   
The tab also displays artifacts downloaded and delivered by the builds of the chain. It also allows grouping/ungrouping builds and highlighting the builds reused from previous chains ([suitable builds](snapshot-dependencies.md#Suitable+Builds)).

 __  __

__See also:__


__Concepts__: [Dependent Build](dependent-build.md)   
__Administrator's Guide__: [Configuring Dependencies](configuring-dependencies.md) | [Build Dependencies Setup](build-dependencies-setup.md)

__ __