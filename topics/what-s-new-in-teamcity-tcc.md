[//]: # (title: What's New in TeamCity Cloud 2024.03)


<snippet id="2024-03-tcc">

## New Bundled Plugin: HashiCorp Vault
{instance="tcc"}

The [HashiCorp Vault Support](https://plugins.jetbrains.com/plugin/10011-hashicorp-vault-support) plugin is now an integral component of the standard TeamCity installation. This plugin allows you to store sensitive data in a remote source, and enables TeamCity parameters to seamlessly retrieve these values during build processes.

<img src="dk-vaultConnection.png" width="460" alt="Vault connection settings"/>

To set up the TeamCity integration with HashiCorp Vault, create a new Vault connection and use it to set up the **Remote secret** parameter in the [updated Add New Parameter dialog](#New+Parameter+Dialog).

Learn more: [](hashicorp-vault.md).


## Untrusted Builds
{instance="tcc"}

With the [](pull-requests.md) feature added to your configurations you can assess new code before integrating it into the primary codebase. This feature comes with filtering options, enabling you to select whether to run builds from any contributors or solely those affiliated with your organization. The former choice poses a notable security risk, potentially exposing your TeamCity server to malicious code camouflaged within pull requests. Conversely, opting for the latter restricts collaboration opportunities with a broader audience.

Version 2024.03 introduces a new setup that eliminates this trade-off between collaboration and security. The new **Untrusted Builds** group under project settings allows TeamCity to differentiate changes authored by trusted users from changes coming from an external source. New builds are triggered regardless of the changes' author, but builds that incorporate unverified changes will remain in queue until a designated reviewer (or a group of reviewers) marks them as safe to run.

<img src="dk-untrustedbuilds-pending.png" width="706" alt="Pending approval"/>

Learn more: [](untrusted-builds.md).


## New dotCover Runner
{instance="tcc"}

The new **dotCover** runner can automatically retrieve code coverage snapshots from multiple preceding [](net.md) steps, and use these individual snapshots to publish a single coverage report.

<img src="dk-dotcover-settings.png" width="706" alt="DotCover Runner Settings"/>

Learn more: [](dotcover-runner.md).


## Automatic Retry of Failed .NET Tests
{instance="tcc"}

If the [](net.md) runner executes the `test` or `vstest` command, the runner's settings now display the new **Test retry count** option. This field allows you to specify how many times during the same build TeamCity can re-run failed test. Failed tests are re-launched until they either achieve success or exhaust the maximum number of attempts.

<img src="dk-test-rerun-flaky.png" width="706" alt="Flaky tests during a re-run"/>

This technique allows you to identify [flaky tests](viewing-tests-and-configuration-problems.md#Flaky+Tests) and distinguish them from genuinely problematic tests that consistently fail regardless of the number of launch attempts.

Learn more: [.NET | Vstest Command](net.md#vstest).



## Gradle Configuration Cache
{instance="tcc"}

Starting with this version, you can enable the [configuration cache](https://docs.gradle.org/current/userguide/configuration_cache.html) feature for Gradle builds running in TeamCity. This feature substantially enhances build performance by caching the configuration phase's result and reusing it in subsequent builds.

Learn more: [](gradle.md#Configuration+Cache).


## Optional Artifact Dependencies
{instance="tcc"}

by other configurations (or by previous builds of the same configuration). To create these dependencies, you need to specify [](artifact-dependencies.md#Artifacts+Rules) that define what files should be downloaded and where they should be stored.

If TeamCity is unable to locate files matching these rules, a build fails with the "Unable to resolve artifact dependency" error. This behavior does not take into account more flexible setups where a downloaded artifact is not mandatory for a dependent build to run.

Starting with version 2024.03, you can run a dependent build even if its artifact rules yield no files. To do so, start an artifact rule with the `?:` prefix.

<img src="dk-relativeBuild-failed.png" width="706" alt="Optional dependency warning"/>

Learn more: [Artifact Dependencies](artifact-dependencies.md#Prefix)


<!--

## Pull Request Branch Filters
{instance="tcc"}

[Branch filters](branch-filter.md) now support filter expressions in the `+|-pr: <attribute>=<value>` format. Using this syntax you can set up fine-grained rules that filter pull requests by their origin and source branches, authors, origin types, and more.

Learn more: [](branch-filter.md#Pull+Request+Branch+Filters).

-->

## Enhanced Git LFS and Submodules Support
{instance="tcc"}

[Large File Systems](https://git-lfs.com) and [submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) are integral parts of many complex software solutions that import standalone repositories and offload massive files (videos, bitmaps, databases, and so on) to external hostings. In version 2024.03, you can add [parameter-based](configuring-build-parameters.md) credentials to your TeamCity projects. When checking out source files, TeamCity will use these credentials to access and download required files. This feature allows you to set up TeamCity integration with [Sonatype Nexus LFS repositories](https://help.sonatype.com/en/git-lfs-repositories.html)) and other popular solutions.

Learn more: [](git.md#LFS+and+Submodules+Support).



## New Parameter Dialog
{instance="tcc"}

In version 2024.03 we have redesigned the **Add/Edit Parameter** dialog that you utilize when configuring [build parameters](configuring-build-parameters.md).

<img src="dk-newparams-singleselect.png" width="460" alt="Single select parameter settings"/>

In addition to other notable enhancements, the updated dialog allows you to select a new parameter type — **Remote secret**. Choose this type for parameters whose values should be retrieved from a remote source (for example, HashiCorp Vault).

Learn more: [](typed-parameters.md).



## Alternative Fetch URLs
{instance="tcc"}

In TeamCity 2024.03, build agents can now fetch sources from a pre-configured repository proxy that mirrors your original Git repository. This capability is especially valuable for large distributed systems, mitigating connectivity issues for agents distant from the primary repository.

Fetch URL mapping rules, defined in agent configuration files, offer granular control over the checkout process per agent. Additionally, wildcard and partial URL support in redirection rules enables the creation of universal, project-agnostic mapping patterns.

Learn more: [Git VCS Root | General Settings](git.md#General+Settings).


## Miscellaneous Changes
{instance="tcc"}

* The [Open Terminal](install-and-start-teamcity-agents.md#Debug+Agents+Remotely) button now opens the terminal in the [checkout directory](build-checkout-directory.md). If invoked from the agent's overview page, the terminal still opens in the `$HOME` directory.
* New [](commit-status-publisher.md) setting allows you to choose whether you want TeamCity to post [Swarm review comments](integrating-with-helix-swarm.md) when a build finishes. If this option is disabled, the build feature will only update the review's **Tests** section.
* The **Parameters | Statistic values** section of [composite builds](composite-build-configuration.md) now includes an additional metric that displays how much time this build saved by [reusing previous builds](snapshot-dependencies.md#Suitable+Builds) instead of running them anew.


</snippet>


