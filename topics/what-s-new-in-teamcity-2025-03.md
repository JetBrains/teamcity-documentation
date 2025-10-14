[//]: # (title: What's New in TeamCity 2025.03)
[//]: # (help-id: What's New in TeamCity 2025.03)


## TeamCity UI Updates
{instance="tc"}

As part of the [previously announced TeamCity/Pipelines merge](what-s-new-in-teamcity-2024-12.md#Pipelines+Merge+Announcement+and+Major+UI+Changes),version 2025.03 introduces another major UI update. Key changes include:

* The **Edit Project/Configuration** button is replaced with the **Settings** toggle. The View and Edit modes toggled by this UI element use different accent colors for distinguishing between the two at a glance. In addition, TeamCity stays in the selected mode unless you explicitly switch it. This means once you've switched to project/configuration settings, you can navigate to another configuration and project without exiting the Edit mode.

    <img src="dk-view-edit-mode-toggle.png" width="706" alt="View/Edit mode toggle"/>

* Project settings have been grouped into categories, making it easier to navigate between them.

* Build configuration settings are now arranged as tabs.

  <img src="dk-configuration-tabs.png" width="706" alt="Configuration settings tabs"/>

* The headers of the Project, Configuration, and Build pages have been redesigned for a lighter, more compact look with improved visibility. For example, builds now display key details — repository branch, total build time, queue time, and more — in dedicated blocks, while the **Actions** menu is accessible via the ellipsis button next to **Run**.

  <img src="dk-build-header-rework.png" width="706" alt="New build page header"/>


## TeamCity Recipes and Deprecation of Meta-Runners
{instance="tc"}

Starting with version 2025.03, **Meta-runners** are evolving into **Recipes**. While the core concept remains — creating custom build steps for frequently used actions — this transition offers multiple key benefits:

* Define recipes in XML or (in future releases) YAML
* Use built-in Recipes crafted by the TeamCity team


Your existing Meta-runners will continue to work and are accessible from the updated **Add Build Step** page.

<img src="dk-add-build-step.png" width="706" alt="Recipes on the Add Build Step page"/>

Learn more: [](working-with-meta-runner.md)

## Output Parameters
{instance="tc"}

The **Parameters** tab of build configuration setting now features two tabs: input and output parameters.

<img src="dk-add-output-param.png" width="706" alt="Create output param"/>

* Input parameters are your regular build parameters that existed before version 2025.03.
* Output parameters are build parameters with an explicit access permission. Values of these parameters can be read by any dependent configuration via the `dep.<config name>.<parameter name>` syntax.

Previously, dependent configurations could access any non-password parameter. Starting with version 2025.03, unless you choose to automatically expose all existing parameters, only newly created output parameters can be shared. This explicit input/output parameter separation allows configuration developers to adjust input parameters as needed without risking issues in external configurations that rely on these parameters.

Learn more: [](use-parameters-in-build-chains.md)


## Perforce Manual and Automatic Merge Support
{instance="tc"}

TeamCity now supports merging code changes from one Perforce stream to another. This enhancement enables two features:

* The [](automatic-merge.md) build feature now supports build configurations that utilize Perforce VCS roots.
* The **Actions** build menu now includes the option to [merge code changes manually](working-with-feature-branches.md#Manual+Branch+Merging).

Learn more: [](automatic-merge.md) | [](working-with-feature-branches.md)


## Docker and Podman Integration Enhancements
{instance="tc"}

### Global Configuration Containers
{instance="tc"}

You can now run all configuration steps within a single Docker/Podman container by adding the [](run-in-docker.md) build feature to your build configuration. This feature enables familiar build step [Container Settings](container-wrapper.md) on a build configuration level, so you only need to set these settings once instead of repeating them for each individual step.

### Kotlin Script Steps
{instance="tc"}

[](kotlin-script.md) build steps now support the [](container-wrapper.md), meaning you can now run these steps in Docker/Podman containers.

### Docker Support Rename
{instance="tc"}

We’ve renamed the [build feature](adding-build-features.md) that enables TeamCity to log in to private container registries and clean up images. Previously called **Docker Support**, it is now [**Docker Registry Connections**](docker-support.md) as of version 2025.03.

The new name more accurately reflects the feature's functionality, aligns with the similar [NPM Registry Connection](nodejs.md#Accessing+Private+NPM+Registries), and prevents confusion with the new [Run in Docker](#Global+Configuration+Containers) feature.

The [](kotlin-dsl.md) syntax changed in line with this rename:

```Kotlin
// Prior to 2025.03
features {
    dockerSupport {
        loginToRegistry = on {
            dockerRegistryId = "PROJECT_EXT_11,PROJECT_EXT_13" }
        cleanupPushedImages = true
    }
}
    
// Version 2025.03 and newer
features {
    dockerRegistryConnections {
        loginToRegistry = on {
            dockerRegistryId = "PROJECT_EXT_11,PROJECT_EXT_13" }
        cleanupPushedImages = true
    }
}
```

## Miscellaneous Changes
{instance="tc"}

* TeamCity now shows a [health report](server-health.md) that alerts you to disconnected but authorized agents, helping you identify issues and maintain a complete agent fleet.
* The Perforce CLI tool is now available on ARM-based [TeamCity Agent Docker images](agent-docker-images.md).





## TeamCity UI Updates
{instance="tcc"}

As part of the previously announced TeamCity/Pipelines merge,version 2025.03 introduces another major UI update. Key changes include:

* The **Edit Project/Configuration** button is replaced with the **Settings** toggle. The View and Edit modes toggled by this UI element use different accent colors for distinguishing between the two at a glance. In addition, TeamCity stays in the selected mode unless you explicitly switch it. This means once you've switched to project/configuration settings, you can navigate to another configuration and project without exiting the Edit mode.

    <img src="dk-view-edit-mode-toggle.png" width="706" alt="View/Edit mode toggle"/>

* Project settings have been grouped into categories, making it easier to navigate between them.

* Build configuration settings are now arranged as tabs.

  <img src="dk-configuration-tabs.png" width="706" alt="Configuration settings tabs"/>

* The headers of the Project, Configuration, and Build pages have been redesigned for a lighter, more compact look with improved visibility. For example, builds now display key details — repository branch, total build time, queue time, and more — in dedicated blocks, while the **Actions** menu is accessible via the ellipsis button next to **Run**.

  <img src="dk-build-header-rework.png" width="706" alt="New build page header"/>


## TeamCity Recipes and Deprecation of Meta-Runners
{instance="tcc"}

Starting with version 2025.03, **Meta-runners** are evolving into **Recipes**. While the core concept remains — creating custom build steps for frequently used actions — this transition offers multiple key benefits:

* Define recipes in XML or (in future releases) YAML
* Use built-in Recipes crafted by the TeamCity team

Your existing Meta-runners will continue to work and are accessible from the updated **Add Build Step** page.

<img src="dk-add-build-step.png" width="706" alt="Recipes on the Add Build Step page"/>

Learn more: [](working-with-meta-runner.md)

## Output Parameters
{instance="tcc"}

The **Parameters** tab of build configuration setting now features two tabs: input and output parameters.

<img src="dk-add-output-param.png" width="706" alt="Create output param"/>

* Input parameters are your regular build parameters that existed before version 2025.03.
* Output parameters are build parameters with an explicit access permission. Values of these parameters can be read by any dependent configuration via the `dep.<config name>.<parameter name>` syntax.

Previously, dependent configurations could access any non-password parameter. Starting with version 2025.03, unless you choose to automatically expose all existing parameters, only newly created output parameters can be shared. This explicit input/output parameter separation allows configuration developers to adjust input parameters as needed without risking issues in external configurations that rely on these parameters.

Learn more: [](use-parameters-in-build-chains.md)


## Perforce Manual and Automatic Merge Support
{instance="tcc"}

TeamCity now supports merging code changes from one Perforce stream to another. This enhancement enables two features:

* The [](automatic-merge.md) build feature now supports build configurations that utilize Perforce VCS roots.
* The **Actions** build menu now includes the option to [merge code changes manually](working-with-feature-branches.md#Manual+Branch+Merging).

Learn more: [](automatic-merge.md) | [](working-with-feature-branches.md)


## Docker and Podman Integration Enhancements
{instance="tcc"}

### Global Configuration Containers
{instance="tcc"}

You can now run all configuration steps within a single Docker/Podman container by adding the [](run-in-docker.md) build feature to your build configuration. This feature enables familiar build step [Container Settings](container-wrapper.md) on a build configuration level, so you only need to set these settings once instead of repeating them for each individual step.

### Kotlin Script Steps
{instance="tcc"}

[](kotlin-script.md) build steps now support the [](container-wrapper.md), meaning you can now run these steps in Docker/Podman containers.

### Docker Support Rename
{instance="tcc"}

We’ve renamed the [build feature](adding-build-features.md) that enables TeamCity to log in to private container registries and clean up images. Previously called **Docker Support**, it is now [**Docker Registry Connections**](docker-support.md) as of version 2025.03.

The new name more accurately reflects the feature's functionality, aligns with the similar [NPM Registry Connection](nodejs.md#Accessing+Private+NPM+Registries), and prevents confusion with the new [Run in Docker](#Global+Configuration+Containers) feature.

The [](kotlin-dsl.md) syntax changed in line with this rename:

```Kotlin
// Prior to 2025.03
features {
    dockerSupport {
        loginToRegistry = on {
            dockerRegistryId = "PROJECT_EXT_11,PROJECT_EXT_13" }
        cleanupPushedImages = true
    }
}
    
// Version 2025.03 and newer
features {
    dockerRegistryConnections {
        loginToRegistry = on {
            dockerRegistryId = "PROJECT_EXT_11,PROJECT_EXT_13" }
        cleanupPushedImages = true
    }
}
```

## Miscellaneous Changes
{instance="tcc"}

* TeamCity now shows a [health report](server-health.md) that alerts you to disconnected but authorized agents, helping you identify issues and maintain a complete agent fleet.
* The Perforce CLI tool is now available on ARM-based [TeamCity Agent Docker images](agent-docker-images.md).



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


