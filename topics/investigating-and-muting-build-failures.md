[//]: # (title: Investigating and Muting Build Failures)
[//]: # (auxiliary-id: Investigating and Muting Build Failures)

When a build fails due to some problem or a failed test, TeamCity allows you to _assign an investigation_ of the build failure to some user or to _mute the failure_ so it does not affect the build status.

You can assign an investigation of the whole failed build configuration, or investigate/mute single [build problems and failed tests](viewing-tests-and-configuration-problems.md). The __Investigate/Mute__ functionality works identically for build problems and failed tests.

## Required Permissions

To be able to manage and view investigations/mutes in a certain project, the user must have the following permissions granted in this project:
* "_Mute/unmute problems in project_" for managing mutes
* "_Assign/unassign investigation_" for managing investigations
* "_View project and all parent projects_" for viewing investigations/mutes

These permissions are granted to Project Administrators and System Administrators by default.

## Assigning Investigations of Failed Build Configurations

To assign an investigation of a failed build configuration:
1. Navigate to the __Overview__ tab of the __[Build Configuration Home](viewing-build-configuration-details.md)__ page (or the __Overview__ tab of the __[Build Results](working-with-build-results.md)__ page) and click __Assign investigation__. <img src="assign-investigation-button.png" alt="Assign an investigation of a failed build" width="676"/>
2. Select a TeamCity user from the __Investigated by__ drop-down menu. <img src="assign-investigation-failed-build.png" width="575" alt="Assigned an investigation of a failed build"/>
3. Select the condition to resolve the investigation:
   * _When build configuration is successful_
   * or, _Manually_; this mode is recommended for build configurations with [flaky tests](viewing-tests-and-configuration-problems.md#Flaky+Tests), when the "Success" build status does not directly indicate that the problem has been resolved.   
   The user to whom the investigation is assigned can later [mark the problem as fixed](#Marking+Problems+as+Fixed) in this window.
4. Save the investigation.

To assign similar investigations for other failed build configurations in the same project, click __more__ and select the configurations in the list.

<img src="assign-more-investigations.gif" width="578" alt="Assign more investigations in the project"/>

## Assigning Investigations of Build Problems and Failed Tests

To assign an investigation of a particular build problem:
1. Navigate to the __Overview__ tab of the [Build Results](working-with-build-results.md) page (or the __[Current Problems](viewing-tests-and-configuration-problems.md#Using+Current+Problems+Tab)__ tab of the __Project Home__), click the arrow next to the build problem, and then click __Investigate/Mute__. 
<img src="investigating-muting-build-problem.png" width="1166" alt="Investigate/mute a build problem"/>   
   The _Investigate/Mute_ dialog opens.
2. Select a TeamCity user from the __Investigated by__ drop-down menu. <img src="assign-investigation-failed-build.png" width="575" alt="Assigned an investigation of a failed build"/>
3. Select the condition to resolve the investigation:
   * _Automatically when fixed_ ([read more](#Notes+on+Automatic+Fix+of+Investigations+and+Muted+Problems))
   * or, _Manually_   
   
   The user to whom the investigation is assigned can later [mark the problem as fixed](#Marking+Problems+as+Fixed) in this window.
4. Save the investigation.

You can similarly assign an investigation of a failed test on the __Overview__ or __[Tests](working-with-build-results.md#Tests)__ tab of the __Build Results__.

## Muting Build Problems

To mute a particular build problem:
1. Navigate to the __Overview__ tab of the [Build Results](working-with-build-results.md) page (or the __[Current Problems](viewing-tests-and-configuration-problems.md#Using+Current+Problems+Tab)__ tab of the __Project Home__), click the arrow next to the build problem, and then click __Investigate/Mute__.   
   The _Investigate/Mute_ dialog opens.
2. Choose where to mute the problem:
   * _Project-wide_, that is muting the problem in all configurations in the current project and its subprojects.
   * _Selected build configurations_, which allows you to select particular build configurations from the same project.
3. Select the condition to unmute the problem:
   * _Automatically when fixed_ ([read more](#Notes+on+Automatic+Fix+of+Investigations+and+Muted+Problems))
   * _On a specific date_
   * or, _Manually_
4. Click __Save__.

## Muting Failed Tests

You can mute a failed test or several tests at once on the __Overview__ or __[Tests](working-with-build-results.md#Tests)__ tab of the __Build Results__, similarly to [muting build problems](#Muting+Build+Problems).

<img src="investigate-mute.png" width="583" alt="Investigate/mute a build problem"/>
   
Muting will only affect builds which fail on the "_At least one test failed_" [build failure condition](build-failure-conditions.md) but not on the other conditions: if any other failure condition applies to your build (for example, non-zero exit code), the build will still fail even if the failing tests are muted.

This feature is useful when some tests fail for a known reason, but it is not currently possible to fix them. For example, the responsible developer is absent, or you are waiting for the system administrator to fix the environment. The feature could also be used if the test is failing intentionally: for example, if the required functionality is not yet ready.   
 In all these cases you can mute such failures and avoid unnecessary disturbance of other developers.

When a test is muted, it is __still run__ in the future builds, but its failure does not affect the build status.

Your build script might need adjustment to make the build successful when there are failing but muted tests. Make sure the build does not fail because of other build failure conditions (for example, _"Fail if build process exit code is not zero"_) in case the only errors encountered were tests failures. See the related issue [TW-16784](http://youtrack.jetbrains.net/issue/TW-16784) for more details.

## Marking Problems as Fixed

A user, to whom the investigation is assigned, can mark the investigated problem as fixed in any _Investigate_ UI dialog corresponding to this problem.    
 The problem will be marked with ![checkmark.png](checkmark.png) on all TeamCity pages it appears. This allows identifying a potentially fixed problem in the UI, but leaves a possibility to continue the investigation if the problem persists in the following build run. If the author of the investigation has the [respective notifications](subscribing-to-notifications.md#investigation-is-updated) turned on, they will be notified about the fix.   
TeamCity will remove the investigation in the following build run after the investigation is marked as fixed, if the problem is not reproduced and if this investigation has the "_Automatically when fixed_" resolution condition.

## Viewing Investigations and Mutes

You can find all the investigations and mutes per project on the __Project Home__ page:
* __Investigations__ tab: see all assigned investigations in the project
* __Muted Problems__ tab: see all muted problems and tests in the project

<img src="investigations-muted-tabs.png" width="1304" alt="Investigations and Muted Problems tabs"/>

Here you can unmute problems and tests, and reassign the investigations to another user or [mark them as fixed](#Marking+Problems+as+Fixed).
 
## Viewing My Investigations

If one or more investigations are assigned to you, the investigation counter will appear next to your name on the TeamCity top panel:

<img src="new_investigations.png" width="200" alt="New investigations"/>

Click it to open the __My Investigations__ page where you can see all the investigations assigned to you in different projects. You can manage the problems directly on this page: mark them as fixed, reassign, mute, or give up the investigations.

For each failed test, you can instantly see:
* all build configurations where the test is currently failing
* current stack trace and the information about the build where the test is currently failing
* information about the first failure of the test, with the stack trace and the build

The investigations assigned to you are also highlighted in the web UI if you enable the "_Highlight my changes and investigations_" option in your [profile settings](managing-your-user-account.md).

## Notes on Automatic Fix of Investigations and Muted Problems

If the "_Automatically when fixed_" option is select in [Investigation](#Assigning+Investigations+of+Build+Problems+and+Failed+Tests) or [Mute](#Muting+Build+Problems) options, TeamCity will analyze the state of each failure in different branches to resolve the investigation or mute smarter:

* If the failure occurs in the default branch and in another active branch, and an investigation (or mute) is assigned to this failure in any of these branches, it will be resolved as soon as the failure is fixed in the default branch.
* If the failure occurs in any active branch other than the default one, the assigned investigation (or mute) will be resolved only when it is fixed in all active branches in which it occurs.

__  __

__See also:__


__Concepts__: [Build Configuration Status](build-configuration.md)   
__User's Guide__: [Viewing Tests and Configuration Problems](viewing-tests-and-configuration-problems.md)

__ __

[//]: # (Internal note. Do not delete. "Muting Test Failuresd219e77.txt")