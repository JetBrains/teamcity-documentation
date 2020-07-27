[//]: # (title: Already Fixed In)
[//]: # (auxiliary-id: Already Fixed In)

For some test failures TeamCity can show the "_Already Fixed In_" build.   
This is the build where this initially failed test was successful and which was run _after_ the build with initial test failure (for the same [Build Configuration](build-configuration.md)). 

Here, _after_ means that
* the build with the successful test has newer changes than the build with initial failure;
* if the changes were the same, the newer build was run after the failed one.

So, if you run [History Build](history-build.md), TeamCity won't consider it as a candidate for "_Already Fixed In_" for test failures in later builds (in terms of changes).

Tests are considered the same when they have the same name. If there are several tests with the same name within the build, TeamCity counts order number of the test with the same name and considers it as well.

Note that, since TeamCity 9.0, we changed the way TeamCity works with a test which is run several times within a single build (for instance, this change affects TestNG tests with the invocation count \> 1). Previously, each test invocation was counted and shown as a separate test. For example, when you saw: Tests passed 1100, these counters included all invocations. Now, instead of 1100 tests you will see 1001 if one of the tests has been invoked 100 times, that is all invocations of the same test are counted as 1.

<seealso>
        <category ref="concepts">
            <a href="first-failure.md">First Failure</a>
        </category>
</seealso>
