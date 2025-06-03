[//]: # (title: Creating and Editing Build Configurations)
[//]: # (help-id: Creating and Editing Build Configurations;Build Configuration)

Build configurations and [pipelines](create-and-edit-pipelines.md) represent actual CI/CD routines. A build configuration stores a sequence of build steps (basic operations to be performed during a build run), and settings required to execute these steps. These settings include:

* [parameters](configuring-build-parameters.md) that allow you to quickly alter the configuration behavior;
* [triggers](configuring-build-triggers.md) that allow TeamCity to automatically start new builds when certain conditions are met;
* [build features](adding-build-features.md) that extend the configuration's functionality;
* [agent requirements](configuring-agent-requirements.md) that allow you to run configuration builds on specific build agents;
* and so on.

It is recommended to have a separate build configuration or a pipeline for each sequence of builds (that is performing a specified task in a dedicated environment). This allows for proper features functioning, like detection of new problems/failed tests, first failed in/fixed in tests status, automatically removed investigations, and so on.


<video src="https://youtu.be/fttWwJG7C38"
title="Improving your first build configuration"/>


## Build Configurations vs Pipelines



Before we dive into creating configurations, it’s important to understand the differences between build configurations and pipelines and when to use each. Note that once created, you cannot convert a build configuration into a pipeline or a job, and vice versa.


<snippet id="configurations-vs-pipelines">

<deflist type="full">

<def title="Parents">

Both build configurations and pipelines are owned by [TeamCity projects](creating-and-editing-projects.md). Each project can have an unlimited number of configurations and pipelines.

</def>


<def title="Children">

* A build directly owns its build steps.
* A pipeline owns jobs, which in turn own regular build steps.

</def>


<def title="Supported VCS types">

* Classic TeamCity build configurations support Git, Subversion, Mercurial, TFS, and Perforce, with integrations for major VCS providers like GitHub, GitLab, Bitbucket, Azure, and others.
* TeamCity Pipelines offer built-in integrations with GitHub, GitLab, and Bitbucket Cloud. Other Git repositories can be connected via direct URLs. Subversion, Mercurial, TFS, and Perforce are not currently supported.

</def>


<def title="Execution mode">

* Pipelines always run from start to finish, executing all jobs unless interrupted by errors like compilation failures or connection issues.
* Build configurations support [conditional step execution](build-step-execution-conditions.md). For example, you can add a step that runs only if a previous one fails.

</def>


<def title="Dependencies">

* Currently, pipelines support dependencies only between jobs within the same pipeline and cannot be linked into a larger sequence.
* Build configurations can form [build chains](build-chain.md) across different TeamCity projects.

</def>


<def title="Configuration as code">

Both pipelines and build configurations can store their settings as code, right next to your project source code. Both entities support branched settings, meaning each repository branch can have its own configuration file.

* Build configurations store their settings in XML or [](kotlin-dsl.md) format. You cannot edit these files from the TeamCity UI.
* Pipelines store their settings in YAML which can be edited directly in TeamCity.

Kotlin DSL support is planned for future pipeline versions. However, there are no current plans to add YAML support for build configurations.

</def>

<def title="Limitations">

* Build configurations are core TeamCity components, offering extensive features and customization options.
* Introduced in TeamCity 2025.07, Pipelines focus on providing the most intuitive way to design CI/CD workflows. However, they currently lack some of the functionality available in build configurations. For example, they do not support the majority of [build steps](configuring-build-steps.md) and have no additional [build features](adding-build-features.md).

</def>

</deflist>


In summary, while both pipelines and build configurations are owned by projects, they serve different needs. Pipelines are ideal for simpler CI/CD workflows in smaller projects (typically up to 10–15 builds). Choose build configurations instead if:

* Your project involves more complex workflows than 10–15 sequential builds.
* You are an experienced user who needs advanced features (such as [](build-approval.md)) not yet available in pipelines.
* You require fine-grained control over which build chain configurations run, when, and how.

</snippet>


## Create Build Configurations in TeamCity UI

Creating new build configurations shares a lot of similarities with [creating projects](creating-and-editing-projects.md): both can utilize [connections](configuring-connections.md) and rely on [VCS roots](configuring-vcs-roots.md) to access remote repositories.


### New Projects

When you create a new TeamCity project [from a repository URL](creating-and-editing-projects.md#From+Repository+URL) or [via a configured connection](creating-and-editing-projects.md#From+a+Configured+Connection), TeamCity guides you through setting up both the project and its build configuration. See the links above for more details.

### Add Configurations to an Existing Project

If a parent project already exists, you can add its child configurations as follows:

1. Use the **+** button in the sidebar...

    <img src="dk-new-configuration-sidebar.png" width="706" alt="Create new build configuration via sidebar"/>

    ...or click **Create build configuration** from the **General** tab of project settings.

    <img src="dk-new-configuration-button.png" width="706" alt="Create new build configuration via button"/>

2. You will be presented with the same set of options as for [creating projects](creating-and-editing-projects.md):

   * [From a repository URL](creating-and-editing-projects.md#From+Repository+URL) — adds a new configuration using a repository link. See this link for more information.
   * [From a configured connection](creating-and-editing-projects.md#From+a+Configured+Connection) — available if this project or any of its parent projects has a VCS provider connection. Faster than the first option since authentication settings are inherited from the underlying connection. See this link for more information.
   * **Manually** — creates a completely blank build configuration that does not target any remote repository. This option allows you to choose a [template](#Build+Configuration+Templates) attached to the configuration.

3. If you created an empty build configuration (using the **Manually** option), you will need to attach an existing [VCS root](configuring-vcs-roots.md) or create a new one to check out remote repositories. A configuration can include multiple VCS roots to handle repositories from the same or different providers.

    <include from="configuring-vcs-settings.md" element-id="attach-or-create-a-root"/>

    See the following articles for more information:

    * [](configuring-vcs-settings.md)
    * [](configuring-vcs-roots.md)


## Create New Projects in Kotlin DSL

The following Kotlin code creates a new build configuration that utilizes the target VCS root to interact with a VCS hosting provider:

```Kotlin
object RunTests: BuildType({
  id("RunTests")
  name = "Run unit tests"
  description = "Runs all unit tests of the project"

  vcs {
    root(MyProjectVCSRoot)
  }

  steps {
    // Add build steps
  }

  triggers {
    // Add triggers
  }
})
```


See these articles for more information:

* [](storing-project-settings-in-version-control.md)
* [BuildType (Kotlin DSL documentation)](https://teamcity.jetbrains.com/app/dsl-documentation/root/build-type/index.html)


## Create Build Configuration in REST API

The following request creates a new empty TeamCity build configuration owned by the specific parent project.


```Shell
export TEAMCITY_SERVER_URL="<Your TeamCity Server URL>"

curl --location $TEAMCITY_SERVER_URL'/app/rest/projects/<parent_project_locator>/buildTypes' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <Your TeamCity Access Token>' \
--data '{
    "name": "New Build Configuration from REST"
}'
```

See the following articles for more information:

* [Create, Move, and Delete Build Configurations](https://www.jetbrains.com/help/teamcity/rest/create-and-delete-build-configurations.html)
* [BuildType API](https://www.jetbrains.com/help/teamcity/rest/buildtypeapi.html)
* [NewBuildTypeDescription Schema](https://www.jetbrains.com/help/teamcity/rest/newbuildtypedescription.html)




## Build Configuration Templates

[Templates](build-configuration-template.md) allow you to quickly spawn multiple configurations with identical settings. After a configuration is created, you can override its settings.

<snippet id="create-build-configuration-templates">

You can create build configuration templates manually or extract them from existing configurations.

<procedure title="Create a template manually" type="steps">

Creating templates is identical to creating build configurations via the **Manually** tile.

<step><include from="common-templates.md" element-id="open-project-settings-tab"><var name="tab-name" value="General"/></include></step>
<step>

Scroll down to the **Build Configuration Templates** section and click **Create template**.

</step>

<step>

Enter the template name and description, and click **Create**.

</step>

<step>

Specify settings that should present in every configuration spawned from this template:

* [Build steps](configuring-build-steps.md)
* [Build parameters](configuring-build-parameters.md)
* [Triggers](configuring-build-triggers.md), and more.

</step>

</procedure>


<procedure title="Extract a template from a configuration" type="steps">

If you already have a build configuration that you want to use as reference, you can extract a template from it.


<step>

Open [build configuration settings](project-administrator-guide.md#Edit+and+View+Modes).

</step>

<step>

Open the **Actions** menu in the top right corner and click **Extract template...**.

<img src="dk-configuration-actions-menu.png" width="706" alt="Configuration actions menu"/>

</step>

<step>

Enter the configuration name and click **Extract**. You can keep the template ID as its autogenerated value.

</step>

<step>

The source configuration will be the first to use the new template. To keep it independent, click **Detach**. Otherwise, any changes to the template will apply to this and all other configurations based on it.

<img src="dk-detach-template-from-configuration.png" width="706" alt="Detach template from a config"/>

</step>

</procedure>

</snippet>



### Create a Build Configuration From a Template

> When you create a template from scratch or extract it from an existing build configuration, this template is available only for this project (and its subprojects). This means you cannot create new configurations in "&lt;Root project&gt;/ProjectA" using the "&lt;Root project&gt;/ProjectB/BuildConfTemplate" template.
> 
{style="tip"}

You can create a templated build configuration in two ways: from the template settings page, or by creating a regular configuration and choosing a template it should utilize.

Option #1:

1. <include from="common-templates.md" element-id="open-project-settings-tab"><var name="tab-name" value="General"/></include>

2. Scroll down to the **Build configuration templates** section and click a required template.

3. Invoke the **Actions** menu in the top right corner, and click **Create build configuration from this template...**.

<img src="dk-template-actions-menu.png" width="706" alt="Template Actions menu"/>

4. Specify the required settings for the new configuration. Do not click any tiles other than **Manually**; otherwise, you will create a new configuration where the selected template is not used.

Option #2:

1. <include from="common-templates.md" element-id="open-project-settings-tab"><var name="tab-name" value="General"/></include>
2. Click __Create build configuration__ under the __Build Configurations__ section.
3. Click the **Manually** tile and choose the required template in the __Based on template__ drop-down menu. This menu is available if this project or any of its parent projects own at least one build configuration template.

    <img src="dk-confBasedOnTemplate.png" width="706" alt="Create a regular configuration"/>

Option #2 is helpful when the **Build configuration templates** section does not show the required template since it is owned by another (sub)project.

<note>

The settings specified in the template cannot be edited in a configuration created from this template. However, some of them can be [redefined in a build configuration](build-configuration-template.md#Redefining+settings+inherited+from+template). Note that modifying the settings of the template itself will affect __all configurations__ based on this template.
</note>

> You can attach two or more templates to the same build configuration. See the following section for more information: [](build-configuration-template.md#Associating+build+configuration+with+multiple+templates).
> 
{style="tip"}



## Re-Arrange Build Configurations

You can view all build configurations of a project on the __Project Overview__ page. By default, they are listed in the alphabetical order, but administrators can [customize this order](ordering-projects-and-build-configurations.md).


## Build Configuration Settings

Build configuration settings include:

* [General settings](configuring-general-settings.md)
* [Version control settings](configuring-vcs-roots.md), defining how the source code is retrieved from VCS, where it is checked out to, and so on
* [Build steps](configuring-build-steps.md), that are sequentially run actions: for example, running msbuild, a script, or unit tests
* [Triggers](configuring-build-triggers.md), which are rules defining when to start a new build
* [Failure conditions](build-failure-conditions.md) specifying when a build will be marked as failed
* Additional [build features](adding-build-features.md)
* Dependencies:
   * for [snapshot dependencies](snapshot-dependencies.md), TeamCity will run all dependent builds on the sources taken at the moment the build they depend on starts
   * for [artifact dependencies](artifact-dependencies.md), before a build is started, all artifacts this build depends on will be downloaded and placed in their configured target locations and then will be used by the build
* [Parameters](configuring-build-parameters.md) which allow sharing settings
* Agent requirements specifying whether a [build configuration](managing-builds.md) can run on a particular [build agent](install-and-start-teamcity-agents.md).

<note>

Build configuration settings and build behavior may vary depending on the type of build configuration.
</note>


## Permissions to Edit Build Configuration

While only users with _Project Administrator's_ permissions can change project and build configuration settings, there are several ways how contributors to the source code can also affect the build settings and environment.

The default _Project Developer_ [role](managing-users-and-roles.md) grants users two permissions:
* _Customize build parameters_ allows changing the values of [build configuration parameters](configuring-build-parameters.md) thus potentially affecting how the source code is executed.
* _Change build source code with a custom patch_ allows running a [custom build](running-custom-build.md) based on a user's local sources, not yet committed to the repository.

Besides, all the users who author the source code or/and can write to the repository with project settings stored in [Kotlin DSL](kotlin-dsl.md), could potentially execute their arbitrary code on common build agents.

We recommend considering this aspect when granting users the permissions mentioned above or writing access to the projects' repositories. If necessary, you can adjust the set of [permissions granted to each role](managing-roles-and-permissions.md).

## Actions in Build Configuration Settings

Use the __Actions__ menu, located in the upper right corner of the settings screen, to:

* [Pause/activate a build configuration](changing-build-configuration-status.md).
* [Copy/move/delete a build configuration](copy-move-delete-build-configuration.md).
* [Attach a build configuration to a template](build-configuration-template.md#Associating+build+configurations+with+templates).
* [Extract a template from a build configuration](#Build+Configuration+Templates).
* [Extract a recipe from a build configuration](working-with-meta-runner.md).
* [Attach a history to a build configuration](kotlin-dsl.md#Restore+Build+History+After+ID+Change).

 
 <seealso>
        <category ref="admin-guide">
            <a href="configuring-dependencies.md">Configuring Dependencies</a>
            <a href="configuring-build-parameters.md">Configuring Build Parameters</a>
            <a href="configuring-vcs-settings.md">Configuring VCS Settings</a>
        </category>
        <category ref="examples">
            <a href="how-to-configure-cicd-for-jetbrains-space.md">How to Configure CI/CD for JetBrains Space</a>
        </category>
</seealso>