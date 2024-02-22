[//]: # (title: What's New in TeamCity 2024.03)
[//]: # (auxiliary-id: What's New in TeamCity 2024.03;What's New in TeamCity)

<!--OnPrem-->

## Untrusted Builds
{product="tc"}

With the [](pull-requests.md) feature added to your configurations you can assess new code before integrating it into the primary codebase. This feature comes with filtering options, enabling you to select whether to run builds from any contributors or solely those affiliated with your organization. The former choice poses a notable security risk, potentially exposing your TeamCity server to malicious code camouflaged within pull requests. Conversely, opting for the latter restricts collaboration opportunities with a broader audience.

Version 2024.03 introduces a new setup that eliminates this trade-off between collaboration and security. The new **Untrusted Builds** group under project settings allows TeamCity to differentiate changes authored by trusted users from changes coming from an external source. New builds are triggered regardless of the changes' author, but builds that incorporate unverified changes will remain in queue until a designated reviewer (or a group of reviewers) marks them as safe to run.

<img src="dk-untrustedbuilds-pending.png" width="706" alt="Pending approval"/>

Learn more: [](untrusted-builds.md).


## Alternative Fetch URLs
{product="tc"}

In TeamCity 2024.03, build agents can now fetch sources from a pre-configured repository proxy that mirrors your original Git repository. This capability is especially valuable for large distributed systems, mitigating connectivity issues for agents distant from the primary repository.

Fetch URL mapping rules, defined in agent configuration files, offer granular control over the checkout process per agent. Additionally, wildcard and partial URL support in redirection rules enables the creation of universal, project-agnostic mapping patterns.

Learn more: [Git VCS Root | General Settings](git.md#General+Settings).

## Optional Artifact Dependencies
{product="tc"}

[](artifact-dependencies.md) allow your build configurations to download files produced by other configurations (or by previous builds of the same configuration). To create these dependencies, you need to specify [](artifact-dependencies.md#Artifacts+Rules) that define what files should be downloaded and where they should be stored.

If TeamCity is unable to locate files matching these rules, a build fails with the "Unable to resolve artifact dependency" error. This behavior does not take into account more flexible setups where a downloaded artifact is not mandatory for a dependent build to run.

Starting with version 2024.03, you can run a dependent build even if its artifact rules yield no files. To do so, start an artifact rule with the `?:` prefix.

<img src="dk-relativeBuild-failed.png" width="706" alt="Optional dependency warning"/>

Learn more: [Artifact Dependencies](artifact-dependencies.md#Prefix)


## Miscellaneous Changes
{product="tc"}

* The [Open Terminal](install-and-start-teamcity-agents.md#Debug+Agents+Remotely) button now opens the terminal in the [checkout directory](build-checkout-directory.md). If invoked from the agent's overview page, the terminal still opens in the `$HOME` directory.



<!--Cloud-->

## Untrusted Builds
{product="tcc"}

With the [](pull-requests.md) feature added to your configurations you can assess new code before integrating it into the primary codebase. This feature comes with filtering options, enabling you to select whether to run builds from any contributors or solely those affiliated with your organization. The former choice poses a notable security risk, potentially exposing your TeamCity server to malicious code camouflaged within pull requests. Conversely, opting for the latter restricts collaboration opportunities with a broader audience.

Version 2024.03 introduces a new setup that eliminates this trade-off between collaboration and security. The new **Untrusted Builds** group under project settings allows TeamCity to differentiate changes authored by trusted users from changes coming from an external source. New builds are triggered regardless of the changes' author, but builds that incorporate unverified changes will remain in queue until a designated reviewer (or a group of reviewers) marks them as safe to run.

<img src="dk-untrustedbuilds-pending.png" width="706" alt="Pending approval"/>

Learn more: [](untrusted-builds.md).


## Optional Artifact Dependencies
{product="tcc"}

by other configurations (or by previous builds of the same configuration). To create these dependencies, you need to specify [](artifact-dependencies.md#Artifacts+Rules) that define what files should be downloaded and where they should be stored.

If TeamCity is unable to locate files matching these rules, a build fails with the "Unable to resolve artifact dependency" error. This behavior does not take into account more flexible setups where a downloaded artifact is not mandatory for a dependent build to run.

Starting with version 2024.03, you can run a dependent build even if its artifact rules yield no files. To do so, start an artifact rule with the `?:` prefix.

<img src="dk-relativeBuild-failed.png" width="706" alt="Optional dependency warning"/>

Learn more: [Artifact Dependencies](artifact-dependencies.md#Prefix)

## Alternative Fetch URLs
{product="tcc"}

In TeamCity 2024.03, build agents can now fetch sources from a pre-configured repository proxy that mirrors your original Git repository. This capability is especially valuable for large distributed systems, mitigating connectivity issues for agents distant from the primary repository.

Fetch URL mapping rules, defined in agent configuration files, offer granular control over the checkout process per agent. Additionally, wildcard and partial URL support in redirection rules enables the creation of universal, project-agnostic mapping patterns.

Learn more: [Git VCS Root | General Settings](git.md#General+Settings).

## Miscellaneous Changes
{product="tcc"}

* The [Open Terminal](install-and-start-teamcity-agents.md#Debug+Agents+Remotely) button now opens the terminal in the [checkout directory](build-checkout-directory.md). If invoked from the agent's overview page, the terminal still opens in the `$HOME` directory.


## Upgrade Notes
{product="tc"}

Before upgrading, we highly recommend reading about important changes in version <!--LINK!!!!!--> [2023.11 compared to 2023.05.4](upgrade-notes.md#Changes+from+2023.05.4+to+2023.11).

## Fixed Issues
{product="tc"}

See the [TeamCity 2024.03 release notes](teamcity-2024-03-release-notes.md) article for the summary of implemented features and fixed issues.



## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.

## Your Feedback Matters


We place a high value on your feedback and encourage you to share your thoughts and suggestions. See this link for more information: [Feedback](feedback.md).


