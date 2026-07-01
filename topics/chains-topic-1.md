[//]: # (title: Build Chains)

<show-structure for="chapter" depth="2"/>

A _build chain_ is a sequence of interconnected [build configurations](creating-and-editing-build-configurations.md) and [pipelines](create-and-edit-pipelines.md) linked by dependencies, where each member waits for its upstream to finish before starting.

<img src="chains-minimap.png" width="706" alt="Build chains viewer" thumbnail="true"/>

Technically, a build chain is a [directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph): it has a well-defined execution order and cannot contain cycles.

## When to Use Build Chains

Build chains are useful whenever multiple build configurations or pipelines need to run in a specific order and share the same state of the codebase. Two common scenarios:

* **Multi-platform testing before release.** Compile the project once, run tests simultaneously on different platforms, then produce a release build only if all tests pass.
* **Offloading a heavy test suite.** A slow test suite is moved into its own configuration and linked back with a snapshot dependency, so it still runs on the same sources as the build it validates while gaining its own history, triggers, and agent requirements.

<img src="compile-test-pack.png" width="401" alt="Compile, test, pack chain"/>

> To speed up a single slow suite by spreading it across agents, you usually do **not** need a hand-built chain: the [Parallel Tests](parallel-tests.md) build feature splits the suite into batches and distributes them automatically. A dedicated chained configuration is still justified when TeamCity cannot split the suite effectively — for example, when all tests live in one class, or the automatically produced batches are too imbalanced (see how [TeamCity groups tests into batches](parallel-tests.md#Run+tests+in+parallel)) — or when the test stage needs its own environment or must be reused by several downstream objects.
>
{style="tip"}

## How a Chain Runs

When you trigger a downstream build, TeamCity does not just start the upstream and wait. It:

1. Resolves the entire chain transitively — all upstream objects, including those several levels deep.
2. Queues all chain members at once and calculates a single sources snapshot shared across all of them. Every member will run on code taken at the same point in time.
3. Runs the chain from upstream to downstream, starting each member as soon as all its direct dependencies have finished.

This shared-revision guarantee is the key difference between a build chain and a simple sequential trigger. It ensures that, for example, the "Deploy" step always operates on exactly the same binaries that the "Test" step validated.

## Chain Members

Both build configurations and pipelines can participate in a chain. Mixed chains — a pipeline depending on a build configuration, or vice versa — are fully supported.

<deflist type="medium">
<def title="Build configurations">

Classic TeamCity entities configured via the web UI or Kotlin DSL. Dependencies between build configurations are called _snapshot dependencies_ and are set up on the **Dependencies** page of configuration settings.

</def>
<def title="Pipelines">

Newer YAML-based entities. Dependencies between pipelines, or between a pipeline and a build configuration, are called _pipeline dependencies_ and are set up in the pipeline settings panel.

</def>
</deflist>

See [](chains-topic-2.md) for setup instructions for both types.

## Upstream and Downstream

Chain dependencies are always declared in the **downstream** object, pointing to the **upstream** one.

To create a "Build → Test → Deploy" chain, you open **Deploy** and add a dependency on **Test**, then open **Test** and add a dependency on **Build** — not the other way around.

<img src="ABC.png" width="311" alt="Linear build chain: C runs first, then B, then A"/>

Two effects follow from this model:

* Upstream objects can always run independently — they have no dependencies themselves.
* Triggering a downstream object automatically queues and runs all its upstream dependencies.

Chains can also fan out: if multiple objects each depend on the same upstream, they run in parallel once that upstream finishes, provided enough idle agents are available.

<img src="B1-B2-A.png" width="126" alt="B1 and B2 run in parallel, both upstream of A"/>

## Build Chains vs Finish Build Triggers

Build chains are the recommended way to link objects in TeamCity. They provide source revision synchronization, build reuse, and fine-grained execution control across both build configurations and pipelines.

[Finish build triggers](configuring-finish-build-trigger.md) are an older, left-to-right push mechanism available only for build configurations. We recommend using build chains over finish build triggers for new setups.

## Next Steps

* [](chains-topic-2.md) — link configurations and pipelines, and tune dependency behavior such as build reuse and revision synchronization
* [](chains-topic-3.md) — transfer files and parameter values between chain members
* [](chains-topic-4.md) — trigger, stop, and partially run chains
* [](chains-topic-5.md) — inspect chain runs and rerun individual steps
* [](chains-topic-6.md) — protect configurations from unauthorized cross-project dependencies
