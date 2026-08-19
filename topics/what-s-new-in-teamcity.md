[//]: # (title: What's New in TeamCity 2026.2)
[//]: # (help-id: What's New in TeamCity 2026.2;What's New in TeamCity)

<show-structure for="chapter" depth="2"/>


## Pipeline Enhancements

### General Availability

TeamCity 2025.07 introduced pipelines as an Early Access Program (EAP) feature — instantly available on TeamCity Cloud, and on request for TeamCity On-Premises. Since then, we've been steadily closing the gap with classic build configurations, adding:

* Compatibility with familiar [build features](job-settings.md#Build+Features) and [steps](job-settings.md#Steps)
* [Integration with build chains](pipeline-settings.md#Pipeline+Dependencies), so pipelines can slot into existing workflows
* [Kotlin DSL support](pipelines-dsl.md) for configuration-as-code
* The [Custom Build dialog](running-custom-build.md), [output parameters](pipeline-settings.md#Parameters), and other familiar elements of build configurations
* [Job debugging](#Debug+Jobs), letting you test a pipeline job without saving your edits first

With this release, pipelines leave the EAP: they are now generally available on both TeamCity Cloud and On-Premises, for projects of any size and complexity.

Pipeline development does not stop here — see our [roadmap](pipelines-roadmap.md) for what's next.

### Debug Jobs

You can now test a single pipeline job without triggering the whole pipeline. Open a job's ellipsis menu and choose **Debug** — TeamCity runs just that job (and anything it depends on) with its current settings, complete with a live build log and terminal access to the agent that picked it up.

<img src="debug-jobs.png" width="706" alt="Debug jobs"/>

Learn more: [](create-and-edit-pipelines.md#Debug+Jobs)

### Improved Branch Handling

Pipelines are now better equipped to work with feature branches: in edit mode, you can switch between different repository branches and design unique workflows for each of them.

<img src="pipelines-edit-mode-branch-selector.png" width="706" alt="Branch selector in edit mode"/>

In addition, TeamCity now handles protected branches correctly. An attempt to commit edits made in the UI to a protected branch now results in a clear warning, and a suggestion to save the updated .yml file to another branch.

<img src="pipelines-save-yaml-branch-selection.png" width="705" alt="Save settings to a protected branch"/>

Learn more: [](pipeline-settings.md#Feature+Branches)


### Run Through Failures

When setting up [job dependencies](job-settings.md#Dependencies), you can now enable **Run job even if upstream fails** so a job keeps running even when the upstream job it depends on fails, instead of being automatically canceled.

<!-- TODO: screenshot of the dependency properties panel -->

The pipeline run is still marked as failed overall, but this lets you guarantee that specific jobs (for example, cleanup or notification steps) always run.

Learn more: [](job-settings.md#Dependencies)


### Promote pipeline runs

**Promote** — the button that triggers the downstream part of a [build chain](build-chain.md) from an older, already-finished build — now works for pipelines, not just build configurations.

<img src="promote-pipeline-run.png" width="706" alt="Promote pipeline run"/>

For example, promote a successful "Build Docker image" run into the "Upload to DockerHub" configuration or pipeline to re-deploy that artifact without rebuilding it.

Learn more: [](run-build-chains.md#Promote+a+build)


### Unbound pipelines

You can now create pipelines without VCS roots attached. Previously, this option was only available for build configurations. Choose the “Without repository” option when creating a pipeline to create a custom workflow that does not check out any remote sources.

<img src="pipeline-without-repo.png" width="706" alt="Pipeline without repo"/>

Learn more: [](create-and-edit-pipelines.md)


## TeamCity AI

We add AI to TeamCity where it solves an actual CI/CD problem, and leave the rest of the decisions to you. AI features stay off until a server administrator enables them, and starting with this release, you also choose which AI provider stands behind them.

### AI Assistant

Starting with TeamCity 2026.2, you're no longer limited to the built-in JetBrains AI: AI Assistant now supports "bring your own key" (BYOK), letting you connect it to a third-party AI provider your organization already has access to. Choose a provider under the **Provider** selector and enter your API key, and AI Assistant will run on that model instead. The bottom of the Assistant panel always shows which model is currently active.

<img src="aia-anthropic.png" width="706" thumbnail="true" alt="AI Assistant using Anthropic models"/>

In addition, AI Assistant now works with pipelines, not just classic build configurations.

Learn more: [](ai-assistant.md#Providers)

### MCP Improvements

TeamCity's MCP server now exposes three new tools for managing [pipelines](create-and-edit-pipelines.md) — retrieve, edit, or delete them straight from your AI agent. You can also connect using OAuth instead of pre-configuring a personal access token.

<img src="air-mcp-oauth.png" width="706" alt="OAuth authentication for TeamCity MCP"/>

Learn more: [](ai-agent-integration.md#TeamCity+MCP).







## Rerun Failed Chain Builds

The **Dependencies** tab of build configuration settings now includes a **Retry settings** group. Enable it to delay a downstream build and automatically retry a failing dependency in place, no need to re-run the entire chain.

<img src="retry-dependency-settings.png" width="706" alt="Rerun failed dependencies"/>

Learn more: [](configuring-dependencies.md#Re-run+failed+chain+builds)




## GitHub Integration

The [Pull Requests](pull-requests.md#GitHub+Pull+Requests) build feature can now match pull requests by their source branch instead of their branch reference. This lets TeamCity recognize separate pull requests in different repositories as related changes and build them together in the same build chain, even when GitHub assigns them different pull request numbers.

Learn more: [GitHub pull requests](pull-requests.md#GitHub+Pull+Requests)




## Miscellaneous Enhancements

* TeamCity now supports the lossless Zstandard compression algorithm for Tape Archive (.tar) files: you can use `.tar.zst` or `.tzst` extension when [publishing](configuring-general-settings.md#Artifact+Paths) and [exchanging](artifact-dependencies.md#Artifact+dependencies) artifacts.
* The Performance Monitor interface has been refreshed with a more modern appearance, improving readability and visual consistency.

    <img src="performance-monitor.png" width="706" thumbnail="true" alt="Performance monitor"/>

* TeamCity now [more accurately](working-with-build-queue.md#build-duration-estimates) estimates duration of highly variable parameterized builds.
* You can now choose where TeamCity compiles versioned DSL settings: [on the server or build agents](kotlin-dsl.md#DSL+Compilation).
{instance="tc"}

## Upgrade Notes
{instance="tc"}

Before upgrading, we highly recommend reading about important changes in version [2026.2 compared to 2026.1](upgrade-notes.md#2026.2).


## Fixed Issues
{instance="tc"}

See the [TeamCity 2026.2 release notes](teamcity-2026-2-release-notes.md) article for the summary of implemented features and fixed issues.


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
