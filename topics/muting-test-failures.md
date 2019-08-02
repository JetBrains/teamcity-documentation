[//]: # (title: Muting Test Failures)
[//]: # (auxiliary-id: Muting Test Failures)

TeamCity provides a way to "mute" any of the currently failing tests so they will not affect build status for future builds.   
Muting will only affect builds which fail on "at least one test failed" [build failure condition](build-failure-conditions.md) but not others: if any of the other failure conditions apply to your build (non\-zero exit code, and so on), it will still fail even if failing tests are muted.

This feature is useful when some tests fail for some known reason, but it is currently not possible to fix them. For example, the responsible developer is on vacation, or you are waiting for the system administrators to fix the environment, or the test is failing intentionally, for example, if the required functionality is not yet written (TDD). In these cases you can mute such failures and avoid unnecessary disturbance of other developers.

When a test is muted, it is __still run__ in the future builds, but its failure does not affect the build status. The test can be unmuted manually on a specific date or after a successful run. Also, tests can be muted only in a single build configuration or in all the build configurations of a specific TeamCity project.

Your build script might need adjustment to make the build green when there are failing but muted tests. Make sure that the build does not fail because of other build failure conditions (for example, _"Fail if build process exit code is not zero"_) in case the only errors encountered were tests failures. See also the related issue [TW-16784](http://youtrack.jetbrains.net/issue/TW-16784).

### How to mute tests

The __Mute/unmute problems in project__ permission required to mute tests is granted to __Project administrator__ and __System administrator__ by default. You can mute a test failure from:
* The __Projects__ page
* The Project overview page
* The Build Configuration home page
* The Current Problems tab

On the build results page you can select several test failures (or all) to be muted or unmuted: select the tests, click the _Investigate..._ button and specify the mute options.

Note that you can start investigation of the problem simultaneously with muting the failure. When muting a test failure, you can specify conditions when it should be unmuted: on a specified date or when it is fixed. Alternatively, you can unmute it manually.

On the [build results](working-with-build-results.md) page you can view the list of muted test failures, their stacktraces and details about the mute status:

<img src="testMute.png" width="800" alt="Muting a test failure"/>

From the __Project Home__ page you can navigate to the __Muted Problems__ tab to view all problems muted in all build configurations within project:

<img src="mutedProblemsTab.PNG" width="800" alt="Mute Problems tab"/>


[//]: # (Internal note. Do not delete. "Muting Test Failuresd219e77.txt")    



