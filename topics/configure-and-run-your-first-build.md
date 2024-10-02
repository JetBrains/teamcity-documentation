[//]: # (title: Configure and Run Your First Build)
[//]: # (auxiliary-id: Configure and Run Your First Build)

This tutorial guides you through the basic features of TeamCity and shows you how to set up a typical project.

To configure and run your first build, complete the [](#Create+a+TeamCity+Project) and [](#Set+Up+a+Build+Configuration) sections. The remaining sections are optional but worth reviewing to familiarize yourself with key TeamCity concepts and features.

## Create a TeamCity Project

The majority of projects built in your TeamCity will be solutions actively developed by your teams, and as such, hosted on your organization or individual VCS accounts. Refer to [Option 1](#Option+1%3A+From+Your+Repository) to learn how to set up a project that targets a repository owned by or shared with you.

In some cases you may also want to set up a project that targets a third-party repository. If this repository is public, you can set up a project using a direct link. See [Option 2](#Option+2%3A+From+a+Third-Party+Public+Repository) for the details.

### Option 1: From Your Repository

1. The simplest way to create projects, configurations, and VCS roots is by utilizing a permanent [connection to a VCS hosting](configuring-connections.md). This approach is particularly efficient when you intend to create multiple projects for repositories hosted under the same VCS account, as it saves you from repeatedly configuring the same access settings.

   This tutorial utilizes a GitHub-hosted repository, so start by navigating to **Administration | Root Project | Connections** and creating a new [GitHub.com connection](configuring-connections.md#GitHub). You can choose between a GitHub App or GitHub OAuth connections.

2. Fork the `https://github.com/JetBrains/Maven-Configuration-TeamCity-Samples` repository to your personal account.

3. Click the "+" icon next to the **Projects** menu item to navigate to the **Create Project** page.

    <img src="dk-create-project-main.png" alt="Main new project menu" width="706"/>

4. The **Create Project** page displays all available connections that you can utilize to access repositories. Click a **From GitHub.com** tile and TeamCity will display the list of all repositories owned by or shared with you.

    <img src="dk-repositories-list.png" alt="Repositories list" width="706"/>

5. Select your forked repository and leave all settings in their default state.

   * Project and build configuration names are public names in TeamCity.
   * Default branch is the specification for the primary repository branch.
   * Branch specification is a pattern that specifies which branches TeamCity should track. The default `refs/heads/*` value allows TeamCity to monitor all regular branches.

6. Click **Proceed** to continue to the [](#Set+Up+a+Build+Configuration) stage.


To create more TeamCity projects that target repositories hosted under the same account, skip the first step. This workflow allows you to set up authorization settings only once, when configuring a TeamCity-to-VCS connection, and then reuse this connection as often as needed.

Further reading: [](configuring-connections.md)


### Option 2: From a Third-Party Public Repository

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

3. The next page allows you to set up basic project settings:

    * Project and build configuration names are public names in TeamCity.
    * Default branch is the specification for the primary repository branch.
    * Branch specification is a pattern that specifies which branches TeamCity should track. The default `refs/heads/*` value allows TeamCity to monitor all regular branches.

    You can leave the default values for all fields and click **Proceed** to continue to the [](#Set+Up+a+Build+Configuration) stage.




## Set Up a Build Configuration


A project does not perform any building routines, all build actions are configured inside a build configuration. For that reason, TeamCity creates an empty build configuration (with the default "Build" name) along with any new project.

After you set core project and configuration settings at the end of the [](#Create+a+TeamCity+Project) stage, TeamCity brings you to the **Build Steps** page of build configuration settings. TeamCity scans the remote repository to identify the application type and suggest corresponding build steps.

<img src="CreateProject3.png" alt="Create a project from a repository URL, Step 3" width="706"/>

You can check the suggested step and click **Use selected**, or add your custom steps:

1. Navigate to configuration settings (**Administration | &lt;Your Configuration&gt;**) and click **Build Steps** in the side menu.
2. Click **Add build step** and choose a required step type. Since this sample tutorial targets the Java application with Maven, choose "Maven" from the list and type a required command in the **Goals** field (for example, `clean test`).

    > You can perform almost any build action using the **Command Line** step, which runs custom scripts on the agent machine's terminal. For example, creating a CLI step with the command `mvn clean test` in the **Custom script** field will perform the same actions as a Maven step.
    > 
    > Command Line steps are ideal for basic system operations (like creating or deleting files and folders) and interacting with custom software installed on agent machines. However, if you're using build tools that have corresponding TeamCity steps (such as Maven, Gradle, .NET, Ant, Node.js, and so on), we recommend using those specific steps. They are tailored to match the build tool and offer unique settings that simplify configuration.
    {style="tip"} 

3. In step settings, click **Show advanced settings** to view available options. For example, you may want to change [step execution conditions](build-step-execution-conditions.md). By default, steps are executed one after another and if one step fails, the following steps are automatically skipped. This setting allows you to change this default behavior.

Further reading: [](configuring-build-steps.md)

## Change VCS Root Settings



## Build New Changes Automatically

### Triggers

### Webhooks


## Configure Additional Build Features


## Working with Pull Requests


## Choose Agents to Run Your Builds


## Configure Project Members


## Set Up Notifications


## Store Project Settings in a VCS














In TeamCity terms, a _build_ is a process that consists of one or more steps and performs a certain CI/CD job.

After you have installed and started TeamCity as described [here](quick-setup-guide.md), you are ready to configure and run your first build.
{instance="tc"}

After you have started TeamCity as described [here](getting-started-with-teamcity-cloud.md#2.+Start+TeamCity), you are ready to configure and run your first build.
{instance="tcc"}

<img src="run-first-build.png" width="611" alt="Run your first build"/>

You can watch a quick video guide or read the full tutorial below.

<video src="https://youtu.be/SYjnb7pW4Cg"
       title="Running your first build in TeamCity"/>

## Create your first project

There are several ways to create a project in TeamCity: automatically from a repository URL, from a connection to a specific VCS, or manually. In this tutorial, we will focus on the default and easiest way – _from a repository URL_. You will only need to provide a path to your repository — TeamCity will scan it and suggest some settings and potential build steps.

>__Sample Repository__  
>To try out the setup flow with a sample project, you can use this [GitHub repository](https://github.com/mkjetbrains/SimpleMavenSample).  
>Or, you can configure the first build based on your own project's sources. In this case, if TeamCity autodetects any build steps in it, we recommend that you read about [their available settings](configuring-build-steps.md) before running the build.
>
{style="note"}

Every TeamCity installation has the default __Root__ project that will contain all the other projects you create. The first project you create will be added as a child of the __Root__ project. To add your first project:

1. Click __Administration__ in the upper right corner of the TeamCity UI and then click __Create project__.
2. On the __Create Project__ page, click __From a repository URL__ and paste the URL of your project's source repo as _Repository URL_. It can be a Git, Subversion, Perforce, Azure, or Mercurial repository. All supported URL formats are listed [here](guess-settings-from-repository-url.md#VCS+URL+Formats). To use our sample repo, enter:
   ```Shell
   https://github.com/mkjetbrains/SimpleMavenSample
   ```
   If access to your repository is restricted, enter the credentials as well.
   <img src="CreateProject1.png" dark-src="CreateProject1_dark.png" alt="Create a project from a repository URL, Step 1" width="706" border-effect="line" style="block"/>
3. Click __Proceed__ and follow the wizard.   
   TeamCity will identify the type of your repository, test the connection, and autoconfigure the repository settings, as well as suggest the project and build configuration names.
   <img src="CreateProject2.png" alt="" width="706" border-effect="line" style="block"/>
4. Click __Proceed__.   
   TeamCity will scan your VCS repository and autodetect the [build steps](configuring-build-steps.md).
5. Check the boxes of the suitable steps, and they will be added to the first build configuration of this project.   
   <img src="CreateProject3.png" alt="Create a project from a repository URL, Step 3" width="706" border-effect="line" style="block"/>

Congratulations! You are now ready to run the first build based on the just created build configuration. You can go straight to [running it](#Run+your+first+build) and [tweak its settings](#Tweak+your+build+configuration+settings) afterwards.

## Run your first build
To run builds in TeamCity, you need [build agents](build-agent.md). A fresh TeamCity server, installed as described [here](quick-setup-guide.md), has one registered build agent that runs on the same computer. Let's use this agent to run a build on the sample project.
{instance="tc"}

On the __Build Configuration Settings__ page, click __Run__ in the upper right corner:

<img src="RunBuild.png" alt="Run a build" width="706" border-effect="line"/>

TeamCity will always assign a build to the first available and [suitable](agent-requirements.md) build agent.

You will be automatically redirected to the __Build Results__ page, where you can watch the build progress and review its results upon the build finish. You can also access your build configuration settings from this page and edit them as necessary:

<img src="BuildResults.PNG" alt="Build results" width="706" border-effect="line"/>

## Tweak your build configuration settings

You might want to configure the following settings first:
* paths to build [artifacts](#Artifacts)
* [automatic triggers](#Automatic+Build+Trigger)
* a custom pattern for a [build number](#Build+Number+Format)

For other settings, see this [chapter](creating-and-editing-build-configurations.md).

### Artifacts

If your build produces installers, WAR files, reports, log files, and so on, you might want to publish them on the TeamCity server after finishing the build. You can specify the paths to such artifacts in __Build Configuration Settings | General Settings__. As you already have a finished build, the build agent has checked out the sources already. Next to the _Artifact paths_ field, click the tree icon to open the checkout directory browser. You can select artifacts from this tree:

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

Each build in TeamCity has a build number, that is a string identifier. It is composed according to the pattern defined in __Build Configuration Settings | General Settings__ (click _Show advanced options_ to display it). If you leave the default value, the build number format will be maintained by TeamCity; the number will be resolved into a next integer value on each new build start. Or, you can customize the pattern as described [here](configuring-general-settings.md#Build+Number+Format).

## Takeaway

To configure a certain CI/CD job in TeamCity:
1. Create a project from your source repository and adjust its main settings.
2. Create a build configuration inside this project.
3. In the build configuration settings, add build steps that will represent stages of the build.
4. Set up other configurations settings. For example, add handy build features and automatic triggers.

After that, you can run a build based on the created configuration manually, or wait until it is triggered automatically, if any triggers are configured.