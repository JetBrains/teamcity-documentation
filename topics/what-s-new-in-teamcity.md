[//]: # (title: What's New in TeamCity 2026.3)
[//]: # (help-id: What's New in TeamCity 2026.3;What's New in TeamCity)

<show-structure for="chapter" depth="2"/>


## Pipeline Enhancements

### Execution timeouts

New **Execution timeout** settings inside pipeline and job **Optimizations** group allow you to set the maximum run duration for your workflows. In addition, pipelines now follow the global default timeout threshold specified in the server [admin area](build-failure-conditions.md#Set+Custom+Build+Execution+Timeout).

Learn more: [](job-settings.md#Optimizations)

### Additional tabs for jobs

The [pipelines](create-and-edit-pipelines.md) run results page now shows additional tabs for a selected job:

* Performance monitor
* [Qodana](qodana.md)
* Custom report tabs
* Allure report
* [Maven](maven.md) build info
* Container info

<img src="pipelines-fullscreen-tabs.png" thumbnail="true" width="706" alt="Pipeline job build result tabs"/>

All tabs except for **Performance monitor** are contextual: they only appear if the selected job performs corresponding actions. For example, the **Container info** tab is shown only for jobs whose steps [run in Docker or Podman containers](job-settings.md#Integrations).

## Upgrade notes
{instance="tc"}

Before upgrading, we highly recommend reading about important changes in version [2026.3 compared to 2026.2](upgrade-notes.md#2026.3).


## Fixed issues
{instance="tc"}

See the [TeamCity 2026.3 release notes](teamcity-2026-3-release-notes.md) article for the summary of implemented features and fixed issues.


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


## Your feedback matters

We place a high value on your feedback and encourage you to share your thoughts and suggestions. See this link for more information: [](troubleshooting.md).
