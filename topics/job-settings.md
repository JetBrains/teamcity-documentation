[//]: # (title: Job Settings)
[//]: # (help-id: Job Settings)

<show-structure for="chapter" depth="2"/>

# Job Settings

Jobs contain individual build steps that run sequentially. This article covers common settings that control how the sequence is executed.


## Edit Job Settings

To view and edit job settings, click the [Settings toggle](project-administrator-guide.md#Edit+and+View+Modes) in the top right corner, then click any job tile (or the "Add" tile to create a new job).

<img src="pipelines-open-job-settings.png" xmlns="" width="706" alt="Open job settings"/>

You can also switch from the visual editor to the code and edit the markup directly.


## Dependencies

This section allows you to arrange stand-alone jobs into a unified workflow. You can also do this in the visual editor: hover over the edge of a job to reveal a plus icon, then drag it to either side of the job you want to connect it to.

<img src="dk-pipeline-dependencies.png" width="706" alt="Create job dependencies"/>

When you add a dependency on an upstream job, you can also configure:

<img src="run-if-upstream-fails.png" width="706" alt="Job dependency settings"/>

* **Ignore shared files** — if enabled, the job does not automatically download files its upstream job [shares](#Output+Files). Available only when the upstream job shares any files.

* **Run job even if upstream fails** — by default, if an upstream job fails, every job that depends on it, directly or indirectly, is removed from the queue and canceled with the "some of the builds it depends on have failed" error. Enabling this option for a specific dependency lets the job run even if that particular upstream job fails; the pipeline run is still marked as failed overall. Use it, for example, to make sure a cleanup or notification job always runs.


## Steps

Use this section to define what the job does, such as building and testing projects, running custom scripts, uploading Docker images, and so on.

Currently, pipelines support four types of steps you can add. All of them are lightweight versions of corresponding [classic build configuration steps](configuring-build-steps.md)

### Script

This is a universal step that executes commands directly in the agent machine terminal. As a result, you can interact with any tool installed on the agent: cURL, Python, MSBuild, Homebrew, and so on.

> This step is also available in classic TeamCity build configurations: [](command-line.md).
>
{style="tip"}

For example, the following step downloads artifacts produced by a target build configuration:

```yaml
jobs:
  Job1:
    name: Job 1
    steps:
      - type: script
        script-content: >-
          curl --location
          'https://example.com/app/rest/builds/buildType:BuildConfigID/artifacts/archived/?locator=pattern%3A*.zip'
          \
          --header 'Content-Type: application/zip' \
          --header 'Accept: application/zip' \
          --header 'Authorization: Bearer %bearer-token%' \
          --output output.zip \
          --data ''
    files-publication:
      - path: output.zip
        share-with-jobs: true
        publish-artifact: true
secrets:
  bearer-token: credentialsJSON:12e5c38b-16a1-4201-a913-5b5411bd7bfe
```

### Gradle

Tailored for interacting with the [Gradle build tool](https://gradle.org), this step can build, test, and package Java, Kotlin, Groovy, Scala, Swift, and other projects.


> This step is also available in classic TeamCity build configurations: [](gradle.md). See this document for more information about step options.
>
{style="tip"}


```yaml
name: Gradle Project
jobs:
  Job1:
    name: 'Job 1: Gradle Build'
    steps:
      - type: gradle
        tasks: clean build -x test
        use-gradle-wrapper: 'true'
  Job1_2:
    name: 'Job 2: Test Suite 1'
    runs-on: Linux-Medium
    steps:
      - type: gradle
        working-directory: test1
        tasks: clean test
        build-file: build.gradle
        use-gradle-wrapper: 'true'
    dependencies:
      - Job1
```


### Maven

The Maven build step is designed to process Java, Kotlin, Groovy, and other projects using [Apache Maven](https://maven.apache.org).


> This step is also available in classic TeamCity build configurations: [](maven.md). See this document for more information about step options.
>
{style="tip"}

```yaml
jobs:
  Job1:
    name: Job 1
    steps:
      - type: maven
        maven-version: bundled_3_6
        pom-location: pom.xml
        goals: '-B -DskipTests clean package'
        jdk-home: '%env.JDK_21_0%'
```


## Build Features
{help-id="build-features"}

Like steps, build features perform specific actions. However, features run at predefined points in the build lifecycle, while steps are more flexible and can be arranged as needed.

For example:

* [Script](command-line.md) is a build step that executes terminal commands. Its behavior depends on your settings: it can run a single command or a full script, use inline or file-based scripts, and execute at any point in the pipeline.
* [Swabra](build-files-cleaner-swabra.md) is a build feature that performs one specific action — cleaning files produced during the build — at a specific time, either before or after the build.

At the moment, pipelines support only a subset of the [build features](adding-build-features.md) available for build configurations:

<deflist type="full">

<def title="Build files cleaner (Swabra)">

Tracks files created, modified, or deleted during the build. New files are removed when the build finishes or when the next build starts, while modified and deleted files are reported in the build log. You can also limit tracking to specific files and directories.

Learn more: [](build-files-cleaner-swabra.md)

</def>

<def title="Build cache">

Improves build performance by reusing files generated during previous runs, such as downloaded [npm](nodejs.md) packages or [Maven local repository artifacts](https://maven.apache.org/guides/introduction/introduction-to-repositories.html).

Learn more: [](build-cache.md)

</def>

<def title="Free disk space">

Automatically cleans the agent disk to make sure enough free space is available for new builds.

Learn more: [](free-disk-space.md)

</def>

<def title="XML report processing">

Lets TeamCity use report files generated by external tools. Supported formats include test framework reports such as JUnit, Maven Surefire/Failsafe, TRX, and Google Test, as well as code analysis reports from tools like SpotBugs, PMD, and Checkstyle.

Learn more: [](xml-report-processing.md)

</def>

</deflist>

> We plan to expand the list of supported features in future releases and will use your feedback to help prioritize this work.
>
> Because pipelines are designed to provide a smoother and more user-friendly experience, some classic build configuration features are not migrated directly. Instead, they may be replaced with more native pipeline functionality. For example, the [](pull-requests.md) feature enables TeamCity build configurations to work with pull (merge) request branches. In pipelines, the same functionality is available as a simple toggle on the **Create pipeline** page when you create a pipeline from [an existing VCS connection](create-and-edit-pipelines.md#Create+and+Set+Up+Pipelines).
>
> <img src="pipelines-pull-requests.png" width="706" alt="Pull requests in pipelines"/>
>
{style="note"}

## Optimizations
{help-id="Optimizations"}

This section covers settings to significantly speed up pipeline runs, saving time, resources, and, for cloud agents, infrastructure costs.

* **Parallel Tests** — Allows Maven and Gradle steps to split test suites into batches, spawning N virtual builds running in parallel on separate build agents.
{help-id="job-parallel-tests"}

    > TeamCity groups tests into batches based on their parent classes or test cases, so the actual number of batches may be lower than specified. For example, tests in a single large test class cannot be effectively split. See this build configuration article for details on how TeamCity selects and runs test batches: [](parallel-tests.md).
    >
    {style="tip"}

* **Reuse Job Results** — If no enabled [repositories](#Repository) contain new changes, TeamCity skips re-running the job and reuses artifacts, status, and results from a previous run. This ensures only jobs affected by recent changes are executed.
{help-id="job-reuse"}

    > See this article to learn how TeamCity identifies builds that can be reused: [](configuring-dependencies.md#Suitable+builds).
    >
    {style="tip"}
    
    Reused jobs are explicitly marked in the UI to avoid any confusion.
    
    <img src="pipelines-job-reuse.png" width="706" alt="Pipeline run reuse"/>
    
    Notice the "Optimization" tile at the top: TeamCity completed this run nearly five times faster than the previous one, with reused runs saving almost 80% of the last run’s duration.


## Agent Requirements

TeamCity automatically tracks agent software to ensure queued runs are assigned only to compatible agents. For example, if a Maven step must run in a container, agents without Docker or Podman are marked incompatible.

Similarly, if a job uses a parameter that is not defined in either project, [pipeline](pipeline-settings.md#Parameters) or [job](#Parameters) **Parameters** sections, TeamCity checks the agent machine as the last remaining potential source of this parameter value. For example, if the command-line step runs `echo %\myParam%` with an unknown parameter reference, only agents with a non-empty "myParam" parameter can run the job.
{id="pipeline-implicit-requirement"}

<img src="pipeline-implicit-requirement.png" width="706" alt="Implicit requirement in pipelines"/>

The **Agent requirements** section allows you to define extra conditions for eligible agents, such as names, hardware specs, or installed tools.


TeamCity displays ready-to-use options for most basic agent hardware requirements: the number of CPU cores, the total amount of agent memory, and the CPU architecture.

<img src="pipelines-agent-requirements.png" width="706" alt="pipelines agent requirements"/>

Click **Add custom requirement** to define your own requirements. Each requirement is an `<agent.parameter> <operator> [value]` expression. TeamCity evaluates these expressions for each authorized agent, marking agents that return "true" as eligible to run the job and labeling the rest as incompatible.

<deflist>

<def title="Agent parameter">

A parameter reported by the agent machine whose value must match the required criteria. Below are a few examples of various agent parameters:

* `teamcity.agent.jvm.os.arch` — reports the agent machine architecture. For example, `aarch64` for macOS agents running on Apple ARM devices.
* `env.ANDROID_SDK_HOME` — returns the path to the Android SDK installed on the agent machine. For example, `/home/builduser/android-sdk-linux`.
* `teamcity.agent.jvm.user.timezone` — stores the timezone of the agent machine. For example, `Etc/UTC`.
* `MonoVersion` — returns the version of the Mono platform. For example, `6.12.0.200`.

Navigate to **Agents | <TeamCity_Agent> | Agent Parameters** tab to check what parameters agents report and find those that store agent hardware and software data.

<img src="tc-agent-parameters.png" width="706" alt="TeamCity agent parameters"/>

See also: [](predefined-build-parameters.md).

</def>


<def title="Operator">

The logical operator used to compare the actual agent parameter value with the given one. For example, "less than", "starts with", "contains", and so on.

See also: [](requirement-conditions.md)

</def>


<def title="Value">

A custom value to compare against the agent's parameter value. The only operator that does not require a value is `exists`, which checks whether the agent reports the required parameter, no matter what actual value it has.

</def>

</deflist>

The following YAML sample defines three requirements: 16 GB of RAM, at least 10 GB of free disk space, and Python 3 installed. Standard TeamCity requirements use the shorter `alias: value` syntax, while custom ones use the complete `<parameter> <operator> [value]` expressions (with an extra `name` parameter for the public title).

```yaml
jobs:
  Job1:
    name: Sample job
    steps:
      - type: script
        script-content: cat artifact.txt
    runs-on:
      self-hosted:
        - ram: 16GB
        - requirement: more-than
          name: Free disk space
          parameter: teamcity.agent.work.dir.freeSpaceMb
          value: '10240'
        - requirement: exists
          name: Python
          parameter: python3.executable
```

> See the following article to learn about agent requirements in classic TeamCity build configurations: [](configuring-agent-requirements.md).
>
{style="note"}


## Parameters

<include from="pipeline-settings.md" element-id="pipeline-parameters-common"/>

* **Job parameters** are typically available only in their own parent jobs. By default, they include the `env.` prefix. To access a job parameter from a **downstream** job, use the `job.<source_job_ID>.<parameter_name>` syntax. The sample below illustrates a job with a single parameter. The downstream job uses a reference to this parameter to specify its own `ParamJobB`.

    ```yaml
    jobs:
      Job1:
        name: Job 1
        steps:
          - type: script
            script-content: |-
              echo "Print Job1 parameter: %env.ParamJobA%"    # prints 'foo'
        parameters:
          env.ParamJobA: foo
      Job2:
        name: Job 2
        dependencies:
          - Job1
        parameters:
          env.ParamJobB: '%job.Job1.env.ParamJobA% bar'
        steps:
          - type: script
            script-content: |-
              echo "Print parameter from upstream Job: %job.Job1.env.ParamJobA%"     # prints 'foo'
              echo "Print modified parameter: %env.ParamJobB%"    # prints 'foo bar'
    ```

  > If you want to reuse a job parameter in multiple jobs, consider adding a pipeline input parameter instead. These parameters are common to all pipeline jobs and do not require any prefixes.
  >
  {style="tip"}

  > A job cannot access a parameter of another job if it's arranged ahead of the target job or not linked to the target job at all.
  >
  {style="note"}


* **Pipeline input parameters** are shared across all jobs of this pipeline. See [Pipeline parameters](pipeline-settings.md#Parameters) to learn more.
* **Pipeline output parameters** cannot be used in the very same pipeline. Instead, they are passed to downstream pipelines and configurations that belong to the same chain. See [](pipeline-settings.md#Pipeline+Dependencies) to learn more.

Job steps can also send the [`setParameter` service message](service-messages.md#set-parameter) to dynamically edit parameter values (or create new parameters). Note that the modified value will be available only after the step that sent this message finishes.

```yaml
parameters:
  env.JobParam: foo
jobs:
  Job1:
    name: Job 1
    steps:
      - type: script
        name: Print original value
        script-content: echo %env.JobParam%    # prints 'foo'
      - type: script
        name: Change param value
        script-content: |-
          echo "##teamcity[setParameter name='env.JobParam' value='bar']"
          echo %env.JobParam%    # prints 'foo', the step is still running
      - type: script
        name: Print modified value
        script-content: echo %env.JobParam%    # prints 'bar'
```

## Output Files
{help-id="pipeline-file-outputs"}

Files shared by a job can serve as artifacts, internal files for downstream jobs, or both.

<deflist type="full">

<def title="Artifacts">

Artifacts are files displayed on the **Artifacts** tab of the run results page. Users with permission to view the project can download these files to local storage.

You can view artifacts in two ways:

* On the run results page, open the **Artifacts** tab to see all artifacts published by jobs in the pipeline.

* From the same page, select a job to open its side panel, then switch to the **Artifacts** tab to view artifacts produced by that specific job.


<img src="dk-pipeline-artifact.png" width="706" alt="Job artifacts tab"/>


</def>

<def title="Shared files">

Shared files are passed down the pipeline to subsequent jobs. These are typically internal files or files that are not yet finalized.

Unlike artifacts, shared files are not displayed in the main **Artifacts** tab of the build results page. However, they show up on the **Artifacts** tab of the job side panel, packed into a hidden .shared_files.zip archive.

<img src="dk-pipelines-shared-files.png" width="706" alt="Shared files visible in the artifacts tab"/>

{style="tip"}

The YAML example below shows one job that creates and modifies a file, and a second job that imports the file and prints its contents. "Job 2" then publishes the file as an artifact.

```yaml
jobs:
  Job1:
    name: Create file
    steps:
      - type: script
        script-content: |-
          touch sample.txt
          echo "File created by Job 1, build #%tc.build.number%" >> sample.txt
    files-publication:
      - path: sample.txt
        share-with-jobs: true
        publish-artifact: false
  Job2:
    name: Print file contents
    dependencies:
      - Job1
    steps:
      - type: script
        script-content: cat sample.txt
    files-publication:
      - path: sample.txt
        share-with-jobs: false
        publish-artifact: true
```

> If both jobs run on the same build agent, this sequence works even if "Job 1" does not define any outputs, because the jobs share the [working directory](build-working-directory.md).
> 
{style="tip"}

</def>

</deflist>

These two types are not mutually exclusive: when adding an output file, you can tick both **Shared file** and **Artifact** checkboxes.

<img src="dk-shared-artifact-file.png" width="706" alt="Published artifact"/>


Note that shared files retain their parent directory hierarchy, whereas artifacts do not. The following sample illustrates a job that produces two files, both in their related folders.

```yaml
jobs:
  Job1:
    name: Job 1
    steps:
      - type: script
        script-content: |-
          mkdir ./artifacts
          cd artifacts
          touch artifact.txt
          echo "This file is published as artifact" >> artifact.txt
      - type: script
        script-content: |-
          mkdir ./sharedfiles
          cd sharedfiles
          touch shared.txt
          echo "This is a shared file" >> shared.txt
    files-publication:
      - path: sharedfiles/shared.txt
        share-with-jobs: true
        publish-artifact: false
      - path: artifacts/artifact.txt
        share-with-jobs: false
        publish-artifact: true
```

Despite the almost identical step scripts and `files-publication` rules, the results slightly differ. Shared files are placed into the hidden ".shared_files.zip" archive along with their parent folders, whereas artifacts are grouped under the "publish" directory as is.

<img src="dk-pipelines-aritfacts-folder-retention.png" width="706" alt="Folder retention for artifacts and shared files"/>

> In classic TeamCity build configurations, any file that needs to be shared with other configurations must be published as an [artifact](build-artifact.md). In addition, configurations need [artifact dependencies](artifact-dependencies.md) to do this.
{style="note"}


<!--
### Output parameters
{help-id="pipeline-parameter-outputs"}

Jobs can work with two types of parameters: **input** and **output**.

* **Input parameters** are name–value pairs that a job uses during its run. See the [](#Parameters) section for more information.
* **Output parameters** store values that a job passes down the pipeline to other jobs.

Output parameters are designed as a separate entity to prevent surprise breakages across pipelines. For example, changing (or removing) a parameter in "Job A" might unexpectedly break "Job B" if it depends on it. By marking a parameter as output, TeamCity signals that it may be used elsewhere, so before changing it, check for dependencies to avoid surprises.

The YAML example below defines a pipeline with two jobs that illustrate this concept:

1. **Job 1** uses the `env.INPUT` parameter to calculate a value.
2. The job’s script sends the `setParameter` [service message](service-messages.md#set-parameter) to write this value to the `result_param` input parameter.  .
3. That parameter is then mapped to the `output_param` output parameter.
4. **Job 2** retrieves this output parameter using the `job.<source_job_ID>.<output-parameter-name>` syntax.


```yaml
jobs:
  Job1:
    name: Calculate value
    steps:
      - type: script
        script-content: |-
          RESULT=$((%env.INPUT% * 2))
          echo $RESULT
          echo "##teamcity[setParameter name='result_param' value='$RESULT']"
    parameters:
      env.INPUT: '5'
      result_param: ''
    output-parameters:
      output_param: The calculated value is %result_param%
  Job2:
    name: Use calculated value
    dependencies:
      - Job1
    steps:
      - type: script
        script-content: echo %job.Job1.output_param%
```

By following this pattern, you can separate parameters used only within a job from those explicitly shared across the pipeline.

> In the example above, `setParameter` service message to change the initial `input_param` value. the `setParameter` service message updates the `result_param` value. Note that changes made by this message are **not** applied until the next step starts — the current step still sees the old value.
>
> ```yaml
>   Job1_3:
>     name: Input param
>     steps:
>       - type: script
>         script-content: |-
>           RESULT=$((%env.INPUT% * 2))
>           echo $RESULT
>           echo "##teamcity[setParameter name='result_param' value='$RESULT']"
>           echo %result_param% # prints the old 'N/A' value
>       - type: script
>         script-content: echo %result_param% # prints the updated value set by the service message
>     parameters:
>       env.INPUT: '5'
>       result_param: N/A
> ```
>
{style="warning"}

-->

## Repository

This section allows you to select which remote repositories this job should check out. To add a repository, create a new entry in the **Repositories** section of [pipeline settings](pipeline-settings.md#Repository).

By default, sources are checked into a sub-folder of the agent work directory. To ensure agents do not constantly lose sources of one job when running another, this subfolder has an auto-generated name unique for each job (for example, `/mnt/agent/work/6fa95896c6cadf54`).

You can specify a custom directory for checked out sources via the corresponding option of a **Repository** section item. The path to the checkout directory can be absolute, however it is highly recommended to use either relative paths (`MyCustomFolder`) or paths that reference pre-defined TeamCity parameters (`%\teamcity.agent.work.dir%/MyCustomFolder`).

```yaml
jobs:
  Job1:
    name: Job 1
    steps: []
    repositories:
      - https://github.com/Johndoe/MySampleApp: # Repository from URL
          path: ''' # Default value, will use a directory that matches the repository name
          enabled: true
      - Root_MyRoot: # Repository from an existing VCS root
          path: sample-java-app-maven.git 
          enabled: true
      - main: # Main repository
          path: Athanor # Custom checkout directory (relative path)
          enabled: false
```

The diagram below outlines the relations between core directories involved in a building process.

<img src="agent-directories.png" width="1680" alt="Agent and build directories" thumbnail="true"/>

Refer to the following articles to learn more:

* [](agent-home-directory.md) — the installation directory of a build agent.
* [](agent-work-directory.md) — the subfolder of agent home directory that stores build-related files.
* [](build-checkout-directory.md) — the subfolder of agent work directory where all checked out sources are downloaded.
* [](build-working-directory.md) — the directory where a build step starts (equals to "build checkout directory" by default).


## Integrations

<include from="pipeline-settings.md" element-id="pipeline-job-integrations"/>