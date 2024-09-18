[//]: # (title: Working with Build and Test Failures)
[//]: # (auxiliary-id: Investigating and Muting Build Failures)

During a build run, you can typically encounter one of the following failure types:

* **test failure** — a failure of a unit or functional test that was attempted during a build.
* **build problem** — any other issue that occurred during a build: inability to access a remote repository, failure to obtain or upload a build artifact, compilation error, and so on.


## View Build Problems

In TeamCity, there are multiple ways to detect and investigate build problems.

### Build Results Page

The [](build-results-page.md) displays the most detailed information about this individual build run. Its **Overview** tab displays expandable sections with a summary of both build and test problems occured during this run.

<img src="dk-buildproblems-overview.png" width="706" alt="Build Results Overview"/>

The first occurrence of a build problem adds "(new)" to the build's status message. This allows you to quickly distinguish unique and repeated problems when scrolling through a build history.

<img src="dk-build-failure-new.png" width="706" alt="New build problem"/>

Click any problem to view a portion of a [build log](build-log.md) that corresponds to this issue.

<img src="dk-buildfailure-expand.png" width="706" alt="Expand build problem"/>

Failed tests also show the **Current failure** / **First failure** toggle that allows you to quickly navigate to a build where the recurring issue first manifested.

<img src="dk-first-failed-test.png" width="706" alt="First failed test"/>

If you are viewing the results of an older build while a newer build has completed without the same test failing, the failure will be crossed out. In this case, the **Already Fixed** tab that points to this newer build is visible.

<img src="dk-already-fixed.png" width="706" alt="Already fixed"/>

> Successful [history builds](history-build.md) are not shown as builds in which failed test are already fixed.
> 
{style="tip"}

If a test frequently changes its status from failed to successful and back, TeamCity marks such test as **flaky**. See the dedicated [](#Flaky+Tests) section for more information.

### Problems Tab

The project's **Problems** tab provides an overview of all currently active build problems, including those from child subprojects.

<img src="dk-problems-tab.png" width="706" alt="Problems tab"/>

This page allows you to switch between build and test failures, as well as filter problems by their states.


## Manage Build Problems and Test Failures

TeamCity's problem management capabilities revolve around two base concepts: investigations and mutes.

### Investigations

**Investigated** problems are those that have TeamCity users assigned to them. These users are responsible for fixing corresponding build problems and/or test failures.

Users see a total number of investigations assigned to them in the TeamCity header. They can click this UI element to open the **My Investigations** page and view the detailed information about these investigations.

<img src="dk-investigation-counter.png" width="706" alt="Investigations counter"/>

> Users who have the **Highlight my changes and investigations** option enabled in their [profile settings](configuring-your-user-profile.md) can spot their assigned investigations easier in TeamCity UI.
> 
{style="tip"}

TeamCity draws a police officer icon to visualize investigated problems. Hovering overing this icon displays more information about the problem and allows users to reassign it or manually label it as fixed.

<img src="dk-investigated-tooltip.png" width="706" alt="Investigation tooltip"/>

If a user marks an investigation as fixed, the icon changes its color to green. This icon will remain visible until a new runs finishes without encountering the same problem.

<img src="dk-fixed-tooltip.png" width="706" alt="Fixed investigation"/>

Manually setting the problem's status to "Fixed" reduces the user's investigation counter in the TeamCity header, but does not prevent newer builds from failing if the problem keeps emerging. If you want TeamCity to ignore this problem, [mute it](#Mutes) instead of labeling as fixed.

TeamCity can automatically create problem investigations and assign them to users who are most likely responsible for these failures. To do so, configure the [](investigations-auto-assigner.md) build feature.

### Mutes

**Muted** problems are problems that do not prevent a build from reporting a successful finish. A build that failed due to a muted problem will appear as successfully finished, and display the number of muted problems.

<img src="dk-muted-overview.png" width="706" alt="Muted problems overview"/>

You can mute build problems or tests to temporarily ignore a known issue that is expected to be resolved, to ignore tests that are expected to fail, or to ensure subsequent steps or [](build-chain.md) configurations run even if the build has encountered an issue.



> Another way to ignore a failed step or chain configuration without muting their problems is to change the related run condition:
> * for build steps, change the [step execution condition](build-step-execution-conditions.md) to "Even if some of the previous steps failed";
> * for [dependent build chain configurations](build-chain.md), ensure the **On failed dependency** setting of a corresponding [snapshot dependency](snapshot-dependencies.md) is set to one of the "Run build..." options.
>
{style="tip"}


TeamCity draws a loudspeaker icon to visualize muted problems. Hovering overing this icon displays more information about this issue.

<img src="dk-mute-tooltip-1.png" width="706" alt="Muted tooltip 1"/>

<img src="dk-mute-tooltip-2.png" width="706" alt="Muted tooltip 2"/>

#### Muting Failing Tests

Muting tests only affects builds that fail on the "_At least one test failed_" [build failure condition](build-failure-conditions.md) but not on the other conditions. If any other failure condition applies to your build (for example, non-zero exit code), the build will still fail even if the failing tests are muted.

Your build script might need adjustment to make the build successful when there are failing but muted tests. Make sure the build does not fail because of other build failure conditions (for example, _"Fail if build process exit code is not zero"_) in case the only errors encountered were tests failures. See the related issue [TW-16784](https://youtrack.jetbrains.com/issue/TW-16784) for more details.

### Manage Active Problems

Both [](#Build+Results+Page) and [](#Problems+Tab) display UI elements that allow users to mute, unmute, investigate problems, mark them as fixed, or reassign them to different users. To apply the same action to multiple problems at once, go to the **Problems** tab and select required entries.

<img src="dk-bulk-mute-old.png" width="706" alt="Bulk mute problems"/>

When editing investigations and mutes, TeamCity allows you to specify the following settings:


* Investigation/mute scope. Choose "Project wide" to mute/investigate this problem in builds that belong to all configurations within the same project (including its child sub-projects). The "Selected build configuration" option allows you to mute/investigate a problem only for required configurations.
  > Project administrators can add the `teamcity.internal.preferredInvestigationProject` [configuration parameter](configuring-build-parameters.md) to a parent project to set this project as the default investigation/mute scope for all of its children. The parameter value must be set to the target project's [ID](identifier.md). This property changes only the option suggested by default, users will still be able to choose a different scope.
  > 
  {style="tip"}
* Investigations and assignees. Muted problems make failed builds appear successful, which can obscure underlying issues. For that reason, we recommend pairing mutes with investigations and avoid keeping muted problems that are not investigated by anyone.
* Automatic unmute/resolve policy. You can specify conditions under which TeamCity should unmute the problem or mark it as fixed.
  * `Automatically when fixed` — if a problem is resolved in a newer build, TeamCity unmutes it and/or closes the related investigation. This is the default scenario that allows your team to quickly respond to the issue, should it re-appear in further builds.

    A "fixed" problem is either a problem whose status was manually changed (see the [](#Investigations) section), or a problem that went away in newer builds. See also: [](#Relation+with+Branches).
  * `Manually` — only TeamCity users can close active investigations and undo mutes. This mode is recommended for muting/investigating [flaky tests](#Flaky+Tests), since these tests are unstable and a successful run cannot guarantee a problem is resolved.
  * `On a specific date` — TeamCity closes the investigation or unmutes a problem on a given date. This condition is useful for non-critical issues that your team cannot fix right away, but you need the CI routines to keep running.

* Copy settings to child subprojects. If you edit a investigation/mute that exists in this project and its child subprojects, TeamCity allows you to choose whether the updated settings should be propagated to these subprojects. Otherwise, investigations/mutes of child subprojects will differ (for example, assigned to a different user).
  <img src="dk-copy-investigation-settings.png" width="706" alt="Copy settings"/>

### Required Permissions

To manage and view investigations/mutes in a project, a user must have the following permissions granted in this project:

* "Mute/unmute problems in project" for managing mutes
* "Assign/unassign investigation" for managing investigations
* "View project and all parent projects" for viewing existing investigations/mutes

These permissions are granted to Project Administrators and System Administrators by default.

### Relation with Branches

Investigations and mutes are not branch-specific. If a problem is investigated or muted, it is investigated/muted for all branches in which it manifests. However, TeamCity checks branch-specific builds to determine whether a problem is fixed. This affects how problems with "Automatically when fixed" unmute/close investigation conditions are handled.

* If a failure occurs in multiple branches one of which is a default branch, the problem is considered fixed when it goes away in the default branch.
* If a failure occurs in non-default active branches only, the problem is considered fixed when it goes away in all these branches.

## Flaky Tests

Flaky tests are unstable tests that can finish both successful or failed with seemingly no differences between the build runs. TeamCity consideres a test flaky if one of the following is true:

* A test has a high flip rate. A flip is one occurrence of a test changing its status from success to failure or back. TeamCity calculates the ratio of such flips to the total invocation count of a given test. This ratio is measured per agent, per build configuration, or over a certain time period (7 days by default). A test that constantly fails or succeeds has its flip rate close to zero. A test that "flips" every time it is invoked has the flip rate close to 100&percnt;.
* A test flipped without underlying code changes. If a test finishes with an opposite status compared to the previous build run that processed the same revision (that is, no new changes were processed), TeamCity considers this test flaky.
* A test invoked multiple times throughout one build finished with different statuses. This heuristic is supported for [TestNG](https://testng.org) unit tests with `invocationCount` greater than 1, and tests carried out by the [](net.md) runner with the **Test retry count** greater than 0.

    <note>

  There is a known issue with parameterized test invocations with TestNG: TeamCity relies on the Surefire and Failsafe Maven plugins for the purposes of test result reporting, and these plugins ignore test parameters in their XML output. As a result, parameterized test invocations within the same build are erroneously treated as multiple identical test invocations, and, if some of the invocations fail, the test is marked as flaky with the _Flaky Failure_ reason. See the [related issue](https://youtrack.jetbrains.com/issue/TW-48097).

    </note>

A test is NOT considered flaky if:

* multiple builds of different configurations are run on the same VCS change and the test results differ between these builds. Such results are most likely to be caused by environmental issues.
* a VCS Root has its [branches](working-with-feature-branches.md#Configuring+Branches) configured and a test flips in a non-default branch.

TeamCity displays flaky tests in the **Tests** tab of the [](build-results-page.md)...

<img src="dk-flaky-in-build-results.png" width="706" alt="Flaky tests in build results"/>

...and on the **Flaky tests** page of a parent project.

<img src="dk-flaky-in-project.png" width="706" alt="Flaky tests in project"/>


For [JUnit](https://junit.org/) tests, a 3rd party [tempus-fugit](http://tempusfugitlibrary.org/) library can be used together with _JUnit_. It is sufficient to annotate a test with `@Intermittent` and use the `IntermittentTestRunner` test runner, as in the minimal example below:


```JavaScript
import org.junit.Test;
import org.junit.runner.RunWith;

import com.google.code.tempusfugit.concurrency.IntermittentTestRunner;
import com.google.code.tempusfugit.concurrency.annotations.Intermittent;

@RunWith(IntermittentTestRunner.class)
public class MultipleViaTempusFugit {
  @Test
  @Intermittent(repetition = 10)
  public void test() {
    // ...
  }
}

```

If such a test flakes at least once at multiple invocations within a single build, the _Flaky Failure_ heuristic will be triggered.


As with any failed test, you can assign investigations for a flaky test (or multiple tests). For flaky tests the resolution method is automatically set to "Manual"; otherwise the investigation will be automatically removed once the test is successful, which does not mean that the flaky test has been fixed.

> Investigation Auto Assigner can [delay automatic assignment of investigations](investigations-auto-assigner.md#delay-auto-assign), which prevents wrong auto assignments in projects with flaky tests.



<!--

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
1. Navigate to the __Overview__ tab of the __[Build Configuration Home](build-configuration-home-page.md)__ page (or the __Overview__ tab of the __[Build Results](working-with-build-results.md)__ page) and click __Assign investigation__. <img src="assign-investigation-button.png" alt="Assign an investigation of a failed build" width="676"/>
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
1. Navigate to the __Overview__ tab of the **[Build Results](working-with-build-results.md)** page (or the __[Current Problems](viewing-tests-and-configuration-problems.md#Using+Current+Problems+Tab)__ tab of the __Project Home__), click the arrow next to the build problem, and then click __Investigate/Mute__. 
<img src="investigating-muting-build-problem.png" width="1166" alt="Investigate/mute a build problem"/>   
   The _Investigate/Mute_ dialog opens.
2. Select a TeamCity user from the __Investigated by__ drop-down menu. <img src="assign-investigation-failed-build.png" width="575" alt="Assigned an investigation of a failed build"/>
3. Select the condition to resolve the investigation:
   * _Automatically when fixed_ ([read more](#Notes+on+Automatic+Fix+of+Investigations+and+Muted+Problems))
   * or, _Manually_   
   
   The user to whom the investigation is assigned can later [mark the problem as fixed](#Marking+Problems+as+Fixed) in this window.
4. Save the investigation.

You can similarly assign an investigation of a failed test on the __Overview__ or __[Tests](build-results-page.md#Tests+Tab)__ tab of the __Build Results__.

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

## Muting Tests

You can mute a failed test or several tests at once on the __Overview__ or __[Tests](build-results-page.md#Tests+Tab)__ tab of the __Build Results__, similarly to [muting build problems](#Muting+Build+Problems).

<img src="investigate-mute.png" width="583" alt="Investigate/mute a build problem"/>
   
Muting will only affect builds which fail on the "_At least one test failed_" [build failure condition](build-failure-conditions.md) but not on the other conditions: if any other failure condition applies to your build (for example, non-zero exit code), the build will still fail even if the failing tests are muted.

This feature is useful when some tests fail for a known reason, but it is not currently possible to fix them. For example, the responsible developer is absent, or you are waiting for the system administrator to fix the environment. The feature could also be used if the test is failing intentionally: for example, if the required functionality is not yet ready.   
 In all these cases you can mute such failures and avoid unnecessary disturbance of other developers.

When a test is muted, it is __still run__ in the future builds, but its failure does not affect the build status.

Your build script might need adjustment to make the build successful when there are failing but muted tests. Make sure the build does not fail because of other build failure conditions (for example, _"Fail if build process exit code is not zero"_) in case the only errors encountered were tests failures. See the related issue [TW-16784](https://youtrack.jetbrains.com/issue/TW-16784) for more details.

## Changing Project Scope of Investigation or Mute

When a user assigns an investigation or mutes a problem, TeamCity suggests the scope where it has to be investigated or muted. By default, it proposes to apply the action within the current project, and users usually confirm this default scope. However, the same problem might often occur not in a single project but in multiple adjacent projects which are children of the same parent. In this case, it makes sense to investigate or mute this problem in the scope of the whole parent project.  
Since version 2021.12, Project Administrators can change the value of the preferred investigation/mute scope per project. Setting the `teamcity.internal.preferredInvestigationProject` [configuration parameter](levels-and-priority-of-build-parameters.md) in the parent project's settings will make this project the default scope in the _Investigate/Mute_ dialog for all its children. The value should contain the parent project [ID](identifier.md). This property changes only the option proposed by default — users will still be able to adjust the scope in the dialog.

## Marking Problems as Fixed

A user, to whom the investigation is assigned, can mark the investigated problem as fixed in any _Investigate_ UI dialog corresponding to this problem.    
 The problem will be marked with ![checkmark.png](checkmark.png) on all TeamCity pages it appears. This allows identifying a potentially fixed problem in the UI, but leaves a possibility to continue the investigation if the problem persists in the following build run. If the author of the investigation has the [respective notifications](adding-notification-rules.md#investigation-is-updated) turned on, they will be notified about the fix.   
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

The investigations assigned to you are also highlighted in the UI if you enable the "_Highlight my changes and investigations_" option in your [profile settings](configuring-your-user-profile.md).

## Viewing Investigation History

Since TeamCity 2020.1, you can view the history of actions applied to each investigation. This is most helpful for big teams and projects when it is not as easy to determine who and when changed or resolved the investigation.

To see the history of an investigation, open any UI page where this [investigation is available](#Viewing+Investigations+and+Mutes) and click __Show Investigation History__ in its context menu. 

## Notes on Automatic Fix of Investigations and Muted Problems

If the "_Automatically when fixed_" option is select in [Investigation](#Assigning+Investigations+of+Build+Problems+and+Failed+Tests) or [Mute](#Muting+Build+Problems) options, TeamCity will analyze the state of each failure in different branches to resolve the investigation or mute smarter:

* If the failure occurs in the default branch and in another active branch, and an investigation (or mute) is assigned to this failure in any of these branches, it will be resolved as soon as the failure is fixed in the default branch.
* If the failure occurs in any active branch other than the default one, the assigned investigation (or mute) will be resolved only when it is fixed in all active branches in which it occurs.

-->

<seealso>
        <category ref="concepts">
            <a href="managing-builds.md">Build Configuration Status</a>
        </category>
        <category ref="external">
            <a href="https://youtu.be/icuhBgEFtVM">Video tutorial: TeamCity for developers</a>
        </category>
</seealso>