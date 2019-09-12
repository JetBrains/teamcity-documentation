[//]: # (title: Working with Meta-Runner)
[//]: # (auxiliary-id: Working with Meta-Runner)

A _Meta\-Runner_ allows you to extract build steps, requirements and parameters from a build configuration and create a [build runner](build-runner.md) out of them. This build runner can then be used as any other build runner in a build step of any other build configuration or template.

Basically, a meta\-runner is a set of build steps from one build configuration that you can reuse in another; it is an xml definition containing build steps, requirements and parameters that you can utilize in xml definitions of other build configurations. TeamCity allows extracting meta\-runners using the web UI.

With a meta\-runner, you can easily reuse existing runners, create new runners for typical tasks (for example, publish to FTP, delete directory), you can simplify your build configuration and decrease a number of build steps.

All meta\-runners are stored on a project level, so they are available within this project and its subprojects only, and are not visible outside. If a meta\-runner is stored on the __&lt;Root project&gt;__ level, it is available globally (in all projects).

You can use the existing meta\-runners from the TeamCity Meta\-Runners Power Pack or create your own meta\-runner.

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>


## Using Meta-Runners Power Pack

[Meta-runners Power Pack for TeamCity](https://github.com/jetbrains/meta-runner-power-pack) available on GitHub is a collection of meta\-runners for various tasks like downloading a file, triggering a build, tagging a build, changing a build status, running PHP tasks, and so on.

Each file called `*MRPP_\<some text\>.xml*` contains a definition of a single Meta\-runner. Download the required meta\-runner (or copy its definition to a file) and install it as described in the section below.

## Installing Meta-Runner

You can install a meta\-runner using the TeamCity web UI. Alternatively, you can do it directly via the file system.
* to install a Meta\-runner via the Web UI, go to the Project Settings page, select Meta\-Runners from the list of settings on the left, click __Upload Meta\-Runner__ and select the Meta\-runner definition file. Save you changes.
* to install a Meta\-runner directly to the file system, 
take the Meta\-runner definition file and put it into the `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`\config\projects\<project ID>\pluginData\metaRunners` directory, 
where `<project ID>` is the identifier of a project where you want to place the Meta\-runner.  
If the `metaRunners` directory does not exist, it should be created. Once you place the file on the disk, TeamCity will detect it and load this Meta\-runner; no server restart is required.

If the Meta\-runner is loaded successfully, you will see it listed on the Meta\-Runners page for the project; if you have appropriate permissions, you can modify the definition directly in the TeamCity UI.

The runner is now available in the list of build runners on the build configuration Build Steps page and is represented as a native TeamCity runner with a convenient UI.

A Meta\-runner placed into a project will be available to all its subprojects and build configurations. To make a Meta\-runner available to all projects, place it in the Root project.

## Creating Meta-Runner

You can create a build configuration via the TeamCity web UI and extract a meta\-runner from it or use the xml definition of an existing build configuration as a meta\-runner.

## Creating Your Own Meta-Runner from UI

Let us consider an example of creating a meta\-runner.

To create a meta\-runner, follow these steps (described below in more detail):
1. Prepare a [build configuration to test the build steps we are going to use in our meta-runner](#Preparing+Build+Configuration).
2. [Make sure the build configuration is working](#Verifying+Build+Configuration+Works+Properly).
3. [Extract a meta-runner to the desired project](#Extracting+and+Using+Meta-Runner).

In this example, we will create a meta\-runner to publish some artifacts to TeamCity with the help of corresponding [service message](build-script-interaction-with-teamcity.md#Publishing+Artifacts+while+the+Build+is+Still+in+Progress).

Usually artifacts configured in a build configuration are published when the build finishes. However, sometimes for long builds with multiple build steps we need artifacts faster. In this example, we will create a runner which can be inserted between any build steps and can be configured to publish artifacts produced by previous steps.

### Preparing Build Configuration

The first step is to prepare a build configuration which will work the same way as the meta\-runner we would like to produce. Let us use the configuration with a single Ant build step: Ant can be executed on any platform where the TeamCity agent runs; besides, Ant runner in TeamCity supports build.xml specified right in the runner settings. This is important because our build configuration must be self\-contained, it cannot take build.xml from the version control repository. So in our case the Ant step settings will look like this:

<img src="ant-build-step.png" width="1323" alt="Adding Ant build step"/>

where `artifact.paths` is a system property. We need to add it on the __Parameters__ tab of the build configuration settings:

<img src="paths-to-artifacts-parameter.png" width="1224" alt="Paths to artifacts in Build Parameters"/>

Note that each parameter can have a specification where we can provide the label, description, type of control and specify validation conditions. Before version 8.0 this specification was used by the custom build dialog only. Now this specification is used by a meta\-runner too.

<tip>

Here the Ant build step is used just as an example. In the initial build configuration, you can use any of the available build runners (e.g. MSBuild, .Net process, etc.) and configure the settings, define the parameters for this build step. When you extract a meta\-runner from this build configuration, all the settings defined in the build step, and all the build parameters will be added to the meta\-runner.
</tip>

### Verifying Build Configuration Works Properly

Once the build steps and parameters are defined, we need to make sure our build configuration works by running a couple of builds through the custom build dialog:

<img src="cutom-build-with-paths-to-artifacts.png" width="752" alt="Running a custom build with paths to artifacts"/>

### Extracting and Using Meta-Runner

If the build configuration works properly, we can create a meta\-runner using the __Actions__ button in the top right\-hand corner, __Extract meta\-runner__ option:

<img src="extract-meta-runner.png" width="650" alt="Extract Meta-Runner"/>

The __Extract Meta\-Runner__ dialog requires specifying the project where the meta\-runner will be created. A meta\-runner created in a project will be available in this project and all its subprojects. In our case the __&lt;Root project&gt;__ is selected, so the meta\-runner will be available in all projects.

We also need to provide the name, description and an ID for the meta\-runner: the name and description will be shown in the web interface, an ID is required to distinguish this meta\-runner from others.

Upon clicking the __Extract__ button, TeamCity will take definitions of all build steps and parameters in this build configuration and create a build runner out of them.

<tip>

Besides build steps and parameters, a meta\-runner can also have requirements: if requirements are defined in the build configuration, they will be extracted to the meta\-runner automatically. Requirements can be useful if the tools used by meta\-runner are available on specific platforms only.
</tip>

Once the meta\-runner is extracted, it becomes available in the build runners selector and can be used in any build step just like any other build runner:

<img src="meta-runner-build-step.png" width="827" alt="Publish Artifacts build step"/>

The current meta\-runner usages can be seen at the project __Meta\-Runners__ page:

<img src="meta-runners.png" width="1418" alt="Meta-Runners"/>

When a meta\-runner is extracted, all steps will be extracted. If you need to reorder parameters or make some quick fixes in the runner script, you can edit its raw xml definition in the web browser: go to the __Project Administration__ | __Meta\-Runners__ and use the __Edit__ option next to the meta\-runner. The parameters will be shown in the same order as `<param>` elements in the xml definition. Definitions of meta\-runners are stored in the `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`\config\projects\<project ID>\pluginData\metaRunners` folder.

## Creating Meta-Runner from XML Definition of Build Configuration

Alternatively, you can use the xml definition of an existing build configuration as a meta\-runner. To do it, save the definition of this build configuration to a file named as follows: &lt;runner id&gt;.xml, where &lt;runner id&gt; is the [ID](identifier.md) of this build runner. Install the meta\-runner as described [above](#Installing+Meta-Runner).

Since a meta\-runner looks and works like any other runner, it is also possible to create another meta\-runner on the basis of an existing meta\-runner.

## Creating Build Configuration from Meta-Runner

If you need to fix a meta\-runner and test your fix, you can create a build configuration from a meta\-runner, can change its steps, adjust parameters and requirements, check how it works, and then use the __Extract meta\-runner__ action to apply the changes to the existing meta\-runner with the same ID.
