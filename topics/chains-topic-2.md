[//]: # (title: Configure Chain Dependencies)

A [build chain](chains-topic-1.md) is assembled by declaring dependencies in **downstream** objects that point to **upstream** ones. The available settings are the same for both build configurations and pipelines — only the UI and code representation differ.

## Snapshot Dependencies

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

> Snapshot dependencies control execution order and revision synchronization only — they do not transfer files between builds. To pass artifacts between chain members, add [artifact dependencies](chains-topic-3.md#artifact-dependencies) as well.
>
{style="tip"}

## Pipeline Dependencies

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

## Dependency Settings

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

See [](chains-topic-4.md#revision-synchronization) for the effects this setting has on a whole build chain.

</snippet>

</def>

<def title="Do not run new build if there is a suitable one">

<snippet id="do-not-run-new-build-if-there-is-a-suitable-one-description">

If this option is enabled, TeamCity does not run a new upstream build when another running or finished build with the appropriate sources' revision already exists. See [](chains-topic-4.md#suitable-builds) for the criteria TeamCity uses to determine a reusable build.

In this case, when a downstream build is triggered, the upstream build is still put into the queue. Then, once the changes for the chain are collected, this upstream build is removed from the queue and the dependency is linked to the suitable finished build instead.

> For upstream pipelines, you might also want to disable the **Reuse Job Results** toggle in [job settings](job-settings.md#Optimizations). Otherwise, jobs can reuse their previous results even when the dependency setting calls for new runs.
>
{style="tip"}

</snippet>

</def>

<def title="Only use successful builds from suitable ones">

<snippet id="reuse-only-successful">

A new triggered build will only use successfully finished [suitable builds](chains-topic-4.md#suitable-builds) as dependencies. If the latest finished suitable build failed, it is rerun.

</snippet>

</def>

<def title="Run build on the same agent">

When enabled, the downstream build runs on the same build agent that ran the upstream build within the same chain. Use this when an upstream build modifies system state — installed tools, environment variables, or local files — that the downstream build relies on.

> This setting is available for build configurations only. For pipelines, use [agent requirements](job-settings.md#Agent+Requirements) to control which agents a job can run on.
>
{style="note"}

</def>

<def title="On failed dependency / On failed to start or canceled dependency">

<snippet id="on-failed-dependency-description">

These settings let you control whether a downstream build should run if its upstream build fails, and, if it should, whether the same build problem should appear in its results.

* **Run build, but add problem**: the downstream build runs and the problem is added to it, changing its status to failed (unless the problem was muted earlier).
* **Run build, but do not add problem**: the downstream build runs and no problem is added.
* **Mark build as failed to start**: the downstream build does not run and is marked as "_Failed to start_".
* **Cancel build**: the downstream build does not run and is marked as "_Canceled_".

</snippet>

</def>

</deflist>
