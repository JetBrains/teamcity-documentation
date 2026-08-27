[//]: # (title: Pipeline Settings)
[//]: # (help-id: Pipeline Settings)

<show-structure for="chapter" depth="2"/>


This article explains settings available for the entire pipeline that specify common pipeline behavior.


## Edit Pipeline Settings

To view and edit core pipelines settings, click the [Settings toggle](project-administrator-guide.md#Edit+and+View+Modes) in the top right corner, then click anywhere within the pipeline canvas area surrounding the jobs in the visual editor.

<img src="pipelines-open-pipeline-settings.png" width="706" alt="Open pipeline settings"/>

You can also switch from the visual editor to the code and edit the markup directly.


## Parameters

<snippet id="pipeline-parameters-common">

Parameters are name-value pairs designed to substitute raw values with references. When TeamCity encounters a parameter reference (`%\param-name%`), it substitutes it with the actual parameter value.

TeamCity supports two layers of parameters: pipeline parameters and job parameters. Pipeline parameters in their turn are available as input and output parameters.

</snippet>



* **Job parameters** are designed to be used in these very jobs, and jobs that depend on it (via the `job.<source-job-ID>.<param-name>` syntax). See [Job parameters](job-settings.md#Parameters) to learn more.


* **Pipeline input parameters** are shared across all jobs within a pipeline. The sample below illustrates a pipeline parameter propagated to two jobs.

    ```yaml
    parameters:
      PipelineParam: foo
    jobs:
      Job1:
        name: Job 1
        steps:
          - type: script
            script-content: 'echo "Pipeline parameter: %PipelineParam%"'    # prints 'foo'
      Job2:
        name: Job 2
        dependencies:
          - Job1
        parameters:
          env.Job2Param: '%PipelineParam% bar'    # job parameter references pipeline input param
        steps:
          - type: script
            script-content: |-
              echo "Original pipeline parameter: %PipelineParam%"    # prints 'foo'
              echo "Modified pipeline parameter: %env.Job2Param%"    # prints 'foo bar'
    ```


* **Pipeline output parameters** are shared to downstream pipelines and build configurations when this pipeline is a part of a [build chain](pipeline-settings.md#Pipeline+Dependencies). Having this separate type grants you more control over which parameters can be exposed and which should stay private.

    Note that output parameters cannot be used inside the same pipeline.

    <snippet id="output-param-in-self">

    ```yaml
    parameters:
      PipelineInputParam: foo
    output-parameters:
      PipelineOutputParam: bar
    jobs:
      Job1:
        name: Job 1
        steps:
          - type: script
            script-content: |-
                # Prints 'foo'
              echo "Input param: %PipelineInputParam%"
                # Unresolved reference: no compatible agents 
              echo "Output param: %PipelineOutputParam%"
    ```
  
    </snippet>

    See this section to learn more about parameters in build chains: [](pipeline-settings.md#Pipeline+Dependencies).


<snippet id="pipeline-param-missing">

> If a job uses a parameter that is not defined on either project, pipeline or job level, this parameter becomes an [agent requirement](job-settings.md#Agent+Requirements) (see [example](job-settings.md#pipeline-implicit-requirement)). These automatically generated requirements are also called [implicit](configuring-agent-requirements.md#Implicit+Requirements), as opposed to user-defined [explicit](configuring-agent-requirements.md#Explicit+Requirements) ones.
> 
{style="note"}

</snippet>


## Secrets

Secrets are protected parameters designed for storing sensitive data. You can reference in the same manner as with regular parameters, but their actual values are hidden from both TeamCity UI and build log.

If you switch to the code view of a pipeline, you will notice that secret values are replaced by names of [tokens](storing-project-settings-in-version-control.md#Managing+Tokens) that store these values. TeamCity automatically creates these tokens to avoid leaking secret data via remotely stored configuration files.

The snippet below illustrates a secret that can be used instead of a password in the [](#Integrations) section of a pipeline. The command-line script that attempts to expose this secret prints the "Secret value: *******" line to the build log.

```yaml
jobs:
  Job1:
    name: Job 1
    steps:
      - type: script
        script-content: |-
          echo "Secret value: %registry-password%"
secrets:
  registry-password: credentialsJSON:c57c2732-1d8c-4c11-8724-f275085f4320
```

## Pipeline Dependencies

This section allows you to link pipelines and build configurations in a single [build chain](build-chain.md).

<img src="dk-pipeline-dependency.png" width="706" alt="Pipeline dependency"/>

When you add a dependency to object "A" in pipeline "B" settings, you create the "A &rarr; B" chain where:

* object A can run solo;
* pipeline B automatically triggers object A when launched.

"A" can be both a classic build configuration or another pipeline.

> If you need to set up the "upstream pipeline &rarr; downstream build configuration" relation, add a [snapshot dependency](configuring-dependencies.md) in this configuration's settings.
> 
{style="tip"}

Pipeline dependencies have the following settings:

<img src="dk-pipeline-dependency-settings.png" width="706" alt="Pipeline dependency settings"/>

<deflist type="full">

<def title="Depend on">

Choose an upstream configuration or pipeline that should finish before your currently edited pipeline can start.

</def>


<def title="Enforce revisions synchronization">

<include from="configuring-dependencies.md" element-id="enforce-rev-sync-description"/>

</def>


<def title="Do not run new build if there is a suitable one">

<include from="configuring-dependencies.md" element-id="do-not-run-new-build-if-there-is-a-suitable-one-description"/>

</def>


<def title="Only use successful builds from suitable ones">

<include from="configuring-dependencies.md" element-id="reuse-only-successful"/>

</def>


<def title="On failed dependency, On failed to start/canceled dependency">

<include from="configuring-dependencies.md" element-id="on-failed-dependency-description"/>

</def>


</deflist>

The snippets below illustrate how to set up dependencies in code.

<tabs>

<tab title="YAML">

<code-block lang="yaml">
jobs:
  ...
dependencies:
- ReverseAndOverrideDep_JobParams_UpstreamPipeline:
  reuse: none
</code-block>

</tab>

<tab title="Kotlin DSL">

<code-block lang="Kotlin">
import jetbrains.buildServer.configs.kotlin.*
import jetbrains.buildServer.configs.kotlin.pipelines.*

object DownsteamPipeline : Pipeline({
    name = "Downsteam pipeline"
        dependencies {
            snapshot(UpstreamPipeline) {
                reuseBuilds = ReuseBuilds.NO
            }
        }
})
</code-block>
</tab>

</tabs>


## Auto-Run Pipeline

This section includes settings that allow TeamCity to automatically run your pipeline on certain conditions. This functionality is available as [triggers](configuring-build-triggers.md).

### On New Changes

These settings trigger new pipeline runs whenever TeamCity detects new changes in a repository. In classic TeamCity build configurations, similar functionality is available via [VCS triggers](configuring-vcs-triggers.md).

> TeamCity supports two modes for detecting changes. 
> 
> * Webhook mode. For pipelines created through a configured Git connection, TeamCity automatically [registers a webhook](create-and-edit-pipelines.md#Webhooks) so that the VCS can instantly notify TeamCity of new commits
> 
> * Polling mode. For pipelines created without a Git connection, or when a webhook fails, TeamCity checks for new changes every 60 seconds.
> 
{style="tip"}

Trigger settings define which changes should start a new run. Use the **Branches** toggle to select stable repository branches. In the example below, TeamCity runs the pipeline automatically only when changes are committed to the "production" branch.

<img src="pipelines-auto-run-trigger.png" width="706" alt="pipelines auto-run trigger"/>

The **Pull requests** toggle includes pull request branches (for example, GitHub `refs/pull/` branches) to the list of available sources. Note that you can enable this option only when a pipeline tracks these pull request branches (see the [](#Repository) section).

> Use caution when enabling the **Pull requests** toggle for pipelines targeting public repositories, as automatically running unverified code from external contributors can pose a security risk.
> 
{style="note"}

### On a Schedule

These settings, similar to a classic build configuration’s [schedule trigger](configuring-schedule-triggers.md), let you define a date-time pattern for when TeamCity should run the pipeline.

<img src="pipelines-schedule-trigger.png" width="706" alt="pipelines schedule trigger"/>

## Repository

The **Repository** section lets you check out multiple repositories during a pipeline run. TeamCity retrieves sources from all added repositories, even if no jobs are configured to process them.

<img src="pipelines-repositories.png" width="706" alt="Pipeline repos"/>

The initial repository entry, created automatically with the pipeline, is called the **main repository**. It cannot be deleted and includes an extra YAML file storage selector.

<img src="pipelines-main-repo-settings.png" width="706" alt="Individual pipeline repository settings"/>

<deflist type="full">

<def title="Repository URL and source">

Core repository settings that allow you to choose which repository a pipeline should check out.

Disabled for main repositories.

</def>

<def title="Default branch and branch specification">

These settings define which branches TeamCity tracks. Untracked branches are completely ignored: they do not report changes to the server, you cannot run for these branches, and so on.

See the following classic build configuration articles to learn more about branch specification syntax: [](working-with-feature-branches.md#Common+Specification+Syntax) and [](working-with-feature-branches.md#Default+Branch). 

</def>


<def title="Pull requests">

When this setting is enabled, TeamCity includes pull (merge) request branches in the branch selector on the main pipeline page.

<img src="pipelines-pull-request-branch-selector.png" width="706" alt="Pull request branch in branch selector"/>

You can also enable the related toggle of the ["On new changes" trigger](#On+New+Changes) to have TeamCity automatically build incoming pull requests.

</def>


<def title="Configuration file storage" id="config-file-storage">

Specifies where to store the pipeline YAML configuration: on the TeamCity server or in the source repository. These settings are available for main repository only.

The configuration file itself can be in two formats: YAML (default) and Kotlin DSL (see [](pipelines-dsl.md)). Depending on the parent project's [Versioned Settings](storing-project-settings-in-version-control.md) and this in repository/on the server toggle, the behavior and available options may differ.

For example, if the project's versioned settings synchronization is off, its pipelines store its setting in YAML regardless of the location. Otherwise, when you turn project synchronization on, pipeline settings can be in either of these formats: existing pipelines that already store their YAML settings in remote repositories will continue to do so, while new pipelines and those that store their settings on the server will convert their YAML to .kts files.

> If you choose to save YAML configuration in a remote repository, you can use the branch selector in edit mode to design different workflows for different branches. See [](#Feature+Branches) to learn more.


</def>


<def title="Publish status to repository">

If this setting is enabled, TeamCity reports pipeline run statuses (running, successful, failed) to the VCS hosting provider. The following image illustrates how GitHub presents this information.

<img src="pipelines-csp.png" width="706" alt="Run statuses in GitHub"/>

For classic build configurations, this functionality is available as the [](commit-status-publisher.md) build feature.

</def>

</deflist>

When adding more repositories, you can choose to reuse an existing connection or VCS root, or enter the repository URL manually.

<img src="pipelines-add-repo-from-root.png" width="706" alt="Add repo from root"/>

Each individual job can choose which of the pipeline repositories it will check out. See the following article for more information: [Job Settings](job-settings.md#Repository).

## Feature Branches

If a pipeline [stores its YAML configuration in a repository](#config-file-storage), you can give individual branches their own workflow:

1. In the settings editor, switch to a branch using the branch selector.
2. Edit jobs, steps, parameters, or any other setting stored in YAML.
3. Click **Save**. TeamCity commits the changes to that branch's YAML file.

<img src="pipelines-edit-mode-branch-selector.png" width="706" alt="Branch selector in edit mode"/>

Branches without their own committed settings inherit the workflow defined in the default branch's YAML file.

> A handful of settings live outside YAML and therefore apply to every branch the same way:
>
> * Pipeline name
> * Repository settings (repository URL, branch specs, build status publishing, pull request settings, and more)
> * Auto-run settings
> * Integrations
>
{style="note"}

### Protected Branches

If the selected branch is protected, TeamCity may be unable to commit your changes directly. In this case, clicking **Save** prompts you to choose a different branch to save to instead.

<img src="pipelines-save-yaml-branch-selection.png" width="705" alt="Save settings to a protected branch"/>

Alternatively, switch to the [code view](#Edit+Pipeline+Settings) of the settings editor, copy the generated YAML, and commit it to the protected branch manually, following your team's branch protection rules.

## Integrations

<snippet id="pipeline-job-integrations">

Both pipeline and job settings panels include an **Integrations** section for connecting to private Docker and NPM registries.

* In the pipeline settings, you manage the full list of available integrations for jobs.
* In job settings, toggles let you select which registries the job should log in to automatically, ensuring build steps can access the required data.

</snippet>

### YAML

Integrations added directly to the pipeline do now expose their settings to YAML. The only part of integrations visible in YAML is the list of integration IDs enabled for each specific job.

```YAML
jobs:
  Job1:
    name: Job 1
    steps:
      - type: script
        name: Run in ECR container
        script-content: |-
          echo "Successfully authenticated and running inside ECR image"
        docker-image: 12345.dkr.ecr.eu-west-1.amazonaws.com/johndoe/my-image:latest
    integrations:
      - AmazonDocker: PROJECT_EXT_112
      - Docker: PROJECT_EXT_30
```

### Inherited Integrations

If you already have a Docker or NPM connection in a project, a pipeline shows it under its "Integrations" section.

<img src="pipelines-inherited-registry-connections.png" width="706" alt="Inherited integrations"/>

These inherited integrations cannot be edited directly via the pipelines settings panel, you need to modify the origin connection in project settings to do so.


### Amazon ECR

Currently, pipelines and jobs support only [inherited](#Inherited+Integrations) Amazon ECR connections — you cannot add them via the pipeline settings sidebar or YAML.

All [Amazon ECR](configuring-connections.md#Amazon+ECR) connections owned by a parent project are displayed under the **Integrations** sections. Individual jobs show toggles that specify whether this job should use this specific connection.

<img src="pipelines-ecr.png" width="706" alt="ECR connections in pipelines" thumbnail="true"/>




### In Build Configurations

Classic TeamCity build configurations support this functionality via the "connection + build feature" combinations:

* The [](docker-support.md) build feature uses [Docker Registry](configuring-connections.md#Docker+Registry) and [Amazon ECR](configuring-connections.md#Amazon+ECR) connections.
* The [NPM Registry](nodejs.md#Accessing+Private+NPM+Registries) build feature uses [corresponding connections](configuring-connections.md#npm-registry-settings).


