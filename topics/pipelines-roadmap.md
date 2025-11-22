# TeamCity Pipelines Roadmap

<show-structure for="chapter" depth="2"/>

Pipelines reimagine the familiar TeamCity experience by combining its full capabilities with a more intuitive, user-centric interface. While they are powered by the same reliable backend, we have intentionally moved away from certain legacy approaches to address long-standing pain points, often redesigning even simple concepts from the ground up.

This approach will pay off as the initiative evolves, but it also means pipelines currently offer less customization and fewer features than classic build configurations. As we determine which areas to prioritize next, your feedback is especially valuable. Our goal is to build a CI/CD solution that truly fits your needs, and your input is essential in helping us shape it.

> The information contained within this article details our projected development plans. Please note that this information is being shared for informational purposes only and does not represent a binding commitment on the part of JetBrains. This roadmap and features listed within it are subject to change.
> 
{style="note"}


## Features In Development

This section shares features that are already in active development. We expect to deliver them in the nearest release cycles.

### Integration with build chains
<secondary-label ref="secondary-roadmap-planned-2026q1"/>

[Build chains](build-chain.md) allow you to link [build configurations](creating-and-editing-build-configurations.md) in a single workflow.

<img src="chains-minimap.png" width="706" alt="Build chains viewer" thumbnail="true"/>

We expect to integrate build chains in the pipeline experience in two ways:

* Provide the ability to link pipelines with build configurations. This will allow you to keep using existing building routines in tandem with lightweight pipelines.
* Support linking multiple pipelines in a single build chain.

### Kotlin DSL Support
<secondary-label ref="secondary-roadmap-planned-2026q1"/>

Both pipelines and build configurations support configuration-as-code, but they use different formats: pipelines use [YAML](pipelines-yaml-syntax.md), while build configurations rely on [](kotlin-dsl.md).

