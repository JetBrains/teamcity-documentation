[//]: # (title: Creating and Editing Build Configurations)
[//]: # (help-id: Creating and Editing Build Configurations;Build Configuration)

<show-structure for="chapter,procedure" depth="2"/>

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

To add a build configuration to a TeamCity project, use the **+** button in the sidebar...

<img src="dk-new-configuration-sidebar.png" width="706" alt="Create new build configuration via sidebar"/>

...or click **Create build configuration** from the **General** tab of project settings.

<img src="dk-new-configuration-button.png" width="706" alt="Create new build configuration via button"/>


### Available Options

A build configuration can be one of two major types: a configuration that builds, tests, or deploys a project stored in a VCS, or one that does not require a remote repository (for example, it may employ a 3rd-party REST API to download and process data).

Configurations that check our remote repositories can in turn be created using TeamCity connections or VCS roots.

All available options are displayed in the corresponding drop-down on the **Set up your build** page.

<img src="build-configuraiton-creation-options.png" width="706" alt="All build config creation options"/>

### Use a TeamCity Connection

TeamCity [connections](configuring-connections.md) store all information required to access an external resource: a VCS hosting, a cloud data storage, a Docker registry, a secrets vault, and so on. Using connections is the most convenient way to build your sources: configure it once and just choose a required repository from the list whenever you add new build configuration or pipeline.

The figure above illustrates a list of existing connections: a GitLab connection, a GitHub connection, few Azure connections, and more. If no project among build configuration parents owns a VCS connection, your only option here will be to create a new one via the **Connect new repository** menu item.


### Use a Repository URL


This option allows you to build configuration in one go using a Git, Subversion, Mercurial, TFS, or Perforce repository (depot) URL. You can use any URL type:

* A regular repository web link: `https://github.com/Johndoe/my-sample-app`
* An HTTPS clone URL: `https://github.com/Johndoe/my-sample-app.git`
* An SSH clone URL: `git@github.com:Johndoe/my-sample-app.git`

To start building a remote repository, follow the steps below.

<procedure type="steps">

<step>

On the **Set up your build** page, choose the **From any Git URL** option.

</step>


<step>

Choose the authentication type.

<deflist type="medium">

<def title="SSH key">

Available if the **Repository URL** is an SSH clone URL. Click **Upload SSH key** to add a private key, which will be saved in the parent project ([**parent project settings**](project-administrator-guide.md#Edit+and+View+Modes) **| SSH keys**) and appear in the drop-down menu when configuring additional projects.

Learn more: [](ssh-keys-management.md)

</def>

<def title="HTTPS">

Available for HTTP(s) clone URLs, this option provides three authentication options:

* Token — issue a personal access token (PAT) on a VCS side and paste it here. You can also use TeamCity [](manage-access-tokens.md) page to issue tokens.
* Password — enter a regular username/password credentials.
* Anonymous — available for public repositories. This option should only be used if you do not intend to leverage write access permissions (for example, to [post TeamCity build statuses](commit-status-publisher.md) back to the VCS).

</def>

</deflist>

</step>


<step>

Set up basic configuration options.

* **Name** and (optionally) **description** — public texts displayed in TeamCity UI.
* **Default branch** — the full name of a branch that will become a default one in TeamCity (for example, `refs/heads/main`). See the following article for more information: [](working-with-feature-branches.md#Default+Branch).

   > By default, TeamCity tracks all branches (`refs/heads/*`). You can change this behavior later by editing the **branch specification** of a VCS root attached to this configuration. See the following topic to learn more: [](working-with-feature-branches.md).

The next page contains mixed settings of both project and a build configuration owned by this project.

* **Start builds on new changes in** — if enabled, adds a [VCS trigger](configuring-vcs-triggers.md) that launches new builds whenever TeamCity detects new commits.

* **Build configuration type** — allows you to select the configuration type depending on its purpose. See [this article](changing-build-configuration-type.md) to learn more about unique features of composite and deployment configurations.


</step>

<step>

Click **Create**. TeamCity will bring you to detailed configuration settings, where you can add build steps, edit the list of monitored branches, enable additional build features, and so on.

</step>

</procedure>


### Use a VCS Root

Every build configuration that processes sources stored in a remote repository does so by leveraging a **VCS root**. This object stores connection settings required to access a single repository, along with advanced settings: the list of monitored branches, the automatic polling interval, the submodule checkout policy, and more.

> See these articles for more information:
> * [What are VCS roots](project-administrator-guide.md#VCS+Roots)
> * [Configuring VCS roots](configuring-vcs-roots.md).
>
{style="tip"}

If you already have a build configuration or a pipeline that builds, tests, or deploys a required repository, you can reuse a VCS root of that configuration/pipeline. To do this, use either of the following methods:

* Create new configuration — select **From an existing root** option on the **Set up your build** page.

    <img src="" width="706" alt="Create configuration from existing root"/>

* Edit existing configuration — navigate to **Build Configuration Settings | Version Control** and click **Attach VCS root**.

    <img src="attach-and-detatch-vcs-roots.png" width="706" alt="Attach and detach VCS roots"/>

Reusing existing VCS roots allows you to save time on setting up required authorization and branch settings, and avoid creating duplicate roots.

> You can only reuse roots owned by current configuration's parent projects.
> 
> A single VCS root can be attached to numerous build configurations. Vice versa, a single build configuration can have multiple VCS roots attached (if you want to check out multiple repositories when a build starts).
> 
{style="note"}


> Since a VCS root can be attached to any number of pipelines, build configurations, and templates, editing its properties will alter the behavior of all of these entities. As a precaution, all root-related settings on the **Set up your build** page are disabled.
> 
> You can edit root settings from **Project Settings | VCS Roots** or **Build Configuration Settings | Version Control** pages. In this case, TeamCity prompts you whether the changes should be applied to all related entities, or it should create a copy of this root with updated settings.
> 
> <img src="edit-used-vcs-root-prompt.png" width="706" alt="Edit or clone VCS root"/>
> 
{style="tip"}


### Configuration Without a Repository

This configuration type doesn’t check out any remote repositories when it runs. Its steps, for example, only execute predefined scripts and send HTTP requests.

You can create a new "unbound: configuration in two ways:

* On the **Set up your build** page, select **Without repository**.
* In the classic UI, open the **New build configuration** page and click the **Manually** tile.

A [VCS root](#Use+a+VCS+Root) controls the VCS provider connection and repository checkout. Therefore, you can make any build configuration unbound by detaching its VCS root(s). Conversely, attaching a VCS root to a configuration without repositories enables it to check out remote sources. Both actions can be performed in the **Version Control** section of build configuration settings.

<img src="attach-and-detatch-vcs-roots.png" width="706" alt="Attach and detach VCS roots"/>



### Configuration With Multiple Repositories

Configurations determine which repositories to check out and which branches to track through their attached VCS roots. A configuration can have any number of roots, from [none](#Configuration+Without+a+Repository) to many.

Typically, a configuration uses a single repository and thus has one VCS root. If you need to build completely separate projects, it’s best to create individual configurations and link them in a [build chain](build-chain.md) if necessary. However, when multiple repositories are related (such as a core product and its plugins), you can attach several VCS roots to the same configuration to build them together. To do this, create a configuration using any of the methods mentioned in this article, then go to the **Version Control** section of its settings. Here you can [create more VCS roots](configuring-vcs-roots.md) that target required repositories.

The Kotlin DSL snippet below illustrates a configuration with two attached roots.

```Kotlin
package _Self.buildTypes

import jetbrains.buildServer.configs.kotlin.*

object MultiRepoBuild : BuildType({
    name = "Multi-repo build"

    vcs {
        root(MavenRepoRoot, "+:. => MavenRepo")
        root(GradleRepoRoot, "+:. => GradleRepo")
    }
})
```

All VCS roots download sources into the same [checkout directory](build-checkout-directory.md). To avoid file conflicts and keep the folder organized, it’s best to download each repository’s sources into separate subdirectories using [root checkout rules](vcs-checkout-rules.md). For example, the snippet above places sources in the "MavenRepo" and "GradleRepo" folders within the default checkout directory.

> When using custom checkout paths, specify build steps' **Working directory** options to help them find files they need to process.
> 
{style="tip"}




### Example: Create a Connection-Based Configuration

In this example, we will add a connection to GitHub and use it to create a new build configuration.

TeamCity supports two GitHub authentication methods: OAuth 2.0 and [GitHub App](https://docs.github.com/en/apps/creating-github-apps/about-creating-github-apps/about-creating-github-apps). In this walkthrough, we will use an automatically configured GitHub App, which requires no customization and takes less than a minute to set up. To learn more about both connection types and other VCS provider connections, refer to this article: [](configuring-connections.md).

You can create a connection-based configuration in two ways: create a connection under project settings and select it on the **Set up your build** page, or do everything from this single page.

<tabs>

<tab title="From the 'Set up your build' page">


<procedure type="steps">

<step>

Use TeamCity UI to [add a build configuration](#Create+Build+Configurations+in+TeamCity+UI) to the required project.

</step>

<step>

Select **Add new VCS connection** from the drop-down menu.

</step>

<step>

Click the **GitHub** tile and choose **GitHub App**.

<img src="new-github-app-redesigned-flow.png" width="706" alt="New GitHub App"/>

</step>

<step>

Click the **Create App** button above connection settings.

</step>

<include from="creating-and-editing-build-configurations.md" element-id="github-app-settings-steps"/>

<step>

Back in TeamCity, specify the connection name and click **Add** to save your new connection.

</step>

<step>

When your new connection is ready, you will navigate back to the **Set up your build** page. Make sure the repository origin menu points to your new connection. You might need to sign in to your GitHub account when you use this new App-based connection for the first time.

</step>

<include from="creating-and-editing-build-configurations.md" element-id="existing-connection-repo-list"/>

</procedure>

</tab>

<tab title="As separate steps">

<procedure title="Step 1: Create a connection" type="steps">

<step>

Open [settings](project-administrator-guide.md#Edit+and+View+Modes) of a project that should own your new GitHub connection. If you want a future connection to be available for any project created on this server, modify the **Root** project.

</step>

<step>

Navigate to the **Connections** tab and click **Add Connection**.

<img src="dk-add-connection-tab.png" width="706" alt="Add new connection"/>

</step>

<step>

Choose the **GitHub App** as the connection type and click **Create App**.

<img src="dk-create-project-github-app-connection.png" width="706" alt="Create GitHub App connection"/>

</step>

<snippet id="github-app-settings-steps">

<step>

TeamCity will redirect you to GitHub to approve the App, choose its installation location (personal account or organization), and optionally restrict its repository access. You can review and edit the TeamCity-configured App anytime via **GitHub Settings | Developer Settings | GitHub Apps** or uninstall it on **GitHub Settings | Applications** page.

</step>

</snippet>

<step>

After installing the App, you will return to TeamCity, where values for all connection settings (App ID, client ID, client secret, and others) will already be filled in. Click **Test connection** to verify the setup, then **Save** to complete it.

</step>

</procedure>

<procedure title="Step 2: Create a build configuration" type="steps">

<step>

Use TeamCity UI to [add a build configuration](#Create+Build+Configurations+in+TeamCity+UI) to the required project.

</step>

<step>

Choose your new connection from the list. If this connection is used for the very first time, you might need to sign in.

</step>

<snippet id="existing-connection-repo-list">

<step>

TeamCity will show a list of repositories accessible via the underlying connection. Use the search panel to find the desired repository, then click it to continue.

<img src="repo-list-from-connection.png" width="706" alt="The list of connection repositories"/>

</step>

</snippet>

</procedure>

</tab>

</tabs>



## Create Build Configurations in Kotlin DSL

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


## Create Build Configurations in REST API

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