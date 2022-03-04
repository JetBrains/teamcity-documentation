[//]: # (title: See Build Where Test is Already Fixed)
[//]: # (auxiliary-id: See Build Where Test is Already Fixed;Already Fixed In)

For some [test failures](viewing-tests-and-configuration-problems.md), TeamCity can show the "_Already Fixed In_" build. This is the build where this initially failed test was successful and which was run _after_ the build with initial test failure (for the same <emphasis tooltip="build-configuration">build configuration</emphasis>).

Here, _after_ means that:
* The build with the successful test has newer changes than the build with initial failure.
* If the changes were the same, the newer build was run after the failed one.

If you run a [history build](history-build.md), TeamCity will not consider it as a candidate for "_Already Fixed In_" for test failures in later builds (in terms of changes).

Tests are considered the same when they have the same name. If there are several tests with the same name within the build, TeamCity counts order number of the test with the same name and considers it as well.

Note that all invocations of the same test within a single build are counted as 1.

<seealso>
        <category ref="admin-guide">
            <a href="first-failure.md">Show First Failure for Test</a>
            <a href="configuring-test-reports-and-code-coverage.md">Configuring Test Reports and Code Coverage</a>
        </category>
</seealso>
