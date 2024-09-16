[//]: # (title: View Build Results)
[//]: # (auxiliary-id: View Build Results)

TeamCity offers a comprehensive overview of build results:
* Detailed build log
* Test results and problems
* Pipeline statuses
* Artifacts
* Changes in sources
* and many more, including custom graphs

In this article, we give a quick overview of how to explore build results in TeamCity. Note that the examples showcase the Sakura UI, but if you prefer the classic UI, you can find many of these features in it as well. To switch between the UIs, use the toggle in the upper right corner of the screen: <img src="mammoth.png" alt="Switch to the classic UI" height="20" width="20"/> for the classic UI and <img src="magic-wand.png" alt="Switch to the Sakura UI" height="20" width="20"/> for the Sakura UI.

## Preview Projects and Build Configurations

TeamCity has two main modes: the __Home__ mode is for viewing results and running builds, and the __Edit__ mode is for configuring settings. The switch between them is located in the upper right corner of the screen. Note that it's only available if you have rights to edit projects' settings.

Both __Project Home__ and __Build Configuration Home__ allow you to monitor builds and perform all the basic actions.

__Build Configuration Home__ shows statuses of the recent builds, per branch or all combined. To preview a build without opening its results, you can just expand it in the list:

<img src="exp-build-home.png" alt="Sakura Build Home page"/>

__Project Home__ gives a more general overview of all the project's subprojects and build configurations. The _Builds_ mode aggregates __Build Configuration Home__ views of the nested configurations. The _Trends_ mode displays configurations as interactive graphs:

<img src="exp-project-home.png" alt="Sakura Project Home page"/>

## Explore Build Results

If you click a build in the list, its __Build Results__ page will open.

<img src="exp-build-details.png" alt="Sakura Build Results page"/>

The heading part of this page shows:
* General information like number, date, and statistics.
* Branch selector.
* The __Actions__ menu from where you can label, pin, compare builds, and perform other operations.
* The __Run__ button.
* A switch to the settings menu.
* A widget with the recent builds' statuses. Hover over any of them to see its details.

Below the heading, there are multiple tabs you can explore for more details. The most important ones are:
* __Overview__: a summary of all the results. Contains an interactive build timeline and collapsible blocks with summary of other tabs.
* __Changes__: revisions and commits that got to this build.
* __Tests__: all the information on tests. Here, you can explore the test history, download test results in CSV, as well as mute tests, assign their investigator, or mark them as fixed.  
  TeamCity puts a lot of effort into making the test preview as informative and rich as possible. Moreover, you can view test results on-the-fly, while the build is still running.
* __Dependencies__: a visualized build chain, or pipeline, the current build belongs to. Three modes with different levels of detail are available.
* __Artifacts__: browse and download the build artifacts.
* __Parameters__: see what parameters' values were passed into this build.

The set of displayed tabs depends on the build's specifics (see [more information](working-with-build-results.md) on the default tabs). Some of them are displayed only if a certain feature is enabled. For example, if you add the [](docker-support.md) feature to a build, the __Docker Info__ tab will appear in its results.

You can also add a tab with [custom charts](custom-chart.md) based on XML.

## View Build Log

A detailed and structured build log is one of the strongest points of TeamCity.

You can access a build log both from the __Overview__ and __Build Log__ tabs. If there are any problems or failed tests in the build, you can quickly view the relevant parts of the log right in the __Overview__. You can also click on any place on the build timeline to open the related part of the log.

<img src="build-timeline.png" width="700" alt="Build timeline"/>

The __Build Log__ tab shows a structured log with highlighted warnings and errors. Its main blocks are by default collapsed, so the log is easier to navigate. You can explore the log manually, search and filter messages by their text and type, or download the full log.

If you want to customize how TeamCity produces the log output, use special [service messages](build-log.md#Customizing+Log+Output).

## Search in All Build Logs
{instance="tc"}

<include element-id="search-in-logs" from="search.md"/>

## Extras

Apart from viewing the __Build Results__ page directly, you can monitor the build statuses by:
* Configuring [notifications](notifications.md) to a messenger, email, or in a browser.
* Integrating TeamCity with a third-party service. For example, if you use Jira Cloud, you can display the main build results right in the tasks related to each build ([learn how](jira-cloud-integration.md)).
* Publishing build statuses to your VCS repositories via [Commit Status Publisher](commit-status-publisher.md).
* Using our plugins for IDEs, such as [IntelliJ Platform Plugin](intellij-platform-plugin.md).
