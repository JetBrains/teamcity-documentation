[//]: # (title: Composite Build Configuration)
[//]: # (auxiliary-id: Composite Build Configuration)

TeamCity provides the _composite_ type of build configuration.

The purpose of the composite build configuration is to aggregate results from several other builds combined by _snapshot dependencies_ and present them in a single place. To some extent, a composite build can be viewed as a build which consists of several parts which can be executed in parallel on different agents. All these parts will have a synchronized snapshot of the source code, and the results can be seen in a single place.

## Creating Composite Configuration

When [creating a build configuration](creating-and-editing-build-configurations.md), you can specify _Composite_ as its type and specify its snapshot dependencies: the build configurations whose builds results the current composite one will aggregate.

There are important differences between composite builds and regular builds with snapshot dependencies:
* A composite build does not occupy an agent; as a result, some settings which require an agent (for example, build steps or requirements) are not applicable.
* A composite build often acts as a __single__ build regardless of the number of dependencies:
    * A composite build is shown as _running_ at the time when the first dependency of the build chain starts, and is shown as _finished_ only when the last dependency finishes.
    * If a composite build is stopped or removed from the queue, all its dependencies, that is the whole build chain, are stopped/removed as well.
    * It is possible to limit the number of running composite builds, but in case with a composite build it will affect the build and all its dependencies: if the limit is set to 1 and a composite build is already running, another composite build with all its dependencies will wait in the queue until the current one finishes.
    * A composite build aggregates results from dependencies (for example, all of the tests) and shows all failed/muted or ignored tests of the chain in a single place, the same applies to code coverage or results of code inspection/code duplicates analysis.
    * The status line of the composite builds reflects the current chain state: the number of running/queued/finished or failed dependencies.
    * The progress indicator of the composite build reflects all of the dependencies, so it actually shows when the whole chain is going to finish.
* A composite build does not have its own artifacts. However, you can make it display the artifacts published by its parts (dependencies) via artifact dependencies.

## Clean-up of Composite Build Artifacts

Composite builds only display the artifacts published by its parts via artifact dependencies, which means сomposite builds do not have their own artifacts (except for internal artifacts). To enforce a certain clean-up policy for artifacts of a composite build, you need to have the same clean-up rules for all its parts.

__ __