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