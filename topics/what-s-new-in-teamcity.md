[//]: # (title: What's New in TeamCity 2026.3)
[//]: # (help-id: What's New in TeamCity 2026.3;What's New in TeamCity)

<show-structure for="chapter" depth="2"/>


## Pipeline enhancements

### Execution timeouts

You can now cap how long a pipeline run or an individual job may take: use the new **Execution timeout** setting in the **Optimizations** group of pipeline and job settings. In addition, pipelines now respect the [default build execution timeout](teamcity-configuration-and-maintenance.md#Build+Settings) configured in the server's global settings.

Learn more: [](job-settings.md#Execution+timeout)

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

### Maven user settings

You can now configure [custom user settings](maven.md#MavenUserSettings) for Maven build steps. Similarly to build configurations, you can choose a file uploaded to a parent project's [**Maven settings** section](maven-server-side-settings.md) or specify a custom path to the required file.

<img src="pipelines-maven-user-settings.png" width="706" alt="User settings in pipeline job" thumbnail="true"/>

Learn more: [](maven.md#MavenUserSettings)
<!--

## Build tools integration

### Maven 4

???

### Advanced Gradle integration mode

In version 2026.2, we have introduced the [advanced Gradle integration mode](gradle.md#Gradle+Integration+Mode) that offers multiple benefits, including a wider range of supported command-line options and isolated projects support.

In this version, this mode becomes the default one: you no longer need to add the `teamcity.internal.gradle.runner.launch.mode=gradle_v2` property.

??? is rollback needed ???

-->


## MCP brave mode

Previous updates introduced several [MCP tools](ai-agent-integration.md#TeamCity+MCP) that AI agents can use to interact with TeamCity. Because the `teamcity_pipeline_post`, `teamcity_pipeline_delete`, and `teamcity_rest_post` tools can edit and remove existing entities, they stayed disabled unless [brave mode](ai-agent-integration.md#Safety+Concerns) was explicitly enabled on your server. Starting with this version, brave mode is the default, so your AI agents can use every available MCP tool.

To limit what your agents can do, switch back to safe mode or, as we recommend, issue fine-grained access tokens that control which operations are available to them. See the [](ai-agent-integration.md#Safety+Concerns) section for more information.

## UI changes

Recent releases gave you plenty of ways to use TeamCity without opening it at all — the [MCP server](ai-agent-integration.md#TeamCity+MCP), the [TeamCity CLI](teamcity-cli.md), [browser-based PKCE authentication](teamcity-cli-authentication.md#pkce), and the [AI Assistant](ai-assistant.md). An agent working on your behalf can now drive most CI/CD tasks end to end.

That makes TeamCity AI-ready, not AI-first. Much of the work still starts with a person opening a build page to read a failure, compare a diff, or decide what to rerun, and newcomers meet the product through its UI long before they script against it. The UI remains just as relevant, so we keep refining it with every release. Here is what changed this time.

### Reworked build Overview tab

The [Overview tab](build-results-page.md#Overview+Tab) of finished pipeline and build configuration runs has been reworked to improve navigation and readability.

* All contextual sections (**Changes**, **Deployment**, **Code coverage**, and so on) now appear as dedicated collapsible panes with unique icons, for a cleaner look and better distinction between them.

    <img src="build-overview-collapsible-panes.png" width="706" alt="Collapsible panes of the Overview tab"/>

* The **Tests** pane now has toggles that switch between a flat list and a tree view.

* Each test row has a **View test history** icon that opens the full run history of this test.

    <img src="build-overview-tests.png" width="706" alt="Tests pane"/>

    <img src="view-test-history2.png" width="706" thumbnail="true" alt="Test history page"/>

* By default, tests are now sorted by failure order: the tests that failed first appear on top. Tests that have failed across multiple builds also display an "in N builds" label that highlights a recurring issue.

### Improved diff viewer

The [**Changes** tab diff viewer](build-results-page.md#Changes+Tab) was updated.

* Side navigation bar, difference counter, and keyboard shortcuts make it easier to traverse and inspect changes.
* A compact header and collapsible change details maximize the main client area.
* Ticking the **Ignore whitespace** option no longer reloads the page.

<img src="diff-viewer-update.png" width="706" alt="Updated diff viewer" thumbnail="true"/>

Learn more: [](build-results-page.md)

## Miscellaneous improvements

* The **Dependencies** tab of the [Build Results page](build-results-page.md#Dependencies+Tab) now accepts configuration IDs. Previously, you could only search by dependency names.
* [Matrix builds](matrix-build.md) now accept the `teamcity.matrix.excludedCombinations` parameter that allows you to skip specific combinations.

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
