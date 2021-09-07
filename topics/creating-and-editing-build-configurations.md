[//]: # (title: Creating and Editing Build Configurations)
[//]: # (auxiliary-id: Creating and Editing Build Configurations)

This page details creating build configurations via the TeamCity web UI. Other options include the [REST API](https://www.jetbrains.com/help/teamcity/rest/manage-build-configurations.html#Build+Configuration+Locator) and using TeamCity project configuration in [DSL based on the Kotlin language](storing-project-settings-in-version-control.md).

To create a build configuration, open __General Settings__ of a project and click __Create build configuration__ under the __Build Configurations__ section.

<img src="create-build-configuration-button.png" alt="Create a build configuration" width="750"/>

TeamCity provides several ways to create a new [build configuration](build-configuration.md) for a project:

<img src="create-build-configuration.png" alt="Create a build configuration from a repository URL" width="828"/>

You can create a new build configuration
* [Manually](#Creating+New+Build+Configuration+Manually)
* [Pointing to a repository URL](#Creating+New+Build+Configuration+from+URL) 
* Pointing to a GitHub repository
* Pointing to Bitbucket repository
* [Create a build configuration template](#Creating+Build+Configuration+Template), and then [create a build configuration based on the template](#Creating+Build+Configuration+From+Template).

When build configurations are created, you can:
* [change their order](#Ordering+Build+Configurations)
* [edit their settings](#Configuring+Settings)
* control them via the [Actions](#Actions+in+Build+Configuration+Settings) menu

## Creating New Build Configuration Manually

1. On the __Administration &gt; Projects__ page, select the desired project in the list. (Alternatively, open the project using the __Projects__ popup, and click the __Edit Project Settings__ link on the right).  The __Project Settings__ page opens.
2. On the Project Settings page, __Build Configurations__ section, click __Create build configuration.__
3. In the _Create Build Configuration_ dialog, specify the name, ID and (optionally) description for the build configuration, click __Create__.
4. Proceed with creating other settings:   
   * [Create/edit VCS roots and specify VCS-specific settings](configuring-vcs-settings.md)
   * On the __Build Steps__ page, configure build steps discovered by the automatic detection. To create them manually by selecting a desired build runner from the drop-down menu. Click __Save__. You can add as many build steps as you need within one build configuration. Note that they will be executed sequentially. In the end, the build gets the merged status and the output goes into the same build log. If some step fails, the rest is executed or not, depending on their [step execution policy](configuring-build-steps.md#Execution+policy).
   * Additionally, configure [build triggering options](configuring-build-triggers.md), [dependencies](configuring-dependencies.md), [properties and variables](configuring-build-parameters.md), and [agent requirements](configuring-agent-requirements.md).

## Creating New Build Configuration from URL

You can create a build configuration using a VCS URL:
1. On the __Administration &gt; Projects__ page, select the desired project in the list. (Alternatively, open the project using the __Projects__ pop-up menu, and click __Edit Project Settings__ on the right). The __Project Settings__ page opens.

2. On the __Project Settings__ page, __Build Configurations__ section, click __Create build configuration from URL__.

3. In the _Create Build Configuration_ dialog, specify a VCS repository URL and, if authentication is required, VCS credentials (username and password/token). Click __Create__.

4. TeamCity will suggest the build configuration name and will configure the rest of settings for you.
   * It will determine the type of the VCS repository and create a [VCS root](vcs-root.md). For a Git repository, it will autodetect the default branch. You have an option to change it now or later, in the [VCS root](vcs-root.md) settings. You can also change the branch specification: by default, TeamCity monitors all branches of the repository but you can choose what exact branches to monitor by entering [custom rules](working-with-feature-branches.md#branch-spec-syntax).
   * It will attempt to autodetect build steps: Ant, NAnt, Gradle, Maven, MSBuild, Visual Studio solution files, PowerShell, Xcode project files, Rake, and IntelliJ IDEA projects. If none found, you will have to [configure build steps manually](configuring-build-steps.md).
   * Next, TeamCity UI will display suggestion icons with prompts to create [build triggers](configuring-build-triggers.md), [failure conditions](build-failure-conditions.md), and [build features](adding-build-features.md). Depending on the build configuration settings, it may suggest some additional configuration options.
5. After the build configuration is created, you can run a build and/or tweak the settings.

## Creating New Build Configuration pointing to GitHub.com repository

1. Click the __Create build configuration__ button and select __Pointing to GitHub.com repository__.
    * If you do not have a GitHub connection configured, you will be redirected to the __Connections__ page. Set up the connection as [described here](integrating-teamcity-with-vcs-hosting-services.md#Connecting+to+GitHub), then follow the steps below.
    * If you have a GitHub connection configured, follow the steps below.
2. On the __Create Build Configuration From GitHub__ page, select a repository. TeamCity will verify the repository [connection](integrating-teamcity-with-vcs-hosting-services.md). If the connection is verified, the new page opens.
3. TeamCity will display the project and build configuration name. If required, modify the names.  
   It will also autodetect the default branch. You have an option to change it now or later, in the [VCS root](vcs-root.md) settings. You can also change the branch specification: by default, TeamCity monitors all branches of the repository but you can choose what exact branches to monitor by entering [custom rules](working-with-feature-branches.md#branch-spec-syntax).  
   Click __Proceed__.
4. TeamCity will add a VCS build trigger and attempt to [autodetect build steps](configuring-build-steps.md#Autodetecting+build+steps): Ant, NAnt, Gradle, Maven, MSBuild, Visual Studio solution files, PowerShell, Xcode project files, Rake, and IntelliJ IDEA projects. On the __Auto-detected Build Steps__ page, select the step(s) to use in your build configuration. Click __Use selected__. If no steps found, you will have to [configure build steps manually](configuring-build-steps.md).
5. Your project and a build configuration are configured. Click __Run__ to start the build. Depending on the build configuration settings, TeamCity can suggest some additional configuration options. Review the suggested settings ![suggestedSettings.PNG](suggestedSettings.PNG) and configure required ones.

## Creating New Build Configuration pointing to Bitbucket Cloud

1. Click the __Create build configuration__ button and select __Pointing to Bitbucket Cloud repository__. 
 * If you do not have a Bitbucket connection configured, you will be redirected to the __Connections__ page. Set up the connection as [described here](integrating-teamcity-with-vcs-hosting-services.md#Connecting+to+Bitbucket+Cloud), then follow the steps below.
 * If you have a Bitbucket connection configured, follow the steps below.
2. On the __Create Build Configuration From Bitbucket Cloud__ page, select a repository. TeamCity will verify the repository [connection](integrating-teamcity-with-vcs-hosting-services.md). If the connection is verified, the new page opens.
3. TeamCity will display the project and build configuration name. If required, modify the names.  
   For a Git repository, it will also autodetect the default branch. You have an option to change it now or later, in the [VCS root](vcs-root.md) settings. You can also change the branch specification: by default, TeamCity monitors all branches of the repository but you can choose what exact branches to monitor by entering [custom rules](working-with-feature-branches.md#branch-spec-syntax).  
   Click __Proceed__.
4. TeamCity will add a VCS build trigger and attempt to autodetect build steps: Ant, NAnt, Gradle, Maven, MSBuild, Visual Studio solution files, PowerShell, Xcode project files, Rake, and IntelliJ IDEA projects.   
On the __Auto-detected Build Steps__ page, select the step(s) to use in your build configuration. Click __Use selected__. If no steps found, you will have to [configure build steps manually](configuring-build-steps.md).
5. Your project and a build configuration are configured. Click __Run__ to start the build. Depending on the build configuration settings, TeamCity can suggest some additional configuration options. Review the suggested settings ![suggestedSettings.PNG](suggestedSettings.PNG) and configure required ones.

## Creating Build Configuration Template

Creating a new [build configuration template](build-configuration-template.md) is similar to creating a new configuration:
1. On the __Administration | Projects__ page, select the desired project in the list. (Alternatively, open the project using the __Projects__ popup, and click the __Edit Project Settings__ link on the right).  The __Project Settings__ page opens.

2. On the Project Settings page, __Build Configuration Templates__ section, click the __Create template__ button and proceed with configuring [general settings](configuring-general-settings.md), [VCS settings](configuring-vcs-settings.md), and [build steps](configuring-build-steps.md).

Alternatively, you can create a build configuration template from an existing build configuration:
1. Open the existing build configuration settings page, click __Actions__ in the upper right corner of the screen, and select the __Extract Template__ option.
2. Specify the settings required and click __Create__.

## Creating Build Configuration From Template

To create a build configuration based on a template:
1. On the __Administration | Projects__ page, select the desired project in the list. (Alternatively, open the project using the __Projects__ popup, and click the __Edit Project Settings__ link on the right).  The __Project Settings__ page opens. 
2. On the Project Settings page, __Build Configuration Templates__ section, locate the desired template and click its name or use __Edit__.
3. Click __Actions__ in the upper right corner of the screen, and select __Create Build Configuration__.
4. Specify the required settings for the new configuration. 

<note>

The settings specified in the template cannot be edited in a configuration created from this template. However, some of them can be [redefined in a build configuration](build-configuration-template.md#Redefining+settings+inherited+from+template). Note that modifying the settings of the template itself will affect __all configurations__ based on this template.
</note>

Alternatively, you can create a build configuration based on a template as follows:
1. On the __Administration | Projects__ page, select the desired project from the list. (Alternatively, open the project using the __Projects__ popup, and click the __Edit Project Settings__ link on the right). The __Project Settings__ page opens.

2. On the Project Settings page, __Build Configurations__ section, click __Create build configuration__.

3. On the configuration settings page, use the __Based on template__ drop-down menu and select a template for your build configuration.

## Ordering Build Configurations

You can view all build configurations of a project on the __Project Overview__ page listed in alphabetical order by default. Administrators can [customize the default order](ordering-projects-and-build-configurations.md).

## Configuring Settings

When you select a build configuration from the list of build configurations, TeamCity displays the __Build Configuration Home__ page where you can preview its recent build results. To access the build configuration's settings, click __Edit Configuration Settings__ in the upper right corner of the screen.

Different build configuration settings are described in the respective articles inside this section.

>Note that editing via the TeamCity web UI will be disabled for build configurations created via the [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html).

## Actions in Build Configuration Settings

Use the __Actions__ menu, located in the upper right corner of the screen, when editing a build configuration to
* [pause/activate a build configuration](build-configuration.md#Pausing+%2F+Activating+a+single+build+configuration)
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