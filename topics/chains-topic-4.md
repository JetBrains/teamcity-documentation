[//]: # (title: Control Chain Execution)

By default, triggering a downstream object runs the entire [build chain](chains-topic-1.md) on a single shared sources snapshot. This article covers the settings and techniques that let you change that behavior: reusing previous builds, triggering chains, running only part of a chain, and mixing source revisions.

<anchor name="suitable-builds"/>

## Build Reuse (Suitable Builds)

Running every upstream build on every chain trigger is often wasteful — if a matching build already exists, TeamCity can reuse it. When the [**Do not run new build if there is a suitable one**](chains-topic-2.md#Dependency+Settings) dependency option is enabled, TeamCity links the dependency to an existing _suitable_ build instead of starting a new one, and drops the redundant queued build.

A build is considered **suitable** when all of the following hold:

* It belongs to the same or the default branch.
* It uses the same sources snapshot as the queued chain (the same revision, or revisions taken at the same moment if VCS roots differ).
* It is successful — if the **Only use successful builds from suitable ones** option is enabled.
* It is a regular, non-[personal](personal-build.md) build with no customized parameters.
* The build configuration settings have not changed since the build ran.
* All of its own dependency builds are also suitable.
* It is not a "hanging" build.

### VCS Settings That Disable Build Reuse

Some VCS root configurations make it impossible for TeamCity to reliably calculate revisions, which disables build reuse entirely. These are:

* **Subversion** — "Checkout, but ignore changes" mode.
* **CVS** — "Checkout by tag" mode.
* **Perforce** — "Stream" or "Client" connection settings, or a label specified as the "Label/revision to checkout".
* **Starteam** — checkout mode set to "view label" or "promotion date".

### Parallel Tests and Build Reuse

<snippet id="parallel-chain-builds">

The **Always run new build** behavior (the [snapshot dependency](chains-topic-2.md#Dependency+Settings) **Do not run new build if there is a suitable one** setting disabled) affects only the main configuration build. Virtual build configurations that spawn dynamically when the [](parallel-tests.md) feature is used might still reuse their previous results. If no new repository commits were detected, only previously failed test batches run new builds, while successful batches are reused.

In the figure below, the "Composite Conf" configuration depends on "Maven App" configuration. The latter runs its tests in two parallel batches. Note that the main "Maven app" build #18 is triggered anew, whereas the dynamically spawned "Maven app 1" configuration reuses its previous successful build (#12).

<img src="dk-reuse-batch.png" width="706" alt="Reuse Test Batch"/>

You can force TeamCity to re-run all virtual configuration builds. In this case, even if no new repository commits were found, every individual test batch will run anew.

<img src="dk-noreuse-batch.png" width="706" alt="Run New Test Batch"/>

To do so, add the `teamcity.internal.splitBuild.dependency.takeStartedBuildWithSameRevisions=false` [parameter](configuring-build-parameters.md) to the configuration with parallel tests feature.

To apply this behavior to all configurations on the server, add this parameter to the [internal properties](server-startup-properties.md#TeamCity+Internal+Properties) list.
{instance="tc"}

</snippet>

## Triggering a Chain

The recommended approach is to add [triggers](configuring-build-triggers.md) only to the **final** (most downstream) object of a chain. When that object is triggered, TeamCity automatically queues all of its upstream dependencies. Upstream objects do not need their own triggers.

This follows the "think about the result" principle: configure the trigger on the build you ultimately want, and let the chain pull in everything it needs.

<snippet id="trigger-on-ssdep-chngs">

The VCS build trigger has another [option](configuring-vcs-triggers.md#Trigger+build+on+changes+in+snapshot+dependencies) that alters triggering behavior for a build chain. With this options enabled, the whole build chain will be triggered even if changes are detected in dependencies, not in the final build.

Let's take a build chain from the example: `pack setup` — depends on — `tests` — depends on — `compile`.

<img src="compile-test-pack.png" width="401"/>

With the VCS Trigger set up in the `pack setup` configuration, the whole build chain is usually triggered when TeamCity detects changes in `pack setup`; changes in `compile` will trigger `compile` only and not the whole chain. If you want the whole chain to be triggered on a VCS change in `compile`, add a VCS trigger with the "_Trigger on changes in snapshot dependencies_" [option](configuring-vcs-triggers.md#Trigger+build+on+changes+in+snapshot+dependencies) enabled to the final build configuration of the chain, `pack setup`. This will not change the order in which builds are executed, but will only trigger the whole build chain, if there is a change in any of snapshot dependencies. In this setup, no VCS triggers are required for the `compile` or `tests` build configuration.

</snippet>

To make upstream changes visible in the downstream object, enable the [Show changes from snapshot dependencies](configuring-vcs-settings.md#show-changes-from-snapshot-dependencies) option in the **Version Control Settings** section. This shows upstream changes in the **Change Log** and **Pending Changes** tabs of the downstream object.

<img src="dk-show-changes-from-dependencies.png" width="706" alt="Show changes from dependencies setting"/>

Regardless of this default, users can include or exclude changes that originate from dependencies when viewing a build's change list.

<img src="changes_popup.png" width="706" alt="Changes from dependencies"/>

<anchor name="partial-chain-execution"/>

## Partial Chain Execution

Sometimes only part of a chain needs to run. TeamCity offers three mechanisms, from ad-hoc to fully automated.

> TeamCity cannot skip a configuration in the **middle** of a chain — doing so would leave dependencies pointing at builds that never run. You can only skip configurations at the edges of the part you trigger.
>
{style="note"}

### Skip Builds On Demand

For a one-off partial run, use the [Run Custom Build dialog](running-custom-build.md). On the **Dependencies** tab, set the **Skip** option for any directly linked configuration you want to ignore.

<img src="dk-skip-chain-builds.png" width="706" alt="Skip builds"/>

You can only skip configurations directly linked to the one you trigger. For the "Build 1 → Build 2 → Build 3 → Build 4" chain, starting "Build 4" lets you skip only "Build 3".

### Conditional Dependencies with Tags

For a repeatable setup, use the `teamcity.build.chain.skipTags` and `teamcity.build.chain.onlyTags` [configuration parameters](configuring-build-parameters.md) (available since 2024.12).

* `teamcity.build.chain.skipTags` — excludes matching configurations. The chain runs everything except them.
* `teamcity.build.chain.onlyTags` — keeps matching configurations and their dependencies. Configurations between kept ones cannot be skipped.

Both parameters accept a comma-separated list of:

* **Tags** — values of the `teamcity.configuration.tags` parameter, which you set on any configuration you want to label.
* **[Configuration IDs](identifier.md)** — shown in configuration settings or copied from configuration URLs.

TeamCity reads these parameters **only from the configuration that triggers the chain**; values on dependency builds are ignored.

> If a skipped build provides an artifact to a downstream build, TeamCity automatically converts the strict artifact rule (`+:`) into an [optional one](chains-topic-3.md#artifact-rules) (`?:`) so the downstream build does not fail. Note that a build which skipped upstream configurations is treated as modified and will not be [reused](#suitable-builds) later.
>
{style="note"}

#### Example: skipTags

A composite "Build All" configuration triggers a full chain. To let it run only the core "Build..." configurations and skip the optional tests, tag the optional configurations and reference that tag:

```Kotlin
object TestWin : BuildType({
    id("TestWin")
    params { param("teamcity.configuration.tags", "optional") }
})
// ... other optional configurations tagged "optional" ...

object BuildAll : BuildType({
    id("BuildAll")
    type = BuildTypeSettings.Type.COMPOSITE
    params {
        param("teamcity.build.chain.skipTags", "optional")
    }
    dependencies {
        snapshot(BuildWin) {}
        snapshot(BuildPlugins) {}
    }
})
```

To run the full chain instead, remove the parameter or set it to a value that matches nothing. A common pattern is a [schedule trigger](configuring-schedule-triggers.md) that overrides the value for full nightly builds:

```Kotlin
triggers {
    schedule {
        schedulingPolicy = daily { hour = 3 }
        triggerBuild = always()
        buildParams {
            param("teamcity.build.chain.skipTags", "nightly-build-mode")
        }
    }
}
```

#### Example: onlyTags

To let users choose a sub-chain at trigger time, style `onlyTags` as a [Select parameter](typed-parameters.md#Single-Select+Parameter) with the "Prompt" display mode:

```Kotlin
object BuildAll : BuildType({
    id("BuildAll")
    type = BuildTypeSettings.Type.COMPOSITE
    params {
        select("teamcity.build.chain.onlyTags", "",
            label = "Choose a sub-chain to run",
            display = ParameterDisplay.PROMPT,
            options = listOf(
                "run all" to "",
                "windows" to "windows",
                "linux" to "linux",
                "plugins" to "plugins"))
    }
    dependencies {
        snapshot(BuildWin) {}
        snapshot(BuildLinux) {}
        snapshot(BuildPlugins) {}
    }
})
```

When triggered manually, the **Run Custom Build** dialog prompts the user to pick a value. An empty value runs the whole chain; a tag runs only the configurations carrying it, plus their dependencies.

<img src="dk-subchain-selector.png" width="706" alt="Subchain selector"/>

### Skip Queued Builds at Runtime

To cancel queued downstream builds dynamically from a running build step, send the [service message](service-messages.md):

```Shell
##teamcity[skipQueuedBuilds tags='value1,value2' comment='Your comment']
```

The `tags` argument accepts the same tags and configuration IDs as the parameters above. This is useful for canceling specific branches of a chain based on runtime conditions — for example, skipping selected test suites from a "Build" step:

```Shell
Build ----|---- Test Suite 1 ----|
          |---- Test Suite 2 ----|---- Deploy
          |---- Test Suite 3 ----|
```

Avoid skipping an entire mid-section, which leaves a confusing "Build → ??? → Deploy" gap. For that case, maintain a separate lean chain instead.

<anchor name="revision-synchronization"/>

## Revision Synchronization Across the Chain

By default, every member of a chain runs on the same sources snapshot. Disabling [**Enforce revisions synchronization**](chains-topic-2.md#enforce-rev-sync) on a specific dependency breaks the chain into independent revision groups, so a build can be promoted across that link onto a newer revision.

The typical use case is deployment: you want to deploy an older, validated build using the **latest** deployment scripts.

<img src="dis-enf-rev-sync.png" width="331" alt="Disabled revision synchronization"/>

Consider a "D → C → B → A" chain where D compiles, C runs integration tests, B runs system tests, and A deploys. Synchronization is **disabled** on B's dependency, but enabled for A and C:

* D and C are synchronized — both run on revision 1.
* B and A are synchronized — both run on revision 3.
* The C → B link is desynchronized, so the two groups can use different revisions.

This lets you promote an older compilation (D, revision 1) directly to B, skipping C, while B and A still run on the latest revision 3.

The one rule to follow: **do not desynchronize a link if its downstream build also synchronizes with another upstream build through a different path.** That creates a contradictory revision requirement. The two safe topologies are: synchronization disabled along one full side of a fork, or disabled on both legs before they rejoin.

<img src="valid-snap-flow1.png" width="211" alt="Valid flow: sync disabled on one side"/>

<img src="valid-snap-flow2.png" width="211" alt="Valid flow: sync disabled on both legs"/>
