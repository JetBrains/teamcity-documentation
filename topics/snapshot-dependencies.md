[//]: # (title: Snapshot Dependencies)
[//]: # (auxiliary-id: Snapshot Dependencies)

By setting a [snapshot dependency](dependent-build.md#Snapshot+Dependency) of a build (for example, build B) on other build's (build A's) sources, you can ensure that build B will start only after the one it depends on (build A) is run and finished. We call build A a _dependency_ build, whereas build B is a _dependent_ build.

The Dependencies page of the build configuration settings displays the configured dependencies and __since TeamCity 2017.1__, the Snapshot dependencies section of the page allows previewing the build chain and its configuration. The preview shows builds of the chain; the builds with automatic triggering configured are marked with this icon ![v.png](v.png).

When adding a new snapshot dependency, the following options need to be specified:

<anchor name="EnforceRevisionsSynchronization"/>

<table><tr>

<td width="150">

Option


</td>

<td>

Description


</td></tr><tr>

<td>

Depend on


</td>

<td>

Specify the build configuration for the current build configuration to depend on.


</td></tr><tr>

<td>

<anchor name="enforce-rev-sync"/>

Enforce revisions synchronization

</td>

<td>

For all builds linked by snapshot dependencies with this option __enabled__, TeamCity will use the same sources snapshot (for example, the same revision for the same VCS root). This is a recommended setting for configurations that need to use the same state of the sources to build successfully.

If you __disable__ this option for a snapshot dependency, then when a dependency build is promoted to the current build configuration, the build of the current build configuration will use the most recent revision of the sources instead of the revision corresponding to the promoted dependency. This is useful when the builds do not have strict sources dependencies (for example, as with package and deploy steps).

Note that the sources snapshot rule is only applied to the [parts of the builds chain](build-chain.md#Disabling+Revisions+Synchronization+Between+Chain+Parts)) linked via the snapshot dependencies with the option enabled.

</td></tr><tr>

<td>

Do not run new build if there is a suitable one


</td>

<td>

If the option is enabled, TeamCity will not run a dependency build, if another running or finished dependency build with the appropriate sources revision exists. See also [Suitable Builds](#Suitable+Builds) below. However, when a dependent build is triggered, the dependency build will also be put into the queue. Then, when the changes for the build chain are collected, this dependency build will be removed from the queue and the dependency will be set to a suitable finished build.

<note>

Note: if there is more than one snapshot dependency on some build configuration, then for builds reusing to work, all of them must have the "Do not run new build if there is a suitable one" option enabled.
</note>




[//]: # (Internal note. Do not delete. "Snapshot Dependenciesd292e62.txt")    





</td></tr><tr>

<td>

Only use successful builds from suitable ones


</td>

<td>

A new triggered build will only "use" successfully finished suitable builds as dependencies. If the latest finished "suitable" build is failed, it will be re\-run.


</td></tr><tr>

<td>

<anchor name="RunOnTheSameAgent"/>

Run build on the same agent


</td>

<td>

When enabled, and B snapshot\-depends on A, then builds of B are run on the same agent where the build of A from the same build chain was run.

<note>

Before starting a build chain having __run on the same agent__ dependencies, TeamCity forms groups of builds combined by __run on the same agent__ dependency, and for each starting build participating in such a group TeamCity chooses agents which can be used by any build of the group. Thus this option makes sense for [composite builds](composite-build-configuration.md) too, even though composite build does not occupy an agent, it still can form a group of builds combined by such dependencies. For instance, composite build B having __run on the same agent__ dependencies on A and C will cause both A and C use the same agent.
</note>


</td></tr><tr>

<td>

<anchor name="on-failed-dependency"/>

On failed dependency/  On failed to start/canceled dependency


</td>

<td>


If a dependency fails, you can manage the status of the dependent build by selecting one of the following options:

* __Run build, but add problem__: the dependent build will be run and the problem will be added to it, changing its status to failed (if problem was not muted earlier)
* __Run build, but do not add problem__: the dependent build will be run and no problems will be added
* __Make build failed to start__: the dependent build will not run and will be marked as "Failed to start"
* __Cancel build__: the dependent build will not run and will be marked as "Canceled".


</td></tr></table>

### Suitable Builds




[//]: # (Internal note. Do not delete. "Snapshot Dependenciesd292e145.txt")    


A "suitable" build in terms of snapshot dependencies is a build which can be used instead a queued dependency build within a [build chain](build-chain.md). That is, a queued build which is a part of a build chain can be dropped and the builds depending on it can be made dependent on another queued, running or already finished "suitable" build. This behavior only works when the "_Do not run new build if there is a suitable one_" option of a corresponding snapshot dependency is selected.

For a build to be considered "suitable", it should comply with all of the conditions below:
* use the same sources snapshot as the entire queued build chain being processed. If the build configurations have the same VCS settings, this basically means the one with the same sources revision. If the VCS settings are different (VCS roots or checkout rules), then "same sources snapshot" revisions means revisions taken simultaneously at some moment in time.
* be successful (if "Only use successful builds from suitable ones" snapshot dependency option is set)
* be a usual, not a [personal build](personal-build.md)
* have no customized parameters, also considering those set via `reverse.dep.` parameters (related feature request: [TW-23700](http://youtrack.jetbrains.com/issue/TW-23700))
* the original build should not be triggered selecting "rebuild" option for the dependency build configuration in question
* have no VCS settings preventing effective revision calculation, see [below](#VCS+Settings+Disabling+Builds+Reuse)
* there is no other build configuration snapshot\-depending on the current one with "Do not run new build if there is a suitable one" option set to "off"
* the running build is not "hanging"
* settings of the build configuration were not changed since the build (that is, the build was run with the current build configuration settings). This also includes no changes to the parameters of all the parent projects of the build configuration. You can check if the settings were changed between several builds by comparing `.teamcity/settings/digest.txt` file in the [hidden build's artifacts](build-artifact.md#Hidden+Artifacts)
* if there is also an artifact dependency in addition to snapshot one, the suitable build should have artifacts
* all the dependency builds (the builds the current one depends on) are "suitable" and are appropriately merged

#### VCS Settings Disabling Builds Reuse


Some settings in VCS roots can effectively disable builds reusing. These settings are:
 
* Subversion: __Checkout, but ignore changes__ mode
* CVS: __Checkout by tag__ mode
* Perforce: __Stream__ or __Client__ connection settings, or label is specified in __Label/revision to checkout__ option

* Starteam: checkout mode option set to __view label__ or __promotion date__
 
  <seealso>
        <category ref="concepts">
            <a href="dependent-build.md">Dependent Build</a>
        </category>
</seealso>
