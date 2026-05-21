[//]: # (title: Configure and Run Your First Build)
[//]: # (help-id: Configure and Run Your First Build)


This tutorial guides you through the basic features of TeamCity and shows you how to set up a typical project.



## Main TeamCity elements

Before we jump to configuring a real project, let's have a quick look at core elements from which any TeamCity CI/CD workflow consists. These elements are described more thoroughly in the [project administrator guide](project-administrator-guide.md#Steps%2C+Configurations+and+Projects) article.

<deflist type="medium">

<def title="Build step">

The smallest TeamCity element that encapsulates one action (or a sequence of actions). For example:

* The `./buildAll.sh` command that launches your custom build script.
* The `mvn clean build` command that uses Maven to build a project.
* A series of consecutive cURL commands that upload your project to an FTP server.

Two distinctive features of a build step are that it can’t be executed partially, and runs on a same machine with its neighboring steps.

</def>

<def title="Build configuration / Pipeline">

[Build configurations](creating-and-editing-build-configurations.md) and [pipelines](create-and-edit-pipelines.md) are parents for build steps. Their main objectives are to manage in which order and on which machines (build agents) these steps should run.

* Pipelines offer a more user-friendly UX and feature the UI/YAML toggle. In pipelines, build steps are grouped into **jobs** that can run in parallel on different build agents.
* Build configurations have much more advanced customization options, but are more challenging to configure for novice users. Configurations directly own build steps, without any intermediate entities, and run from start to finish on one build agent.

</def>


<def title="Project">

TeamCity projects own other projects (subprojects), along with pipelines and build configurations. Projects don’t define any executable actions; their main objective is to categorize your build configurations and pipelines in an easy-to-navigate hierarchy.

In addition, TeamCity users have [roles and permissions](managing-roles-and-permissions.md) that specify which actions they’re allowed to perform. These roles and permissions are project-scoped, meaning your organization administrators can configure separate top-level projects for each team where each member can access only their related subprojects, configurations, and pipelines.

</def>

<def title="Build chain">

A sequence of build configurations and/or pipelines with right-to-left dependencies. For example, if "Build" and "Test" are two stand-alone pipelines, you can configure the "Build &rarr; Test" chain where:

* "Build" can be triggered independently;
* "Test" has a dependency on "Build";
* Because of this dependency, triggering "Test" automatically starts "Build" first. "Test" can begin only after "Build" finishes.

    > TeamCity includes multiple smart features that optimize your workflows. For example, downstream elements of build chains can reuse upstream builds. This means in the example above, "Test" does not necessarily trigger a fresh "Build" run whenever it starts. If there were no new code changes, it can reuse the previous "Build" run and start immediately.

Parts of a build chain can belong to one or multiple projects.

</def>

</deflist>


## Step 1: Create a pipeline

1. Fork the [Gradle & Docker Pipeline (TeamCity Samples)](https://github.com/JetBrains/Gradle-Docker-Pipeline-TeamCity-Samples/) repository. You can configure your first project that’ll process this public repo directly, but you’ll have more options with a forked one. For example, you’ll be able to create and build pull requests, and publish TeamCity statuses back to GitHub.

2. On a new TeamCity installation, you need to first [create a project](creating-and-editing-projects.md) that’ll house our sample pipeline. You can add further configurations and pipelines to this same project, or create new projects and subprojects to create a neat build server hierarchy.

    Click the plus icon in the TeamCity sidebar to add a new project, then enter the project name and optional description.

    <img src="dk-crete-project-sidebar-1.png" alt="Create new project" width="706"/>

    > See this article for more information about projects and various ways to create them: [](creating-and-editing-projects.md).

3. A project doesn’t directly own any CI/CD actions and serves as a shell for build configurations and pipelines. As such, once you configure basic project settings, TeamCity asks you to choose the child element type.

    Click the **Pipeline** tile and open the dropdown menu that lists all available options to create a pipeline.

    <img src="build-configuraiton-creation-options.png" width="706" alt="All build config creation options"/>

    > This menu is automatically populated with new options as you keep adding more TeamCity projects. See the following articles for more information about each option and differences between adding configurations and pipelines:
    > * [](create-and-edit-pipelines.md)
    > * [](creating-and-editing-build-configurations.md)

4. In the drop-down menu, click **Connect new repository** and choose any of the following options to connect to your new forked repository:

    * Create a pipeline using a direct repository URL. If you choose this option, you’ll require manually specifying auth options (SSH key, user/password credentials, access token, or anonymous). In the end, TeamCity will have access only to this repository.
    * Click the GitHub icon to configure a permanent [OAuth or App connection](configuring-connections.md#GitHub) to GitHub. Since this option involves installing and authorizing the application on GitHub side, it takes a few more clicks to configure. However, it’s much more beneficial in the long run. Having a connection to a VCS provider makes the process of adding new configurations and pipelines as easy as choosing a required repository from the list — the connection will take care of all auth settings automatically.

        <img src="connection-repo-list.png" width="706" alt="Repository list retrieved from a connection"/>
    
5. Leave all settings in their default states. We’ll change some of them later.

    <img src="gs-default-pipeline-settings.png" width="706" alt="Default settings"/>

    * Default branch — the repository branch that TeamCity should consider a [default one](working-with-feature-branches.md#Default+Branch).
    * Start new builds on new changes — the list of branches in which new commits [automatically trigger new TeamCity builds](pipeline-settings.md#Auto-Run+Pipeline).
    * Pull requests — allows TeamCity to [track pull requests](pipeline-settings.md#Repository) in addition to regular changes in stable branches.
    * Publish statuses to repository — if enabled, TeamCity reports build statuses (started, running, successful, and failed) back to GitHub. These statuses are visible on the main repository page.

6. Click **Create** to save your new project with pipeline. You can now add jobs with build steps that perform required actions.

7. Select the job tile and add a "Gradle" step in its [Steps section](job-settings.md#Steps).

8. Set the following step settings, then click **Save** to finish the setup.

    * Step name — "Build app".
    * Tasks — `clean build`.
    * JDK — Choose JDK 11 with the architecture that matches your build agent.
   
    For example, on ARM macOS machines you should end with something like the following (switch the top left toggle from **Visual** to **YAML** to view and edit settings in yml format):

    ```yaml
    jobs:
      Job1:
        name: Job 1
        steps:
          - type: gradle
            name: Build app
            tasks: clean build
            jdk-home: '%env.JDK_11_0_ARM64%'
    ```
   
## Step 2: Run your first build

Now that your first pipeline is ready, let's run it and make sure everything works as expected.

1. Click **Run** in the top right corner to build the app. You can track the progress in the **Build Log** tab.

    <img src="gs-first-run.png" width="706" alt="First run"/>

2. Try toggling the **Settings** button on and off. You can do so to switch between the edit and view modes of the current project, build configuration, or pipeline.

3. Click on your pipeline in the breadcrumbs panel or side navigation bar to go to the overview page. Here you can see the history of all runs for this pipeline.

    <img src="gs-overview-page.png" width="706" alt="Pipeline overview page"/>

    From this page, click on any build number to zoom into this specific run for additional info: run time, build log, test results, published artifacts, and so on.

4. Try running the same pipeline again. You will notice that it finishes instantly, and TeamCity displays "0 seconds" for the duration. This happens thanks to one of TeamCity optimization features, **build reuse**: if your previous run was successful and neither the remote repository nor the job itself changed, TeamCity will simply "clone" previous results for a fresh run and add the "Job reused" label to a job tile. You can read more about job optimizations in TeamCity [here](job-settings.md#Optimizations).


## Step 3: Add testing jobs

A pipeline can include multiple jobs running consecutively or in parallel on different build agents. In this step, you’ll add two upstream jobs and learn how to deal with build problems.

1. Click **Settings** to enter the edit mode for your pipeline and edit its YAML configuration as follows:

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
      Job2:
        name: Test suite A
        steps:
          - type: gradle
            name: Run test suite A
            tasks: test
            jdk-home: '%env.JDK_11_0_ARM64%'
            working-directory: test1
      Job3:
        name: Test suite B
        steps:
          - type: gradle
            name: Run test suite B
            working-directory: test2
            tasks: test
            jdk-home: '%env.JDK_11_0_ARM64%'
        allow-reuse: false
    ```

2. Switch to the **Visual** editor and take a minute to check out your new pipeline configuration.

    <img src="gs-parallel-jobs.png" width="706" alt="Parallel testing jobs"/>

   * Select your initial build job and expand its **Dependencies** settings section. Both testing jobs are checked, which means the build job depends on them. The new jobs have no dependencies. As a result, the build job waits for the testing jobs to finish, while the testing jobs have equal priority and can run simultaneously on different build agents.
   * Inspect the settings of a Gradle build step in any of the testing jobs. Unlike in the building job, these steps have a custom [working directory](build-working-directory.md) set. This means that `gradle test` tasks will start in respective directories rather than the repository root.

3. Save and run your updated pipeline. Since both new jobs are running tests, you can use the **Tests** tab of the run results page in addition to **Build log** to track the results.

    <img src="gs-failed-tests.png" width="706" alt="Failed tests"/>

    * "Test suite A" finishes its tests successfully.
    * Tests from the "SecondTestCase" suite run with mixed results: five finish successfully while two fail. As a result, the entire job is labeled as failed.
    * Because of "Test suite B" job failing, the main building job fails without running. This is the default TeamCity logic: if some downstream stage of a workflow is unsuccessful, there’s no point in running the upstream operations that depend on it.
   
        > You can override this behavior for failing elements of a [build chain](pipeline-settings.md#Pipeline+Dependencies) and for [build configuration steps](configuring-build-steps.md#Step+Execution+Conditions). We plan to introduce similar functionality for individual jobs in future releases. Follow the [pipelines roadmap](pipelines-roadmap.md) to stay up to date with our development plans.

4. Fixing a build problem can take time. To keep it from failing subsequent builds, you can **mute** it. A muted failure does not block later stages or cause the entire workflow to be reported as failed.

    Select failing tests in the **Tests** tab and click **Mute**.

    <img src="gs-mute-problems.png" width="706" alt="Mute test problems"/>

5. In the **Investigate / Mute** dialog, specify the following settings:

    * Investigated by — assign the investigation to a TeamCity user so that your teammates know someone is working on it.
    * Mute in — allows you to choose the mute scope. Your only available option at the moment will be "Project-wide", since you only have one pipeline.
    * Unmute — set the unmute policy. The default "Automatically when fixed" option means TeamCity should stop ignoring this problem once you invoke this dialog again and select "Mark as fixed" under investigation options.
   
6. If you assigned the investigation to yourself, you can quickly access them from the **My investigations** page.

    <img src="gs-my-investigations.png" width="706" alt="My investigations"/>

7. Mute the related build problem in the **Problems** tab similarly to how you muted the failing tests in the **Tests** tab, then re-run your pipeline. It should behave as follows:

    * 

<!--This tutorial guides you through the basic features of TeamCity and shows you how to set up a typical project.

> To configure and run your first build, complete the [](#Create+a+TeamCity+Project) and [](#Set+Up+a+Build+Configuration) sections. The remaining sections are optional but worth reviewing to familiarize yourself with key TeamCity concepts and features.
>
{style="note"}

## Create a TeamCity Project

The majority of projects built in your TeamCity will be solutions actively developed by your teams, and as such, hosted on your organization or individual VCS accounts. Refer to [Option 1](#Option+1%3A+Using+a+Connection) to learn how to set up a project that targets a repository owned by or shared with you.

In some cases you may also want to set up a project that targets a third-party repository or your own VCS hosting provider that holds only one relevant repo (so that you don't need a dedicated connection to this VCS). If this repository is public, you can set up a project using a direct link. See [Option 2](#Option+2%3A+From+a+repository+URL) for the details.


### Option 1: Using a Connection

<video src="../media/create-project-from-connection.mp4" preview-src="../media/create-project-from-connection-cover.png"/>

1. The simplest way to create projects, configurations, and VCS roots is by utilizing a permanent [connection to a VCS hosting](configuring-connections.md). This approach is particularly efficient when you intend to create multiple projects for repositories hosted under the same VCS account, as it saves you from repeatedly configuring the same access settings.

   This tutorial utilizes a GitHub-hosted repository, so start by navigating to **[Root Project Settings](project-administrator-guide.md#Edit+and+View+Modes) | Connections** and creating a new [GitHub.com connection](configuring-connections.md#GitHub). You can choose between a GitHub App or GitHub OAuth connections.

2. Fork the `https://github.com/JetBrains/Maven-Configuration-TeamCity-Samples` repository to your personal account.

3. Click the "+" icon next to the **Projects** menu item to navigate to the **Create Project** page.

    <img src="dk-create-project-main.png" alt="Main new project menu" width="706"/>

4. The **Create Project** page displays all available connections that you can utilize to access repositories. Click a **From GitHub.com** tile and TeamCity will display the list of all repositories owned by or shared with you.

    <img src="dk-repositories-list.png" alt="Repositories list" width="706"/>

5. Select your forked repository and leave all settings in their default state.

    * Versioned settings are project descriptions stored in the repository ".teamcity" folder. Choose to ignore these settings for now.
    * Project and build configuration names are public names in TeamCity.
    * Default branch is the specification for the primary repository branch.
    * Branch specification is a pattern that specifies which branches TeamCity should track. The default `refs/heads/*` value allows TeamCity to monitor all regular branches.

6. Click **Proceed** to continue to the [](#Set+Up+a+Build+Configuration) stage.


To create more TeamCity projects that target repositories hosted under the same account, skip the first step. This workflow allows you to set up authorization settings only once, when configuring a TeamCity-to-VCS connection, and then reuse this connection as often as needed.

Further reading: [](configuring-connections.md)


### Option 2: From a repository URL

<video src="../media/create-project-from-url.mp4" preview-src="../media/create-project-from-url-cover.png"/>

1. Click the "+" icon next to the **Projects** menu item to navigate to the **Create Project** page.

    <img src="dk-create-project-main.png" alt="Main new project menu" width="706"/>

2. Click the **From a repository URL** tile and enter `https://github.com/JetBrains/Maven-Configuration-TeamCity-Samples` to the **Repository URL** field.

     <img src="CreateProject1.png" alt="Create a project from a repository URL, Step 1" width="706"/>

   The target repository is public and available without authentication, so leave the rest of the properties unchanged and click **Proceed**.

   > Note that you can also insert a repository URL in SSH format: `git@github.com:JetBrains/Maven-Configuration-TeamCity-Samples.git`. TeamCity recognizes such URLs and automatically switches the **Authentication** from "Password / Access token" to "SSH key". In this case you will need to provide a valid SSH key to continue.
   >
   > For the sake of this tutorial, keep using the default HTTPS address. We will switch to a more secure SSH option in the [](#Change+VCS+Root+Settings) section.
   >
   {style="tip"}

3. The next page allows you to set up basic [project settings](project-administrator-guide.md#Edit+and+View+Modes):

    * Whether to apply versioned settings, if any were found.
    * Project and build configuration names are public names in TeamCity.
    * Default branch is the specification for the primary repository branch.
    * Branch specification is a pattern that specifies which branches TeamCity should track. The default `refs/heads/*` value allows TeamCity to monitor all regular branches.

   For the sake of this tutorial, choose to ignore versioned settings for now and leave the default values for all fields. Click **Proceed** to continue to the [](#Set+Up+a+Build+Configuration) stage.




## Set Up a Build Configuration


A project does not perform any building routines, all build actions are configured inside a build configuration. For that reason, TeamCity creates an empty build configuration (with the default "Build" name) along with any new project.

After you set core project and configuration settings at the end of the [](#Create+a+TeamCity+Project) stage, TeamCity brings you to the **Build Steps** page of build configuration settings. TeamCity scans the remote repository to identify the application type and suggest corresponding build steps.

<img src="CreateProject3.png" alt="Create a project from a repository URL, Step 3" width="706"/>

You can check the suggested step and click **Use selected**, or add your custom steps:

1. Navigate to [Build Configuration Settings](project-administrator-guide.md#Edit+and+View+Modes) and click **Build Steps** in the side menu.
2. Click **Add build step** and choose a required step type. Since this sample tutorial targets the Java application with Maven, choose "Maven" from the list and type a required command in the **Goals** field (for example, `clean test`).

   > You can perform almost any build action using the **Command Line** step, which runs custom scripts on the agent machine's terminal. For example, creating a CLI step with the command `mvn clean test` in the **Custom script** field will perform the same actions as a Maven step.
   >
   > Command Line steps are ideal for basic system operations (like creating or deleting files and folders) and interacting with custom software installed on agent machines. However, if you're using build tools that have corresponding TeamCity steps (such as Maven, Gradle, .NET, Ant, Node.js, and so on), we recommend using those specific steps. They are tailored to match the build tool and offer unique settings that simplify configuration.
   {style="tip"}

3. In step settings, click **Show advanced settings** to view available options. For example, you may want to change [step execution conditions](build-step-execution-conditions.md). By default, steps are executed one after another and if one step fails, the following steps are automatically skipped. This setting allows you to change this default behavior.

Further reading: [](configuring-build-steps.md)





## Change VCS Root Settings


A [VCS Root](project-administrator-guide.md#VCS+Roots) is a project-owned object that stores settings required to access a repository and perform basic checkout operations.

In both [Option 1](#Option+1%3A+Using+a+Connection) and [Option 2](#Option+2%3A+From+a+repository+URL) workflows you end up with a project that already owns an operational VCS Root. However, you may manually create roots and attach them to build configurations. This is typically required in the following cases:

* If you choose the **Manually** option on the **Create Project** page. This option leaves you with a completely blank project that you should manually set up from ground up.
* If your build configuration should have steps that build or test different projects from different repositories.

In this tutorial we will slightly modify an existing root to alter the checkout process.

**On a project level:**

<video src="../media/ssh-root-sd.mp4" preview-src="../media/ssh-root-sd-cover.png"/>

1. Open the **[Project Settings](project-administrator-guide.md#Edit+and+View+Modes) | VCS Roots** page and click your root to edit its settings. When editing a root from the project level, you edit its core properties common to any configuration that uses this root.
2. In the **General Settings** section, change the **Fetch URL** to an SSH format. For example, `git@github.com:JetBrains/Maven-Configuration-TeamCity-Samples.git`.
3. Scroll down to **Authentication Settings** and set **Authentication method** to "Custom Private Key". Private keys are typically [uploaded to the Root project](ssh-keys-management.md) so that you can easily reuse them, but since we have none at the moment, we will just specify the key manually.
4. Enter the path to your private SSH key. For example, `/Users/John.Doe/.ssh/id_ed25519`.
   > Frequently used SSH keys can be [uploaded to TeamCity](ssh-keys-management.md).
5. If needed, enter username and passphrase. Note that if username is set, it will override the URL included in fetch URL.
6. Test the VCS connection with your updated settings and save the root.


**On a build configuration level:**

<video src="../media/checkout-rules.mp4" preview-src="../media/checkout-rules-cover.png"/>

1. Go to **[Build Configuration Settings](project-administrator-guide.md#Edit+and+View+Modes) | Version Control Settings** page. This page lists all VCS roots attached to this specific configuration. You can click any root to edit same settings as on the project level, with an option to save your edits for this configuration only.

    The **Additional Options** section below the list of attached roots includes settings that control how these roots are used by this configuration. For example, the **Clean build** option allows you to force TeamCity to clean the agent checkout directory and re-acquire files from the remote repository whenever a new build starts. Normally TeamCity does not pull remote files if the repository has no new commits.

2. Click **Edit checkout rules** link in the attached root. [](vcs-checkout-rules.md) allow you to specify subdirectories where remote files should be placed. For configurations that utilize more than one root, this is a crucial step that prevents checkout conflicts and file losses.

3. Enter the `+:.=>custom` rule and click **Save**. This rule tells TeamCity that the entire repository targeted by this root should be checked to the "custom" folder of the original checkout folder. The final location of your pulled sources will look like the following: `<Agent home>/work/78ffbb580fc61c13/custom`.

4. Your new change will cause the build to fail since the Maven build step can no longer find the `<Agent checkout directory>/ch-simple/pom.xml` file. Go to step settings and include the new "custom" directory in the path.

    > Tip: There are multiple places in TeamCity that require you to enter paths: checkout rules, artifact rules, working directories, and so on. To avoid errors when entering these paths, use the directory tree button instead of typing them by hand. This dialog allows you to browse the existing contents of the target directory, so you need to run a build at least once to see your new "custom" folder on the agent storage.
    > 
    {style="tip"}

5. Save the step settings and run the build to ensure it finishes successfully.


Further reading: ??? VCS ROOT ADVANCED SETTINGS LINKS

## Build New Changes Automatically

### Triggers

### Webhooks


## Configure Additional Build Features


## Working with Pull Requests


## Choose Agents to Run Your Builds


## Configure Project Members


## Set Up Notifications


## Store Project Settings in a VCS


-->



In TeamCity terms, a _build_ is a process that consists of one or more steps and performs a certain CI/CD job.

After you’ve installed and started TeamCity as described [here](quick-setup-guide.md), you’re ready to configure and run your first build.
{instance="tc"}

After you’ve started TeamCity as described [here](getting-started-with-teamcity-cloud.md), you’re ready to configure and run your first build.
{instance="tcc"}

<img src="run-first-build.png" width="611" alt="Run your first build"/>

You can watch a quick video guide or read the full tutorial below.

<video src="https://youtu.be/SYjnb7pW4Cg"
       title="Running your first build in TeamCity"/>

## Create your first project

You can create a project in TeamCity in several ways: automatically from a repository URL, from a VCS connection, or manually. This tutorial focuses on the easiest method — creating a project from a repository URL. Simply provide your repository path, and TeamCity will scan it, detect build steps, and suggest configuration settings.

>__Sample Repository__  
>To explore the setup flow, you can use [this sample GitHub repository](https://github.com/JetBrains/Maven-Configuration-TeamCity-Samples).
>
{style="note"}

Each TeamCity installation includes a default **Root** project, which contains all other projects. Your first project will be added as a child of this Root. Follow these steps to create it:

1. Open the **Projects** tab and click "**+**".

   <img src="get-started-new-project.png" width="706" alt="Create new project"/>

2. Enter a project name and optional description. You can leave the autogenerated Project ID as is.

    <img src="create-page-new-design.png" width="706" alt="Main create project page"/>

3. A TeamCity project doesn’t perform builds itself — it serves as a container [build configuratons](creating-and-editing-build-configurations.md) and [pipelines](create-and-edit-pipelines.md). Projects only help you organize related builds, manage permissions, and share resources such as [connections](configuring-connections.md) and [build parameters](configuring-build-parameters.md).

   After saving the project details, TeamCity will open the **Set up your build** page. Here you can choose to create a build configuration or a pipeline. For this tutorial, select a classic build configuration.

    <img src="build-configuraiton-creation-options.png" width="706" alt="All build config creation options"/>

4. From the dropdown, select **Connect new repository | Any Git URL** from the drop-down menu.

   As you add more projects, you’ll gain access to other creation options. For example, creating [from an existing VCS root](creating-and-editing-build-configurations.md#Use+a+VCS+Root) or selecting a repository via an [authenticated VCS connection](configuring-connections.md).

5. Paste your repository URL. TeamCity supports Git, Subversion, Perforce, Azure, and Mercurial( all supported URL formats are listed [here](guess-settings-from-repository-url.md#VCS+URL+Formats)). For the sample project, use:
    
    ```Shell
    https://github.com/JetBrains/Maven-Configuration-TeamCity-Samples
    ```

   This is a public repo, so you can choose **HTTPS | Anonymous** authentication. For private repositories or when write access is needed (for example, to [post build statuses](commit-status-publisher.md)), select a different authentication method. See [Creating and Editing Build Configurations](creating-and-editing-build-configurations.md#Use+a+Repository+URL) for details.

    <img src="getting-started-auth-settings.png" width="706" alt="Auth settings for public repository"/>

6. After you click **Proceed**, TeamCity will locate the repository, check its branches, and prompt you to select the [default one](working-with-feature-branches.md#Default+Branch). You can also enable automatic build triggers for new commits.

7. Click **Create** to finish the initial setup. You now have a project with a child build configuration, and are looking at build configuration settings. Use the **Settings** button in the top right to switch between "Edit" (modify configuration) and "View" (review build history) modes.

   You can run the build configuration immediately. A TeamCity agent delegated with this build will fetch the sources, but since no build steps exist yet, the build will end quickly. Let’s add some of these steps now.

8. In configuration settings, open the **Build Steps** tab. Here you define the actions your configuration performs when triggered.

   Click **Auto-detect build steps** to let TeamCity scan your repository. For the sample project, it’ll suggest two [Maven](maven.md) steps. Select the one that runs `clean test` for the `ch-simple/pom.xml` file and click **Use selected**.
    
    > Build steps execute sequentially by default, but you can customize their behavior:
    >
    > * Disable steps temporarily if they’re not needed.
    > * Set execution conditions to control when a step runs. For example, only if the previous step fails or depending on the build parameter value (such as [`teamcity.agent.jvm.os.name` parameter](predefined-build-parameters.md) that allows you to run different steps on Linux, Windows, and macOS agents).
    > * Add bootstrap steps that run before TeamCity checks out sources.
    >
    > See the [](configuring-build-steps.md) article for more information on these and other options.
    > 
    {style="tip"}


Congratulations! You created your first TeamCity project and build configuration. You can now [run the build](#Run+your+first+build) and [tweak configuration settings](#Tweak+your+build+configuration+settings) as needed.

## Run your first build

To run builds in TeamCity, you need [build agents](install-and-start-teamcity-agents.md). A fresh TeamCity server, installed as described [here](quick-setup-guide.md), has one registered build agent that runs on the same computer. Let's use this agent to run a build on the sample project.
{instance="tc"}

On the __[Build Configuration Settings](project-administrator-guide.md#Edit+and+View+Modes)__ page, click __Run__ in the upper right corner:

<img src="RunBuild.png" alt="Run a build" width="706" border-effect="line"/>

TeamCity will always assign a build to the first available and [suitable](configuring-agent-requirements.md) build agent.

You’ll be automatically redirected to the __Build Results__ page, where you can watch the build progress and review its results upon the build finish. You can also access your build configuration settings from this page and edit them as necessary:

<img src="BuildResults.PNG" alt="Build results" width="706" border-effect="line"/>

## Tweak your build configuration settings

You might want to configure the following settings first:

* paths to build [artifacts](#Artifacts)
* [automatic triggers](#Automatic+Build+Trigger)
* a custom pattern for a [build number](#Build+Number+Format)

For other settings, see this [chapter](creating-and-editing-build-configurations.md).

### Artifacts

If your build produces installers, WAR files, reports, log files, and so on, you might want to publish them on the TeamCity server after finishing the build. You can specify the paths to such artifacts in __[Build Configuration Settings](project-administrator-guide.md#Edit+and+View+Modes) | General Settings__. As you already have a finished build, the build agent has checked out the sources already. Next to the _Artifact paths_ field, click the tree icon to open the checkout directory browser. You can select artifacts from this tree:

<img src="artifactPaths.png" alt="Artifact paths" width="706" border-effect="line"/>

TeamCity will place their paths into the text field, so you can modify them if necessary:

<img src="artifactPathsArchive.png" alt="Modify the artifact paths" width="706" border-effect="line"/>

Save the build configuration settings. Now, when you run a build, TeamCity will put all the specified reports into an archive and publish them.

The __Build Configuration Home__ page lists all run builds and allows viewing their artifacts:

<img src="artifactsPublished.png" alt="Published artifacts" width="706" border-effect="line"/>

You can also view and download artifacts from the __Build Results__ page:

<img src="artifactsPublishedResults.png" alt="Artifacts in build results" width="706" border-effect="line"/>

Read more details [here](configuring-general-settings.md#Artifact+Paths).

### Automatic Build Trigger

Automatic build triggering on a change in the repository is essential to any CI. TeamCity will add a [VCS trigger](configuring-vcs-triggers.md) automatically when creating a project / build configuration from a repository URL. You can also add it manually on __Build Configurations Settings | Triggers__ page:

<img src="addTrigger.png" alt="Add a trigger" width="706" border-effect="line"/>

>TeamCity provides multiple [types of triggers](configuring-build-triggers.md) — learn about them all to optimize your pipeline.

### Build Number Format

Each build in TeamCity has a build number, that’s a string identifier. It is composed according to the pattern defined in __[Build Configuration Settings](project-administrator-guide.md#Edit+and+View+Modes) | General Settings__ (click _Show advanced options_ to display it). If you leave the default value, the build number format will be maintained by TeamCity; the number will be resolved into a next integer value on each new build start. Or, you can customize the pattern as described [here](configuring-general-settings.md#Build+Number+Format).

## Takeaway

To configure a certain CI/CD job in TeamCity:
1. Create a project from your source repository and adjust its main settings.
2. Create a build configuration inside this project.
3. In the build configuration settings, add build steps that’ll represent stages of the build.
4. Set up other configurations settings. For example, add handy build features and automatic triggers.

After that, you can run a build based on the created configuration manually, or wait until it is triggered automatically, if any triggers are configured.