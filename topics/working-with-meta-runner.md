[//]: # (title: Working with Recipes)
[//]: # (help-id: Working with Meta-Runner;Working with Recipes)

**Recipes** are custom build steps based on one or multiple standard TeamCity steps. If TeamCity's built-in steps lack a needed option, and you frequently emulate it (for example, use a [CLI step](command-line.md) to upload artifacts via a cloud provider API), you can save this custom step as a reusable recipe.

Creating a recipe is a simpler alternative to developing a [TeamCity plugin](https://plugins.jetbrains.com/docs/teamcity/developing-teamcity-plugins.html) that implements a custom build step.


## Key Takeaways

**What are recipes?**<br/>
Recipes are custom build steps made from default TeamCity build steps pre-set in the specific manner. Complex recipes can include other recipes as building blocks.

**What's the point of a recipe?**<br/>
Recipes allow you to wrap pre-customized TeamCity build steps into a new step that can be easily shared across build configurations.

**What's the difference between recipes and meta-runners?**<br/>
In version 2025.03, "meta-runners" were renamed to "recipes". While based on the same concept, recipes offer added benefits like YAML support and easy sharing on JetBrains Marketplace.

**Can I continue use my existing meta-runners and [TeamCity Meta-Runner Pack](https://github.com/jetbrains/meta-runner-power-pack)?**<br/>
Yes, meta-runners are still functional under the new name and require no manual updates.

**What are public recipes?**<br/>
Public recipes are those shared at [JetBrains Marketplace](https://plugins.jetbrains.com/teamcity_recipe). These include both recipes hand-crafted by JetBrains and those shared by other TeamCity users.

**Are public recipes safe?**<br/>
Yes, all recipes published on JetBrains Marketplace are verified by our employees. You can always inspect the source recipe code on the Marketplace or directly in TeamCity UI before installing it.

**I want to create a recipe, what should I do?**<br/>
Find an existing or create a new build configuration that performs an action you want to save as a custom build step, and [extract a recipe](#Extract+a+Recipe+From+a+Build+Configuration) using the configuration **Actions** menu in TeamCity UI. Doing so allows you to save an XML recipe. To create a YAML recipe, you need to write its definition from scratch. Inspect the source code for public Marketplace recipes and check out the [](recipe-yaml-syntax.md) article to learn more.

**How to use a recipe?**<br/>
In the same way you utilize regular build steps: [add them](#Use+a+Recipe) to the configuration's "Build steps" list.

**Are recipes editable?**<br/>
Yes, you do not need to re-configure a source configuration and re-extract a recipe every time you need to make a change. Private recipes can be [edited](#Edit+a+Private+Recipe) on the **Recipes** page of [project settings](project-administrator-guide.md#Edit+and+View+Modes). Public recipes are authored by external parties and cannot be edited directly.



## Extract a Recipe From a Build Configuration

> This section explains how to extract an XML recipe from a configuration. Currently, YAML recipes cannot be extracted: to create a YAML recipe you need to create an .yml definition file from scratch.
> 
{style="note"}

The most straightforward way to create a new recipe is to extract it from an existing configuration that uses a required step or sequence of steps. For example, the [Kotlin DSL](kotlin-dsl.md) example below shows a build configuration with two [CLI](command-line.md) build steps: one uses cURL to download a file, and the other runs `ls` to list the working directory contents.


```Kotlin
import jetbrains.buildServer.configs.kotlin.*

object SourceConfiguration : BuildType({
    name = "Source Configuration"
    
    params {
        param("URL", "")
        param("fileName", "")
    }
    
    steps {
        script {
            id = "simpleRunner"
            scriptContent = "curl -o %URL% %fileName%"
        }
        script {
            id = "simpleRunner_1"
            scriptContent = "ls"
        }
    }
})
```
{ignore-vars="true"}

To extract a recipe from an existing configuration:

1. In [configuration settings](project-administrator-guide.md#Edit+and+View+Modes), invoke the **Actions** menu and click **Extract recipe**.

    <img src="dk-extract-recipe.png" width="706" alt="Extract recipe"/>

2. In the popup dialog, enter the recipe's internal ID, public name, and description. Recipes show these strings on the [Add build step](configuring-build-steps.md) page.

3. Click **Extract** to create your new recipe. You recipe should look like the following:

    ```XML
    <meta-runner name="cURL: File Download">
        <description>A two-step recipe that utilizes the "curl -o %URL% %fileName%" command to download a file, and calls "ls" command to print the contents of a working directory afterwards</description>
        <settings>
            <parameters>
                <param name="URL" value="" spec="text description='The URL of a file to be downloaded' display='normal' label='Download URL:'"/>
                <param name="fileName" value="" spec="text description='Enter the saved file name or leave blank to keep the origin name' label='File name:'" />
                <!--other parameters-->
            </parameters>
            <build-runners>
                <runner name="" type="simpleRunner">
                    <parameters>
                        <param name="script.content" value="curl -o %URL% %fileName%" />
                        <param name="teamcity.step.mode" value="default" />
                        <param name="use.custom.script" value="true" />
                    </parameters>
                </runner>
                <runner name="" type="simpleRunner">
                    <parameters>
                        <param name="script.content" value="ls" />
                        <param name="teamcity.step.mode" value="default" />
                        <param name="use.custom.script" value="true" />
                    </parameters>
                </runner>
            </build-runners>
            <requirements />
        </settings>
    </meta-runner>
    ```
   {ignore-vars="true"}

   > By default, a recipe includes all parameters from the source configuration. You can remove unnecessary ones and edit parameter `spec` attributes to adjust appearance and behavior on the Recipes page: [](#Edit+a+Private+Recipe).
   >
   {style="note"}


Recipes are saved to the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`\config\projects\<project_ID>\pluginData\metaRunners` directory. They are owned by a project whose configuration was used as a source. As such, recipes are by default available only for their origin project and its subprojects.

> To make a recipe available for the entire TeamCity server, move its configuration file to the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`\config\projects\_Root\pluginData\metaRunners` directory.
>
{style="tip"}


## Use a Recipe

Recipes are custom build steps, and as such, are added to build configurations in the same manner.

1. <include from="common-templates.md" element-id="open-configuration-settings-tab"><var name="configuration-tab-name" value="Build steps"/></include>
2. Click the **Add build step** button.
3. Choose a recipe from the right column that shows:

    * private recipes owned by this project or its parent project;
    * public recipes from JetBrains Marketplace.

    <img src="dk-add-recipe.png" width="706" alt="Add a recipe"/>

4. Set up required recipe settings in the same manner you do this for regular TeamCity steps.

You can explore public recipes authored by the TeamCity developers and other TeamCity users at [https://plugins.jetbrains.com/teamcity_recipe](https://plugins.jetbrains.com/teamcity_recipe). We hope to expand our collection in future release cycles and welcome your ideas and feedback.

If you do not see any Marketplace recipe options, verify they are enabled for your project:

1. <include from="common-templates.md" element-id="open-project-settings-tab"><var name="tab-name" value="Recipes"/></include>
2. Switch the **Public JetBrains Marketplace recipes** setting to "Enabled". If this setting is "Disabled" and grayed-out, edit the settings of a parent project that enforces this behavior or talk to a person who administers this project.


## Upload a Recipe From a File

If you have a recipe .xml definition file, you can upload this file to a required project manually. For example, you may want to move a recipe from one project to another or downloaded a recipe manually from [JetBrains Marketplace](https://plugins.jetbrains.com/teamcity_recipe).


To install a recipe from a file, do the following:

1. <include from="common-templates.md" element-id="open-project-settings-tab"><var name="tab-name" value="Recipes"/></include>
2. Click the <b>Upload Recipe</b> button.
3. Choose a configuration file and enter a unique recipe name.
4. Click <b>Save</b>. Your uploaded recipe is now available for all configuration of this project and its subprojects.

<!--<procedure title="In TeamCity UI">
<step>
<include from="common-templates.md" element-id="open-project-settings-tab"><var name="tab-name" value="Recipes"/></include>
</step>

<step>Click the <b>Upload Recipe</b> button.</step>

<step>Choose a configuration file and enter a unique recipe ID.</step>

<step>Click <b>Save</b>. Your uploaded recipe is now available for all configuration of this project and its subprojects.</step>
</procedure>

<procedure title="Add File Manually">
<step>
Navigate to the <a href="teamcity-data-directory.md"><code>&lt;TeamCity Data Directory&gt;</code></a><code>\config\projects\</code> directory.
</step>

<step>Open a directory that corresponds to a project that shown own your new recipe. For example, open <code>_Root</code> if the recipe should be available for all configurations on this server.</step>

<step>In the project folder, navigate to <code>\pluginData\metaRunners</code>.</step>

<step>Paste the recipe .xml file in this folder. Once you place the file on the disk, TeamCity will detect it and load the recipe; no server restart is required.</step>
</procedure>

-->


## Manage Existing Recipes

The **Recipes** page of project settings allows you to:

* enable or disable public (Marketplace-based) recipes for this project and all of its subprojects;
* inspect all public and private recipes used in this project: view their usages and spot any issues.

<img src="dk-recipes-in-root-project.png" width="706" alt="Recipes page in Root project"/>

You can open this page for the Root project to view a server-wide usage report. Recipe tags notify you when a newer recipe version is available, the current version or the entire recipe is no longer available on Marketplace, or TeamCity cannot contact JetBrains Marketplace and retrieve recipe data.

## Share Recipes on Marketplace

You can share your YAML-based recipes with TeamCity community on JetBrains Marketplace. See this article for more information: [Uploading TeamCity Recipes](https://plugins.jetbrains.com/docs/marketplace/uploading-a-new-plugin.html#upload-teamcity-recipes).



## Edit a Private Recipe

Recipes extracted from a build configuration or uploaded from a file are stored locally on your server machine and can be edited in TeamCity UI.

1. <include from="common-templates.md" element-id="open-project-settings-tab"><var name="tab-name" value="Recipes"/></include>

2. A private recipe to view and edit its configuration file.

For example, a recipe extracted from an existing configuration copies all parameters from this configuration. You can remove parameters unrelated to actual build steps performed by a recipe.

### XML Parameter Specification

XML recipe parameter specification has the `spec="type attribute='value'` format. You can edit this specifications to modify parameter/editor appearance and behavior settings.

```XML
<parameters>
    <param name="internalName" value="" spec="type attribute1='value1' attribute2='value2'/>
</parameters>
```

For example:


```XML
# Checkbox parameter
<param name="enabled" value="" spec="checkbox checkedValue='true' uncheckedValue='false' label='Enable debug' description='Tick this setting to run in debug mode'"/>

# Select parameter
<param name="logBehavior" value="" spec="select data_1='All' data_2='Errors only' data_3='Errors and warnings' label='Logging verbosity:' description='Choose whether only critical or all messages should be logged'"/>

# Prompt parameter that cannot have an empty value
<param name="tag" value="default" spec="text description='This value cannot be empty' label='Tag: ' validationMode='not_empty' display='prompt'" />
```


> YAML recipes have different specification syntax. See the [](recipe-yaml-syntax.md) article to learn more.
> 
{style="note"}



## Recipe Autonomy

Recipes are designed to be reused throughout build configurations and as such, should be configuration-agnostic. This means your recipes should ideally perform actions that can be executed regardless of other configuration settings.

In addition, recipes have the same project requirements as their origin build steps. You may opt for cross-platform build steps (like [](command-line.md) or [](kotlin-script.md)) as a basis for your recipes to make them compatible with as many build agents as possible.

### Example 1: VCS Roots

[VCS Roots](configuring-vcs-roots.md) are not baked into a recipe configuration file. As such, if you create a recipe that performs operations on repository files and folders, configurations without suitable VCS Roots will fail. If you need such recipe, do the following:

1. Go the [settings](project-administrator-guide.md#Edit+and+View+Modes) of a build configuration that imports a recipe.
2. Switch to the **Version Control Settings** tab.
3. Click the **Attach VCS root** button.
4. In the **Attach existing VCS root** choose the same root your origin configuration uses.
5. Click **Save** at the bottom of the page and run your build configuration. Since it now has a connection to a VCS repository, build steps can access required files and are able to finish successfully.

### Example 2: Build Files

[](gradle.md), [](maven.md), [](ant.md), and other build steps process build files like `build.xml` or `pom.xml`. If your custom recipe includes such a step, ensure the importing build configuration can locate the required file to avoid failures.


Build steps that allow defining a build file directly rather than just specifying a path are particularly well-suited for use in recipes. For example, the [](ant.md) build step.

<img src="dk-ant-step-embedded-config.png" width="706" alt="Build config file embedded in Ant step settings"/>


## Launch Recipes in Containers

Individual build steps comprise recipes have settings that allow TeamCity to run these steps inside [Docker/Podman containers](container-wrapper.md). Same settings are available for recipes themselves.

<img src="dk-docker-container-settings.png" width="706" alt="Container settings in steps and recipes"/>

If you want all of your steps to be executed inside a required container, set up the required image on the recipe level. Container settings of individual steps have a priority over these recipe settings and allow you to run each step in its unique container.

[Kotlin sample](kotlin-dsl.md) of a recipe that runs its steps inside "ubuntu" Linux container:

```Kotlin
object Build : BuildType({
    steps {
        step {
            id = "SimpleMetaRunner"
            type = "idSimpleMetaRunner"
            executionMode = BuildStep.ExecutionMode.DEFAULT
            param("plugin.docker.imageId", "ubuntu")
            param("plugin.docker.imagePlatform", "linux")
            param("plugin.docker.pull.enabled", "true")
            param("plugin.docker.run.parameters", "")
        }
    }
})
```

The XML markup for this recipe is shown below. Step #1 runs in the `python:3.9.20-bullseye` container. Step #2 has no personal container settings and runs inside the `ubuntu` container as defined in the Kotlin code above.

```XML
<meta-runner name="SimpleMetaRunner">
    <description>A Py/CLI sample recipe</description>
    <settings>
        <parameters/>
        <build-runners>
            <runner name="Py" type="python-runner">
                <parameters>
                    <param name="plugin.docker.imageId" value="python:3.9.20-bullseye" />
                    <!-- Python step parameters -->
                </parameters>
            </runner>
            <runner name="" type="simpleRunner">
                <parameters>
                    <!-- CLI step parameters -->
                </parameters>
            </runner>
        </build-runners>
        <requirements />
    </settings>
</meta-runner>
```

> To run all build steps inside the same Docker or Podman container (except for steps that do not support this mode), add the [](run-in-docker.md) build feature.
> 
{type="tip"}

<!--
A _recipe_ allows you to extract build steps, requirements, and parameters from a build configuration and create a [build step](configuring-build-steps.md) out of them. This build runner can then be used as any other build runner in a build step of any other build configuration or template.

With recipes, you can:
* Reuse existing runners
* Create new runners for typical tasks (for example, publish to FTP, delete a directory)
* Simplify your build configuration and decrease a number of build steps

Basically, a recipe is a set of build steps from one build configuration that you can reuse in another; it is an XML definition, containing build steps, requirements, and parameters, that you can utilize in XML definitions of other build configurations.

> Note that [VCS roots](configuring-vcs-roots.md) are not baked into recipes. If build steps of your recipes perform operations on repository files and folders, root-less configurations that reuse these recipe steps will fail. You can reuse a VCS root the recipe's origin configuration utilizes to fix this issue.
>
{style="note"}

TeamCity allows extracting recipe using the web UI.

All recipe are stored on a project level, so they are available within this project and its subprojects only, and are not visible outside. If a recipe is stored on the Root project level, it is available globally (in all projects).

You can use the existing recipe from the TeamCity Meta-Runners Power Pack or create your own recipe.

## Using Meta-Runners Power Pack

[Meta-runners Power Pack for TeamCity](https://github.com/jetbrains/meta-runner-power-pack), available on GitHub, is a collection of recipes for various tasks such as downloading a file, triggering a build, tagging a build, changing a build status, running PHP tasks, and so on.

Each `*MRPP_*.xml*` file contains a definition of a single recipes. Download the required recipe (or copy its definition to a file) and install it as described in the section below.

## Installing Recipe

You can install a recipe using the TeamCity web UI. Alternatively, you can do it directly via the file system.

<deflist>
<def title="Install a recipe via the Web UI">


1. <include from="common-templates.md" element-id="open-project-settings-tab"><var name="tab-name" value="Recipes"/></include>
2. Click __Upload Recipe__, and select the recipe definition file.
3. Save your changes.

</def>

<def title="Install a recipe directly to the file system">

Put the recipe definition file into the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`\config\projects\<project_ID>\pluginData\metaRunners` directory, where `<project_ID>` is the identifier of a project under which you want to place the recipe. If the `metaRunners` directory does not exist, create it manually.

Once you place the file on the disk, TeamCity will detect it and load the recipe; no server restart is required.

</def>
</deflist>




If the recipe is loaded successfully, you will see it listed on the __recipe__ page in the project settings; if you have appropriate permissions, you can modify the definition directly in the TeamCity UI.

The runner is now available in the list of build runners on the __[Build Configuration Settings](project-administrator-guide.md#Edit+and+View+Modes) | Build Steps__ page and is represented as a native TeamCity runner with a convenient UI.

A recipe placed into a project will be available to all its subprojects and build configurations. To make a recipe available to all projects, place it in the Root project.

## Creating a Recipe

You can create a build configuration via the TeamCity web UI and extract a recipe from it or use the XML definition of an existing build configuration as a recipe.

## Creating Your Own Recipe from UI

Let us consider an example of creating a recipe.

To create a recipe, follow these steps (described below in more detail):
1. [Prepare a build configuration](#Preparing+Build+Configuration) to test the build steps to be used in the recipe.
2. [Make sure the build configuration is working](#Verifying+Build+Configuration+Works+Properly).
3. [Extract a recipe to the desired project](#Extracting+and+Using+Recipe).

In this example, we will create a recipe to publish some artifacts to TeamCity with the help of the corresponding [service message](service-messages.md#Publishing+Artifacts+While+Build+is+in+Progress).

Usually artifacts configured in a build configuration are published when the build finishes. However, sometimes for long builds with multiple build steps we need artifacts faster. In this example, we will create a runner which can be inserted between any build steps and can be configured to publish artifacts produced by previous steps.

### Preparing Build Configuration

The first step is to prepare a build configuration which will work the same way as the recipe we would like to produce. Let us use the configuration with a single Ant build step: Ant can be executed on any platform where the TeamCity agent runs; besides, Ant runner in TeamCity supports `build.xml` specified right in the runner settings. This is important because our build configuration must be self-contained — since recipes do not include VCS roots of their origin configurations, a target configuration cannot take `build.xml` from the version control repository. In our case, the Ant step settings will look like this:

<img src="ant-build-step.png" width="750" alt="Adding Ant build step"/>

where `artifact.paths` is a system property. We need to add it on the __Parameters__ tab of the build configuration settings:

<img src="paths-to-artifacts-parameter.png" width="750" alt="Paths to artifacts in Build Parameters"/>

Note that each parameter can have a specification where we can provide the label, description, type of control, and specify validation conditions.

If your recipe contains steps that need to access files and folders of a remote repository, do the following:

1. Go the settings of a build configuration that imports a recipe.
2. Switch to the **Version Control Settings** tab.
3. Click the **Attach VCS root** button.
4. In the **Attach existing VCS root** choose the same root your origin configuration uses.
5. Click **Save** at the bottom of the page and run your build configuration. Since it now has a connection to a VCS repository, build steps can access required files and are able to finish successfully.

>Here the Ant build step is used just as an example. In the initial build configuration, you can use any of the available build runners (for example, MSBuild or .NET process), and configure the settings and define the parameters for this build step. When you extract a recipe from this build configuration, all the settings defined in the build step, and all the build parameters will be added to the recipe.

### Verifying Build Configuration Works Properly

Once the build steps and parameters are defined, we need to make sure our build configuration works by running a couple of builds through the custom build dialog:

<img src="cutom-build-with-paths-to-artifacts.png" width="750" alt="Running a custom build with paths to artifacts"/>

### Extracting and Using Recipe

If the build configuration works properly, we can create a recipe by clicking the __Actions__ button in the upper right corner of the __[Build Configuration Settings](project-administrator-guide.md#Edit+and+View+Modes)__ page and selecting the __Extract recipe__ option:

<img src="extract-meta-runner.png" width="650" alt="Extract recipe"/>

The __Extract recipe__ dialog requires specifying the project where the recipe will be created. A recipe created in a project will be available in this project and all its subprojects. In our case the Root project is selected, so the recipe will be available in all projects.

We also need to provide the name, description, and an ID for the recipe: the name and description will be shown in the web interface, an ID is required to distinguish this recipe from others.

Upon clicking the __Extract__ button, TeamCity will take definitions of all build steps and parameters in this build configuration and create a build runner out of them.

>Besides build steps and parameters, a recipe can also have requirements: if requirements are defined in the build configuration, they will be extracted to the recipe automatically. Requirements can be useful if the tools used by recipe are available on specific platforms only.

Once the recipe is extracted, it becomes available in the build runners' selector, under the name of the project it belongs to, and can be used in any build step just like any other build runner:

<img src="meta-runner-build-step.png" width="750" alt="Publish Artifacts build step"/>

The current recipe usages can be seen at the project __recipe__ page:

<img src="meta-runners.png" width="750" alt="recipes"/>

When a recipe is extracted, all steps are extracted. If you need to reorder parameters or make some quick fixes in the runner script, you can edit its raw XML definition in the web browser: go to __Project Administration | recipe__ and use the __Edit__ option next to the recipe. The parameters will be shown in the same order as `<param>` elements in the XML definition. Definitions of recipe are stored in the [`<TeamCity Data Directory>`](teamcity-data-directory.md)`\config\projects\<project_ID>\pluginData\metaRunners` directory.

## Creating Recipe from XML Definition of Build Configuration

Alternatively, you can use the XML definition of an existing build configuration as a recipe. To do it, save the definition of this build configuration to a file named as `<runner_id>.xml` where `<runner_id>` is the [ID](identifier.md) of this build runner. Install the recipe as described [above](#Installing+Recipe).

Since a recipe looks and works like any other runner, it is also possible to create another recipe on the basis of an existing recipe.

## Creating Build Configuration from Recipe

If you need to fix a recipe and test your fix, you can create a build configuration from a recipe, change its steps, adjust parameters and requirements, check how it works, and then use the __Extract recipe__ action to apply the changes to the existing recipe with the same ID.


-->