[//]: # (title: What's New in TeamCity 2025.07)
[//]: # (help-id: What's New in TeamCity 2025.07)


## UI Improvements
{instance="tc"}

We remain committed to delivering a modern, intuitive TeamCity experience for all users, whether you prefer pipelines or classic build chains for your CI/CD workflows. This update brings several UI improvements, including:

* A redesigned navigation sidebar with a **+** button for quickly adding subprojects, configurations, and pipelines. You can also set the panel to auto-hide to maximize workspace.
* A new **What’s New** widget to keep you informed about major updates in each release.
* An ability to open [build logs](build-log.md) in full-screen mode.


## Public Marketplace Recipes
{instance="tc"}

In 2025.03, we announced a shift from meta-runners to recipes: lightweight, YAML-based custom steps [available on JetBrains Marketplace](https://plugins.jetbrains.com/teamcity_recipe). At the moment, the Marketplace offers more than a dozen of JetBrains-made recipes that automate tasks like pinning builds, downloading artifacts, and updating build statuses.

Starting with this release, third-party recipes are also supported. Browse community-made options, inspect their source code to view implementation details, [upload your own recipes to Marketplace](https://plugins.jetbrains.com/docs/marketplace/uploading-a-new-plugin.html#upload-teamcity-recipes), and expand TeamCity’s extensive arsenal of build steps with custom tools.

The **Recipes** project settings page now displays a comprehensive summary of both private and public recipes available for this project and its children. For public recipes, TeamCity collects recipe data from the Marketplace and shows warnings when a recipe is outdated or taken down.

<img src="dk-recipes-in-root-project.png" width="706" alt="Recipes page in Root project"/>

Learn more: [](working-with-meta-runner.md)


## Perforce Integration Enhancements
{instance="tc"}

* You can now add multiple [Perforce Shelve triggers](perforce-shelve-trigger.md) to your configurations. to a configuration. Previously, adding one Shelve trigger locked you out of adding more via the TeamCity UI.
* We have implemented multiple new options that allow you to set up periodic workspace clean-ups. See the [](perforce-workspace-handling-in-teamcity.md#Workspace+Deletion) article for more information.


## Kubernetes Executor Updates
{instance="tc"}

Introduced a few releases ago, [Kubernetes executor](kubernetes-executor.md) leverages your existing Kubernetes clusters by turning them into independent orchestrators for TeamCity builds. Unlike with regular cloud agents that are fully managed by TeamCity, this integration allows the server to offload the build queue to a k8s cluster, granting the later full control over pod lifecycle.

TeamCity 2025.07 introduces a range of Kubernetes executor updates:

* Executors are now natively integrated into TeamCity default prioritization mechanism. When a build is queued, TeamCity first checks for a free self-hosted agent, then for cloud profiles that can launch a compatible agent. If none are available, the build is offloaded to an executor.
* [Implicit agent requirements](configuring-agent-requirements.md#Implicit+Requirements) are now correctly recognized. Build steps can impose implicit tooling requirements on agents, like requiring [Docker or Podman](integrating-teamcity-with-container-managers.md) for containerized steps, or the .NET 8 SDK for .NET builds. As of 2025.07, TeamCity can correctly match these requirements with pod specifications, ensuring builds are never offloaded to executors that cannot run them.
* Numerous bug fixes, such as resolving the ignored maximum build limit, PowerShell steps failing to run, excessive build log errors, and more.



## Pipelines EAP
{instance="tc"}

TeamCity 2025.07 introduces the first iteration of [TeamCity Pipelines](https://www.jetbrains.com/teamcity/pipelines/) integrated directly into standard TeamCity On-Premises and Cloud servers.

<img src="dk-create-button-project.png" width="706" alt="Create button in project header"/>

Pipelines are designed for easy setup and include unique features like YAML support and an advanced visual editor.

<img src="dk-main-pipeline-view.png" width="706" alt="Main pipeline view"/>

Currently in Early Access, pipelines may lack some features needed for production CI/CD workflows. As such, they are hidden by default in the UI. Enable them by clicking **Join Early Access program** on the welcome screen or What’s New widget, or go directly to the [Request Access](https://lp.jetbrains.com/teamcity-pipelines-in-enterprise-eap-4708/) page.

## New Approval Rules
{instance="tc"}

In [](build-approval.md) and [](untrusted-builds.md) settings, you can now combine individual users with user groups in a single entity with a shared vote count. For example, the following rule expects three votes to start a build:

```Text
(users:john.doe,jane.doe,tcadmin,groups:PROJECT_ADMINS):3
```

These three votes can come from any combination of the specified users or groups.

## Miscellaneous Enhancements
{instance="tc"}

* [SSH keys](ssh-keys-management.md) uploaded to or generated in TeamCity are now stored encrypted in the [](teamcity-data-directory.md) in encrypted form. TeamCity uses a [custom encryption key](teamcity-configuration-and-maintenance.md#encryption-settings) from the general server settings, or a built-in key if none is specified. Note that only newly uploaded or generated keys are encrypted, re-upload existing keys to apply encryption.
<!--* The [](parallel-tests.md) build feature now includes the **Artifacts** setting that allows TeamCity to categorize artifacts into "Batch N" folders on the main build results page. Previously, you had to implement this behavior manually by adding the `teamcity.build.parallelTests.currentBatch` parameter reference to artifact paths.

    <img src="dk-auto-categorized-batch-artifacts.png" width="706" alt="Artifacts in separate batch folders"/>
-->

* If the [](kotlin-dsl.md) "pom.xml" file includes the `<kotlin.compiler.incremental>true</kotlin.compiler.incremental>` line, TeamCity Maven plugin will now switch to the [incremental compilation mode](https://kotlinlang.org/docs/maven.html#enable-incremental-compilation). Previously, this setting was ignored.



## UI Improvements
{instance="tcc"}

We remain committed to delivering a modern, intuitive TeamCity experience for all users, whether you prefer pipelines or classic build chains for your CI/CD workflows. This update brings several UI improvements, including:

* A redesigned navigation sidebar with a **+** button for quickly adding subprojects, configurations, and pipelines. You can also set the panel to auto-hide to maximize workspace.
* A new **What’s New** widget to keep you informed about major updates in each release.
* An ability to open [build logs](build-log.md) in full-screen mode.


## Public Marketplace Recipes
{instance="tcc"}

In 2025.03, we announced a shift from meta-runners to recipes: lightweight, YAML-based custom steps [available on JetBrains Marketplace](https://plugins.jetbrains.com/teamcity_recipe). At the moment, the Marketplace offers more over a dozen of JetBrains-made recipes that automate tasks like pinning builds, downloading artifacts, and updating build statuses.

Starting with this release, third-party recipes are also supported. Browse community-made options, inspect their source code to view implementation details, [upload your own recipes to Marketplace](https://plugins.jetbrains.com/docs/marketplace/uploading-a-new-plugin.html#upload-teamcity-recipes), and expand TeamCity’s extensive arsenal of build steps with custom tools.

The **Recipes** project settings page now displays a comprehensive summary of both private and public recipes available for this project and its children. For public recipes, TeamCity collects recipe data from the Marketplace and shows warnings when a recipe is outdated or taken down.

<img src="dk-recipes-in-root-project.png" width="706" alt="Recipes page in Root project"/>

Learn more: [](working-with-meta-runner.md)


## Perforce Integration Enhancements
{instance="tcc"}

* You can now add multiple [Perforce Shelve triggers](perforce-shelve-trigger.md) to your configurations. to a configuration. Previously, adding one Shelve trigger locked you out of adding more via the TeamCity UI.
* We have implemented multiple new options that allow you to set up periodic workspace clean-ups. See the [](perforce-workspace-handling-in-teamcity.md#Workspace+Deletion) article for more information.


## Kubernetes Executor Updates
{instance="tcc"}

Introduced a few releases ago, [Kubernetes executor](kubernetes-executor.md) leverages your existing Kubernetes clusters by turning them into independent orchestrators for TeamCity builds. Unlike with regular cloud agents that are fully managed by TeamCity, this integration allows the server to offload the build queue to a k8s cluster, granting the later full control over pod lifecycle.

TeamCity 2025.07 introduces a range of Kubernetes executor updates:

* Executors are now natively integrated into TeamCity default prioritization mechanism. When a build is queued, TeamCity first checks for a free self-hosted agent, then for cloud profiles that can launch a compatible agent. If none are available, the build is offloaded to an executor.
* [Implicit agent requirements](configuring-agent-requirements.md#Implicit+Requirements) are now correctly recognized. Build steps can impose implicit tooling requirements on agents, like requiring [Docker or Podman](integrating-teamcity-with-container-managers.md) for containerized steps, or the .NET 8 SDK for .NET builds. As of 2025.07, TeamCity can correctly match these requirements with pod specifications, ensuring builds are never offloaded to executors that cannot run them.
* Numerous bug fixes, such as resolving the ignored maximum build limit, PowerShell steps failing to run, excessive build log errors, and more.


## Pipelines EAP
{instance="tcc"}

TeamCity 2025.07 introduces the first iteration of [TeamCity Pipelines](https://www.jetbrains.com/teamcity/pipelines/) integrated directly into standard TeamCity On-Premises and Cloud servers.

<img src="dk-create-button-project.png" width="706" alt="Create button in project header"/>

Pipelines are designed for easy setup and include unique features like YAML support and an advanced visual editor.

<img src="dk-main-pipeline-view.png" width="706" alt="Main pipeline view"/>

Currently in Early Access, pipelines may lack some features needed for production CI/CD workflows. We recommend using them for relatively simple CI/CD routines with no more than 10 to 15 separate jobs linked in a single pipeline.



## New Approval Rules
{instance="tcc"}

In [](build-approval.md) and [](untrusted-builds.md) settings, you can now combine individual users with user groups in a single entity with a shared vote count. For example, the following rule expects three votes to start a build:

```Text
(users:john.doe,jane.doe,tcadmin,groups:PROJECT_ADMINS):3
```

These three votes can come from any combination of the specified users or groups.

## Miscellaneous Enhancements
{instance="tcc"}


<!--* The [](parallel-tests.md) build feature now includes the **Artifacts** setting that allows TeamCity to categorize artifacts into "Batch N" folders on the main build results page. Previously, you had to implement this behavior manually by adding the `teamcity.build.parallelTests.currentBatch` parameter reference to artifact paths.

    <img src="dk-auto-categorized-batch-artifacts.png" width="706" alt="Artifacts in separate batch folders"/>
-->
* If the [](kotlin-dsl.md) "pom.xml" file includes the `<kotlin.compiler.incremental>true</kotlin.compiler.incremental>` line, TeamCity Maven plugin will now switch to the [incremental compilation mode](https://kotlinlang.org/docs/maven.html#enable-incremental-compilation). Previously, this setting was ignored.



## Upgrade Notes
{instance="tc"}

Before upgrading, we highly recommend reading about important changes in version [2025.03 compared to 2024.12](upgrade-notes.md#2025.03).


## Fixed Issues
{instance="tc"}

See the [TeamCity 2025.03 release notes](teamcity-2025-03-release-notes.md) article for the summary of implemented features and fixed issues.


## Roadmap

See the [TeamCity roadmap](https://www.jetbrains.com/teamcity/roadmap/#teamcity-roadmap) to learn about future updates.


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


