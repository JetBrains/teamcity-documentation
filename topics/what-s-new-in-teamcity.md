[//]: # (title: What's New in TeamCity 2026.2)
[//]: # (help-id: What's New in TeamCity 2026.2;What's New in TeamCity)

<show-structure for="chapter" depth="2"/>


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


