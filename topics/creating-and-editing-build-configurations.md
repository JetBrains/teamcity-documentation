[//]: # (title: Creating and Editing Build Configurations)
[//]: # (auxiliary-id: Creating and Editing Build Configurations)

This page details creating build configurations via the TeamCity UI.

>Build configurations can also be created via [TeamCity REST API](https://www.jetbrains.com/help/teamcity/rest/get-build-details.html#Get+Specific+Builds) and as a versioned [Kotlin DSL code](storing-project-settings-in-version-control.md).

<img src="create-build-configuration-button.png" alt="Create a build configuration" width="750"/>

## Where to Create Build Configuration

To find the build configuration creation wizard:
1. Go to __Administration | Projects__ and open the required project.  
   Alternatively, open the project using the __Projects__ pop-up menu and click __Edit Project Settings__.  
   The __Project Settings__ page will open.
2. On the __Project Settings__ page, click __Create build configuration__ under the __Build Configurations__ section.
3. Follow the instructions specific to your preferred method:
   * [manually](#Creating+Build+Configuration+Manually)
   * [pointing to a repository URL](#Creating+Build+Configuration+from+URL)
   * pointing to a repository in GitHub.com, GitLab CE/EE, Bitbucket Cloud, Azure DevOps Services, and JetBrains Space
   * based on a [build configuration template](#Creating+Build+Configuration+Template)

<img src="create-build-configuration.png" alt="Create a build configuration from a repository URL" width="828"/>

After build configurations are created, you can:
* [change their order](#Ordering+Build+Configurations)
* [edit their settings](#Configuring+Settings)
* control them via the [Actions](#Actions+in+Build+Configuration+Settings) menu

## Creating Build Configuration from URL

In the [build configuration creation wizard](#Where+to+Create+Build+Configuration):
1. Click __From a repository URL__.
2. Enter your VCS repository URL and, if authentication is required, VCS credentials (username and password/token).
3. Click __Create__.

TeamCity will suggest the build configuration name and will configure the rest of the settings for you:
* It will determine the type of the VCS repository and create a [VCS root](vcs-root.md). For a Git repository, it will autodetect the default branch. You have an option to change it now or later, in the [VCS root](vcs-root.md) settings. You can also change the branch specification: by default, TeamCity monitors all branches of the repository, but you can choose what exact branches to monitor by entering [custom rules](working-with-feature-branches.md#branch-spec-syntax).
* It will attempt to autodetect build steps: Ant, NAnt, Gradle, Maven, MSBuild, Visual Studio solution files, PowerShell, Xcode project files, Rake, and IntelliJ IDEA projects. You can then always add and edit them [manually](configuring-build-steps.md).

Next, TeamCity will display suggestion icons with prompts to create [build triggers](configuring-build-triggers.md), [failure conditions](build-failure-conditions.md), and [build features](adding-build-features.md). Depending on the build configuration settings, it may suggest some additional configuration options.

After the build configuration is created, you can run a build and/or tweak the settings.

## Creating Build Configuration Pointing to Specific VCS

In the [build configuration creation wizard](#Where+to+Create+Build+Configuration):
1. Click the button respective to your VCS: from GitHub.com, GitLab CE/EE, Bitbucket Cloud, Azure DevOps Services, or JetBrains Space.  
   Note that to be able to access the selected VCS, TeamCity needs to always have the connection parameters at its disposal. You can configure the connection preset in advance and select the target VCS server in the wizard, or let TeamCity redirect you to the __Connections__ page right from the wizard. The connections settings of supported VCS providers are described [here](configuring-connections.md).
2. Verify the connection to the VCS.
3. TeamCity will propose the build configuration name. You can change it if needed.  
   For a Git repository, it will also autodetect the default branch. You have an option to change it now or later, in the [VCS root](vcs-root.md) settings. You can also change the branch specification: by default, TeamCity monitors all branches of the repository, but you can choose what exact branches to monitor by entering [custom rules](working-with-feature-branches.md#branch-spec-syntax).  
   Click __Proceed__.
4. TeamCity will add a VCS build trigger and attempt to [autodetect build steps](configuring-build-steps.md#Autodetecting+build+steps): Ant, NAnt, Gradle, Maven, MSBuild, Visual Studio solution files, PowerShell, Xcode project files, Rake, and IntelliJ IDEA projects. On the __Auto-detected Build Steps__ page, select the step(s) to use in your build configuration. Click __Use selected__. You can then always add or edit steps [manually](configuring-build-steps.md).  
  Depending on the build configuration settings, TeamCity can suggest some additional configuration options. Review the suggested settings ![suggestedSettings.PNG](suggestedSettings.PNG) and configure the required ones.

The build configuration is created. Click __Run__ to start its first build.

## Creating Build Configuration Manually

In the build configuration creation wizard:
1. Click __Manually__
2. In the _Create Build Configuration_ dialog, specify the name, ID and (optionally) description for the build configuration, click __Create__.
3. Proceed with creating other settings:
   * [Create/edit VCS roots and specify VCS-specific settings](configuring-vcs-settings.md)
   * On the __Build Steps__ page, configure build steps discovered by the automatic detection. To create them manually by selecting a desired build runner from the drop-down menu. Click __Save__. You can add as many build steps as you need within one build configuration. Note that they will be executed sequentially. In the end, the build gets the merged status and the output goes into the same build log. If some step fails, the rest is executed or not, depending on their [step execution policy](configuring-build-steps.md#Execution+policy).
   * Additionally, configure [build triggering options](configuring-build-triggers.md), [dependencies](configuring-dependencies.md), [properties and variables](configuring-build-parameters.md), and [agent requirements](configuring-agent-requirements.md).

## Creating Build Configuration Template

Creating a new [build configuration template](build-configuration-template.md) is similar to creating a new configuration:
1. On the __Administration | Projects__ page, select the target project in the list.  
  Alternatively, open the project using the __Projects__ pop-up menu and click __Edit Project Settings__.  
  The __Project Settings__ page opens.
2. On the __Project Settings__ page, click __Create template__ under the __Build Configuration Templates__ section.
3. Proceed with configuring [general settings](configuring-general-settings.md), [VCS settings](configuring-vcs-settings.md), and [build steps](configuring-build-steps.md).

Alternatively, you can create a build configuration template from an existing build configuration:
1. Open the existing build configuration settings page, click __Actions__ in the upper right corner of the screen, and select the __Extract Template__ option.
2. Specify the required settings and click __Create__.

## Creating Build Configuration from Template

To create a build configuration based on a template:
1. On the __Administration | Projects__ page, select the target project in the list.  
   Alternatively, open the project using the __Projects__ pop-up menu and click __Edit Project Settings__.  
   The __Project Settings__ page opens.
2. On the __Project Settings__ page, click the required template in the __Build Configuration Templates__ section.
3. Click __Actions__ in the upper right corner of the screen and then click __Create Build Configuration__.
4. Specify the required settings for the new configuration.

Alternatively to steps 2-4, you can:
1. On the __Project Settings__ page, click __Create build configuration__ under the __Build Configurations__ section.
2. In the configuration settings, open the __Based on template__ drop-down menu and select the required template.

<note>

The settings specified in the template cannot be edited in a configuration created from this template. However, some of them can be [redefined in a build configuration](build-configuration-template.md#Redefining+settings+inherited+from+template). Note that modifying the settings of the template itself will affect __all configurations__ based on this template.
</note>

## Ordering Build Configurations

You can view all build configurations of a project on the __Project Overview__ page. By default, they are listed in the alphabetical order, but administrators can [customize this order](ordering-projects-and-build-configurations.md).

## Configuring Settings

When you select a build configuration from the list of build configurations, TeamCity displays the __Build Configuration Home__ page where you can preview its recent build results. To access the build configuration's settings, click __Edit Configuration Settings__ in the upper right corner of the screen.

Different build configuration settings are described in the respective articles inside this section.

>Note that editing via the TeamCity UI will be disabled for build configurations created via the [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html).

## Actions in Build Configuration Settings

Use the __Actions__ menu, located in the upper right corner of the settings screen, to
* [pause/activate a build configuration](build-configuration.md#Pausing+Build+Configuration)
* [copy/move/delete a build configuration](copy-move-delete-build-configuration.md)
* [attach a build configuration to a template](build-configuration-template.md#Associating+build+configurations+with+templates)
* [extract a template from a build configuration](build-configuration-template.md#Creating+build+configuration+template)
* [extract a meta-runner from a build configuration](working-with-meta-runner.md#Extracting+and+Using+Meta-Runner)
* [attach a history to a build configuration](kotlin-dsl.md#Restoring+Build+History+After+ID+Change)
 
 <seealso>
        <category ref="admin-guide">
            <a href="configuring-dependencies.md">Configuring Dependencies</a>
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
            <a href="configuring-vcs-settings.md">Configuring VCS Settings</a>
        </category>
</seealso>