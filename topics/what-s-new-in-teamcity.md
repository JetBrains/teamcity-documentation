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

Learn more: [](create-and-edit-pipelines.md#Debug+Jobs).



## Rerun Failed Chain Builds

The **Dependencies** tab of build configuration settings now includes a **Retry settings** group. Enable it to delay a downstream build and automatically retry a failing dependency in place, no need to re-run the entire chain.

<img src="retry-dependency-settings.png" width="706" alt="Rerun failed dependencies"/>

Learn more: [](configuring-dependencies.md#Re-run+failed+chain+builds).

## GitHub

The [Pull Requests](pull-requests.md#GitHub+Pull+Requests) build feature can now match pull requests by their source branch instead of their branch reference. This lets TeamCity recognize separate pull requests in different repositories as related changes and build them together in the same build chain, even when GitHub assigns them different pull request numbers.

Learn more: [GitHub pull requests](pull-requests.md#GitHub+Pull+Requests).


## Miscellaneous Enhancements

* TeamCity now supports the lossless Zstandard compression algorithm for Tape Archive (.tar) files: you can use `.tar.zst` extension when [publishing](configuring-general-settings.md#Artifact+Paths) and [exchanging](artifact-dependencies.md#Artifact+dependencies) artifacts.


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


