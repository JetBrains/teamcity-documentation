[//]: # (title: Run Build Chains)

<show-structure for="chapter" depth="2"/>

By default, triggering a downstream object runs the entire [build chain](build-chain.md) on a single shared sources snapshot. This article covers how to trigger a chain, run only part of it, and stop it.

> The dependency settings that govern *which* builds actually run — [build reuse](configuring-dependencies.md#Build+reuse) and [revision synchronization](configuring-dependencies.md#Revision+synchronization) — are described in [](configuring-dependencies.md).
>
{style="tip"}

## Triggering a chain

The recommended approach is to add [triggers](configuring-build-triggers.md) only to the **final** (most downstream) object of a chain. When that object is triggered, TeamCity automatically queues all of its upstream dependencies. Upstream objects do not need their own triggers.

This follows the "think about the result" principle: configure the trigger on the build you ultimately want, and let the chain pull in everything it needs.

<snippet id="trigger-on-ssdep-chngs">

The VCS build trigger has another [option](configuring-vcs-triggers.md#Trigger+build+on+changes+in+snapshot+dependencies) that alters triggering behavior for a build chain. With this option enabled, the whole build chain will be triggered even if changes are detected in dependencies, not in the final build.

Let's take a build chain from the example: `pack setup` — depends on — `tests` — depends on — `compile`.

<img src="compile-test-pack.png" width="401"/>

With the VCS Trigger set up in the `pack setup` configuration, the whole build chain is usually triggered when TeamCity detects changes in `pack setup`; changes in `compile` will trigger `compile` only and not the whole chain. If you want the whole chain to be triggered on a VCS change in `compile`, add a VCS trigger with the "_Trigger on changes in snapshot dependencies_" [option](configuring-vcs-triggers.md#Trigger+build+on+changes+in+snapshot+dependencies) enabled to the final build configuration of the chain, `pack setup`. This will not change the order in which builds are executed, but will only trigger the whole build chain if there is a change in any of the snapshot dependencies. In this setup, no VCS triggers are required for the `compile` or `tests` build configuration.

</snippet>

To make upstream changes visible in the downstream object, enable the [Show changes from snapshot dependencies](configuring-vcs-settings.md#show-changes-from-snapshot-dependencies) option in the **Version Control Settings** section. This shows upstream changes in the **Change Log** and **Pending Changes** tabs of the downstream object.

<img src="dk-show-changes-from-dependencies.png" width="706" alt="Show changes from dependencies setting"/>

Regardless of this default, users can include or exclude changes that originate from dependencies when viewing a build's change list.

<img src="changes_popup.png" width="706" alt="Changes from dependencies"/>

## Partial chain execution

Sometimes only part of a chain needs to run. TeamCity offers three mechanisms, from ad-hoc to fully automated.

> TeamCity cannot skip a configuration in the **middle** of a chain — doing so would leave dependencies pointing at builds that never run. You can only skip configurations at the edges of the part you trigger.
>
{style="note"}


### Promote a build

Sometimes you don't want to run a chain from the very beginning — you want to reuse one specific finished build and continue the chain from there. Open that build's results page, click **Actions | Promote**, and TeamCity triggers the downstream portion of the chain using this build as its source.

<img src="promote-pipeline-run.png" width="706" alt="Promote a build"/>

This is useful for two common scenarios:

* **Reusing an older build's results instead of the latest.** For example, promote a successful "Build Docker image" run into the "Upload to DockerHub" configuration or pipeline to re-deploy that same artifact without rebuilding it.
* **Manually starting a downstream object that has no automatic trigger** — for example, a deployment configuration you only want to run on demand.

Promotion is a one-time override: it affects only this specific run. Afterwards, both build configurations and pipelines revert to their normal dependency logic (the latest successful or pinned build).

### Skip builds on demand

For a one-off partial run, use the [Run Custom Build dialog](running-custom-build.md). On the **Dependencies** tab, set the **Skip** option for any directly linked configuration you want to ignore.

<img src="dk-skip-chain-builds.png" width="706" alt="Skip builds"/>

You can only skip configurations directly linked to the one you trigger. For the "Build 1 → Build 2 → Build 3 → Build 4" chain, starting "Build 4" lets you skip only "Build 3".

### Conditional dependencies with tags

For a repeatable setup, use the `teamcity.build.chain.skipTags` and `teamcity.build.chain.onlyTags` [configuration parameters](configuring-build-parameters.md) (available since 2024.12).

* `teamcity.build.chain.skipTags` — excludes matching configurations. The chain runs everything except them.
* `teamcity.build.chain.onlyTags` — keeps matching configurations and their dependencies. Configurations between kept ones cannot be skipped.

Both parameters accept a comma-separated list of:

* **Tags** — values of the `teamcity.configuration.tags` parameter, which you set on any configuration you want to label.
* **[Configuration IDs](identifier.md)** — shown in configuration settings or copied from configuration URLs.

TeamCity reads these parameters **only from the configuration that triggers the chain**; values on dependency builds are ignored.

> If a skipped build provides an artifact to a downstream build, TeamCity automatically converts the strict artifact rule (`+:`) into an [optional one](artifact-dependencies.md#Artifact+rules) (`?:`) so the downstream build does not fail. Note that a build which skipped upstream configurations is treated as modified and will not be [reused](configuring-dependencies.md#Suitable+builds) later.
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

### Skip queued builds at runtime

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

## Stopping chain builds

When you stop or remove from the queue a build that is part of a chain, TeamCity shows the message "_This build is a part of a build chain_" and lists the other running or queued chain members under **Stop other parts**.

* Each listed build you can access has a checkbox. It is selected by default when stopping the current build would inevitably cause that build to fail.
* Builds you lack permission to stop are shown without a checkbox.
* Builds you lack permission to view are hidden, replaced by a warning that you cannot see all parts of the chain.

If all other parts of the chain have already finished, no additional information is shown.

## Running personal builds in a chain

When a [personal build](personal-build.md) triggers a chain, all of its upstream dependencies also run as personal builds. The exception is [build reuse](configuring-dependencies.md#Build+reuse): if reuse is enabled and a finished non-personal build satisfies the revision requirements, TeamCity uses it instead of running a personal upstream build that would add no value.
