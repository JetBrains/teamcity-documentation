[//]: # (title: Create and Edit Pipelines)
[//]: # (help-id: Create and Edit Pipelines)

Pipelines are user-centric, simplified alternatives for traditional [build configurations](creating-and-editing-build-configurations.md) and [build chains](build-chain.md) available in TeamCity 2025.07 and newer. While still in early access and not recommended for complex setups due to potential issues, they are not marked as experimental and can be safely used in production for smaller, less demanding projects.


> For TeamCity Professional servers, pipelines have a separate limit of 10 per server and do not count toward the build configuration limit.
> 
{style="tip"}


## Pipelines vs Build Configurations and Chains

Before we dive into creating pipelines, it’s important to understand the differences between build configurations and pipelines and when to use each. Note that once created, you cannot convert a pipeline (or any of its jobs) into a build configuration, and vice versa. See the [](#Limitations+and+Special+Notes) section for more information.

<include from="creating-and-editing-build-configurations.md" xmlns="" element-id="configurations-vs-pipelines"/>


## Create and Set Up Pipelines

To add a pipeline to a project, use the **+** button in the project header or TeamCity sidebar.

<img src="dk-create-button-project.png" width="706" alt="Create button in project header"/>

TeamCity will ask you to choose a remote repository that will be processed by this pipeline: you can choose an existing [Git VCS root](git.md), a supported [connection to a VCS hosting](configuring-connections.md), or enter a Git URL manually. Default branch and branch specifications follow the same rules as regular build configurations, see [](working-with-feature-branches.md#Default+Branch) and [](working-with-feature-branches.md#Common+Specification+Syntax) for more information. Note that if you select an existing root, branch settings will be disabled — modify root settings to edit them.

<img src="dk-pipeline-branch-spec.png" width="706" alt="Pipeline branch specs"/>

Every new pipeline has an empty job. You can click a job tile to view its settings, or click a corresponding area to add new jobs.

<img src="dk-main-pipeline-view.png" width="706" alt="Main pipeline view"/>

To view global pipeline settings, click a highlighted area in the visual editor.

<img src="dk-pipeline-settings.png" width="706" alt="Pipeline settings"/>

To arrange pipeline jobs in the required order, select a job and check its upstream jobs in the **Dependencies** section. You can also use the visual editor to drag and drop a line from one job side to another.

<img src="dk-pipeline-dependencies.png" width="706" alt="Create job dependencies"/>

See [](pipeline-settings.md) and [](job-settings.md) for more information on basic operations.


## Job Outputs

Pipelines aim to streamline the user experience and present TeamCity concepts in a more accessible way. A key example is how they handle build results shared with end users and downstream jobs.

In classic TeamCity, files produced during a build and the parameters calculated along the way are mostly treated separately.

* Files are published as artifacts using paths [defined in the build configuration settings](configuring-general-settings.md#Artifact+Paths). These artifacts appear on the [](build-results-page.md) page and can be downloaded by users or passed to other configurations via manually configured [artifact dependencies](artifact-dependencies.md).
* Parameters [can be accessed by external configurations](use-parameters-in-build-chains.md) using the `dep.<source_config_ID>.<parameter_name>` syntax. In earlier versions, project administrators had no clear way to see which external configurations relied on their parameters, making it easy to unintentionally break CI routines used by other teams. Starting with TeamCity 2025.03, reusable parameters are now defined explicitly as [output parameters](use-parameters-in-build-chains.md#Input+and+output+parameters), separate from standard "input" parameters intended for use only within their parent project.


Pipelines offer greater transparency by letting you manage both artifacts and parameters in a single, centralized interface. Click a job to open its settings, and expand the **Job Outputs** section. From here, you can mark any parameter or a file as an output.



### Parameters

Parameters defined in the **Job Outputs** settings are same [output parameters](use-parameters-in-build-chains.md) supported in build configurations. For example, the following markup demonstrates a job with three parameters:

* `FORMAT` is used inside its parent job script to set the `DATE` parameter. Neither of them are shared.
* `DateOutput` is an output parameter that exposes the `DATE` parameter.

```yaml
Job1:
  name: Calculate date
  parameters:
    env.FORMAT: '%%-d %%B, %%Y'
    env.DATE: 'null'
  steps:
    - type: script
      script-content: |-
        DF=$(date +"$FORMAT")
        echo "Date is $DF"
        echo "##teamcity[setParameter name='env.DATE' value='$DF']"
  output-parameters:
    DateOutput: '%env.DATE%'
```
{ignore-vars="true"}

Now any downstream job can use the `DateOutput` value via the `job.<source_job_ID>.<parameter_name>` syntax.

```yaml
 Job2:
   name: Print date
   dependencies:
     - Job1
   steps:
     - type: script
       script-content: 'echo "Today''s date is: %job.Job1.DateOutput%"'
```
{ignore-vars="true"}

### Shared Files

Jobs can share files produced during a run. These files can be shared with either downstream jobs or TeamCity users (or both).

* Files shared with downstream jobs. In this case, jobs that follow the current one can import shared files and use them in their own scripts.

    The sample below illustrates a three-job pipeline whose jobs work with a file created and shared by the first job. 

    ```yaml
    jobs:
      Job1:
        name: Create file
        steps:
          - type: script
            script-content: touch output.txt
        files-publication:
          - path: output.txt
            share-with-jobs: true
            publish-artifact: false
      Job2:
        name: Modify file
        dependencies:
          - Job1:
              files:
                - output.txt
        steps:
          - type: script
            script-content: 'echo "Modified by Job #2" >> output.txt'
      Job1_2:
        name: Print file
        dependencies:
          - Job2
        steps:
          - type: script
            script-content: cat output.txt
    ```
  
    Shared files are displayed as [hidden artifacts](build-artifact.md#Hidden+Artifacts) in the ".shared_files.zip" archive.

* Files shared with users (artifacts). These are identical to [build artifacts](build-artifact.md) produced by classic build configurations. TeamCity shows artifacts in the **Artifacts** tab of a run results page.

To choose who a job should share a file with, tick related checkboxes under the job's **Outputs** section entry.

<img src="dk-pipelines-sharedFiles.png" width="706" alt="Shared files"/>



## Dependencies

Another classic TeamCity concept reworked in Pipelines is dependencies. In Pipelines, [snapshot](snapshot-dependencies.md) and [artifact](artifact-dependencies.md) dependencies are merged in a single option. Click a job to view its settings, choose which jobs should precede it, and decide whether you want this job to import their outputs.

<img src="dk-choose-dependency.png" width="706" alt="Choose import type"/>

You can also choose whether to import files by clicking a dependency line in the visual editor.




## Webhooks

Classic TeamCity supports two methods for detecting repository changes: periodic polling and webhooks. While webhooks offer near-instant updates and lower server load, they require manual setup. For more information, see the [](project-administrator-guide.md#Collecting+Changes) section.

Pipelines use webhooks by default as a faster and more efficient alternative to the polling mechanism. When you create a pipeline from a connection, TeamCity automatically registers a webhook in your repository settings. Polling remains as a fallback if the webhook is removed or fails to deliver updates.

> TeamCity Pipelines creates webhooks only for pipelines created via connections. Using existing VCS roots and the "Any Git URL" option is not supported.
> 
{style="warning"}


## Publish Run Statuses to VCS

[](commit-status-publisher.md) is one of the most popular TeamCity build features that communicates build statuses back to the VCS side. If you create a pipeline via a connection, this integration is available automatically.

<img src="dk-pipelines-csp.png" width="706" alt="Create Pipeline with CSP"/>

The figure below illustrates TeamCity run statuses reported to the repository page on GitHub.

<img src="dk-pipelines-csp-github.png" width="706" alt="TeamCity run statuses on GitHub"/>

You can click the status icon to open the detailed description. The **Details** link leads to the corresponding pipeline run on the TeamCity server.

<img src="dk-pipelines-csp-statuses.png" width="706" alt="Detailed run info"/>

To disable this integration, click a pipeline to edit corresponding repository settings and toggle **Publish status to repository** off.

<img src="dk-edit-pipeline-repository.png" width="706" alt="Edit repository settings"/>


> This integration is available only for pipelines created via connections. Using existing VCS roots and the "Any Git URL" option is not supported.
>
{style="warning"}


## Limitations and Special Notes

TeamCity Pipelines are in early access and, while built on core TeamCity functionality, currently lack some features available in classic build configurations. We plan to expand the Pipelines toolset and add the most requested features in future releases.


<deflist type="full">

<def title="Build steps">

TeamCity pipeline jobs support three dedicated build steps for [](maven.md), [](gradle.md), and [](nodejs.md).

While other step types from classic build configurations are not yet supported, the **Script** step (equivalent to [](command-line.md) in classic TeamCity) offers a flexible alternative. For example, instead of using a [](net.md) build step, you can add a Script step to run the `dotnet build` command.

</def>

<def title="Connections">

TeamCity Pipelines currently support [GitHub OAuth](configuring-connections.md#github-oauth), [GitLab](configuring-connections.md#GitLab), and [](configuring-connections.md#Bitbucket+Cloud) connections.

Note that you do not need configured connections to create pipelines, you can do so [from any Git repository URL](#Create+and+Set+Up+Pipelines).

</def>


<def title="VCS roots">

Pipelines use VCS roots internally but present a simplified **Repositories** section instead of exposing VCS root settings directly. As a result, options like clean and checkout policies, custom polling intervals, and submodule handling are not configurable through the pipelines UI.

However, you can still create and configure a VCS root in the classic TeamCity UI, then create a pipeline from this root.

</def>


<def title="Build features">

Pipelines are designed to offer the simplest, most user-friendly way to set up CI/CD routines. To support this goal, we are working on integrating key functionality directly into pipelines, avoiding the need for separately configured  [build features](adding-build-features.md).

For example, the [](commit-status-publisher.md) is [enabled automatically](#Publish+Run+Statuses+to+VCS) is enabled automatically when using connections, and registry connections are managed as **Integrations** within pipeline and job settings (rather than through separate [](docker-support.md) and [NPM Registry Connection](nodejs.md#Accessing+Private+NPM+Registries) build features).

<img src="dk-pipelines-add-integration.png" width="706" alt="Add pipeline integrations"/>

While traditional build features are not supported in pipelines, we are committed to bringing the most commonly used capabilities into the pipeline experience through more streamlined alternatives.

</def>


<def title="Triggers">

TeamCity Pipelines currently support two types of [triggers](configuring-build-triggers.md) that allow CI routines to start automatically:

* [Schedule trigger](configuring-schedule-triggers.md) that starts new runs on the given date and time.
* [VCS trigger](configuring-vcs-triggers.md) that starts runs on new code changes.

Both are configured in the **Auto-Run Pipeline** section of pipeline settings.

<img src="dk-pipelines-configure-triggers.png" width="706" alt="Configure triggers in pipelines"/>

Other trigger types (for example, the [Finish build trigger](configuring-finish-build-trigger.md) or [GitHub checks trigger](github-checks-trigger.md) are currently not supported).

</def>


</deflist>