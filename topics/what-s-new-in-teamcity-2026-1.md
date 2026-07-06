[//]: # (title: What's New in TeamCity 2026.1)
[//]: # (help-id: What's New in TeamCity 2026.1)

<show-structure for="chapter" depth="2"/>


## Release Cycle Updates

Starting with this release, we return to the pre-2022 versioning scheme: major TeamCity versions will use the “YYYY.N” format, where “N” indicates the release number rather than the month. This change makes the release cycle more predictable and better aligned with other JetBrains products.

This year, we also expect to separate the release cadence for TeamCity On-Premises and Cloud. On-Premises will continue to receive two major releases per year. Cloud, on the other hand, will be updated more frequently, so new features and improvements become available sooner without waiting for a major On-Premises release.


## Java 21 Migration

TeamCity server and build agents no longer support Java versions older than Java 21. See this article for upgrade instructions: [](java-21.md).

> This requirement only defines the Java version needed for the server and agents to start successfully; it does not restrict the Java versions your TeamCity projects can build, test, or deploy against.

## TeamCity CLI

TeamCity 2026.1 adds a new way to work with your TeamCity instances: TeamCity CLI. Alongside the browser-based UI and the extensive [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html), you can now use a command-line tool to interact with TeamCity directly from the terminal.

Install the CLI on any machine to check build statuses, start new builds, investigate failures, and handle many other routine tasks without leaving the command line.

<img src="showcase.gif" alt="TeamCity CLI in action" border-effect="rounded"/>

[Learn more...](teamcity-cli.md)


## Integration with AI Agents

Version 2026.1 also makes it easier to connect AI tools such as chatbots and agentic IDEs to TeamCity. You can choose between two integration options:

* The TeamCity `<server-url>/app/mcp` endpoint provides [MCP tools](ai-agent-integration.md#TeamCity+MCP) that let AI agents interact with TeamCity.
* The TeamCity CLI includes an [agent skill](ai-agent-integration.md#TeamCity+CLI) that helps agents work with TeamCity through terminal commands.

[Learn more...](ai-agent-integration.md)


## Pipeline Enhancements

* Version 2026.1 introduces a major enhancement for fully integrating pipelines into your CI/CD workflows: you can now [include them in build chains](pipeline-settings.md#Pipeline+Dependencies). This lets you create fine-grained setups with pipeline-to-pipeline, pipeline-to-configuration, and configuration-to-pipeline dependencies.

    <img src="dk-pipeline-dependency.png" width="706" alt="Pipeline dependency"/>

* We redesigned the build details side panel to include all familiar [Build Results](build-results-page.md) tabs (such as Overview, Build Log, Parameters, and more), giving you a complete view of each build. It now also includes a pipeline/job switch, so you can quickly filter these tabs by job, making pipelines easier to inspect, debug, and troubleshoot.

    <img src="pipelines-job-switch.png" width="706" alt="Pipeline-Job switch in the side panel"/>

* You can now use the [Custom build dialog](running-custom-build.md) that allows you to run pipelines with one-time custom settings.

    <img src="pipelines-run-custom-build.png" width="706" alt="Run build buttons in TeamCity"/>

* Jobs can now use the following build features, previously available only for [build configurations](adding-build-features.md):

    <img src="pipelines-build-features.png" width="706" alt="Build features in pipelines"/>

  * [](build-files-cleaner-swabra.md)
  * [](free-disk-space.md)
  * [](build-cache.md)
  * [](xml-report-processing.md)

* When editing pipeline [**Repositories**](pipeline-settings.md#Repository) settings, you can now add repositories from existing VCS roots owned by this parent project. Previously, the option to reuse a root was only available when you create a new pipeline.

* All pipelines that connect to Git repositories using HTTP credentials support repository options that [allow these pipelines to track pull requests and publish run statuses](pipeline-settings.md#Repository). Previously, these options were available only to pipelines created via a configured TeamCity OAuth connection.

    <img src="pipelines-csp-and-pr.png" width="706" alt="CSP and PR in pipelines"/>

* Pipeline parameters were redesigned to better support pipeline dependencies and provide a more consistent, intuitive model.

  * Jobs no longer define separate input and output parameters. Instead, all job parameters are automatically available to downstream jobs in the same pipeline.
  * Pipeline parameters can now be explicitly marked as input or output, giving you finer control over which values are exposed to downstream configurations and pipelines.
  * Parameters from parent projects no longer need to be imported as they are now automatically available to child pipelines and jobs.

* If a parent project enables its [versioned settings](storing-project-settings-in-version-control.md), pipeline YAML is automatically converted to DSL. Currently, the DSL support in pipelines is limited: changes made via TeamCity UI are displayed on a new **Kotlin DSL** tab, but are not committed automatically and prevent you from running the pipeline. Commit these changes manually to a remote `.kts` file to restore the pipeline's ability to run.

    <img src="pipelines-dsl.png" width="706" alt="DSL in pipelines"/>


## Dynamic Build Step Credentials

The new [](build-scoped-token.md) feature lets your builds securely generate short-lived GitHub access tokens (up to 60 minutes) on the fly. Pass them to build steps as parameters to enable seamless access to repositories.

<img src="dk-build-scoped-token-settings.png" width="706" alt="Main settings"/>

[Learn more...](build-scoped-token.md)


## SSH Known Hosts

The [SSH Keys](ssh-keys-management.md) page now includes additional options that allow TeamCity to verify VCS providers it connects to, and abort any additional operations if the host's public key does not match any of the known entries.

<img src="ssh-known-hosts.png" width="706" alt="SSH Known hosts"/>

[Learn more...](ssh-keys-management.md#Known+SSH+Hosts)

## Third-party Integration Enhancements

### Git

* For security reasons, Git VCS roots no longer support [local and UNC file URLs](git.md#Supported+Git+Protocols) by default. To re-enable them, set the `teamcity.git.allowFileUrl=true` [internal property](server-startup-properties.md#TeamCity+Internal+Properties).

* When choosing the **Shallow clone** [Git checkout policy](git.md#git-checkout-policy), you can now add the `teamcity.git.agent.shallowCloneDepth` and `teamcity.git.agent.submodules.shallowCloneDepth` parameters to set the [`--depth`](https://git-scm.com/docs/git-clone) attribute.

* The [GitLab CE/EE connection](configuring-connections.md#GitLab) now allows you to configure integration with system webhooks. This enhancement allows TeamCity to receive near-instant notifications about new repository changes, as opposed to periodically [polling the repository](project-administrator-guide.md#Collecting+Changes).

* The [](commit-status-publisher.md) build feature now includes the option to set up a custom build configuration name when posting statuses to Git VCS providers (GitHub, GitLab, Bitbucket, and more).

    <img src="csp-custom-build-name.png" width="706" alt="CSP statuses in GitHub"/>

### Perforce

* When building [Perforce shelved changelists](integrating-teamcity-with-perforce.md#Running+Builds+on+Perforce+Shelved+Files), earlier versions of TeamCity replaced checked-out files with corresponding shelved ones. Starting with version 2026.1, TeamCity uses a more sophisticated approach by running `p4 resolve` after unshelving, allowing it to detect and resolve conflicting changes.

  > Due to exclusive UAsset file checkout, this change can negatively affect Unreal Engine projects built in parallel. You can restore the previous behavior by adding the `teamcity.internal.perforce.useUnshelve=false` property to your project or server internal properties.

* You can now specify multiple [shelved changelist IDs](integrating-teamcity-with-perforce.md#Running+Builds+on+Perforce+Shelved+Files) when triggering a custom build for Perforce build configurations.

### Kubernetes

* Kubernetes [cloud profiles](setting-up-teamcity-for-kubernetes.md#Kubernetes+Cloud+Profile+Configuration) and [connections](configuring-connections.md#Kubernetes) now include settings that allow you to configure outgoing connections behind a proxy.

### HashiCorp Vault

* The [HashiCorp Vault Connection](hashicorp-vault.md#Set+Up+a+Vault+Connection) now supports authentication via [Google Cloud Platform authentication](https://developer.hashicorp.com/vault/docs/auth/gcp).

### Jira

* When configuring [connections to on-premises Jira instances](jira.md), you can now choose between authentication via regular username/password credentials or a personal access tokens issued on the issue tracker side.

## Miscellaneous Enhancements

* All TeamCity build configurations now automatically record agent hardware usage during builds. This change introduces the following updates:

  * The **PerfMon** build feature is no longer required and has been renamed to **Performance Monitor (Legacy)**.
  * The corresponding tab on the **Build Results** page is now called [Performance Monitor](build-results-page.md#Performance+Monitor+Tab), reflecting that the data is no longer tied to the deprecated feature.
  * A new `teamcity.perfmon.feature.enabled` parameter allows you to disable CPU, disk, and memory usage collection for specific build configurations or projects.


* Users with trial TeamCity Enterprise licenses can now use [](ai-assistant.md).

* The list of available [Get artifacts from...](artifact-dependencies.md) options now includes **Build from the same chain** that fails the build if both target and source configuration/pipeline do not belong to the same build chain. Previously, only the **Build from the same chain or last finished** option was available.

* In addition to the existing **Maximum concurrent builds for this build configuration** setting in [general build configuration settings](configuring-general-settings.md#Limit+Number+of+Simultaneously+Running+Builds), the new **If the limit is reached** option lets you choose whether TeamCity should queue excess builds or cancel the oldest running ones to free up capacity.

* We have implemented the [`override.dep.`](use-parameters-in-build-chains.md#Override+parameters+of+upstream+objects) parameter prefix that may completely replace the older `reverse.dep.` syntax in future TeamCity versions. New parameters can resolve parameter references and do not forcibly push edits to configurations/pipelines that do not have matching parameters.

*  The [SAML Authentication](configuring-authentication-settings.md#HTTP+SAML+2.0) plugin is now bundled with TeamCity, so you no longer need to install it separately to enable authentication through external SSO providers.

* We have [improved](https://youtrack.jetbrains.com/issue/TW-98507/) the Gradle version selection logic in our Gradle plugin. This change only adds a few info-level entries to the build logs and introduces no visible behavior changes, but it should improve the plugin’s overall stability.


## Upgrade Notes
{instance="tc"}

Before upgrading, we highly recommend reading about important changes in version [2026.1 compared to 2025.11](upgrade-notes.md#2026.1).


## Fixed Issues
{instance="tc"}

See the [TeamCity 2026.1 release notes](teamcity-2026-1-release-notes.md) article for the summary of implemented features and fixed issues.


## Roadmap

See the [TeamCity Roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) and [](pipelines-roadmap.md) articles to learn about future updates.


## Update TeamCity On-Premises
{instance="tc"}

We recommend using the [](upgrading-teamcity-server-and-agents.md#Automatic+Update) for the easiest and most reliable upgrade. For more information on the upgrade process and available options, see [](upgrading-teamcity-server-and-agents.md).

To download a `.tar.gz` or `.exe` installer for any TeamCity major or bug-fix version, visit the [](previous-releases-downloads.md) article.

For TeamCity servers running in Docker containers, see [this article](upgrading-teamcity-server-and-agents.md#manual-update-of-docker-image).


## Update TeamCity Cloud
{instance="tcc"}

JetBrains maintains TeamCity Cloud servers, so no action is needed on your part. During an update, your instance is briefly unavailable. We will notify you beforehand via email.

If you do not see the latest features described here, your instance may not be upgraded yet. [Contact our support team](troubleshooting.md) for assistance.


## Your Feedback Matters

We place a high value on your feedback and encourage you to share your thoughts and suggestions. See this link for more information: [](troubleshooting.md).


