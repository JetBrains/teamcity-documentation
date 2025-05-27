[//]: # (title: Create and Edit Pipelines)
[//]: # (help-id: Create and Edit Pipelines)

Pipelines are user-centric, simplified alternatives for traditional [build configurations](creating-and-editing-build-configurations.md) and [build chains](build-chain.md) available in TeamCity 2025.07 and newer. While still in early access and not recommended for complex setups due to potential issues, they are not marked as experimental and can be safely used in production for smaller, less demanding projects.

> TeamCity pipelines share most of its concepts (triggers, step settings, parameters, and more) with regular [build configurations](creating-and-editing-build-configurations.md). This article focuses largely on unique pipeline features. For more information on basic pipelines setup, see the dedicated documentation: [TeamCity Pipelines](https://www.jetbrains.com/help/teamcity/pipelines/teamcity-pipelines.html).
> 
{style="note"}

> For TeamCity Professional servers, pipelines have a separate limit of 10 per server and do not count toward the build configuration limit.
> 
{style="tip"}


## Pipelines vs Build Configurations

Before we dive into creating pipelines, it’s important to understand the differences between build configurations and pipelines and when to use each.

<include from="creating-and-editing-build-configurations.md" element-id="configurations-vs-pipelines"/>


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

See [TeamCity Pipelines](https://www.jetbrains.com/help/teamcity/pipelines/teamcity-pipelines.html) for more information on basic operations.


## Job Outputs

Pipelines aim to streamline the user experience and present TeamCity concepts in a more accessible way. A key example is how they handle build results shared with end users and downstream jobs.

In classic TeamCity, files produced during a build and the parameters calculated along the way are mostly treated separately.

* Files are published as artifacts using paths [defined in the build configuration settings](configuring-general-settings.md#Artifact+Paths). These artifacts appear on the [](build-results-page.md) page and can be downloaded by users or passed to other configurations via manually configured [artifact dependencies](artifact-dependencies.md).
* Parameters [can be accessed by external configurations](use-parameters-in-build-chains.md) using the `dep.<source_config_ID>.<parameter_name>` syntax. In earlier versions, project administrators had no clear way to see which external configurations relied on their parameters, making it easy to unintentionally break CI routines used by other teams. Starting with TeamCity 2025.03, reusable parameters are now defined explicitly as [output parameters](use-parameters-in-build-chains.md#Input+and+Output+Parameters), separate from standard "input" parameters intended for use only within their parent project.


Pipelines offer greater transparency by letting you manage both artifacts and parameters in a single, centralized interface. Click a job to open its settings, and expand the **Job Outputs** section. From here, you can mark any parameter or a file as an output.

**!!!IMAGE!!!**

### Parameters

Parameters defined in the **Job Outputs** settings are same [output parameters](use-parameters-in-build-chains.md) supported in build configurations. For example, the following markup demonstrates a job with two parameters:

* `DATE_FORMAT` is used inside its parent job script and is not intented to be shared.
* `J1_DATE` is an output parameter that shares the results of this job.

```yaml
Job1:
  name: Calculate date
  parameters:
    DATE_FORMAT: '%%-d %%B, %%Y'
  steps:
    - type: script
      script-content: |-
        export DF="%DATE_FORMAT%"
        formatted_date=$(date +"$DF")
        echo "##teamcity[setParameter name='J1_DATE' value='$formatted_date']"
  output-parameters:
    J1_DATE: ''
```

The `DATE_FORMAT` is not available for downstream jobs, but `J1_DATE` is accessible via the `job.<source_job_ID>.<parameter_name>` syntax.

```yaml
Job2:
    name: Print date
    dependencies:
      - Job1
    steps:
      - type: script
        script-content: 'echo "Today''s date is: %job.Job1.J1_DATE%"'
```

### Shared Files

