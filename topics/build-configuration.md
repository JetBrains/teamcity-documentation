[//]: # (title: Build Configuration)
[//]: # (auxiliary-id: Build Configuration)

A _Build Configuration_ is a collection of settings used to start a build and group the sequence of the builds in the UI. Examples of build configurations are _distribution_, _integration tests_, _prepare release distribution_, _"nightly" build_. 

A build configuration belongs to a [project](project.md) and contains builds. You can explore details of a build configuration on its [home page](viewing-build-configuration-details.md) and modify its settings on the [editing page](creating-and-editing-build-configurations.md).

It is recommended to have a separate build configuration for each sequence of builds (that is performing a specified task in a dedicated environment). This allows for proper features functioning, like detection of new problems/failed tests, first failed in/fixed in tests status, automatically removed investigations, and so on.

To tackle an increased number of build configurations you can use [build configuration templates](build-configuration-template.md) and project-level [parameters](configuring-build-parameters.md).

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## Build Configuration Settings

Build configuration settings include:
* [General settings](configuring-general-settings.md)
* [Version control settings](vcs-root.md), defining how the source code is retrieved from VCS, where it is checked out to, and so on
* [Build steps](configuring-build-steps.md), that are sequentially run actions: for example, running msbuild, a script, or unit tests
* [Triggers](configuring-build-triggers.md), which are rules defining when to start a new build
* [Failure conditions](build-failure-conditions.md) specifying when a build will be marked as failed
* Additional [build features](adding-build-features.md)
* Dependencies:  
    * for [snapshot dependencies](snapshot-dependencies.md), TeamCity will run all dependent builds on the sources taken at the moment the build they depend on starts
    * for [artifact dependencies](artifact-dependencies.md), before a build is started, all artifacts this build depends on will be downloaded and placed in their configured target locations and then will be used by the build
* [Parameters](configuring-build-parameters.md) which allow sharing settings
* Agent requirements specifying whether a [build configuration](build-configuration.md) can run on a particular [build agent](build-agent.md).


<note>

Build configuration settings and build behavior may vary depending on the type of build configuration.
</note>

## Build Configuration Types

The following build configuration types exist in TeamCity:

* regular build configuration, defining actions and rules to apply to the source code; all the settings above are applicable
* [deployment build configuration](deployment-build-configuration.md), which deploys artifacts of other builds to some environment
* [composite build configuration](composite-build-configuration.md), which aggregates results from several other builds combined by snapshot dependencies and presents them in a single place

## Build Configuration State

A build configuration is characterized by its state visible in the UI which can be _paused_ or _active_. By default, when created all configurations are active and can be paused manually as described below or automatically if the project is [archived](archiving-projects.md).

If a build configuration is _paused_, its [automatic build triggers](configuring-build-triggers.md) are disabled until the configuration is activated. Still, you can start a build of a paused configuration manually or automatically as a part of a [Build Chain](build-chain.md). Besides, information on paused build configurations is not displayed on the [Changes](viewing-your-changes.md) page.

It is possible to manually pause all or selected build configurations for a project.

### Pausing / Activating a single build configuration

On the __Build Configuration Settings__ or __Home__ page:
* Open the __Actions__ menu, click __Pause__, and enter your comment in the _Pause_ dialog (optional).  
To remove the builds of the paused build configuration from the [build queue](build-queue.md), check the _Cancel already queued builds_ box.
Click __Pause__ to confirm.
* To activate a paused build configuration, select __Activate__ in the __Actions__ menu.

### Pausing / Activating several build configurations of a project

__To pause several build configurations of a project__:

1. On the __Project Settings__ page, open the __Actions__ menu and click __Pause/Activate__.
2. In the dialog that opens, select the box next to the project to pause all its build configurations or check the boxes of the configurations selectively: the checked build configurations will be paused.
3. Add an optional comment.
4. To remove all the builds of the paused configurations from the [build queue](build-queue.md), check the _Cancel already queued builds if build configuration is paused_ box.
5. Click __Apply__. 

__To activate several build configurations of a project:

1. On the __Project Settings__ page, open the __Actions__ menu and click __Pause/Activate__.
2. In the dialog that opens, clear the box next to the project to activate all its build configurations or clear the boxes of the configurations selectively: the unselected build configurations will be activated.
3. Add an optional comment.
4. Click __Apply__. 

## Build Configuration Status

In general, a build configuration status reflects the status of its last finished build.

Note that [Personal builds](personal-build.md) do not affect the build configuration status.

You can view the status of all build configurations for all/particular project on the __Projects__ Overview page or Project Home Page, when the details are collapsed.

Build configuration status icons:

<table><tr>

<td width="250">

Icon


</td>

<td>

Description


</td></tr><tr>

<td>

![Successful.png](Successful.png)


</td>

<td>

The last build on default branch executed successfully.


</td></tr><tr>

<td>

![Failed.png](Failed.png)


</td>

<td>

The last build on the default branch executed with errors or one of the currently running builds is failing. The build configuration status will change to "failed" when there's at least one currently running and failing build, even if the last finished build was successful.


</td></tr><tr>

<td>

![investigate.gif](investigate.gif)


</td>

<td>

Indicates that someone has started investigating the problem, or already fixed it. (see [Investigating and Muting Build Problems](investigating-and-muting-build-problems.md)).


</td></tr><tr>

<td>

_no icon_


</td>

<td>

There were no finished builds for this configuration, the status is unknown. If none of the build configurations in a project have finished builds, the ![NoBuildsProject.png](NoBuildsProject.png) is displayed next to a project name


</td></tr><tr>

<td>

![BCPaused.png](BCPaused.png)


</td>

<td>

The build configuration is paused; no builds are triggered for it. Click on the link next to the status to view by whom it was paused, and activate configuration if needed.


</td></tr></table>

### Status Display for Set of Build Configurations

It is possible to filter out the build configurations whose status you want to be displayed in TeamCity or externally.

To display the status of selected build configurations in __TeamCity__:
* configure visible projects on the Projects Overview page to display the status of build configurations belonging to these projects only
* implement a [custom Java plugin](https://confluence.jetbrains.com/display/TCD18/Developing+TeamCity+Plugins) for TeamCity to make the page available as a part of  the TeamCity web application

To display the status for a set of build configurations __externally__ (on your company's website, wiki, Confluence, or any other web page), you can:
* use the [external status widget](configuring-general-settings.md#HTML+Status+Widget)
* use the [build status icon](rest-api.md#Build+Status+Icon)
* use any of the available [visualization plugins](https://plugins.jetbrains.com/search?correctionAllowed=true&pr=teamcity&orderBy=relevance&tags=Notification%2FVisualizers&search=)
* implement a separate page or application which will get the build configuration status via the TeamCity [REST API](rest-api.md)

 __  __

__See also:__

__Administrator's Guide__: [Creating and Editing Build Configurations](creating-and-editing-build-configurations.md)
