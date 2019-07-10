[//]: # (title: Configuring Dependencies)
[//]: # (auxiliary-id: Configuring Dependencies)

A build configuration can be made dependent on the artifacts or sources of builds of some other build configurations. 


For _[snapshot dependencies](snapshot-dependencies.md)_, TeamCity will run all dependent builds on the sources taken at the moment the build they depend on starts.

For _[artifact dependencies](artifact-dependencies.md)_, before a build is started, all artifacts this build depends on will be downloaded and placed in their configured target locations and then will be used by the build.


<tip>

The dependencies of the build can later be viewed on the build results page \- the Dependencies tab. This tab also displays indirect dependencies, e.g. if a build A depends on a build B which depends on builds C and D, then these builds C and D are indirect dependencies for build A.
</tip>



 __  __


__See also:__

__Concepts__: [Dependent Build](dependent-build.md)

__Administrator's Guide__: [Patterns For Accessing Build Artifacts](patterns-for-accessing-build-artifacts.md) | [Snapshot Dependencies](snapshot-dependencies.md) | [Artifact Dependencies](artifact-dependencies.md)

__External Resources__: [http://ant.apache.org/ivy/](http://ant.apache.org/ivy/) (additional information on Ivy)
