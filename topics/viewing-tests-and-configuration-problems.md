[//]: # (title: Viewing Tests and Configuration Problems)
[//]: # (auxiliary-id: Viewing Tests and Configuration Problems;Already Fixed In;First Failure)

## Viewing Problems on Project Overview Page

The __Project Overview__ page also displays the number of tests failed in build configurations as well as other problems. When the data in the problematic build configuration is not expanded, the link under the tests number takes you to the __Current Problems__ tab (see [below](#Using+Current+Problems+Tab)).

When you expand the build configuration data, the problem summary appears. The presentation of problems is shortened if your browser does not have enough horizontal space.

## Using Current Problems Tab

To view problematic build configurations and tests in your project, open the __Project Home__ page and go to the __Current Problems__ tab.

By default, the __Current Problems__ tab displays data for all build configurations within a project. To limit the data displayed, use the _Filter by build configuration_ drop-down menu.

From this page you can view problems in your project divided into the following groups:
* build configuration problems
* failed tests
* [muted test failures](investigating-and-muting-build-failures.md#Muting+Tests)
* build problems

Each of the sections is expandable and you can further drill down to the smallest relevant item using the up and down arrows ![UpDownArrows.PNG](UpDownArrows.PNG).

The links appearing in build configuration problems and build problems sections enable you to monitor a great deal of useful data, for example, you can navigate to build results, view build log and changes. More options are available when you click the arrow next to a link.

The failed test and muted failures sections have grouping options and allow viewing the test stack trace available when clicking the test name link. You can also view the test history, open the test in the active IDE, start investigating a particular test failure, fix and unmute a test or start investigating a particular test failure.

<anchor name="ViewingTestsandConfigurationProblems-FlakyTests"/>

## Flaky Tests

TeamCity detects flaky tests and displays them on the dedicated tab for a given project.

A flaky test is a test that is unstable (can exhibit both a passing and a failing result) with the same code.

Flaky test detection is based on the following heuristics:
1. High flip rate (Frequent test status changes). A flip in TeamCity is a test status change — either from "OK" to "Failure", or vice versa. The Flip Rate is the ratio of such "flips" to the invocation count of a given test, measured per agent, per build configuration, or over a certain time period (7 days by default). A test which constantly fails, while having a 100% failure rate, will have its flip rate close to zero; a test which "flips" each time it is invoked will have the flip rate close to 100%.If the flip rate is too high, TeamCity will consider the test flaky.
2. If the status of a test "flipped" in the new build with no changes, i.e. a previously successful test failed in a build without changes or a previously failing test passed in a build without changes, TeamCity will consider the test flaky.
3. Different test status for multiple invocations in the same build (flaky failure): if the same test is invoked multiple times and the test status flips, TeamCity will consider the test flaky.    
This heuristic is supported for [TestNG](https://testng.org/) unit tests with `invocationCount` \[[1](https://testng.org/doc/documentation-main.html)\]\[[2](https://testng.org/javadocs/org/testng/annotations/Test.html#invocationCount--)\] greater than `1`.

A test is not considered flaky if multiple builds of different configurations are run on the same VCS change and the test results differ between these builds. Such results are most likely to be caused by environmental issues.

<note>

There is a known issue with parameterized test invocations with TestNG: TeamCity relies on the Surefire and Failsafe Maven plugins for the purposes of test result reporting, and these plugins ignore test parameters in their XML output. As a result, parameterized test invocations within the same build are erroneously treated as multiple identical test invocations, and, if some of the invocations fail, the test is marked as flaky with the _Flaky Failure_ reason. See the [related issue](https://youtrack.jetbrains.com/issue/TW-48097).
</note>

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

Such tests are displayed on the dedicated project tab, __Flaky Tests__,  along with the total number of test failures, the flip rate for the given test and reasons for qualifying the test as a flaky one.

You can also see if the test is flaky when viewing the expanded stack trace for a failed test on the build results page.

<img src="flaky-test.png" width="750" alt="Flaky test in Build Overview"/>

As with any failed test, you can assign investigations for a flaky test (or multiple tests). For flaky tests the resolution method is automatically set to "Manual"; otherwise the investigation will be automatically removed once the test is successful, which does not mean that the flaky test has been fixed.

>Investigation Auto Assigner can [delay automatic assignment of investigations](investigations-auto-assigner.md#delay-auto-assign), which prevents wrong auto assignments in projects with flaky tests.

Note that if [branches](working-with-feature-branches.md#Configuring+Branches) are configured for a VCS Root, flaky tests are detected for the default branch only.

## See Where Test is Already Fixed In

For some [test failures](viewing-tests-and-configuration-problems.md), TeamCity can show the "_Already Fixed In_" build. This is the build where this initially failed test was successful and which was run _after_ the build with initial test failure (for the same <tooltip term="build-configuration">_build configuration_</tooltip>).

Here, _after_ means that:
* The build with the successful test has newer changes than the build with initial failure.
* If the changes were the same, the newer build was run after the failed one.

If you run a [history build](history-build.md), TeamCity will not consider it as a candidate for "_Already Fixed In_" for test failures in later builds (in terms of changes).

Tests are considered the same when they have the same name. If there are several tests with the same name within the build, TeamCity counts order number of the test with the same name and considers it as well.

Note that all invocations of the same test within a single build are counted as 1.

## See Where Test First Failed

TeamCity displays the "_First Failure_" build for some tests. This is the build where TeamCity detected the first failure of this test for the same [build configuration](managing-builds.md), which means that starting from the current build, TeamCity goes back throughout the build history to find out when this test failed for the first time. Builds without this test are skipped, and if a successful test run was found, the search stops.  
Here, _back throughout the history_ means that builds are analyzed with regard to changes as detected by TeamCity; as a result, [history builds](history-build.md) will be processed correctly.

A test which runs several times within a single build is counted as one test (that is all invocations of the same test are counted as one).

<seealso>
        <category ref="concepts">
            <a href="testing-frameworks.md">Testing Frameworks</a>
        </category>
        <category ref="user-guide">
            <a href="working-with-build-results.md">Working with Build Results</a>
        </category>
        <category ref="external">
            <a href="https://youtu.be/icuhBgEFtVM">Video tutorial: TeamCity for developers</a>
        </category>
</seealso>