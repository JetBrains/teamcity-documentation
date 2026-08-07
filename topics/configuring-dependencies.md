[//]: # (title: Configuring Chain Dependencies)
[//]: # (help-id: Build Configuration Dependencies;Configuring Dependencies;Snapshot Dependencies;Build Dependencies Setup;Dependent Build)

<show-structure for="chapter" depth="2"/>

A [build chain](build-chain.md) is assembled by declaring dependencies in **downstream** objects that point to **upstream** ones. The available settings are the same for both build configurations and pipelines — only the UI and code representation differ.

## Snapshot dependencies

_Snapshot dependencies_ link two build configurations in a chain. The term "snapshot" refers to source revision synchronization: both the upstream and downstream builds share the same code snapshot, which is the key guarantee that makes the chain meaningful.

To add a snapshot dependency to a build configuration:

1. <include from="common-templates.md" element-id="open-configuration-settings-tab"><var name="configuration-tab-name" value="Dependencies"/></include>
2. Click **Add new snapshot dependency** and select the upstream configuration.

```Kotlin
// "Build → Test → Deploy" chain.
// Upstream configurations have no dependencies — add them only to downstream ones.

object Test : BuildType({
    name = "Test"
    dependencies {
        snapshot(Build) {}   // Test runs after Build
    }
})

object Deploy : BuildType({
    name = "Deploy"
    dependencies {
        snapshot(Test) {}    // Deploy runs after Test
    }
})
```

> Snapshot dependencies control execution order and revision synchronization only — they do not transfer files between builds. To pass artifacts between chain members, add [artifact dependencies](artifact-dependencies.md#Artifact+dependencies) as well.
>
{style="tip"}

## Pipeline dependencies

_Pipeline dependencies_ link pipelines to other pipelines or to classic build configurations.

To add a pipeline dependency:

1. Click anywhere in the pipeline canvas area (outside any job) to open pipeline-level settings.
2. Click **Add** in the **Pipeline dependencies** section.
3. Select the upstream pipeline or build configuration from the **Depend on** list.

<img src="dk-pipeline-dependency.png" width="706" alt="Pipeline dependency"/>

<tabs>
<tab title="YAML">

```yaml
# Simple form — uses default settings
dependencies:
  - UpstreamPipeline

# Extended form — override specific settings
dependencies:
  - UpstreamPipeline:
      reuse: none
      enforce-revisions-synchronisation: true
      on-failed-dependency: run-add-problem
      on-incomplete-dependency: cancel
```

</tab>
<tab title="Kotlin DSL">

```Kotlin
object DownstreamPipeline : Pipeline({
    name = "Downstream Pipeline"
    dependencies {
        snapshot(UpstreamPipeline) {
            reuseBuilds = ReuseBuilds.NO
        }
    }
})
```

</tab>
</tabs>

> To set up the reverse direction — a build configuration depending on a pipeline — add a snapshot dependency in the configuration settings and select the pipeline as the upstream object.
>
{style="tip"}

## Dependency settings

The following settings apply to both snapshot dependencies and pipeline dependencies.

<img src="dk-pipeline-dependency-settings.png" width="706" alt="Dependency settings"/>

<deflist type="full">

<def title="Depend on">

Choose the upstream configuration or pipeline that must finish before this object can start.

</def>

<def title="Enforce revisions synchronization" id="enforce-rev-sync">

<snippet id="enforce-rev-sync-description">

Specifies whether TeamCity should ensure both objects linked by a dependency use the same revision of code sources.

* **Revision synchronization enabled**: recommended for setups that need to use the same state of the sources. For example, in the "A &rarr; B" chain: "A" starts on revision 1.2 and is promoted to "B" when finished. Build "B" runs on the same 1.2 revision even if its latest revision is 1.4.

* **Revision synchronization disabled**: use this setup when builds do not have strict sources' dependencies (for example, separate package and deploy steps). In this case, a downstream build uses the latest available revision. In the "A &rarr; B" chain: "A" starts on revision 1.2 and is promoted to "B", but "B" runs on its latest 1.4 revision.

See [Revision Synchronization](configuring-dependencies.md#Revision+synchronization) for the effects this setting has on a whole build chain.

</snippet>

</def>

<def title="Do not run new build if there is a suitable one">

<snippet id="do-not-run-new-build-if-there-is-a-suitable-one-description">

If this option is enabled, TeamCity does not run a new upstream build when another running or finished build with the appropriate sources' revision already exists. See [Suitable Builds](configuring-dependencies.md#Suitable+builds) for the criteria TeamCity uses to determine a reusable build.

In this case, when a downstream build is triggered, the upstream build is still put into the queue. Then, once the changes for the chain are collected, this upstream build is removed from the queue and the dependency is linked to the suitable finished build instead.

> For upstream pipelines, you might also want to disable the **Reuse Job Results** toggle in [job settings](job-settings.md#Optimizations). Otherwise, jobs can reuse their previous results even when the dependency setting calls for new runs.
>
{style="tip"}

</snippet>

</def>

<def title="Only use successful builds from suitable ones">

<snippet id="reuse-only-successful">

A new triggered build will only use successfully finished [suitable builds](configuring-dependencies.md#Suitable+builds) as dependencies. If the latest finished suitable build failed, it is rerun.

</snippet>

</def>

<def title="Run build on the same agent">

When enabled, the downstream build runs on the same build agent that ran the upstream build within the same chain. Use this when an upstream build modifies system state — installed tools, environment variables, or local files — that the downstream build relies on.

> This setting is available for build configurations only. For pipelines, use [agent requirements](job-settings.md#Agent+Requirements) to control which agents a job can run on.
>
{style="note"}

</def>

<def title="On failed dependency / On failed to start or canceled dependency" id="on-failed-dependency">

<snippet id="on-failed-dependency-description">

These settings let you control whether a downstream build should run if its upstream build fails, and, if it should, whether the same build problem should appear in its results.

* **Run build, but add problem**: the downstream build runs and the problem is added to it, changing its status to failed (unless the problem was muted earlier).
* **Run build, but do not add problem**: the downstream build runs and no problem is added.
* **Mark build as failed to start**: the downstream build does not run and is marked as "_Failed to start_".
* **Cancel build**: the downstream build does not run and is marked as "_Canceled_".

</snippet>

</def>

</deflist>


## Re-run failed chain builds
{instance="tc"}

Build failures generally fall into two categories: true failures that recur on every run (a syntax error, a missing reference), and transient ones that a plain retry can resolve — flaky tests, checkout hiccups, or a temporarily unavailable external resource (AWS S3, Dockerhub, NuGet, maven.org, and so on). Re-running an entire chain to work around a transient failure at its far end can be costly, so TeamCity offers three ways to retry a failed build without restarting the whole chain.

### Automatic retries

If a build can no longer continue due to an infrastructure issue (for example, TeamCity loses connection to its agent), TeamCity starts a replacement build automatically, for both standalone and chain builds. This requires no manual configuration on your side.

### Retry build triggers

Add a [Retry build trigger](configuring-retry-build-trigger.md) to a configuration to start a new build automatically whenever the previous one fails.

```Kotlin
import jetbrains.buildServer.configs.kotlin.*

object BuildB : BuildType({
    name = "Build B"

    steps { ... }

    triggers {
        retryBuild {
            branchFilter = ""
        }
    }

    dependencies { ... }
})
```

For a configuration that is part of a chain, also enable **Trigger a new build with the same revisions**. TeamCity will then reuse every successful build from the previous chain run and only rebuild the failed dependencies, on the same revision.

This trigger does not pause the chain: a new build is queued to replace the failed one, but downstream builds proceed based on the original failure. Thus, a downstream build can still end up red with the "Snapshot dependency failed" error.

### Dependency retry settings

Unlike a retry trigger, dependency retry settings make a downstream build wait. If a direct or indirect snapshot dependency fails, TeamCity delays the downstream build and retries the failed dependency automatically, up to a set number of attempts, before proceeding. While a retry is pending, the unsuccessful upstream build is marked as canceled rather than failed.

<img src="retry-dependency-overview.png" width="706" alt="Retry failed dependencies"/>

These settings can be configured in the **Dependencies** tab of build configuration settings.

```Kotlin
import jetbrains.buildServer.configs.kotlin.*
import jetbrains.buildServer.configs.kotlin.buildSteps.script

object DownstreamBuild : BuildType({
    name = "Downstream build"

    steps { ... }
    
    dependencies {
        snapshot(UpstreamBuild) { ... }
        retrySettings {
            maxAttempts = 3
            retryOnSameFailure = true
        }
    }})
```

<deflist type="medium">

<def title="Use custom retry settings">

Sets up retry behavior for this configuration explicitly. Otherwise, with **Use retry settings from the nearest dependent (downstream) build** enabled, the configuration inherits its settings from whichever build depends on it. You can define retry settings once, on the last configuration in the chain, and have them apply to every upstream build that does not define its own.

</def>

<def title="Retry dependency even if the failure is the same">

Keeps TeamCity retrying a failed dependency, even if every attempt fails for the same reason, until it succeeds or runs out of attempts. If disabled, a repeated failure is left as is and is not retried again.

</def>

</deflist>


## Build reuse

Running every upstream build on every chain trigger is often wasteful — if a matching build already exists, TeamCity can reuse it. This is what makes a chain more than a fixed sequence: rather than blindly rerunning everything, TeamCity decides which upstream builds to execute and which to substitute with earlier results.

Reuse is controlled by the [**Do not run new build if there is a suitable one**](#Dependency+settings) dependency option. When it is enabled, TeamCity looks for a _suitable_ build to use instead of starting a new one.

### Suitable builds

A _suitable_ build is an existing build that TeamCity can reuse in place of a queued upstream build. When build reuse is enabled, TeamCity searches for a suitable build and, if one is found, links the dependency to it and drops the redundant queued build.

A build is considered **suitable** when all of the following hold:

* It belongs to the same or the default branch.
* It uses the same sources snapshot as the queued chain (the same revision, or revisions taken at the same moment if VCS roots differ).
* It is successful — if the **Only use successful builds from suitable ones** option is enabled.
* It is a regular, non-[personal](personal-build.md) build with no customized parameters.
* The build configuration settings have not changed since the build ran.
* All of its own dependency builds are also suitable.
* It is not a "hanging" build.

If no build meets every criterion, TeamCity runs a new upstream build instead.

### VCS settings that disable build reuse

Some VCS root configurations make it impossible for TeamCity to reliably calculate revisions, which disables build reuse entirely. These are:

* **Subversion** — "Checkout, but ignore changes" mode.
* **CVS** — "Checkout by tag" mode.
* **Perforce** — "Stream" or "Client" connection settings, or a label specified as the "Label/revision to checkout".
* **Starteam** — checkout mode set to "view label" or "promotion date".

### Parallel tests and build reuse

<snippet id="parallel-chain-builds">

The **Always run new build** behavior (the [snapshot dependency](configuring-dependencies.md#Dependency+settings) **Do not run new build if there is a suitable one** setting disabled) affects only the main configuration build. Virtual build configurations that spawn dynamically when the [](parallel-tests.md) feature is used might still reuse their previous results. If no new repository commits were detected, only previously failed test batches run new builds, while successful batches are reused.

In the figure below, the "Composite Conf" configuration depends on "Maven App" configuration. The latter runs its tests in two parallel batches. Note that the main "Maven app" build #18 is triggered anew, whereas the dynamically spawned "Maven app 1" configuration reuses its previous successful build (#12).

<img src="dk-reuse-batch.png" width="706" alt="Reuse Test Batch"/>

You can force TeamCity to re-run all virtual configuration builds. In this case, even if no new repository commits were found, every individual test batch will run anew.

<img src="dk-noreuse-batch.png" width="706" alt="Run New Test Batch"/>

To do so, add the `teamcity.internal.splitBuild.dependency.takeStartedBuildWithSameRevisions=false` [parameter](configuring-build-parameters.md) to the configuration with the parallel tests feature.

To apply this behavior to all configurations on the server, add this parameter to the [internal properties](server-startup-properties.md#TeamCity+Internal+Properties) list.
{instance="tc"}

</snippet>




## Revision synchronization

By default, every member of a chain runs on the same sources snapshot. Disabling [**Enforce revisions synchronization**](#enforce-rev-sync) on a specific dependency breaks the chain into independent revision groups, so a build can be [promoted](run-build-chains.md#Promote+a+build) across that link onto a newer revision.

The typical use case is deployment: you want to deploy an older, validated build using the **latest** deployment scripts.

<img src="dis-enf-rev-sync.png" width="331" alt="Disabled revision synchronization"/>

Consider a "D → C → B → A" chain where D compiles, C runs integration tests, B runs system tests, and A deploys. Synchronization is **disabled** on B's dependency, but enabled for A and C:

* D and C are synchronized — both run on revision 1.
* B and A are synchronized — both run on revision 3.
* The C → B link is desynchronized, so the two groups can use different revisions.

This lets you promote an older compilation (D, revision 1) directly to B, skipping C, while B and A still run on the latest revision 3.

The one rule to follow: **do not desynchronize a link if its downstream build also synchronizes with another upstream build through a different path.** That creates a contradictory revision requirement. The two safe topologies are: synchronization disabled along one full side of a fork...

<img src="valid-snap-flow1.png" width="211" alt="Valid flow: sync disabled on one side"/>

...or disabled on both legs before they rejoin.


<img src="valid-snap-flow2.png" width="211" alt="Valid flow: sync disabled on both legs"/>


<!--

<include from="project-administrator-guide.md" element-id="configuration-dependencies"/>

> A build (build configuration) has snapshot or artifact dependencies on another one is called a **dependent build (configuration)**.
> 
{style="tip"}

This section focuses on build chains and artifact dependencies. To learn more about other options, see the following articles instead:

* [](configuring-finish-build-trigger.md)
* [](create-and-edit-pipelines.md)




<seealso>
        <category ref="external">
            <a href="https://ant.apache.org/ivy/">Additional information on Ivy</a>
        </category>
        <category ref="admin-guide">
            <a href="patterns-for-accessing-build-artifacts.md">Patterns For Accessing Build Artifacts</a>
            <a href="configuring-dependencies.md">Snapshot Dependencies</a>
            <a href="artifact-dependencies.md">Artifact Dependencies</a>
        </category>
</seealso>

-->