[//]: # (title: Working with Build Results)
[//]: # (auxiliary-id: Working with Build Results)

In TeamCity a build goes through several states:
* Upon some event the build trigger adds the build to the queue where the build stays waiting for a free agent.
* The build starts on the agent and performs all configured build steps.
* The build finishes and becomes a part of the build history of this build configuration.

In TeamCity all information about a particular build, whether it is queued, running or finished, is accumulated on the __Build Results__ page. The page can be accessed by clicking the build number or build status link.

Besides providing the build information, this page enables you to:
* [run a custom build](triggering-a-custom-build.md) using the __Run__ button
* use the __Actions__ menu to do the following: 
  * add a build to [favorites](favorite-build.md)
  * add a comment
  * [pin the build](pinned-build.md)
  * [tag the build](build-tag.md)
  * change the build status, marking the build as [failed](changing-build-status-manually.md#Marking+build+as+failed) or [successful](changing-build-status-manually.md#Marking+build+as+successful)
  * [label this build sources](vcs-labeling.md)
  * remove the build
* [edit the configuration settings](creating-and-editing-build-configurations.md#Creating+Build+Configuration+From+Template)

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## Build Details

The __Build Results__ page can be accessed from the __Build Configuration Home__ page and from various places in the TeamCity web UI where a build number or build status appears as a link. Some data is accessible only after the build is finished, some details like [Changes](#Changes), [Build Parameters](#Parameters), and [Dependencies](#Dependencies) are also applicable to the build while it is waiting in the queue.

The build information available on the page is described in sections below.

Depending on the build runners enabled as your build steps, the number of tabs on the page and the information on the __Overview__ tab may vary.

### Build Overview

The __Overview__ tab displays general information about the build, such as the build duration, the agent used, trigger, dependencies, and so on. If a build is queued, the tab displays the position of the build in the queue, the time the build is supposed to start, and so on.

If a build is running, the tab displays the build progress. You can also stop a running build using the corresponding link on the __Overview__ tab or the appropriate option from the __Actions__ button drop-down.

If another build of this same build configuration is concurrently running, a small window appears on the __Overview__ tab with the following information:
* The build results of that build linking to the __Build results__ page
* A changes link with a drop-down list of changes included in that build
* The build number, time of start, and a link to the agent it is running on.

If a build is probably hanging, the corresponding notification is displayed at the top of the __Overview__ tab. In this case TeamCity provides a link to view the process tree of the running build and the thread dumps of each Java or .NET process in a separate frame. If the process is not Java or .NET, its command line is retrieved. The possibility to view the thread dump is supported for the following platforms:
* Windows, with JDK 1.3 and higher
* Windows, with JDK 1.6\-1.8, using jstack utility
* Non\-Windows, with JDK 1.5\-1.8, using jstack utility

The information on the tab may vary depending on the build runners enabled. If configured, you will see the __Code Coverage Summary__ and a link to the full report or/ and the number of duplicates found in your code and a link opening the __Duplicates__ tab, and so on. Refer to the sections below for details on [Code Coverage](#Code+Coverage+Results) and [Duplicates](#Duplicates) Tabs.

If there were problems in the build, the __Overview__ tab also displays them.

If the build has failed tests, you can view them on the __Overview__ tab of the __Build Results__ page.

#### Tests

Successful or failed tests in this build (if any) are displayed on the __Overview__ tab.

For each failed test, you can view its stacktrace, the diff between the expected and actual values, jump to the test [history](#Test+History), [assign a team member to investigate its failure](investigating-and-muting-build-failures.md), [open the test in your IDE](installing-tools.md) and/or start fixing it right away.

To view all tests related to the build, use the dedicated __Tests__ tab. [Learn more](#All+Tests).

## Changes

From the __Changes__ tab you can:
* Review all changes included in the build with their corresponding [revisions](revision.md) in version control 
  * Review changes included in the build that the current build depends on: if the current build configuration has an artifact dependency, and the artifact downloaded in the current build has changed compared to the one downloaded in the previous build of the current configuration, the __Artifact dependencies changes__ node appears displaying the build which was used to download artifact dependencies from, and the changes which were included in that build.
* [Label the build sources](vcs-labeling.md) (prior to TeamCity 8.1)
* [Configure the VCS settings](configuring-vcs-settings.md) of the build configuration (if you have enough permissions).

For each change on this page you can:
* Explore the change in details
* View which dependent build the changes come from or builds with snapshot dependencies with the  "Show changes from snapshot dependencies" option  enabled (__since TeamCity 2017.1)__. On hovering over the ![link.png](link.png) icon, the number of the dependent build is displayed; clicking the link opens the Сhanges tab of the dependent build.
* Navigate to the __Change Details__ by clicking a changed file link
* [Trigger a custom build](triggering-a-custom-build.md) with this change
* Download patch
* Download patch to your IDE
* Review the change in an [external change viewer](external-changes-viewer.md), if configured by the administrator.

The __Changes__ tab provides advanced filtering capabilities for the list of changes. Enabling __Show graph__ displays the changes as a graph of commits to the VCS roots related to this build.

The graph appears on the left of the list and allows you to get the view of the changes with a variable level of detailing. You can:
* View the VCS roots which were changed in this build, each of the roots being represented as a bar.
* Navigate to a graph node to display the VCS root revision number.
* Click a bar to select a single VCS root. The changes not pertaining to this root are grayed out.
* If there have been merges between the branches of the repository, the graph displays them. To collapse a bar, navigate to its darker area and click it to hide history of merges. The dotted line will indicate that the bar is expandable.
* If your VCS root has subrepositories (marked S in the list of changes), navigate to a node in the parent to see which commits in subrepositories are referenced by this revision in the parent.

You can select to view the modified files by checking the __Show files__ box. Clicking a file name opens the diff viewer.

## All Tests

To view all the tests for a particular build, open the __Build Results__ page, and navigate to the __Tests__ tab. On this page:

<table><tr>

<td>

Item


</td>

<td>

Description


</td></tr><tr>

<td>

Download all tests in CSV


</td>

<td>

Click the link to download a file containing all the build tests results.


</td></tr><tr>

<td>

Filtering options


</td>

<td>

Use this area to filter the test list:

* Select the type of items to view: tests, suites, packages/namespaces or classes.
* Type in the string (for example, test name from the list) thus providing change of scope for the list.
* Select a test status.


</td></tr><tr>

<td>

Show


</td>

<td>

Select the number of tests to be shown on a page.


</td></tr><tr>

<td>

Status


</td>

<td>

Shows the status (OK, Ignored, and Failure) of the test. Failed tests are shown as red __Failure__ links, which you can click to view and analyze the test failure details. Click the header above this column to sort the table by status.


</td></tr><tr>

<td>

Test


</td>

<td>

Click the name of a class, a namespace/package, or a suite to view only the items included in it. Click the arrow next to the test name to view the test history, open the test in the Build Log, start investigation of the failed test, or open the failed test in IDE.


</td></tr><tr>

<td>

Duration


</td>

<td>

Shows the time it took to complete the test. You can view the Test Duration Graph described below by clicking this icon: ![StatsIcon.png](StatsIcon.png).


</td></tr><tr>

<td>

Order


</td>

<td>

Shows the sequence in which the tests were run. Click the header above this column to sort by the test order number.


</td></tr></table>

#### Test History

To navigate to the history of a particular test, click the arrow next to the test name and select __Test History__ from the drop-down menu.   
There are several places where tests are listed and from where you can open Test History.   
For example:
* __Project Home__ page | __Current Problems__ tab
* __Project Home__ page | __Current Problems__ tab | __Problematic Tests__
* __Build Results__ page | __Overview__ tab
* __Build Results__ page | __Tests__ tab
* __Projects__ | __&lt;build with failed tests&gt;__ | build results drop-down menu


Clicking the __Test history__ link opens the __Test details__ page where you can find following information:
* The test details section including test success rate and test's run duration data:
* the Test Duration Graph. For more information, refer to the [Test Duration Graph](#Test+Duration+Graph) description below.
* Complete test history table containing information about the test status, its duration, and information on the builds this test was run in.


#### Test Duration Graph

The Test Duration graph (see the screenshot above) is useful for comparing the amount of time it takes individual tests to run on the builds of this build configuration.

Test duration results are only available for the builds which are currently in the build history. Once a build has been [cleaned up](clean-up.md), this data is no longer available.

You can perform the following actions on the Test Duration Graph:
* Filter out the builds that failed the test by clearing the __Show failed__ option.
* Calculate the daily average values by selecting the __Average__ option.
* Click a dot plotted on the graph to jump to the page with the results of the corresponding build.
* View a build summary in the tooltip of a dot on the graph and navigate to the corresponding __Build Results__ page.
* Filter information by agents selecting or clearing a particular agent or by clicking __All__ or __None__ links to select or clear all agents.


## Build Log

For each build you can view and download its build log. More information on build logs in TeamCity is available [here](build-log.md).

## Parameters

All system properties and environmental variables which were used by a particular build are listed on the __Parameters__ tab of the __Build Results__ page. [Learn more about build parameters](configuring-build-parameters.md).

The __Reported statistic values__ page shows [statistics values](custom-chart.md#Default+Statistics+Values+Provided+by+TeamCity) reported for the build and displays a statistics chart for each of the values on clicking the _View Trend_ icon ![ViewTrend.PNG](ViewTrend.PNG).

## Dependencies

If a finished build has artifact and/or snapshot dependencies, the __Dependencies__ tab is displayed on the __Build Results__ page. Here you can explore builds whose artifacts and/or sources were used for creating this build (Downloaded artifacts) as well as the builds which used the artifacts and/or sources of the current build (Delivered artifacts). Additionally, you can view indirect dependencies for the build. That is, for example, if build A depends on build B which depends on builds C and D, then these builds C and D are indirect dependencies for build A.

## Related Issues

If you have [integration with an issue tracker ](integrating-teamcity-with-issue-tracker.md) configured, and if there is at least one issue mentioned in the comments for the included changes or in the comments for the build itself, you will see the list of issues related to the current build in the __Issues__ tab.

<tip>

If you need to view all the issues related to a build configuration and not just to particular build, you can navigate to the __Issues Log__ tab available on the build configuration home page, where you can see all the issues mapped to the comments or filter the list to particular range of builds.
</tip>

## Build Artifacts

If the build produced [artifacts](build-artifact.md), they all are displayed on the dedicated __Artifacts__ tab.

## Code Coverage Results

If you have code coverage configured in your build runner, a dedicated tab with the full HTML code coverage report appears on the __Build Results__ page.

By clicking the links in the __Coverage Breakdown__ section, you can drill\-down to display statistics for different scopes, for example, Namespace, Assembly, Method, and Source Code.

## Code Inspection Results

If configured, the results of the [Code Inspection](code-inspection.md) build step are shown on the __Code Inspection__ tab. Use the left pane to navigate through the inspection results; the filtered inspections are shown in the right pane.
* Switch from the __Total__ to __Errors__ option, if you're not interested in warnings.
* Use the scope filter to limit the view to the specific directories. This makes it easier for developers to manage specific code of interest.
* Use the inspections tree view under the scope filter to display results by specific inspection.
* Note that TeamCity displays the source code line number that contains the problem. Click it to jump the code in your IDE.

## Duplicates

If your build configuration has Duplicates build runner as one of the build steps, you will see the __Duplicates__ tab in the __Build Results__.

The tab consists of:
* A list of duplicates found. The __new only__ option enables you to show only the duplicates that appeared in the latest build.
* A list of files containing these duplicates. Use the left and right arrow buttons to show selected duplicate in the respective pane in the lower part of the tab.
* Two panes with the source code of the file fragments that contain duplicates.
* Scope filter in the upper\-left corner lists the specific directories that contain the duplicates. This filtering makes it easier for developers to manage the code of interest.

## Maven Build Info

For each Maven build the TeamCity agent gathers Maven specific build details, which are displayed on the __Maven Build Info__ tab of the __Build results__ after the build is finished.

[//]: # (Internal note. Do not delete. "Working with Build Resultsd371e671.txt")    

## Internal Build ID

In the URL of the build result page you can find the parameter `buildId` with a numeric value. This number is internal build id uniquely identifying the build in the TeamCity installation. You might need this ID when constructing URL manually. For example for [REST API](rest-api.md), [downloading artifacts](patterns-for-accessing-build-artifacts.md).

__  __

__See also:__


__Concepts__: [Build Log](build-log.md) | [Build Artifact](build-artifact.md) | [Change](change.md) | [Code Coverage](code-coverage.md)    
__User's Guide__: [Investigating and Muting Build Failures](investigating-and-muting-build-failures.md) | [Viewing Tests and Configuration Problems](viewing-tests-and-configuration-problems.md)   
__Administrator's Guide__: [Creating and Editing Build Configurations](creating-and-editing-build-configurations.md)

__ __