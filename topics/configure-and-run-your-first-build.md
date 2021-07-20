[//]: # (title: Configure and Run Your First Build)
[//]: # (auxiliary-id: Configure and Run Your First Build)

After you have installed and started TeamCity as described [here](quick-setup-guide.md), you are ready to configure and run your first build.
{product="tc"}

After you have started TeamCity as described [here](getting-started-with-teamcity-cloud.md#2.+Start+TeamCity), you are ready to configure and run your first build.
{product="tcc"}

<img src="run-first-build.png" width="611" alt="Run your first build"/>

You can watch a quick video guide or read the full tutorial below.

<video href="SYjnb7pW4Cg"
       title="Running your first build in TeamCity"/>

## Create your first project

Every TeamCity installation has the default __Root__ project that will contain all the other projects you create. To add your first project, click __Administration__ in the upper right corner of the TeamCity UI and then click __Create project__.

There are several options to create a project in TeamCity:
* [From any repository URL](#Create+project+from+repository+URL) (default)
* From a repository of a specific VCS:
    * __GitHub.com__ and __GitHub Enterprise__
    * __Bitbucket Cloud__
    * __GitLab CE/EE__ and __GitLab.com__
    * __Azure DevOps__, or formerly __Visual Studio Team Services__  
  
   You can find the example for GitHub.com [below](#Create+project+pointing+to+GitHub.com+repository). Connection settings for other VCSs are detailed [here](integrating-teamcity-with-vcs-hosting-services.md).
* [Manually](#Create+project+manually)

In the first two scenarios, TeamCity will automatically suggest some settings and scan your repo to identify potential build steps.

>__Sample Repository__  
>To try out the setup flow with a sample project, you can use this [GitHub repository](https://github.com/mkjetbrains/SimpleMavenSample).  
>You can also configure the first build based on your own project's sources. In this case, if TeamCity autodetects any build steps in it, we recommend that you read about [their available settings](configuring-build-steps.md) before running the build.
>
{type="note"}

### Create project from repository URL

This is the fastest way to create a project.

1. On the __Create Project__ page, click __From a repository URL__ and paste the URL of your project's source repository as _Repository URL_. The supported URL formats are listed [here](guess-settings-from-repository-url.md#VCS+URL+Formats).   
If access to your repository is restricted, enter the credentials as well.   
   <img src="CreateProject1.png" alt="Create a project from a repository URL, Step 1" width="1069"/>
2. Click __Proceed__ and follow the wizard.   
   TeamCity will identify the type of your repository, test the connection, and autoconfigure the repository settings, as well as suggest the project and build configuration names.   
   <img src="CreateProject2.png" alt="Create a project from a repository URL, Step 2" width="750"/>
3. Click __Proceed__.   
   TeamCity will scan your VCS repository and autodetect the [build steps](configuring-build-steps.md).
4. Check the boxes of the suitable steps, and they will be added to the build configuration.   
   <img src="CreateProject3.png" alt="Create a project from a repository URL, Step 3" width="925"/>

Congratulations! You are now ready to run the first build based on the just created build configuration. You can go straight to [running it](#Run+your+first+build) or [tweak its settings](#Tweak+your+build+configuration+settings) beforehand.

### Create project pointing to GitHub.com repository

The most convenient way to create projects is to integrate TeamCity with your VCS by a dedicated [connection](integrating-teamcity-with-vcs-hosting-services.md) preset. All connections of a parent project are available to all its subprojects. That is, if you configure a connection to GitHub.com in the Root project settings, you will be able to instantly create projects from GitHub.com repos you have access to.

>In TeamCity Cloud, you can skip creating a connection for GitHub.com, GitLab.com, and Bitbucket Cloud, as these are already predefined in the Root project's settings.
>
{product="tcc"}

Let's start with creating a connection:

1. On the __Create Project__ page, click __From a repository URL__ and then click the GitHub icon next to the _Repository URL_ field.
2. The __Add Connection__ dialog opens (you can also open it from __Project Settings | Connections__).  
   Here, you need to enter a client ID and secret for accessing your GitHub repo. But to get these parameters, you first need to register a new OAuth application for TeamCity in GitHub. Click _register TeamCity_ to do this.   
   <img src="add-github-connection.png" alt="Add a GitHub connection, Step 1" width="584"/>
3. The __Register a new OAuth application__ page opens in GitHub. Make sure you are signed in as a GitHub user and follow these steps:
   * Specify the app name and homepage/callback URLs as declared in TeamCity. Click ![fromGH0_Copy.png](fromGH0_Copy.png) to copy each parameter from TeamCity.   
      <img src="CreateGHConnectionOAuth.png" alt="Register a new OAuth application" width="700"/>
   * Click __Register application__.
   * Scroll down and click __Update application__.   
   You will see the client ID and secret granted to your new TeamCity application.
4. Go back to TeamCity and, in the __Add Connection__ form, enter the granted client ID and secret.   
   <img src="CreateGHConnectiontokens.png" alt="Add a GitHub connection, Step 2" width="601"/>
5. Save your settings.
6. Go back to __Administration | Projects__ and click __Create project__.
7. Select __From GitHub.com__ and click  __Sign in to GitHub__:   
   <img src="CreateGHsignIntoGH.png" alt="Sign in to GitHub" width="938"/>
8. On the opened page, authorize the TeamCity application. The authorized application will be granted full control of private repositories and the _Write repository hooks_ permission in GitHub.

>A handy GitHub icon will now become active in several places of the UI. Click it to quickly select a GitHub repository URL. It is available when creating a [project](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL) or build configuration from URL, a [VCS root from URL](guess-settings-from-repository-url.md), or integrating with a [GitHub](github.md) issue tracker.

The connection is now configured — you can continue with creating your project. After the app is authorized, TeamCity will list all the repositories available to your GitHub user. Start typing to filter the list and select the required repository:   
   <img src="CreateGHConnectionProjects.png" alt="Choose a repository" width="1030"/>   

After you select the repo, TeamCity will verify the connection to it and open the __Create Project__ page. It will propose the project and build configuration name. If necessary, you can modify them and click __Proceed__.

Then, it will scan your VCS repository and autodetect the potential build steps (it may take some time). Check the boxes of the suitable steps, and they will be added to the build configuration.

Congratulations! You are now ready to run the first build based on the just created build configuration. You can go straight to [running it](#Run+your+first+build) or [tweak its settings](#Tweak+your+build+configuration+settings) beforehand.

### Create project manually

If you don't want TeamCity to autodetect build steps, you can create a project manually.

1. On the __Create Project__ page, click __Manually__. Specify the project's name, [ID](identifier.md) (autogenerated but modifiable), and an optional description.  
 <img src="createProjectManually.png" alt="Create a project manually, Step 1" width="1310"/>
2. Click __Create__.  
   TeamCity will prompt to add at least one build configuration. Click __Create build configuration__.   
The configurations can be created automatically (similarly to creating projects) or manually as described below. If you select to do it manually, specify the build configuration name, [ID](identifier.md), and an optional description.   
 <img src="createdBC.png" alt="Create a project manually, Step 2" width="1270"/>
3. TeamCity will offer to create and attach a new VCS root.  
   
   >To be able to create a build, TeamCity needs to know where the source code resides. A [VCS root](vcs-root.md) is a collection of VCS settings (paths to sources, login, password, and so on) that define how TeamCity communicates with a VCS to monitor changes and get sources for a build. Each build configuration has to have at least one VCS root attached to it.  
    
   A VCS root can be created [automatically](guess-settings-from-repository-url.md) or manually. To create it manually, select a type of VCS from the drop-down menu (_Git_ in the example below), specify the required information (name and URL), test your connection, and click __Create__.   
 <img src="createdVCSRoot.png" alt="Create a project manually, Step 3" width="826"/>   
4. Optionally, configure [checkout rules](vcs-checkout-rules.md). They can instruct TeamCity to exclude some directories from the checkout, or, if necessary, change the location where the checked out sources are stored on the [build agent](build-agent.md).  
   You can also specify whether you want TeamCity to check out the sources [on the agent or server](vcs-checkout-mode.md). If you want to use agent-side checkout, note that it is [not supported for some VCSs](vcs-checkout-mode.md), and that you need to have a version control client installed on at least one agent.
5. After the VCS root is created, TeamCity will display the build configuration settings. The first thing to do here is to configure [build steps](configuring-build-steps.md). You can either instruct TeamCity to scan your repository and detect them automatically, or configure them manually as described in this example.   
6. Click __Add build step__ and select a convenient build runner from the drop-down menu.   
 <img src="NewBuildStep.png" alt="Selecting a build step" width="827"/>
7. Fill in the required fields and save the build step.
    
>If your project resides in several version control systems, you can attach as many VCS roots to it as you need. For example, if you store a part of your project in Perforce and the rest in Git, you need to create and attach two VCS roots: one for Perforce, another for Git. [Learn more about configuring VCS roots](configuring-vcs-roots.md).

Congratulations! You are now ready to run the first build based on the just created build configuration. You can go straight to [running it](#Run+your+first+build) or [tweak its settings](#Tweak+your+build+configuration+settings) beforehand.

## Run your first build

A fresh TeamCity server, installed as described [here](quick-setup-guide.md), has one registered [build agent](build-agent.md) that runs on the same computer. Agents are responsible for running TeamCity builds, so let's use it to run the first one, based on our recently created configuration.
{product="tc"}

On the __Build Configuration Settings__ page, click __Run__ in the upper right corner:

<img src="RunBuild.png" alt="Run a build" width="750"/>

TeamCity will always assign a build to the first available and [suitable](agent-requirements.md) [build agent](build-agent.md).

You will be automatically redirected to the __Build Results__ page where you can watch the build progress and review its results upon the build finish. You can also access your build configuration settings from this page and edit them as required:

<img src="BuildResults.PNG" alt="Build results" width="750"/>

>Try our [experimental UI](teamcity-experimental-ui.md) for a more modern interface of __Build Results__. This mode also provides better visualization and an improved build log preview.

## Tweak your build configuration settings

You might want to configure the following settings first:
* paths to build [artifacts](#Artifacts)
* how to [trigger builds automatically](#Automatic+Build+Trigger)
* set a custom pattern for a [build number](#Build+Number+Format)

For other settings, see this [chapter](creating-and-editing-build-configurations.md).

### Artifacts

If your build produces installers, WAR files, reports, log files, and so on, you might want to publish them on the TeamCity server after finishing the build. You can specify the paths to such artifacts in __Build Configuration Settings | General Settings__. As you already have a finished build, the build agent has checked out the sources already. Next to the _Artifact paths_ field, click the tree icon to open the checkout directory browser. You can select artifacts from this tree:

<img src="artifactPaths.png" alt="Artifact paths" width="750"/>

TeamCity will place their paths into the text field, so you can modify them if necessary:

<img src="artifactPathsArchive.png" alt="Modify the artifact paths" width="750"/>

Save the build configuration settings. Now, when you run a build, TeamCity will put all the specified reports into an archive and publish them.

The __Build Configuration Home__ page lists all run builds and allows viewing their artifacts:

<img src="artifactsPublished.png" alt="Published artifacts" width="750"/>

You can also view and download artifacts from the __Build Results__ page:

<img src="artifactsPublishedResults.png" alt="Artifacts in build results" width="750"/>

Read more details [here](configuring-general-settings.md#Artifact+Paths).

### Automatic Build Trigger

Automatic build triggering on a change in the repository is essential to any CI. TeamCity will add a [VCS trigger](configuring-vcs-triggers.md) automatically when creating a project / build configuration from a repository URL. You can also add it manually on __Build Configurations Settings | Triggers__ page:

<img src="addTrigger.png" alt="Add a trigger" width="750"/>

>TeamCity provides multiple [types of triggers](configuring-build-triggers.md) — learn about them all to optimize your pipeline.

### Build Number Format

Each build in TeamCity has a build number, that is a string identifier. It is composed according to the pattern defined in __Build Configuration Settings | General Settings__ (click _Show advanced options_ to display it). If you leave the default value, the build number format will be maintained by TeamCity; the number will be resolved into a next integer value on each new build start. Or, you can customize the pattern as described [here](configuring-general-settings.md#Build+Number+Format).