[//]: # (title: TeamCity Sakura UI)
[//]: # (auxiliary-id: TeamCity Sakura UI)

> The Sakura UI is now the default UI for new TeamCity users.

The Sakura UI, first introduced in TeamCity 2019.1 as an alternative to the classic UI, is now the default TeamCity UI. While the classic UI is still available and is maintained, the new product features will be supported in the Sakura UI only.

We recommend that you use the Sakura UI and benefit from a fresh, modern interface created with accessibility in mind. This new UI is actively developed, and your [feedback is welcome](https://surveys.jetbrains.com/s3/feedback-form-for-teamcity?tcv=2020.10).

## Enabling and disabling Sakura UI

When you sign in to TeamCity for the first time, the Sakura UI is enabled by default.

If you were using the classic UI, after upgrading TeamCity will offer you to switch to the Sakura UI and will remember your choice.

You can change the default UI in __Your Profile | UI settings__ via the _Use Sakura UI_ checkbox.

All pages supporting the new UI have a toggle to quickly access the classic UI: <img src="mammoth.png" alt="Switch to the classic UI" height="20" width="20"/>.

To return to the Sakura UI, click the magic wand icon: <img src="magic-wand.png" alt="Switch to the Sakura UI" height="20" width="20"/>.


The section below provides an overview of the differences between the Sakura UI and the classic TeamCity UI.

## Features of Sakura UI

The new UI comes with a handy __[sidebar](#Sakura+sidebar)__ that serves for quick navigation.

Currently, the Sakura UI is available for the following pages:
* __[Project Home](#Sakura+Project+Home+page)__ and __[Build Configuration Home](#Sakura+Build+Configuration+Home+page)__ pages: redesigned __Overview__ tab
* __[Build Results](#Sakura+Build+Results+page)__ page: redesigned __Overview__, __Tests__ and __Test History__, __Changes__, __Build Log__, and __Dependencies__ tabs
* __[Agents](#Sakura+Agents+page)__ page
* __[Queue](#Sakura+Queue+page)__ page


Below, you’ll find a recap of these features.

### Sakura sidebar

This improved sidebar displays the project hierarchy and, for the Agents page, shows agent hierarchy and serves for better navigation between projects, build configurations, or build agents.

You can change the sidebar width by dragging its frame border and hide/show it anytime by clicking the corresponding button at its bottom.

To customize a sidebar, click ![wn-pencil.png](wn-pencil.png) in its upper-right corner. In the customization menu, you can move a project or build configuration in the list via a keyboard or using the arrow UI button, or mark/clear projects as your favorite.

#### Projects sidebar

The _Projects_ sidebar lists all the projects available to the current TeamCity user and allows searching them by name. 
You can expand any project to see its nested subprojects / build configurations and quickly switch between them. 
The status icons and counters for all its nested objects are also displayed in the sidebar.
<img src="sidebar_search.gif" alt="Sakura projects sidefar" width="300" style="block"/>

The sidebar allows accessing the __Favorite projects__ and __Favorite builds__ views with the lists of your favorite projects and favorite build configurations respectively.  
You can also toggle the display of archived projects.

You can use the __Q__ keyboard shortcut to focus on the projects' search field. When focused, use __↑__ and __↓__ keyboard arrows to navigate between search results. To remove the focus, press __Esc__.

### Sakura Project Home page

The __Overview__ tab of the __Project Home__ page provides more visibility of the project's subprojects and build configurations. The page has two main views: __Builds__ and __Trends__.

The __Trends__ view comprises cards that represent build configurations of the selected project.

<img src="project-trends.png" alt="Sakura Project Trends page"/>

Each card contains a preview of the most recent builds displayed as bars on a timescale. You can hover over any bar to instantly see more information about the build: its duration, queue statistics, test results, used agent, and more. The card also displays the number of pending changes.
![project-trends.gif](project-trends.gif){style="block"}

The __Builds__ view is similar to the classic UI and displays the list of the recent builds in subprojects and build configurations of the current project.
![project-builds.png](project-builds.png){style="block"}

### Sakura Build Configuration Home page

The __Overview__ tab of the Sakura __Build Configuration Home__ page provides two views:

* __Branches__, listing the recent builds in active branches
![bc-branches.png](bc-branches.png){style="block"}

* __Builds__, listing all the recent builds of the configuration
![bc-builds-history.png](bc-builds-history.png){style="block"}
 
Every build item in the list is expandable: click it to preview the most important information about the build and get quick access to any of the __Build Results__ tabs.

Click a specific build problem or failed test to see the related stack trace: ![build-test-stacktrace.png](build-test-stacktrace.png){style="block"}

Click __Open in build log__ to open the new build log exactly at the line where the problem occurred.

The _Changes_ pop-up block displays build changes sorted chronologically and grouped by their origin: user commits to the code and changes in artifact dependencies. You can also filter the changes by their author and display changes made in the build configuration settings.

Example of the _Changes_ pop-up block:

<img src="exp-changes-popup.png" alt="Sakura Changes pop-up" width="400"/>

### Sakura Build Results page

This page visualizes build results and provides several handy widgets.

The __Actions__ menu offers several handy options: ![build-actions.png](build-actions.png){style="block"}

<tip>

Choose __Select for comparison__ to compare the parameters and results of the current build with any other selected build side-by-side. 
This option is also available from the build's context menu in the build list.
</tip>

#### Overview tab

In the upper right corner, you can see the trends for the previous builds of the current build configuration. Hovering over a bar brings up a pop-up with the build details.

<img src="build-trends-preview.png" width="250" alt="Build trends preview"/>

The interactive graphic timeline reflects the duration of each build stage and indicates build problems if any:

<img src="build-timeline.png" width="600" alt="Build timeline"/>

Click any stage to open the corresponding line of the build log. The long logs can also be displayed directly in the preview, without downloading.

#### Changes tab

The __Changes__ tab displays more information about the changes in the build, separately for user commits and artifact changes. 
You can filter changes by their author and display changes made in the build configuration settings.![build-changes.png](build-changes.png){style="block"}

#### Tests tab

The __Tests__ tab allows switching between failed, ignored, and succeeded tests. Click a test to quickly view its details or, for example, to assign an investigation. You can also view its history on the new __Test History__ page.

The __Test History__ page has better performance now. It also has an adjustable range slider allowing you to select a period of the test history that will be displayed below. The tests timeline is interactive:![build-test-history.gif](build-test-history.gif){style="block"}

The list of builds that ran this test has improved usability now: on clicking a build, the stacktrace is displayed.


#### Dependencies tab

The __Dependencies__ tab boasts of a new more user-friendly design.

It provides three alternative modes of displaying the build dependencies: a visual timeline, structured list, and build chain. Choose the mode that is the most helpful for your current task.

The interactive timeline shows the sequence of builds and the duration of each build in this pipeline. It has an adjustable range slider allowing you to select a build of the pipeline. ![dependencies-timeline.png](dependencies-timeline.png){style="block"}

The list is a flat line of the builds, which also has a lot of interactive features: you can click and expand a build line and review the results.![dependencies-list.png](dependencies-list.png){style="block"}

The chain page loads faster and demonstrates build dependencies more clearly. ![dependencies-chains.png](dependencies-chains.png){style="block"}

### Sakura Agents page

The Sakura __Agents__ page loads faster for a large number of agents and allows switching between agent details quickly.

The page provides a better hierarchical view of agent pools and makes it easier to see all their assigned projects and cloud images. 

The __All Agents__ view gives a quick preview of all agents' statuses and allows managing them side by side, on a single dashboard.

You can edit the scope of [agent pools](configuring-agent-pools.md) and quickly assign agents and projects to them.

To edit an agent pool, click **Assign agents** in its settings. In this dialog, you can choose what agents you want to assign to the pool:

<img src="edit-agent-pool.png" alt="Edit agent pool in new TeamCity UI" width="460"/>

Note that whenever you select a cloud image, you actually assign all its instances to the pool. If this pool has a limit of agent slots, each cloud instance will take a single slot, just like any regular agent.

Similarly, you can also associate projects with this pool: open the **Projects** tab of the pool's settings and click **Assign projects**. This way, agents assigned to this pool will be allowed to run builds only in the selected projects.

#### Agents sidebar

The _Agents_ sidebar allows browsing the agent pool hierarchy including cloud profiles and searching agents and pools by name. 
The __Overview__ view provides statistics about all the agents on the server.![agents-sidebar-overview.png](agents-sidebar-overview.png){style="block"}


### Sakura Changes page

The Sakura [Changes page](viewing-user-changes-in-builds.md) comes with filters providing flexible search options allowing you to sort changes by comment (commit message), by path to the changed file, and by revision number.

![changes-page.png](changes-page.png)


### Sakura Queue page

On this page, you can see the position of a build in the queue and view the queued build's details:

<img src="queue-page.png" alt="Sakura build queue"/>

### Plugin API

The Sakura UI extends the TeamCity plugin API and provides [a set of handy tools](https://plugins.jetbrains.com/docs/teamcity/front-end-extensions.html) to create plugins. ![plugin-api-qodana.png](plugin-api-qodana.png){style="block"}
