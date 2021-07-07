[//]: # (title: IntelliJ Shared Indexes)
[//]: # (auxiliary-id: IntelliJ Shared Indexes)

>This runner is not currently bundled with TeamCity. To get it, download [this plugin]() from JetBrains Marketplace and install it as described [here](installing-additional-plugins.md).
>
{type="note"}

TeamCity can help generate and host shared indexes of your IntelliJ projects.

[Indexing](https://www.jetbrains.com/help/idea/indexing.html) creates a virtual map of your code. This map serves as a base for syntax highlighting, code completion, and other handy features. To index a big project, an IDE needs a lot of time. But, if your team works on this project from multiple machines, you can [share a once created index](https://www.jetbrains.com/help/idea/shared-indexes.html) between them and significantly reduce this time.

The IntelliJ Shared Indexes build runner can create a shared index of the project sources during the build. The TeamCity server will automatically start an HTTP server for shared indexes so your IDE can access it directly. As a result, the indexing time in the IDE will be reduced by an average of 50%.

The runner works on Windows, Linux, and macOS, and supports all the recent versions of IntelliJ-based IDEs, such as IDEA and PyCharm.

## Runner Settings

After you download the [plugin], the IntelliJ Shared Indexes runner becomes available as a build step. In its settings, you only need to set a path to the project and choose a version of IDE to use an indexer from.

During the build, the runner will:
1. Download and unpack the selected IDE.
2. Run the IDE in the background (headless) mode and generate the shared index for the project.
3. Upload the shared index to the build artifacts.

TeamCity will create a necessary HTTP layout and metadata for accessing the index from an IDE.

## IDE Settings

To be able to use precompiled shared indexes hosted on a given TeamCity server, you need to connect your IDE to this server. This is done only once, and all the later indexes, produced by the builds on this server, will be instantly available for download from the connected IDE.

To configure the connection, create an `intellij.yaml` file in the project root directory in VCS. The content of the file depends on the preferred authentication method.

### Use Access Token

This is a recommended approach. To be able to access the server, a TeamCity user should create their token in the [profile settings](managing-your-user-account.md#Managing+Access+Tokens).

```yaml
sharedIndex:
  project:
    - url: <TeamCity_Server_URL>/repository/intellij-shared-indexes/<build_configurationID>/project/teamcity
      authParams:
        type: permanent-token
```

Before downloading a shared index from TeamCity, the IDE will prompt you to enter your token. Note that the respective TeamCity user should have enough rights to access the indexed project.

### Use Guest Access

This method allows accessing all build artifacts — and shared indexes — without authentication, if this operation is permitted to the [Guest User]() role.

```yaml
sharedIndex:
  project:
    - url: <TeamCity_Server_URL>/guestAuth/repository/intellij-shared-indexes/<build_configurationID>/project/teamcity
```

## Using Shared Indexes

After you configure the `yaml` file and a shared index becomes available on the TeamCity server, the IDE will show a respective pop-up message. Confirm the download, and the IDE will start downloading the index in a background mode. You can track the status in the Event Log.