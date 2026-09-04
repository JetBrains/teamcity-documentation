# TeamCity Pipelines Roadmap

<show-structure for="chapter" depth="2"/>

Pipelines reimagine the familiar TeamCity experience with a more intuitive visual interface.
While pipelines are powered by the same reliable backend, we have intentionally moved away from our traditional approaches, simplified core concepts and redesigned them from the ground up.

We expect this approach to pay off as the initiative evolves, but it also means pipelines currently offer less customization and fewer features than classic build configurations. As we determine which areas to prioritize next, your feedback is especially valuable. Our goal is to build a CI/CD solution that truly fits your needs, and your input is essential in helping us shape it.

[Join our Slack](https://jb.gg/TeamCitySlack) to share and discuss your ideas, or send bug reports to [Zendesk / YouTrack](troubleshooting.md).

> The information contained within this article details our projected development plans. Please note that this information is being shared for informational purposes only and does not represent a binding commitment on the part of JetBrains. This roadmap and features listed within it are subject to change.
> 
{style="note"}

## Planned features


### All build steps and features

We have implemented a mechanism that allows pipelines to reuse familiar build steps and features. At the moment, only a few of them are publicly available: [](net.md), [](build-files-cleaner-swabra.md), [](xml-report-processing.md), and a few others. We expect the majority of build features and steps available in pipelines by default in the 2026.3 update.

### Job failure conditions
<secondary-label ref="secondary-roadmap-planned-priority"/>

We plan to introduce failure conditions similar to [those in build configurations](build-failure-conditions.md). This will give you finer control over when a job is marked as failed and allow downstream jobs to run even if earlier ones fail.

### Execution timeouts
<secondary-label ref="secondary-roadmap-planned-priority"/>

We’re exploring timeout settings that let you define maximum run durations. Jobs or pipelines that exceed the threshold would be automatically canceled and marked as failed.

### PR event triggers

Pipelines can already identify pull requests and [automatically build their changes](pipeline-settings.md#Repository). We hope to introduce a more sophisticated PR handling by adding dedicated event triggers.

### GitHub Checks support

Pipelines are capable of reporting basic run statuses (started, failed, successful, and more [to the VCS](pipeline-settings.md#Repository). GitHub Checks API support would greatly extend that functionality, as it allows external systems to report detailed feedback, specific line-by-line annotations, and re-run individual tasks.

### Recipes support

[Recipes](working-with-meta-runner.md) complement custom build steps by letting you package commonly used logic into reusable assets and download community-created steps from JetBrains Marketplace. Adding recipe support would greatly expand what pipelines can do.

### Build step conditions

Classic build configurations support [step execution conditions](build-step-execution-conditions.md) that specify criteria for when a step should run. We plan to add a similar feature for steps inside pipeline jobs.

### Typed parameters

Pipelines currently support only single-value text parameters (including masked [secret](pipeline-settings.md#Secrets) parameters for sensitive values). We aim to implement more parameter types [available in classic build configurations](typed-parameters.md), such as checkboxes, multi-selects, and values pulled from external sources.

### Templates

[Templates](build-configuration-template.md) help configure multiple build configurations that share similar settings. We plan to bring an equivalent concept to pipelines, enabling you to define reusable YAML templates.


<!--
## Features in development

This section shares features that are already in active development. We expect to deliver them in the nearest release cycles.

### More build steps and build features
<secondary-label ref="secondary-roadmap-planned-2026q3"/>

With support for [.NET](net.md) build steps in version 2025.11 and [four job-level build features](job-settings.md#Build+Features) in 2026.1, pipelines have closed one of the larger functionality gaps with classic build configurations.

<img src="pipelines-build-features.png" width="706" alt="Build features in pipelines"/>

We plan to add more steps and features based on your feedback. If you need the [Python step](python.md), the [Build approval feature](build-approval.md), or anything else, [let us know](slack-code-of-conduct.md) so we can prioritize accordingly.


### Checkout rules for partial checkout

Sometimes a job does not need the entire repository to do its job (pun very much intended). This is especially true for large monorepositories, where checkout can add a serious delay to your workflows.

To help with this, we plan to support [checkout rules](vcs-checkout-rules.md), which are already available in build configurations. The value this feature brings to the table is especially meaningful in TeamCity, which is smart enough to ignore commits to files excluded from checkout. This means more [build reuses](job-settings.md#Optimizations) and less manual tinkering to keep your runs fast.

-->

## Implemented features

This section lists planned features that were implemented in previous versions.


### Debug Jobs
<secondary-label ref="secondary-roadmap-implemented-20262"/>

You can now test a single pipeline job without triggering the whole pipeline. Open a job's ellipsis menu and choose **Debug** — TeamCity runs just that job (and anything it depends on) with its current settings, complete with a live build log and terminal access to the agent that picked it up.

<img src="debug-jobs.png" width="706" alt="Debug jobs"/>

Learn more: [](create-and-edit-pipelines.md#Debug+Jobs)

### Improved Branch Handling
<secondary-label ref="secondary-roadmap-implemented-20262"/>

Pipelines are now better equipped to work with feature branches: in edit mode, you can switch between different repository branches and design unique workflows for each of them.

<img src="pipelines-edit-mode-branch-selector.png" width="706" alt="Branch selector in edit mode"/>

In addition, TeamCity now handles protected branches correctly. An attempt to commit edits made in the UI to a protected branch now results in a clear warning, and a suggestion to save the updated .yml file to another branch.

<img src="pipelines-save-yaml-branch-selection.png" width="705" alt="Save settings to a protected branch"/>

Learn more: [](pipeline-settings.md#Feature+Branches)


### Run Through Failures
<secondary-label ref="secondary-roadmap-implemented-20262"/>

When setting up [job dependencies](job-settings.md#Dependencies), you can now enable **Run job even if upstream fails** so a job keeps running even when the upstream job it depends on fails, instead of being automatically canceled.

<!-- TODO: screenshot of the dependency properties panel -->

The pipeline run is still marked as failed overall, but this lets you guarantee that specific jobs (for example, cleanup or notification steps) always run.

Learn more: [](job-settings.md#Dependencies)


### Promote pipeline runs
<secondary-label ref="secondary-roadmap-implemented-20262"/>

**Promote** — the button that triggers the downstream part of a [build chain](build-chain.md) from an older, already-finished build — now works for pipelines, not just build configurations.

<img src="promote-pipeline-run.png" width="706" alt="Promote pipeline run"/>

For example, promote a successful "Build Docker image" run into the "Upload to DockerHub" configuration or pipeline to re-deploy that artifact without rebuilding it.

Learn more: [](run-build-chains.md#Promote+a+build)


### AI enhancements
<secondary-label ref="secondary-roadmap-implemented-20262"/>

TeamCity MCP endpoint now allows your AI agents to retrieve three additional tools for managing pipelines: `teamcity_pipeline_get`, `teamcity_pipeline_post`, and `teamcity_pipeline_delete`. In addition, you can now use [AI Assistant](ai-assistant.md) for help with configuring and debugging pipelines.

Learn more: [](ai-agent-integration.md)


### Integration with build chains
<secondary-label ref="secondary-roadmap-implemented-20261"/>

[Build chains](build-chain.md) can now consist of both build configurations and pipelines.

<img src="dk-pipeline-dependency.png" width="706" alt="Pipeline dependency"/>

When configuring pipeline dependencies, you have the same familiar options as for build configuration [snapshot dependencies](configuring-dependencies.md): revision synchronization mode, execution policy for failed dependencies, and more.

Learn more: [](pipeline-settings.md#Pipeline+Dependencies).


### Kotlin DSL support
<secondary-label ref="secondary-roadmap-implemented-20261"/>

If a parent project [stores its settings in Kotlin DSL](kotlin-dsl.md), you now have the option to continue storing pipeline settings in a remote YAML, or include its settings in Kotlin format to the project's `.kts` file.

<img src="pipelines-dsl.png" width="706" alt="DSL in pipelines"/>

This change benefits teams that prefer Kotlin DSL for their configuration-as-code workflows, and do not wish to have "gaps" in their configuration files.

Learn more: [](pipelines-dsl.md).


### Custom runs
<secondary-label ref="secondary-roadmap-implemented-20261"/>

[Running custom builds](running-custom-build.md) is a great way to trigger a tailored build sequence without changing configuration settings. You can schedule a build, pick a specific agent, override parameters, skip dependencies, and more. Starting from version 2026.1, this functionality is available for both classic build configurations and pipelines.

<img src="pipelines-run-custom-build.png" width="706" alt="Run build buttons in TeamCity"/>

Learn more: [](running-custom-build.md).


### Job-level build features
<secondary-label ref="secondary-roadmap-implemented-20261"/>

Version 2026.1 adds support for [build features](adding-build-features.md) that were previously available only in build configurations. In pipelines, you can now add these features to jobs just as you add build steps.

<img src="pipelines-build-features.png" width="706" alt="Build features in pipelines"/>

At the moment, pipelines support four such features, not including natively integrated ones like the [commit status publisher](create-and-edit-pipelines.md#Publish+Run+Statuses+to+VCS):

* [](build-files-cleaner-swabra.md)
* [](free-disk-space.md)
* [](build-cache.md)
* [](xml-report-processing.md)

We expect to support more features based on your feedback.

Learn more: [](job-settings.md#Build+Features).


### .NET build steps
<secondary-label ref="secondary-roadmap-implemented-202511"/>

In version 2025.11, we're bringing the familiar [](net.md) build step to pipelines. Instead of one single step with dozens of settings that depend on the selected step command, pipelines split this build step into a series of task-specific units.

Learn more: [](net.md).

### Project registry connections support
<secondary-label ref="secondary-roadmap-implemented-202511"/>

Starting with version 2025.11, [Docker](configuring-connections.md#Docker+Registry) and [NPM](configuring-connections.md#npm-registry-settings) connections owned by projects are available as [integrations](pipeline-settings.md#Integrations) in pipeline and job settings.

<img src="pipelines-inherited-registry-connections.png" width="706" alt="Inherited integrations"/>

Learn more: [](pipeline-settings.md).


### Advanced build and test actions
<secondary-label ref="secondary-roadmap-implemented-202511"/>

Starting with version 2025.11, pipelines support some of advanced features that was previously available only in build configurations. Users can now process build and test failures: [assign investigations](investigating-and-muting-build-failures.md#Investigations), [mute irrelevant failures](investigating-and-muting-build-failures.md#Mutes), and manually label as fixed issues that are expected to be resolved in future builds.

<img src="dk-pipelines-investigations.png" width="706" alt="Investigations and mutes in pipelines"/>

In addition, the run actions menu now includes options to [pin, tag, and comment](build-actions.md) individual pipeline runs.

<img src="dk-build-actions-pipelines.png" width="706" alt="Pin, tag, and comment actions in pipelines"/>

Learn more: [](investigating-and-muting-build-failures.md), [](build-actions.md)


### Parameter import
<secondary-label ref="secondary-roadmap-implemented-202511"/>

> Starting with version 2026.1, you no longer need to manually import project-level parameters as they are instantly available in pipelines and their jobs.

Previously, a parameter owned by a project could not be used inside pipelines. Referencing such parameters would result in an implicit agent requirement: only agents that provide a value for this parameter were eligible to run this pipeline.

Starting with version 2025.11, you can import any parameter from a direct or indirect project and use it as any other native pipeline parameter.

<img src="pipelines-import-params.png" width="706" alt="Import parameters"/>

Learn more: [Pipeline parameters](pipeline-settings.md#Parameters), [](configuring-build-parameters.md)






