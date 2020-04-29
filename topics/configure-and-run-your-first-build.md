[//]: # (title: Configure and Run Your First Build)
[//]: # (auxiliary-id: Configure and Run Your First Build)

After you have installed and started TeamCity as described [here](installing-and-configuring-the-teamcity-server.md), the server is accessible locally on the default port (on Windows as [`http://localhost/`](http://localhost:8111/), and on Linux/OS X as [`http://localhost:8111/`](http://localhost:8111/)) and has one registered build agent that runs on the same computer.

Now we can get building!

## Create your first project

In TeamCity, there is the default \<Root project\> containing all other projects in TeamCity. To create a project, click the __Administration__ link in the upper right corner and then click __Create project__. The __Create Project__ page is displayed.

There are several options to create a project:
* [From a repository URL](#Create+a+project+from+a+repository+URL) (default)
* From an existing [connection](integrating-teamcity-with-vcs-hosting-services.md#Configuring+Connections) to an external repository:
    * __GitHub.com__ (see example [below](#Create+a+project+pointing+to+GitHub.com+repository))
    * __Bitbucket Cloud__
    * __GitLab CE/EE__ and __GitLab.com__
    * __Azure DevOps__, or formerly __Visual Studio Team Services__
* [Manually](#Create+a+project+manually)

### Create a project from a repository URL

This is the fastest way to create your first build.

1. On the __Create Project__ page, click __From a repository URL__ and paste the URL of your project's repository into the _Repository URL_ field. The supported URL formats are listed in [VCS URL Formats](guess-settings-from-repository-url.md#VCS+URL+Formats).   
If required, specify your repository credentials.   
   <img src="CreateProject1.png" alt="Create a project from a repository URL, Step 1" width="1069"/>
2. Click __Proceed__ and follow the wizard.   
   TeamCity will do the rest for you: it will identify the type of the VCS repository, test the connection, and autoconfigure VCS repository settings, as well as suggest the project and build configuration names.   
   <img src="CreateProject2.png" alt="Create a project from a repository URL, Step 2" width="750"/>
3. Click __Proceed__.   
   TeamCity will scan your VCS repository and autodetect your build steps.
4. Check the boxes of the steps and use them in your build configuration.   
   <img src="CreateProject3.png" alt="Create a project from a repository URL, Step 3" width="925"/>   
   The selected build step is added to the build configuration.

Congratulations! You've configured your first build containing one build step. Now you can [run your build](#Run-your-first-build) and [tweak its settings](#Tweak-your-build-configuration-settings) if necessary.

### Create a project pointing to GitHub.com repository

<tip>

If a parent project of the project to be created is already integrated with GitHub via an existing [connection](integrating-teamcity-with-vcs-hosting-services.md#Configuring+Connections), the quick option __From GitHub.com__ appears. Click it to create a project from the predefined GitHub connection URL, similarly to [creating a project from a repository URL](#Create+a+project+from+a+repository+URL).

</tip>


1. On the __Create Project__ page, click __From a repository URL__ and then click the GitHub icon next to the _Repository URL_ field.   
Note that if one or more [connections](integrating-teamcity-with-vcs-hosting-services.md#Configuring+Connections) to GitHub are predefined for the parent project, TeamCity will suggest picking one of these connections to create a project from it. If not, proceed with Step 2.
2. The __Connections__ page with the __Add connection__ dialog opens. It provides the parameters to be used when registering your TeamCity application in GitHub service.   
Click the _register TeamCity_ link.   
   <img src="add-github-connection.png" alt="Add a GitHub connection, Step 1" width="584"/>
3. The GitHub page opens.   
   You need to register TeamCity as an [OAuth application](https://developer.github.com/v3/oauth/) in GitHub. The following steps are performed in your GitHub account:
   * Sign into your GitHub account. On the __Register a new OAuth application__ page specify the name (and an optional description), homepage URL, and callback URL as provided by TeamCity. Use the ![fromGH0_Copy.png](fromGH0_Copy.png) icon to copy the required parameters.   
      <img src="CreateGHConnectionOAuth.png" alt="Register a new OAuth application" width="700"/>
   * Click __Register application__.
   * Scroll down and click __Update application__.   
   The page is updated with the Client ID and the client secret information for your TeamCity application.
4. Continue configuring the connection in TeamCity: on the __Add Connection__ page, specify the Client ID and the client secret.   
   <img src="CreateGHConnectiontokens.png" alt="Add a GitHub connection, Step 2" width="601"/>
5. Save your settings.
6. Next you need to authorize TeamCity in the VCS: go to the Project administration, click __Create project__, select __From GitHub.com__, and click  __Sign in to GitHub__:   
   <img src="CreateGHsignIntoGH.png" alt="Sign in to GitHub" width="938"/>
7. On the page that opens, authorize the TeamCity application.   
The authorized application will be granted full control of private repositories and the _Write repository hooks_ permission in GitHub.   
The connection is configured: you can continue with creating your project in TeamCity. All the repositories available to the user will be listed. Start typing to filter the list and select the required repository:   
   <img src="CreateGHConnectionProjects.png" alt="Choose a repository" width="1030"/>   
   TeamCity will verify the repository connection. If the сonnection is verified, the __Create Project__ page opens. TeamCity will display the project and build configuration name. If required, modify the names and click __Proceed__.  
8. TeamCity will scan your VCS repository and autodetect your build steps (it may take some time). Check the boxes of the steps and use them in your build configuration.
9. The selected build step is added to the build configuration.

Congratulations! You've configured the GitHub connection and your first build containing one build step. Now you can [run your build](#Run-your-first-build) and [tweak its settings](#Tweak-your-build-configuration-settings) if necessary.

<tip>

When the connection is configured, a small GitHub icon becomes active in several places of the UI where a repository URL can be specified (when creating a [project from URL](creating-and-editing-projects.md#Creating+project+pointing+to+repository+URL), build configuration from URL, [VCS root from URL](guess-settings-from-repository-url.md), [Git](git.md) VCS root, or [GitHub](github.md) issue tracker for the current project and all of its subprojects), making it easier to create these entities in TeamCity.
</tip>

### Create a project manually

You can create a project __manually__ if autodetection of settings is not suitable for you.
1. Click the __Administration__ link in the upper right corner, then click __Create project__ and select __Manually__. Specify the project's name, [ID](identifier.md) (autogenerated, modifiable), and an optional description.   
Click __Create__:   
 <img src="createProjectManually.png" alt="Create a project manually, Step 1" width="1310"/>
2. When a project is created, TeamCity prompts to populate it with build configurations. Click __Create build configuration__.   
The configurations can be created automatically (similarly to creating projects) or manually as described below. If you select to do it manually, specify the build configuration name, [ID](identifier.md) (autogenerated, modifiable), and an optional description.   
 <img src="createdBC.png" alt="Create a project manually, Step 2" width="1270"/>
3. TeamCity offers to create and attach a __new VCS Root__: to be able to create a build, TeamCity has to know where the source code resides, and a [VCS root](vcs-root.md) is a collection of VCS settings (paths to sources, login, password, and other settings) that defines how TeamCity communicates with a version control (SCM) system to monitor changes and get sources for a build. Each build configuration has to have at least one VCS root attached to it. A VCS root can be created [automatically](guess-settings-from-repository-url.md) or manually.   
 To create it manually, select a type of VCS from the drop-down menu (_Git_ in the example below), specify the required information (name and URL), test your connection, and click __Create__.   
 <img src="createdVCSRoot.png" alt="Create a project manually, Step 3" width="826"/>   
   After you have created a VCS root, you can instruct TeamCity to exclude some directories from checkout, or map some paths (copy directories and all their contents) to a location on the build agent different from the default one. This can be done by means of [checkout rules](vcs-checkout-rules.md).   
   You can also specify whether you want TeamCity to checkout the sources [on the agent or server](vcs-checkout-mode.md). Note that the agent-side checkout is [supported not for all VCSs](vcs-checkout-mode.md), and in case you want to use it, you need to have a version control client installed on at least one agent.   
    After the VCS Root is created, the build configuration settings are displayed on the left.
4. Now you can configure [Build steps](configuring-build-steps.md) by selecting the corresponding setting on the left. You can either instruct TeamCity to automatically detect the build steps after scanning your repository or configure build steps manually as described in this example.   
 Click __Add build step__ and select a build runner from the drop-down menu.   
 <img src="NewBuildStep.png" alt="Selecting a build step" width="827"/>
5. Fill in the required fields and save the build step.

<tip>
    
If your project resides in several version control systems, you can attach as many VCS Roots to it as you need. For example, if you store a part of your project in Perforce, and the rest in Git, you need to create and attach two VCS roots: one for Perforce, another for Git. [Learn more about configuring VCS roots](configuring-vcs-roots.md).
</tip>

Congratulations! You've configured your first build containing one build step. Now you can [run your build](#Run-your-first-build) and [tweak its settings](#Tweak+your+build+configuration+settings) if necessary.

## Run your first build

Your build configuration currently has one build step, and you can now launch your first build by clicking __Run__ in the upper right corner of the TeamCity web UI:

<img src="RunBuild.png" alt="Run a build" width="750"/>

You will be redirected to the build result page, where you can watch the build progress and review its results upon the build finishing. You can also access your build configuration settings from this page and edit them as required:

<img src="BuildResults.PNG" alt="Build results" width="750"/>

<tip>

You can add as many steps as you like and reorder them if required. You can add steps manually or ask TeamCity to detect them automatically. TeamCity will also suggest settings, such as triggers, failure conditions, and build features. Depending on the build configuration settings, it may prompt some additional options. Follow the suggestions and add the settings to configure your build.   
You can always tweak the settings after running your first build.
</tip>
 

## Tweak your build configuration settings

You might want to configure the following settings:
* [artifacts](#Artifacts)
* [automatic build trigger](#Automatic+Build+Trigger)
* [build number](#Build+Number+Format)

### Artifacts

If your build produces installers, WAR files, reports, log files, and so on, and you want them to be published on the TeamCity server after finishing the build, you can specify the paths to such artifacts in the __General Settings__ section of the build configuration settings. As you have a finished build, the build agent has the checkout sources already and the _Artifact paths_ field has the checkout directory browser. You can select artifacts from the tree:

<img src="artifactPaths.png" alt="Artifact paths" width="750"/>

TeamCity will place the paths to them into the input field, so you can modify them as needed:

<img src="artifactPathsArchive.png" alt="Modify the arifact paths" width="750"/>

Save your settings. Now when you run a build, TeamCity will put the required reports into an archive. The build configuration home page lists all builds that were run and enables you to view the available artifacts:

<img src="artifactsPublished.png" alt="Published artifacts" width="750"/>

You can also view and download artifacts from the build results page:

<img src="artifactsPublishedResults.png" alt="Artifacts in build results" width="750"/>

More details are available in [Artifact Paths](configuring-general-settings.md#Artifact+Paths).

### Automatic Build Trigger

Automatic build triggering on detecting a change in the version control is essential to any CI. TeamCity will add a [VCS trigger](configuring-vcs-triggers.md) automatically when creating a project / build configuration from a URL or repository. You can also do it manually using the _Edit Build Configuration Settings_ drop-down men, the __Triggers__ page:

<img src="addTrigger.png" alt="Add a trigger" width="750"/>

### Build Number Format

Each build in TeamCity has a build number, which is a string identifier composed according to the pattern specified on the __General Settings__ page of your build configuration (the field is available on clicking the _Show advanced options_ link). You can leave the default value here, in which case the build number format will be maintained by TeamCity and will be resolved into a next integer value on each new build start. More details are available in the [dedicated section](configuring-general-settings.md#Build+Number+Format) of our documentation.

__ __