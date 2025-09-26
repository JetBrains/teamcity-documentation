# Common Job Settings

Jobs contain individual build steps that run sequentially. This article covers common settings that control how the sequence is executed.


## Edit Job Settings

To view and edit job settings, click the [Settings toggle](project-administrator-guide.md#Edit+and+View+Modes) in the top right corner, then click any job tile (or the "Add" tile to create a new job).

<img src="pipelines-open-job-settings.png" width="706" alt="Open job settings"/>

You can also switch from the visual editor to the code and edit the markup directly.


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



## Optimizations

This section covers settings to significantly speed up pipeline runs, saving time, resources, and, for cloud agents, infrastructure costs.

* **Parallel Tests** — Allows Maven and Gradle steps to split test suites into batches, spawning N virtual builds running in parallel on separate build agents.

    > TeamCity groups tests into batches based on their parent classes or test cases, so the actual number of batches may be lower than specified. For example, tests in a single large test class cannot be effectively split. See this build configuration article for details on how TeamCity selects and runs test batches: [](parallel-tests.md).
    >
    {style="tip"}

* **Reuse Job Results** — If no enabled [repositories](#Repository) contain new changes, TeamCity skips re-running the job and reuses artifacts, status, and results from a previous run. This ensures only jobs affected by recent changes are executed.

    > See this article to learn how TeamCity identifies builds that can be reused: [](snapshot-dependencies.md#Suitable+Builds).
    >
    {style="tip"}

    Reused jobs are explicitly marked in the UI to avoid any confusion.

    <img src="pipelines-job-reuse.png" width="706" alt="Pipeline run reuse"/>

    Note the "Optimization" tile on the top of the page: TeamCity was able to finish this run nearly five times faster than the previous run, with the amount of time saved due to reusing runs attributed to nearly 80% of the last known run duration.


## Agent Requirements

TeamCity automatically tracks agent software to ensure queued runs are assigned only to compatible agents. For example, if a Maven step must run in a container, agents without Docker or Podman are marked incompatible.

Similarly, if a command-line step runs `echo %\myParam%` and "myParam" is not defined in pipeline or job [parameters](#Parameters) sections, TeamCity checks the agent machine as the last remaining potential source of this parameter value. Only agents with a non-empty "myParam" parameter can run the job.
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

The following YAML sample defines three requirements: 16 GB of RAM, at least 10 GB of free disk space, and Python 3 installed. Standard TeamCity requirements use the shorter alias: value syntax, while custom ones use full `<parameter> <operator> [value]` expressions (with an extra `name` parameter for the public title).

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

> If a job uses a parameter that is not defined on either pipeline or job level, this parameter becomes an [agent requirement](#Agent+Requirements) (see [example](#pipeline-implicit-requirement)). These automatically generated requirements are also called [implicit](configuring-agent-requirements.md#Implicit+Requirements), as opposed to user-defined [explicit](configuring-agent-requirements.md#Explicit+Requirements) ones.
> 
{style="note"}

## Repository

TBD