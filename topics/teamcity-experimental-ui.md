[//]: # (title: TeamCity Experimental UI)
[//]: # (auxiliary-id: TeamCity Experimental UI)

In version 2019.1, TeamCity introduced an experimental UI option as an alternative to the classic UI. Our experimental UI is a work in progress: we present the changes in functionality in the early stages of development so you can benefit from the new features as soon as possible. Based on our vision and your feedback, we are constantly releasing new UI features and improving existing ones.
 
Our goal is to provide a support for all the important classic features in the new UI before making it a default option. However, many users of TeamCity have successfully switched to the new UI already and use it in terms of their production pipeline.

We encourage you to try the TeamCity experimental UI and leave your feedback via our [UI survey](https://surveys.jetbrains.com/s3/feedback-form-for-teamcity?tcv=2019.2).
  
## Enabling and disabling experimental UI

When you sign in to TeamCity for the first time, it automatically offers you to switch to the experimental UI and remembers your choice.

You can change the default UI representation anytime in __My Settings & Tools | General__ via the _Use experimental UI_ checkbox.
 
Any page that supports the experimental UI has a toggle that allows quickly accessing a UI mode alternative to the default one. For example, if you miss any of the familiar classic options in the new UI, you can switch the page to the classic UI with the ![mammoth.png](mammoth.png) button. To return to the experimental UI, click ![tube.png](tube.png).

## Available features of experimental UI

Currently, the experimental UI is available for the following pages:
* __[Project Home](#Experimental+Project+Home+page)__ and __[Build Configuration Home](#Experimental+Build+Configuration+Home+page)__ pages: redesigned __Overview__ tab
* __[Build Details](#Experimental+Build+Details+page)__ page: redesigned __Overview__, __Tests__, __Changes__, __Build Log__, and __Dependencies__ tabs
* __[Agents](#Experimental+Agents+page)__ page

Each experimental page comes with a handy __[sidebar](#Experimental+sidebar)__ that serves for quick navigation and preview of build/agent statuses.

Below, you can find a recap of these features. Please note that the described functionality is a work in progress. In case you face any unpredictable behavior in the new UI, feel free to contact us via our [feedback channels](https://confluence.jetbrains.com/display/TW/Feedback).

### Experimental Project Home page

The __Overview__ tab of the experimental __Project Home__ page strives to provide more visibility of the project's nested subprojects and build configurations. The page has two main views: __Builds__ and __Trends__.

The __Builds__ view resembles the classic UI and displays a list of the recent builds in subprojects and build configurations of the current project.

The __Trends__ view comprises cards that represent build configurations, grouped by their projects. Each card contains a preview of the most recent builds displayed as bars on a timescale. You can hover over any bar to instantly see more information about the build: its duration, queue statistics, test results, used agent, and more. The card also displays the number of pending changes.

The __Trends__ view of the __Project Home__:

<img src="exp-project-home.png" alt="Experimental Project Home page"/>

All the classic UI tabs are also available on the experimental page: click __More__ and select the required tab in the list.

### Experimental Build Configuration Home page

The __Overview__ tab of the experimental __Build Configuration Home__ page provides two already familiar views: __Builds__, listing all the recent builds of the configuration, and __Branches__, listing the recent builds in active branches.

Every build item in the list is expandable: click it to preview the most important information about the build and get quick access to any of the __Build Details__ tabs.

<img src="exp-build-home.png" alt="Experimental Build Home page"/>

Click a specific build problem or failed test to see the related stack trace: <img src="exp-failed-test-preview.png" alt="Failed test preview"/>

Click __Open in build log__ to open the new build log exactly at the line where the problem occurred. 

The _Changes_ pop-up block has also been reworked. Now, build changes are sorted chronologically and grouped by their origin: user commits to the code and changes in artifact dependencies. You can also filter the changes by their author and display changes made in the build configuration settings.

Example of the _Changes_ pop-up block:

<img src="exp-changes-popup.png" alt="Experimental Changes pop-up" width="400"/>

All the classic UI tabs of __Build Configuration Home__ are also available on the experimental page: click __More__ and select the required tab in the list.

### Experimental Build Details page

The new __Build Details__ page offers better visualization of the build results and provides a few handy widgets.

<img src="exp-build-details.png" alt="Experimental Build Details page"/>

With the __Trends__ block, you can instantly preview all the previous builds and their details, without leaving the current build page:

<img src="build-trends-preview.png" width="250" alt="Build trends preview"/>

The graphic timeline reflects the duration of each build stage and indicates build problems:

<img src="build-timeline.png" width="600" alt="Build timeline"/>

Click any stage to open the corresponding line of the build log. In the new UI, even a long log can be displayed directly in the preview, with no need to download it.

<tip>

The __Actions__ menu offers a handy new option – __Compare with__. Choose this action to compare the parameters and results of the current build with any other selected build side-by-side.   
This option is also available from the build's context menu in the build list.

</tip>

Apart from the __Overview__ tab, you can use the revamped __Changes__, __Tests__, and __Dependencies__ tabs:

* The __Changes__ tab displays more information about changes in the build, separately for user commits and artifact changes. You can filter changes by their author and display changes made in the build configuration settings.
* The __Tests__ tab allows switching between failed, ignored, and succeeded tests. Click a test to quickly view its details or, for example, to assign an investigation.
* The __Dependencies__ tab provides three alternative modes of displaying the build dependencies: a visual timeline, structured list, and build chain. Choose the mode that is the most helpful for your current task.

Other classic UI tabs are also available: click __More__ and select the required tab in the list.

### Experimental Agents page

The experimental __Agents__ page loads faster for a large number of agents and allows quickly switching between agent details.

<img src="exp-agents-page.png" alt="Experimental Agents page"/>

The page provides a better hierarchical view of agent pools and makes it easier to see all their assigned projects and cloud images.

The __All Agents__ view gives a quick preview of all agents' statuses and allows managing them side by side, on a single dashboard.

### Experimental sidebar

The experimental sidebar is available for the project hierarchy and agent hierarchy. Depending on the current page, it serves for better navigation between either projects and build configurations, or build agents.

You can change the sidebar width by dragging its frame border and hide/show it anytime by clicking the corresponding button at its bottom.

To customize a sidebar, click ![wn-pencil.png](wn-pencil.png) in its upper-right corner. In the customization menu, you can move a project or build configuration in the list via a keyboard or using the arrow UI button, or mark/clear projects as favorite.

#### Projects sidebar

The _Projects_ sidebar lists all the projects available to the current user of TeamCity and allows searching them by name. You can expand any project to see its nested subprojects / build configurations and quickly switch between them. If a project is added to your Favorites, you will also see the status icons and counters for all its nested objects directly in the sidebar.

The sidebar allows accessing the __Favorite projects__ and __Favorite builds__ views with the lists of your favorite projects and favorite build configurations respectively.   
You can also toggle the display of archived projects.

You can use the __Q__ keyboard shortcut to focus on the projects' search field. When focused, use __↑__ and __↓__ keyboard arrows to navigate between search results. To remove the focus, press __Esc__.

#### Agents sidebar

The _Agents_ sidebar allows browsing the agent pool hierarchy and searching agents and pools by name. The __Overview__ view provides statistics about all the agents on the server.

## Roadmap

The next improvements of the experimental UI will affect the following areas:
* Build Queue page
* Sidebar and Header areas
* Mutes/Investigations
