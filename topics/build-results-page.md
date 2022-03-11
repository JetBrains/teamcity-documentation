[//]: # (title: Build Results Page)
[//]: # (auxiliary-id: Build Results Page;Build History)

This article gives an overview of the __Build Results__ page of the new TeamCity UI. Most of its features are also available in the classic UI mode.

To display the build's results, click its name in any list when browsing TeamCity in the __[Home](working-with-build-results.md)__ mode.

## Build Results Title Panel

The __Build Results__ page contains several tabs, whose set depends on the current build's specifics, and a few common elements:

* Branch selector which displays the branch of the current build. When in __Build Configuration Home__, this selector allows actually filtering the list of displayed builds by a specfic branch.
* The __Actions__ menu (described [here](build-actions.md)).
* The __Details__ block: when expanded, shows the main info about the current block.
* The __Investigation__ widget which lets you quickly assign an [investigation](investigating-and-muting-build-failures.md) of this build or see the details of the open investigation.
* The __Trends__ widget which shows all the previous builds and their details. Hover a build to see its details.

## Overview Tab

The __Overview__ tab of __Build Results__ shows the summary of other tabs and an interactive build log timeline.

<img src="exp-build-details.png" alt="Experimental Build Results page"/>

## Changes Tab

The __Changes__ tab displays more information about changes in the build, separately for user commits and artifact changes. You can filter changes by their author and display changes made in the build configuration settings.

## Tests Tab

The __Tests__ tab allows switching between failed, ignored, and succeeded tests, and use various filters. Click a test to quickly view its details or, for example, to assign an [investigation](investigating-and-muting-build-failures.md). To see a detailed test or investigation history, click __Show test history__ or __Show investigation history__ in its context menu.

## Build Log Tab

The graphic build log timeline reflects the duration of each build stage and indicates build problems:

<img src="build-timeline.png" width="600" alt="Build timeline"/>

Click any stage to open the corresponding line of the build log.

## Artifacts Tab

## Parameters Tab

## Dependencies Tab

The __Dependencies__ tab provides three modes of displaying the build dependencies: a visual timeline, structured list, and build chain. Choose the mode that is the most helpful for your current task.

## Classic UI Tabs

Other classic UI tabs, not yet reproduced in the new UI, are also available: click __More__ and select the required tab in the list.
