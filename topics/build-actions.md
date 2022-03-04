[//]: # (title: Build Actions)
[//]: # (auxiliary-id: Build Actions;Working with Build Results;Pinned Build;Build Tag;Changing Build Status Manually)

This article describes what actions can be applied to <emphasis tooltip="build">builds</emphasis> in TeamCity.

## Run Build

TeamCity allows running builds:
* Automatically, with various [build triggers](configuring-build-triggers.md).
* Manually, on demand.

To run a build manually, click __Run__ in the upper right corner of the screen. This action is available in both __Build Configuration Settings__ and __Build Configuration Home__ modes. If the __Run__ button does not appear for a certain build configuration, it means you do not have enough permissions to start builds in it.

The context menu next to the __Run__ button opens the build's settings menu, so you can initiate a <emphasis tooltip="custom-build-run">custom build run</emphasis>. A custom run allows using different settings or/and source code than if running a regular build in the current configuration. This is handy if you want to try different build parameters or pretest local code without affecting common build settings or committing the code to the common repository. Read more about this functionality [here](running-custom-build.md).

<img src="custom-run-menu.png" alt="Run Custom Build" width="460"/>

## Cancel Build

To cancel a running build, click the stop icon next to its progress bar. TeamCity will prompt to you to enter an optional reason for cancelling; you can also choose if this build should be automatically re-added to the queue.

Note that there is no way to pause and then continue a single build (though you can [pause a whole build configuration](managing-builds.md#Pausing+Build+Configuration)). If cancelled, a build should be restarted from the first step.

## Build Actions Menu

The following actions can be invoked from the single-build __Actions__ menu. To open this menu from __Build Configuration Home__, click ![VCS-browserIcon.png](VCS-browserIcon.png) opposite the build in the list. To open it from the build's __Overview__, click __Actions__ in the upper left corner.

<img src="build-actions-menu.png" alt="Build Actions Menu" width="296"/>

### Re-run Build

This action will restart the current build only, omitting other builds in its <emphasis tooltip="build-chain">chain</emphasis> if any. It might be helpful if the build failed due to certain infrastructure problems.

### Remove Build

This action will delete the build from the list.

### Pin Build

You can pin a build to prevent it from being removed during a scheduled [clean-up](teamcity-data-clean-up.md).

If the current build is a part of a <emphasis tooltip="build-chain">chain</emphasis> and has [snapshot dependencies](snapshot-dependencies.md) on other builds, you can choose to apply this action to all its preceding builds in the chain.

A pinned build can be unpinned manually anytime.

### Add Tags to Build

Build tags are labels that can help:
* Organize the history of your builds.
* Quickly navigate to the builds marked with a specific tag.
* [Search](search.md) for a build with a particular tag.
{product="tc"}
* Create an [artifact dependency](artifact-dependencies.md) on a build with a particular tag.

In the _Edit tags_ dialog, enter one or more tags separated by space, comma, or semicolon. For example, `v2022 release` will create two tags: `v2022` and `release`.

If you click a certain tag when viewing a build list, this will filter out other builds, so you can focus only on the builds with this tag.

Apart from the __Actions__ menu, you can tag a build in the _[Run Custom Build](running-custom-build.md)_ dialog and in the _Pin Build_ dialog.

You can also add and modify build tags using [TeamCity REST API](https://www.jetbrains.com/help/teamcity/rest/manage-finished-builds.html#Manage+Build+Tags).

[//]: # (Internal note. Do not delete. "Build Tagd46e113.txt")

### Add Build to Favorites

[//]: # (Internal note. Do not delete. "Favorite Buildd142e4.txt")

You can add builds to _favorites_ to have quick access to them. Favorite builds can also be set as a filter option for [notifications](set-up-notifications.md).

When added to favorites, a build will be marked with a star icon ![star_filled.png](star_filled.png). Clicking it will remove the "favorite" status.

TeamCity can automatically mark your manually triggered and [personal builds](personal-build.md) as favorites. To achieve this, enable the respective setting in your [user profile settings](configuring-your-user-profile.md).

To view your favorites, click your avatar in the upper right corner of the screen and choose __Favorite Builds__.

### Manually Change Build Status to Failed or Successful

Project administrators can manually change the successful build's status to failed, and vice versa. There are following common use cases for this:

* **From successful to failed**:
  * The build has some problem which did not affect the final [build status](build-state.md), but you want to reflect the problem's presence in the build history.
  * There is a known problem with the build, and you want this build to be ignored by a QA team.
  * You have previously mistakenly marked the build as successful.
* **From failed to successful**:
  * You need to change the _last successful build_ anchor when using [build failure conditions](build-failure-conditions.md). If your last build failed because of an incorrect value of a metric and this new value is valid, you can mark this build with a _successful_ anchor.
  * You want to allow using an incorrectly failed build with good artifacts in __[Artifact Dependencies](artifact-dependencies.md#Configuring+Artifact+Dependencies+Using+Web+UI)__.
  * For a running [personal build](personal-build.md), you can mark the current failures as non-relevant to allow the pretested commit to pass.

The "_Mark as successful_" action is not available for [builds that failed to start](build-state.md#Failed+to+Start+Builds).

### Label Build Sources

TeamCity can label (tag) sources of a particular build in your VCS repository. The list of applied labels and their application status is displayed on the __[Changes](working-with-build-results.md#Changes)__ tab of __Build Results__.

You can configure [automatic labeling](vcs-labeling.md) or label each build manually, from the __Actions__ menu. Manual labeling uses the VCS settings actual for the build.

### Merge Build Sources

TeamCity can merge the code of one source branch to another: for example, after an approved code review.

You can configure [automatic merge](automatic-merge.md) or do it manually for each build, from the __Actions__ menu. The pop-up dialog will prompt you to select the destination branch for a merge and enter a merge commit message.

### Actions to See Build Details

The __Actions__ menu has a few options for viewing build details:
* Comparing two builds with one another.
* Viewing the build's investigation history.

These actions are described in the [View Build Details](viewing-build-details.md) article.