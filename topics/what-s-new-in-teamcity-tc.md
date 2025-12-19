[//]: # (title: What's New in TeamCity On-Premises 2025.11)

<snippet id="2025-11-tc" instance="tc">

## AI Assistant

<secondary-label ref="secondary-eap"/>

TeamCity is a powerhouse packed with advanced features that let you shape CI/CD workflows to your exact needs. But this flexibility comes with a challenge: to get the most out of TeamCity, you need to be aware of its features and understand their inner workings. Concepts like [reverse parameter dependencies](use-parameters-in-build-chains.md#Override+Input+Parameters+of+Preceding+Configurations) or [checkout rules](vcs-checkout-rules.md) can prove challenging to grasp.

To make TeamCity more approachable for everyone, we’ve launched the [pipelines](create-and-edit-pipelines.md) initiative, and are investing heavily in reimagining the familiar UX. Complementing these efforts, we are excited to introduce the **TeamCity AI Assistant**.

<img src="aia-whats-new.png" width="706" thumbnail="true" alt="AI Assistant"/>

TeamCity AI Assistant aims to help both beginners and experts, from offering general guidance on TeamCity concepts to delivering troubleshooting insights for misconfigured or failing builds.

Learn more: [](ai-assistant.md)

## New Pipelines Features

See the [](pipelines-roadmap.md) article for more information our pipeline-specific development plans.

### New Build Steps

In version 2025.11, we're bringing the familiar [](net.md) build step to pipelines. Instead of one single step with dozens of settings that depend on the selected step command, pipelines split this build step into a series of task-specific units.

<img src="dk-dotnet-pipelines.png" width="706" thumbnail="true" alt=".NET steps in pipelines"/>

In addition to .NET, we are testing other build steps available in classic build configurations: Python, Xcode, Unity, and more. While not yet included in the official 2025.11 release, these build steps can be enabled on your TeamCity servers. Join our [Slack channel](slack-code-of-conduct.md) or [contact our support](ticket-based-support.md) to request these currently hidden steps!

> We have also enabled build steps shipped with external plugins to allow plugin developers to test their custom build steps in pipelines.
> 
{style="tip"}

Learn more: [](net.md).

### Project Registry Connections Support

Starting with version 2025.11, [Docker](configuring-connections.md#Docker+Registry) and [NPM](configuring-connections.md#npm-registry-settings) connections owned by projects are available as [integrations](pipeline-settings.md#Integrations) in pipeline and job settings.

<img src="pipelines-inherited-registry-connections.png" width="706" alt="Inherited integrations"/>

Learn more: [](pipeline-settings.md).

### Advanced Build and Test Actions

Starting with version 2025.11, pipelines support some of advanced features that was previously available only in build configurations. Users can now process build and test failures: [assign investigations](investigating-and-muting-build-failures.md#Investigations), [mute irrelevant failures](investigating-and-muting-build-failures.md#Mutes), and manually label as fixed issues that are expected to be resolved in future builds.

<img src="dk-pipelines-investigations.png" width="706" alt="Investigations and mutes in pipelines"/>

In addition, the run actions menu now includes options to [pin, tag, and comment](build-actions.md) individual pipeline runs.

<img src="dk-build-actions-pipelines.png" width="706" alt="Pin, tag, and comment actions in pipelines"/>

Learn more: [](investigating-and-muting-build-failures.md), [](build-actions.md)

### Parameter Import

Previously, a parameter owned by a project could not be used inside pipelines. Referencing such parameters would result in an implicit agent requirement: only agents that provide a value for this parameter were eligible to run this pipeline.

Starting with version 2025.11, you can import any parameter from a direct or indirect project and use it as any other native pipeline parameter.

<img src="pipelines-import-params.png" width="706" alt="Import parameters"/>

Learn more: [Pipeline parameters](pipeline-settings.md#Parameters), [](configuring-build-parameters.md)


## UX Improvements

We strongly believe that simplicity leads to greater power: an intuitive, easy-to-use product reduces configuration errors and helps you find the right setup faster. The recent [introduction of Pipelines](https://www.jetbrains.com/help/teamcity/2025.07/what-s-new-in-teamcity.html#Pipelines+EAP) has given fresh momentum to our ongoing effort to make TeamCity simpler and more enjoyable to use. Building on that progress, we’re excited to share another round of UI updates aimed at making your daily work in TeamCity smoother and more efficient.

### Reimagined Creation Flows

Every new TeamCity journey starts with "New Project", "New Build Configuration", and "New Connection" pages (unless you are a [Kotlin DSL](kotlin-dsl.md) or [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html) expert!). In version 2025.11, we are rethinking these pages for a faster, more intuitive creation process. Whether you are reusing an existing connection, sharing a VCS root, or creating a build configuration without a repository, everything you need is right at your fingertips.

<img src="build-configuraiton-creation-options.png" width="706" alt="All build config creation options"/>

Learn more: [Creating projects](creating-and-editing-projects.md), [Creating build configurations](creating-and-editing-build-configurations.md).

### New Pipeline and Build Chain Viewer

Real-world CI/CD workloads often include dozens of build configurations and jobs that combine building, testing, and deployment tasks into a single flow. To make it easier to explore these complex workflows, TeamCity now offers an enhanced visualization that displays [pipelines](create-and-edit-pipelines.md) and [build chains](build-chain.md) in a dedicated client that supports zoom and drag-and-drop, and features a minimap for easy navigation.

<img src="chains-minimap.png" width="706" alt="Build chains viewer" thumbnail="true"/>



## Server Encryption Enhancements
{product="tc"}

TeamCity lets you configure a custom 128-bit AES key to [encrypt all SSH keys and secrets](teamcity-configuration-and-maintenance.md#encryption-settings) to encrypt sensitive data, replacing the default key and enhancing overall server security.

Version 2025.11 adds two key improvements:

* The **Encryption settings** section in global server properties now includes a link to forcibly re-encrypt all existing entities. Use this option after rotating an encryption key to avoid keeping both old and new keys.

    <img src="start-reencryption.png" width="706" alt="Start reencryption"/>

* TeamCity can now import encryption keys from the `TEAMCITY_ENCRYPTION_KEYS` environment variable. This method is more secure than setting keys manually in the UI, as the keys are not stored in [`TeamCity Data Directory`](teamcity-data-directory.md)`/config/encryption-config.xml`, making it safer to [keep the data directory in a remote repository](teamcity-data-directory.md#TeamCityDataDirectory-centralRepository).

Learn more: [](teamcity-configuration-and-maintenance.md#encryption-settings).


## Miscellaneous Enhancements

<!--* [Azure DevOps OAuth 2.0 connection](configuring-connections.md#Azure+DevOps) settings were updated to support the Entra ID authentication policy.-->
* We have updated our TeamCity License Agreement to align it with JetBrains’ standards (such as definitions, structure, etc.); it has been fully restructured to make it clearer, more user-friendly, and easier to navigate. There are no changes to TeamCity's business model or users’ rights to how to use TeamCity.

    The only material change is an adjustment of our liability clause, which now follows JetBrains’ standard approach used across other JetBrains products.

* [](commit-status-publisher.md) no longer posts intermediate failure statuses in the following cases:

    * [](build-failure-conditions.md) include the **support test retry** option.
    * Tests are run by the [](gradle.md) build step with the [Gradle test retry](https://github.com/gradle/test-retry-gradle-plugin) plugin.
    * Tests are run by the `test` or `vstest` command of the [.NET](net.md#vstest) build step with a **Test retry count** greater than zero.

    TeamCity now posts only the final test status, determined after all required re-runs. This prevents VCS statuses from displaying false alarms for failures that are automatically resolved on retries.

* The **Miscellaneous** section of the [agent details page](viewing-build-agent-details.md) now includes the **Dump memory snapshot on agent** link that allows you to download an `.hprof` memory dump file.
* [Parallel tests](parallel-tests.md#Publish+Artifacts+Produced+By+Batch+Builds) and [Matrix build](matrix-build.md#Publishing+Artifacts) features now include the option to automatically place artifacts produced by virtual builds into separate folders. This prevents newer virtual builds from overriding artifacts with identical names produced by older batches.


</snippet>