Each approach has its strengths. YAML is widely used and familiar, while Kotlin DSL offers the flexibility of a full programming language, including [extending standard types with custom functionality](https://blog.jetbrains.com/teamcity/2019/04/configuration-as-code-part-4-extending-the-teamcity-dsl/), [creating objects at runtime](https://blog.jetbrains.com/teamcity/2019/04/configuration-as-code-part-3-creating-build-configurations-dynamically/).

As we work toward making pipelines a complete solution for any CI/CD task, our goal is to give you the best of both worlds. Bringing Kotlin DSL to pipelines will make it easier to use versioned settings and choose the approach that best fits your workflow.

### Run Custom Build Support
<secondary-label ref="secondary-roadmap-planned-2026q1"/>

[Running custom builds](running-custom-build.md) is a great way to trigger a tailored build sequence without changing configuration settings. You can schedule a build, pick a specific agent, override parameters, skip dependencies, and more.

<img src="dk-customRun-general.png" alt="Run custom build" width="706"/>

We expect to support a similar functionality in pipelines.


### Job-level Build Features
<secondary-label ref="secondary-roadmap-planned-2026q1"/>

[Build features](adding-build-features.md) enhance build configurations with advanced capabilities: from simple cleanup with [Swabra](build-files-cleaner-swabra.md) to [Matrix builds](matrix-build.md) that spawn dozens of virtual builds cycling through the predefined set of parameters. Our plan is to bring the most widely used features to pipelines as native functionality rather than plug-in add-ons.

As with other configuration-only features, we aim to support what matters without cluttering pipelines with rarely used options. Your feedback is essential here: reach out via [Slack](https://jb.gg/TeamCitySlack) or our [issue tracker](troubleshooting.md) to help us prioritize the features that matter most.


### More Build Steps
<secondary-label ref="secondary-roadmap-planned-2026q1"/>

TeamCity 2025.11 introduces [.NET](net.md) build steps: one of many step types previously available only in classic build configurations.

<img src="dk-dotnet-pipelines.png" width="706" thumbnail="true" alt=".NET steps in pipelines"/>

More steps are on the way, but as with [build features](#Job-level+Build+Features), we want to focus on what users truly need. Our research shows that while many appreciate specialized steps, the universal [](command-line.md) step remains the most commonly used. To keep pipelines simple and approachable, we aim for quality over quantity and would greatly appreciate your feedback.

Let us know which steps you’d like to see next — [](python.md), [](powershell.md), [](xcode-project.md), or anything else — so we can prioritize them for future releases.


## Implemented Features

This section lists planned features that were implemented in previous versions.

### New Build Steps
<secondary-label ref="secondary-roadmap-implemented-202511"/>

In version 2025.11, we're bringing the familiar [](net.md) build step to pipelines. Instead of one single step with dozens of settings that depend on the selected step command, pipelines split this build step into a series of task-specific units.

See the [](#More+Build+Steps) section for more information on other steps currently available only in build configurations.

Learn more: [](net.md).

### Project Registry Connections Support
<secondary-label ref="secondary-roadmap-implemented-202511"/>

Starting with version 2025.11, [Docker](configuring-connections.md#Docker+Registry) and [NPM](configuring-connections.md#npm-registry-settings) connections owned by projects are available as [integrations](pipeline-settings.md#Integrations) in pipeline and job settings.

<img src="pipelines-inherited-registry-connections.png" width="706" alt="Inherited integrations"/>

Learn more: [](pipeline-settings.md).


### Advanced Build and Test Actions
<secondary-label ref="secondary-roadmap-implemented-202511"/>

Starting with version 2025.11, pipelines support some of advanced features that was previously available only in build configurations. Users can now process build and test failures: [assign investigations](investigating-and-muting-build-failures.md#Investigations), [mute irrelevant failures](investigating-and-muting-build-failures.md#Mutes), and manually label as fixed issues that are expected to be resolved in future builds.

<img src="dk-pipelines-investigations.png" width="706" alt="Investigations and mutes in pipelines"/>

In addition, the run actions menu now includes options to [pin, tag, and comment](build-actions.md) individual pipeline runs.

<img src="dk-build-actions-pipelines.png" width="706" alt="Pin, tag, and comment actions in pipelines"/>

Learn more: [](investigating-and-muting-build-failures.md), [](build-actions.md)


### Parameter Import
<secondary-label ref="secondary-roadmap-implemented-202511"/>

Previously, a parameter owned by a project could not be used inside pipelines. Referencing such parameters would result in an implicit agent requirement: only agents that provide a value for this parameter were eligible to run this pipeline.

Starting with version 2025.11, you can import any parameter from a direct or indirect project and use it as any other native pipeline parameter.

<img src="pipelines-import-params.png" width="706" alt="Import parameters"/>

Learn more: [Pipeline parameters](pipeline-settings.md#Parameters), [](configuring-build-parameters.md)





## Backlog

Below are the features we’re considering for future pipeline releases. Join our [Slack Workspace](https://jb.gg/TeamCitySlack) or contact us through our [usual support channels](troubleshooting.md) to help us identify the most important items and refine our priorities.

### Job Failure Conditions
<secondary-label ref="secondary-roadmap-planned-priority"/>

We plan to introduce failure conditions similar to [those in build configurations](build-failure-conditions.md). This will give you finer control over when a job is marked as failed and allow downstream jobs to run even if earlier ones fail.

### Execution Timeouts
<secondary-label ref="secondary-roadmap-planned-priority"/>

We’re exploring timeout settings that let you define maximum run durations. Jobs or pipelines that exceed the threshold would be automatically canceled and marked as failed.

### Recipes Support

[Recipes](working-with-meta-runner.md) complement custom build steps by letting you package commonly used logic into reusable assets and download community-created steps from JetBrains Marketplace. Adding recipe support would greatly expand what pipelines can do.

### Build Step Conditions

Classic build configurations support [step execution conditions](build-step-execution-conditions.md) that specify criteria for when a step should run. We plan to add a similar feature for steps inside pipeline jobs.

### Typed Parameters

Pipelines currently support only single-value text parameters (including masked [secret](pipeline-settings.md#Secrets) parameters for sensitive values). We aim to implement more parameter types [available in classic build configurations](typed-parameters.md), such as checkboxes, multi-selects, and values pulled from external sources.

### Templates

[Templates](build-configuration-template.md) help configure multiple build configurations that share similar settings. We plan to bring an equivalent concept to pipelines, enabling you to define reusable YAML templates.

### VCS YAML Recognition

You can already save pipeline settings to a repository. Next, we want TeamCity to do the opposite: detect pipeline YAML files in supported VCS hosts (GitHub, GitLab, Bitbucket, and so on) and automatically create pipelines from them.