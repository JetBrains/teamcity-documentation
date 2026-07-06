# Project Settings

When you enter an [edit mode](project-administrator-guide.md#Edit+and+View+Modes), TeamCity projects display a sidebar with settings grouped into categories.

<img src="dk-project-settings.png" width="706" alt="Project settings"/>

Note that some of these settings can be inaccessible to users with [insufficient permissions](managing-roles-and-permissions.md).


## General

This section lists basic project settings, such as its public description and name, as well as buttons that create child subprojects, [build configurations](creating-and-editing-build-configurations.md), and [pipelines](create-and-edit-pipelines.md).

For more information on template-related settings, see the [](build-configuration-template.md#Defining+default+template+for+project) and [](build-configuration-template.md#Enforcing+settings+inherited+from+template) sections.

## VCS Roots

This section lists all VCS roots owned by this project, whether they are attached to any of the project configuration or not.

Related article: [](configuring-vcs-roots.md)

## Parameters

The **Parameters** tab allows you to create name-value pairs that are available to all configurations owned by this project and its subprojects.

Related articles: [](configuring-build-parameters.md) | [](use-parameters-in-build-chains.md#Input+and+output+parameters)


## Recipes

Recipes are custom configuration [build steps](configuring-build-steps.md) that do not ship with TeamCity. Project administrators can add recipes by doing the following:

* Extract a recipe from a regular build step.
* Download a recipe created by TeamCity developers or community from the JetBrains Marketplace.

This section allows you to control whether the second option is available.

Related article: [](working-with-meta-runner.md)


## Versioned Settings

This section allows you to set up configuration-as-code: store all project and build configurations in a remote repository in Kotlin DSL or XML format.

> Pipeline settings are stored in a separate YAML file. Open the **Repositories** section of pipeline settings to configure its behavior.
> 
{style="note"}

Related articles: [](storing-project-settings-in-version-control.md) | [](kotlin-dsl.md)


## Connections

A TeamCity **connection** is an entity that stores settings required to access resources on a 3rd-party service: a VCS hosting, a cloud hosting provider, an image registry, and so on. This section allows you to create connections available to all subprojects and build configurations owned by this project.

Related article: [](configuring-connections.md)

## Maven Settings

This tab allows you to upload Maven settings that will be available for individual [](maven.md) build steps.

Related article: [](maven-server-side-settings.md)


## Issue Trackers

Integrations with issue trackers allow TeamCity to show links to corresponding issue tracker tickets when displaying new code changes. Jira, Bugzilla, YouTrack, GitHub, GitLab, Bitbucket (Cloud, Server, Data Center), and Azure DevOps Server (formerly TFS) are supported out of the box.

Related article: [](integrating-teamcity-with-issue-tracker.md)

## Cloud Profiles

This section lets you configure cloud-hosted build agents that start on demand and automatically stop when idle. These agents help you scale your TeamCity build server based on current workload.

You can also set up a Kubernetes Executor here, which works as an independent orchestrator for the TeamCity build queue.

> Only the project (and its subprojects) that owns a cloud profile can use the agents it creates. To make cloud agents available to all projects on the server, configure a cloud profile for the [Root project](project-administrator-guide.md#Steps%2C+Configurations+and+Projects).
> 
{style="note"}

Related articles: [](teamcity-integration-with-cloud-solutions.md) | [](kubernetes-executor.md)


## Artifacts Storage
{instance="tc"}

The **Artifacts Storage** section allows you to configure an external storage (for example, an S3 bucket) for artifacts produced by your builds.

Related articles: [](configuring-artifacts-storage.md) | [](artifacts-migration-tool.md)


## SSL / HTTPS Certificates

> This tab is only available for the Root project settings.
> 
{style="note"}

The **SSL / HTTPS Certificates** tab allows you to upload certificates that TeamCity will consider as trusted when establishing HTTPS/SSL connections.

Related article: [](uploading-ssl-certificates.md)

## VCS Auth Tokens

This tab allows you to keep track of existing access tokens and utilize a configured [OAuth connection](#Connections) to issue new ones. Use these tokens to set up authentication settings for objects that require access to a 3rd-party service: [VCS roots](#VCS+Roots), [Commit Status Publishers](commit-status-publisher.md), [Pull Requests](pull-requests.md), and so on.

Related article: [](manage-access-tokens.md)

## Untrusted Builds

Adding [VCS Triggers](configuring-vcs-triggers.md) to build configurations allows them to automatically run builds that process new commits. However, if your configuration targets a public repository and the [](pull-requests.md) feature is configured, this setup can pose a security risk: TeamCity can automatically start builds that process malicious changes from external users.

This section lets you define detailed conditions to control which pull requests are safe to process automatically and which require manual approval from designated team members.

Related article: [](untrusted-builds.md)

## Project Isolation

TeamCity build configurations can use two types of [dependencies](configuring-dependencies.md) to interact with other configurations:

* [](configuring-dependencies.md) — link multiple configurations in a [build chain](build-chain.md), where a downstream build waits for an upstream build to complete.
* [](artifact-dependencies.md) — allow a configuration to use artifacts produced by another configuration.

These dependencies are set up in upstream configurations, which means administrators of other TeamCity projects can add dependencies on your configurations. If your project contains sensitive configurations that should not be triggered by or provide artifacts to external projects, use the **Project Isolation** settings to restrict access.

Related article: [](secure-chain-dependencies.md)


## SSH Keys

This section allows you to upload (or generate new) private keys. These keys allow build configurations to check out repositories using SSH protocol.

Related article: [](ssh-keys-management.md)

## Report Tabs

If your reporting tool produces reports in HTML format, you can extend TeamCity with a custom tab to show the information provided by the third-party reporting tool.

Related article: [](including-third-party-reports-in-the-build-results.md)


## Usages Report

This tab is hidden by default and appears when you check which entities depend on a specific TeamCity object. It helps you see where the object is used and understand which parts of your CI workflow might be affected if you edit or delete it.

* VCS roots — roots show the **View usages** link on their settings pages and inside the [](#VCS+Roots) table. Click this link to view all build configurations to which this root is attached.

    <img src="dk-usages-root.png" width="706" alt="Usages report for a VCS root"/>

* Templates — click the **Usage** tab of template settings to view all configurations that are based on this template, and all projects that use this template as a default one.

  <img src="dk-usages-template.png" width="706" alt="Usages report for a template"/>

* Build configurations — click the **Usage** tab of configurations settings to view all configurations dependent on this one. Dependent configurations are those that have [snapshot](configuring-dependencies.md) and/or [artifact](artifact-dependencies.md) dependencies on this configuration.


Related articles: [](configuring-vcs-roots.md) | [](build-configuration-template.md) | [](configuring-dependencies.md)

## Clean-up Rules

TeamCity periodically performs a server-wide cleanup to remove outdated data: obsolete builds, their artifacts, build caches, and more. This scheduled activity is configured on the **Admin | Clean-up Settings** page. The **Clean-up Rules** section allows you to set up per-project rules that override global settings.

Related article: [](teamcity-data-clean-up.md)

## Shared Resources

The Shared Resources feature allows limiting concurrently running builds using an external (to the CI server) resource, for example, a test database, or a server with a limited number of connections.

Related article: [](shared-resources.md)

## NuGet Feed
{instance="tc"}

If you want to publish your NuGet packages to a limited audience (for example, to use them internally), you can use TeamCity as a NuGet feed. You can configure multiple NuGet feeds for a TeamCity project.

The built-in TeamCity NuGet feed supports v1, v2, and v3 API versions.

Related article: [](using-teamcity-as-nuget-feed.md)


## Suggestions

This tab shows automatic TeamCity suggestions that aim to resolve active [health reports](server-health.md).

Related article: [](server-health.md)