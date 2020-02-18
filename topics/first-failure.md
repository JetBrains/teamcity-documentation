[//]: # (title: First Failure)
[//]: # (auxiliary-id: First Failure)

TeamCity displays the "_First Failure_" build for some tests.

This is the build where TeamCity detected the first failure of this test for the same [build configuration](build-configuration.md), which means that starting from the current build, TeamCity goes back throughout the build history to find out when this test failed for the first time. Builds without this test are skipped, and if a successful test run was found, the search stops.   
Here, _back throughout the history_ means that builds are analyzed with regard to changes as detected by TeamCity; as a result, [history builds](history-build.md) will be processed correctly.

A test which runs several times within a single build is counted as one test (that is all invocations of the same test are counted as one).


__  __

__See also:__

__User Guide__: [Test History](working-with-build-results.md#Test+History)
__Concepts__: [Already Fixed In](already-fixed-in.md)

__ __