# Main Pipeline Settings

This article explains settings available for the entire pipeline that specify common pipeline behavior.


## Edit Pipeline Settings

To view and edit core pipelines settings, click the [Settings toggle](project-administrator-guide.md#Edit+and+View+Modes) in the top right corner, then click anywhere within the pipeline canvas area surrounding the jobs in the visual editor.

<img src="pipelines-open-pipeline-settings.png" width="706" alt="Open pipeline settings"/>

You can also switch the editor to the configuration-as-code mode, and edit the markup directly.


## Parameters

Parameters are name-value pairs designed to substitute raw values with references. When TeamCity encounters a parameter reference (`%\param-name%`), it substitutes it with the actual parameter value.

TeamCity supports two layers of parameters: pipeline parameters and job parameters.

* **Pipeline parameters** are designed to be accessible from any individual job owned by this pipeline.

    ```yaml
    jobs:
      Job1:
        name: Job 1
        steps:
          - type: script
            script-content: echo %pipeline-param1%
      Job2:
        name: Job 2
        dependencies:
          - Job1
        steps:
          - type: script
            script-content: echo %pipeline-param2%
    parameters:
      pipeline-param1: foo
      pipeline-param2: bar
    ```

    Typically, these are [configuration parameters](configuring-build-parameters.md) (without the `env.` name prefix) most commonly used to store values used by multiple jobs, or quickly alter global pipeline settings.

* **Job parameters** or **input parameters** are typically environment variables (with the `env.` name prefix) available only for this specific job. To pass this value to another job, you need to reference them inside an output parameter. See [](create-and-edit-pipelines.md#Parameters) and [](job-settings.md) for more information.

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


<def title="YAML storage">

Specifies where to store the pipeline YAML configuration: on the TeamCity server or in the source repository. See also: [](storing-project-settings-in-version-control.md).

These settings are available for main repository only.

</def>


<def title="Publish status to repository">

If this setting is enabled, TeamCity reports pipeline run statuses (running, successful, failed) to the VCS hosting provider. The following image illustrates how GitHub presents this information.

<img src="pipelines-csp.png" width="706" alt="Run statuses in GitHub"/>

For classic build configurations, this functionality is available as the [](commit-status-publisher.md) build feature.

</def>

</deflist>



## Integrations

Set up authorization for NPM or Docker registries here. After adding the required integration, open the job that needs repository access, scroll to its **Integration** section, and ensure the toggle for the required entry is enabled.

Classic TeamCity build configurations support this functionality via the "connection + build feature" combinations:

* [Docker Registry](configuring-connections.md#Docker+Registry) connection and [](docker-support.md) build feature for Docker.
* [NPM Registry](configuring-connections.md#npm-registry-settings) connection and the [related build feature](nodejs.md#Accessing+Private+NPM+Registries) for Node.js registries. 