[//]: # (title: Create Build Chain)
[//]: # (help-id: Create Pipeline)


This tutorial explains how to create a separate pipeline that works in tandem with the one created in the previous [](configure-and-run-your-first-build.md) walkthrough.


<img src="gs-chains-overview.png" width="706" alt="Chain overview"/>


Topic covered in this tutorial:

* Pipeline dependencies and build chains
* Building Docker images
* Job agent requirements
* Build agent terminal
* Reusing upstream chain builds
* Publishing and exchanging artifacts


## Basic concepts

In TeamCity, there are two main methods of linking standalone entities into a single workflow.

<deflist type="medium">

<def title="Build chain">

A [](build-chain.md) is a sequence of interconnected build configurations and pipelines.

Relationships between these standalone objects are configured from right to left, or downstream to upstream. For example, to run two pipelines in the sequence "Pipeline A → Pipeline B", add a dependency on "Pipeline A" to "Pipeline B". In other words, you tell TeamCity that "B" depends on "A".

This has two effects:

* "Pipeline A" has no dependencies and can run solo. Running it will not trigger "Pipeline B".

* "Pipeline B" depends on "Pipeline A", so it cannot run independently. When you trigger "B", it requires a completed "A" run. Depending on your chain settings, TeamCity will either start a new "Pipeline A" run and wait for it to finish, or reuse the results of a previous successful "Pipeline A" run and start "B" immediately.

</def>

<def title="Finish build trigger">

[Finish build triggers](configuring-finish-build-trigger.md) are the opposite of build chain dependencies. They allow you to create left-to-right relations between build configurations (pipelines are not currently supported). In this case, the "Config A → Config B" sequence runs when you trigger an upstream "Config A". When it finishes, it automatically triggers the downsteram "Config B". In this setup, "Config B" can run solo without triggering any external builds.

Finish build triggers are mostly used in conjunction with regular build chains.

</def>

</deflist>

See this section for more information about different ways to create relations between TeamCity entities: [](project-administrator-guide.md#Set+Up+Dependencies).



## Step 1: Create a pipeline

1. Go to the **General** settings of a project that owns the pipelines created in the [previous tutorial](configure-and-run-your-first-build.md).

2. Click **Create pipeline**.

3. Since you already have a pipeline that checks out the required project from GitHub, you can select the **From an existing VCS root** option. This way you can instantly reuse all settings required to access the repo.

    <img src="gs-second-pipeline.png" width="706" alt="Create the second pipeline"/>

4. Add a [Script](command-line.md) build step that uses `./docker/Dockerfile` to build the Docker image. If you are unsure how to configure build steps or utilize the YAML editor, see the [previous part](configure-and-run-your-first-build.md).

    ```yaml
    jobs:
      Job1:
        name: Docker build
        steps:
          - type: script
            script-content: docker build -f ./docker/Dockerfile -t johndoe/myapp:%build.number% .
    ```
   
    > In the build step above, we use the `build.number` parameter reference to assign a build number as an image tag. See this article to learn more about default TeamCity parameters: [](predefined-build-parameters.md).
    > 
    {style="tip"}

5. Since this job builds an image, you want it to run on build agents that have either [Docker or Podman](integrating-teamcity-with-container-managers.md) installed. To it being assigned to a build agent that has no required tooling, add an [agent requirement](job-settings.md#Agent+Requirements) using yet another [predefined TeamCity parameter](predefined-build-parameters.md), `container.engine`.

    ```yaml
    jobs:
      Job1:
        ...
        runs-on:
          self-hosted:
            - requirement: exists
              name: ImageBuilderTool
              parameter: container.engine
    ```

    > TeamCity automatically adds this condition when you add the [](docker.md) build step (available by default in classic build configurations). In this tutorial, we use the generic Script step instead, so we need to add this condition manually.
    > 
    {style="tip"}

6. Run the pipeline and ensure it finishes successfully. You can verify the image was built by running `docker image ls` in the agent terminal.

    <img src="gs-docker-images.png" width="706" alt="Image names in terminal"/>

    > Use the **Open terminal** link in the build results sidebar to open the terminal on a machine that hosts the corresponding build agent. This action allows you to [debug build agents](install-and-start-teamcity-agents.md#Debug+Agents+Remotely): verify installed tools, check SDK versions and paths, and so on.
    >   
    > <img src="gs-open-terminal.png" width="706" alt="Open agent terminal"/>

    > If this pipeline runs on an agent that has not run the upstream pipeline before, you may see the `file '/build/libs/todo.jar' not found` error. This is expected and will be addressed in the next steps.
    >
    > For now, add agent requirements to both pipelines so they run on the same agent.
    >
    > ```yaml
    > jobs:
    >   Job1:
    >     ...
    >     runs-on:
    >       self-hosted:
    >         ...
    >         - requirement: equals
    >           name: Agent name
    >           parameter: system.agent.name
    >           value: DefaultAgent1
    > ```
    > 
    {style="note"}


## Step 2: Configure a build chain   

You now have two separate pipelines: one that builds and tests your app, and another that produces a Docker image. To connect them, create a build chain.

The Docker pipeline should be downstream because the image should be produced only after the app has been built and tested. Since chain dependencies are [configured from right to left](#Basic+concepts) (owned by downstream objects and pointing to upstream ones), you need to add the dependency in the Docker pipeline.

1. Open Docker pipeline settings and select the pipeline to view [its settings](pipeline-settings.md) instead of individual job settings.

2. Click **Add** next to the [**Pipeline dependencies**](pipeline-settings.md#Pipeline+Dependencies) section.

3. Choose your another pipeline from the **Depend on** list and click **Done**.

    <img src="gs-add-chain-dependency.png" width="706" alt="Add pipeline dependency"/>

    > We will adjust some of these dependency settings later in this tutorial. To learn more about each setting, see [](pipeline-settings.md#Pipeline+Dependencies).
    > 
    {style="tip"}

4. Disable all **Job settings | Optimizations | Reuse Job Results** options in both pipelines. In real-world workflows, you would likely keep some of these optimizations enabled, but for now we’ll turn them off to focus on dependency settings without adding another layer of reuse logic.

5. Run your Docker build configuration. You should see both of your pipelines running. Switch to the **Chain** tab of Docker pipeline's run results page to view detailed information about each section of the chain: build number, run duration, and so on.

    <img src="gs-view-chain-run-results.png" width="706" alt="View chain run results"/>

    > Note that the **Triggered by** block of upstream pipeline builds lists two sources: your TeamCity user and "Snapshot dependency". This is because you triggered the chain, while the upstream pipeline itself was triggered by the dependent Docker pipeline.
    >
    {style="tip"}

6. Re-run the Docker pipeline. Because the pipeline dependency configured in step #3 has **Do not run new build if there is a suitable one** enabled, TeamCity will reuse the previous run of the upstream pipeline and run only the Docker pipeline again.

    You can confirm this on the **Chain** tab: the upstream pipeline build number should stay the same.

7. Run your first build/test pipeline. Notice that it runs alone and does not trigger the Docker pipeline.

8. Open Docker pipeline settings and edit your existing dependency. Disable the **Do not run new build...** setting.

    <img src="gs-disable-dependency-reuse.png" width="706" alt="Disable dependency reuse"/>

9. Run the Docker pipeline a few times. Now that the reuse setting is off, you should see both pipelines starting anew every time.

    <img src="gs-run-chain-no-reuse.png" width="706" alt="Run chain with no reuse"/>

   

## Step 3: Publish and exchange artifacts

In [step 1](#Step+1%3A+Create+a+pipeline), you may have encountered the `file '/build/libs/todo.jar' not found` error. To reproduce it, run the two pipelines on separate agents with both `{agent_home}/work` directories cleared to ensure a clean environment.


```yaml
#Build/test pipeline
jobs:
  Job1:
    name: Build app
    ...
    runs-on:
      self-hosted:
        - requirement: equals
          name: Agent name
          parameter: system.agent.name
          value: Agent1

# Docker pipeline
jobs:
  Job1:
    name: Docker build
    ...
    runs-on:
      self-hosted:
        - requirement: exists
          name: ImageBuilderTool
          parameter: container.engine
        - requirement: equals
          name: Agent name
          parameter: system.agent.name
          value: Agent2
    ...
```

This error occurs because:

* The Dockerfile copies `./build/libs/todo.jar` into the image.
* This file, along with its parent directory, is generated during the build phase and is not stored in the repository.
* The agent that runs the Docker pipeline checks out the remote sources and runs `docker build`. Since it has not run `gradle clean build` first, the expected `todo.jar` file is missing.

To resolve this issue, you need to pass the `./build/libs/todo.jar` generated by the build/test pipeline down the chain.


1. Open the settings of your upstream build/test pipeline and select the building job.
2. In the [Output files](job-settings.md#Output+Files) settings section, add the `./build/libs/todo.jar` file with both **Shared file** and **Artifact** checkboxes selected.

    ```yaml
    jobs:
      Job1:
        name: Job 1
        steps:
          - type: gradle
            name: Build app
            tasks: clean build
            jdk-home: '%env.JDK_11_0_ARM64%'
        dependencies:
          - Job2
          - Job3
        allow-reuse: false
        runs-on:
          self-hosted:
            - requirement: equals
              name: Agent name
              parameter: system.agent.name
              value: macOS J21
        files-publication:
          - path: ./build/libs/todo.jar
            share-with-jobs: true
            publish-artifact: true
      ...
    ```

3. Run this build and ensure the target file is published on the **Artifacts** tab. Any TeamCity user with sufficient permissions can download published artifacts from build result pages.

    <img src="gs-publish-artifact.png" width="706" alt="Published artifact"/>

4. The **Shared file** checkbox makes the file available to downstream jobs, but only within the same pipeline. External pipelines and configurations further down the chain do not import shared files automatically, so you need to import them manually.

    In classic build configurations, you can do this by declaring [artifact dependencies](artifact-dependencies.md), which work similarly to the pipeline dependencies you used to link the two pipelines. In pipelines, artifact dependencies are not fully supported yet and can only be added by editing the YAML configuration file.

    ```yaml
    jobs:
      Job1:
        name: Docker build
        steps:
          - type: script
            script-content: docker build -f ./docker/Dockerfile -t johndoe/myapp:%build.number% .
        ...
        download-artifacts:
        - GSFirstBuild_GradleDockerPipelineTeamCitySamples: # same ID as in 'dependencies' block
            from: dependency
            artifact-rules: todo.jar=>./build/libs
            clean-destination: true
    dependencies:
      - GSFirstBuild_GradleDockerPipelineTeamCitySamples:
          reuse: none
    ```

5. Run the build chain and ensure it now finishes successfully, even when each pipeline is processed on a separate agent.