[//]: # (title: Configure and Run Your First Build)
[//]: # (auxiliary-id: Configure and Run Your First Build)

In TeamCity terms, a _build_ is a process that consists of one or more steps and performs a certain CI/CD job.

After you have installed and started TeamCity as described [here](quick-setup-guide.md), you are ready to configure and run your first build.
{product="tc"}

After you have started TeamCity as described [here](getting-started-with-teamcity-cloud.md#2.+Start+TeamCity), you are ready to configure and run your first build.
{product="tcc"}

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
   <img src="CreateProject1.png" dark-src="CreateProject1_dark.png" alt="Create a project from a repository URL, Step 1" width="706" border-effect="line"/>
3. Click __Proceed__ and follow the wizard.   
   TeamCity will identify the type of your repository, test the connection, and autoconfigure the repository settings, as well as suggest the project and build configuration names.
   <img src="CreateProject2.png" alt="" width="706" border-effect="line"/>
4. Click __Proceed__.   
   TeamCity will scan your VCS repository and autodetect the [build steps](configuring-build-steps.md).
5. Check the boxes of the suitable steps, and they will be added to the first build configuration of this project.   
   <img src="CreateProject3.png" alt="Create a project from a repository URL, Step 3" width="706" border-effect="line"/>

Congratulations! You are now ready to run the first build based on the just created build configuration. You can go straight to [running it](#Run+your+first+build) and [tweak its settings](#Tweak+your+build+configuration+settings) afterwards.

## Run your first build
To run builds in TeamCity, you need [build agents](build-agent.md). A fresh TeamCity server, installed as described [here](quick-setup-guide.md), has one registered build agent that runs on the same computer. Let's use this agent to run a build on the sample project.
{product="tc"}

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