[//]: # (title: Configuring Dependencies)
[//]: # (auxiliary-id: Configuring Dependencies)

A build configuration can be made dependent on the artifacts or sources of builds of some other build configurations. 

For _[snapshot dependencies](snapshot-dependencies.md)_, TeamCity will run all dependent builds on the sources taken at the moment the build they depend on starts.

For _[artifact dependencies](artifact-dependencies.md)_, before a build is started, all artifacts this build depends on will be downloaded and placed in their configured target locations and then will be used by the build.

>The dependencies of the build can later be viewed on the build results page — the __Dependencies__ tab. This tab also displays indirect dependencies: for example, if build A depends on build B which depends on builds C and D, then these builds C and D are indirect dependencies for build A.

<seealso>
        <category ref="external">
            <a href="https://ant.apache.org/ivy/">Additional information on Ivy</a>
        </category>
        <category ref="concepts">
            <a href="dependent-build.md">Dependent Build</a>
        </category>
        <category ref="admin-guide">
            <a href="patterns-for-accessing-build-artifacts.md">Patterns For Accessing Build Artifacts</a>
            <a href="snapshot-dependencies.md">Snapshot Dependencies</a>
            <a href="artifact-dependencies.md">Artifact Dependencies</a>
        </category>
</seealso>