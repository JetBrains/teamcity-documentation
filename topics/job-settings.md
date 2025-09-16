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
          --data ''
    files-publication:
      - path: ''
        share-with-jobs: true
        publish-artifact: false
secrets:
  bearer-token: credentialsJSON:12e5c38b-16a1-4201-a913-5b5411bd7bfe
```

