[//]: # (title: View and Manage Chain Runs)

TeamCity visualizes [build chains](chains-topic-1.md) in several places and lets you rerun individual steps, continue partially finished chains, and stop running ones.

## Build Chains Tab

Once a chain has been triggered, a **Build Chains** tab appears on both the **Project Home** page and the **Home** pages of the participating build configurations. It lists all chains that include builds from the current project or configuration, sorted so that the chain with the most recently finished build is on top.

<img src="buildChainsCollapsed.png" alt="Collapsed build chains"/>

Each chain row shows a pie chart summarizing the statuses of its builds. Expand a chain to see:

* Every build in the chain, ordered by execution (the first build to run is on the left).
* The status of each build: not triggered, queued, running, or finished.

<img src="Build-Chains1.png" width="750" alt="Build chain example"/>

Clicking a build highlights it together with all its direct dependencies, transitively. The tab also offers **Group by projects** and **Hide details** display options, and a compact representation that merges chains sharing the same dependency builds.

From this tab you can:

* **Continue a chain** that has "not triggered" builds — click **Run** to start the remaining builds on the existing chain revisions.
* **Rerun a build** — open the [custom build dialog](running-custom-build.md) with the chain revisions preselected.

## Chain Tab in Build Results

The **Chain** tab on a build's results page shows the directed graph for that specific run: which builds ran, which were reused from a previous chain, how long each took, and the overall chain duration. Use it to confirm, for example, that an upstream build was reused (its build number stays the same across reruns) rather than executed again.

## Dependencies Tab in Build Results

The **Dependencies** tab lists the build's direct and indirect dependency builds. For example, if A depends on B, and B depends on C and D, then C and D appear as indirect dependencies of A.

The tab also shows the artifacts each build delivered and downloaded, lets you group or ungroup builds, and highlights builds that were reused as [suitable builds](chains-topic-4.md#suitable-builds) from earlier chains.

## Stopping Chain Builds

When you stop or remove from the queue a build that is part of a chain, TeamCity shows the message "_This build is a part of a build chain_" and lists the other running or queued chain members under **Stop other parts**.

* Each listed build you can access has a checkbox. It is selected by default when stopping the current build would inevitably cause that build to fail.
* Builds you lack permission to stop are shown without a checkbox.
* Builds you lack permission to view are hidden, replaced by a warning that you cannot see all parts of the chain.

If all other parts of the chain have already finished, no additional information is shown.

## Running Personal Builds in a Chain

When a [personal build](personal-build.md) triggers a chain, all of its upstream dependencies also run as personal builds. The exception is build reuse: if reuse is enabled and a finished non-personal build satisfies the revision requirements, TeamCity uses it instead of running a personal upstream build that would add no value.

## Clean-up

Build chains interact with [clean-up](teamcity-data-clean-up.md) in two ways worth knowing:

<deflist type="full">
<def title="Chain builds are preserved by default">

TeamCity does not clean up builds that are part of a chain unless you explicitly allow it. You can disable this protection per configuration in its clean-up settings.

</def>
<def title="Downloaded artifacts follow their consumer">

Artifacts downloaded by a build are not cleaned up while that consuming build still exists. For a configuration with artifact dependencies, clean-up settings let you choose whether artifacts it downloaded from other builds can be cleaned early or kept.

</def>
</deflist>
