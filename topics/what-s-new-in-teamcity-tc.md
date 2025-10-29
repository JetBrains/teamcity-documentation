[//]: # (title: What's New in TeamCity On-Premises 2025.11)

<snippet id="2025-11-tc">


## New Pipelines Features

### New Build Steps

In version 2025.11, we're bringing the familiar [](net.md) build step to pipelines. Instead of one single step with dozens of settings that depend on the selected step command, pipelines split this build step into a series of task-specific units.

<img src="dk-dotnet-pipelines.png" width="706" thumbnail="true" alt=".NET steps in pipelines"/>

In addition to .NET, we are testing other build steps available in classic build configurations: Python, Xcode, Unity, and more. While not yet included in the official 2025.11 release, these build steps can be enabled on your TeamCity servers. Join our [Slack channel](slack-code-of-conduct.md) or [contact our support](ticket-based-support.md) to request these currently hidden steps!

Learn more: [](net.md).

### Project Registry Connections Support

Starting with version 2025.11, [Docker](configuring-connections.md#Docker+Registry) and [NPM](configuring-connections.md#npm-registry-settings) connections owned by projects are available as [integrations](pipeline-settings.md#Integrations) in pipeline and job settings.

<img src="pipelines-inherited-registry-connections.png" width="706" alt="Inherited integrations"/>

Learn more: [](pipeline-settings.md).

## UI Enhancements

We strongly believe that simplicity leads to greater power: an intuitive, easy-to-use product reduces configuration errors and helps you find the right setup faster. The recent [introduction of Pipelines](https://www.jetbrains.com/help/teamcity/2025.07/what-s-new-in-teamcity.html#Pipelines+EAP) has given fresh momentum to our ongoing effort to make TeamCity simpler and more enjoyable to use. Building on that progress, we’re excited to share another round of UI updates aimed at making your daily work in TeamCity smoother and more efficient.

### Updated "New Project" and "New Build Configuration" Pages

### New Pipeline and Build Chain Viewer

Real-world CI/CD workloads often include dozens of build configurations and jobs that combine building, testing, and deployment tasks into a single flow. To make it easier to explore these complex workflows, TeamCity now offers an enhanced visualization that displays [pipelines](create-and-edit-pipelines.md) and [build chains](build-chain.md) in a dedicated, zoomable client area with a minimap for easy navigation.

<img src="chains-minimap.png" width="706" alt="Build chains viewer" thumbnail="true"/>


## AI Assistant

<secondary-label ref="secondary-eap"/>

TeamCity is a powerhouse packed with advanced features that let you shape CI/CD workflows to your exact needs. But this flexibility comes with a challenge: to get the most out of TeamCity, you need to be aware of its features and understand their inner workings. Concepts like [reverse parameter dependencies](use-parameters-in-build-chains.md#Override+Input+Parameters+of+Preceding+Configurations) and [agentless "Executor" setups](kubernetes-executor.md) can be complex to configure and manage.

To make TeamCity more approachable for every DevOps engineer, we’ve launched the [pipelines](create-and-edit-pipelines.md) initiative and are investing heavily in a redesigned UI. Complementing these efforts, we are excited to introduce the **TeamCity AI Assistant**.

<img src="aia-whats-new.png" width="706" thumbnail="true" alt="AI Assistant"/>

TeamCity AI Assistant aims to help both beginners and experts, from offering general guidance on TeamCity concepts to delivering troubleshooting insights for misconfigured or failing builds.

Learn more: [](ai-assistant.md).


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

</snippet>
