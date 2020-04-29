[//]: # (title: Already Fixed In)
[//]: # (auxiliary-id: Already Fixed In)
For some test failures TeamCity can show "Already Fixed In" build.

This is the build where this initially failed test was successful and which was run _after_ the build with initial test failure (for the same [Build Configuration](build-configuration.md)). 


"After" means here that 


	
* build with successful test has newer changes than the build with initial failure
	
* if the changes were the same, the newer build was run after the failed one


So, if you run [History Build](history-build.md), TeamCity won't consider it as a candidate for "Already Fixed In" for test failures in later builds (in term of changes).

Tests are considered the same when they have the same name. If there are several tests with the same name within the build, TeamCity counts order number of the test with the same name and considers it as well. 

Note that __since TeamCity 9.0__, the way TeamCity counts tests [Hajipur 9.0 EAP1 (build 31423) Release Notes](http://confluence.jetbrains.com/TW/Hajipur+9.0+EAP1+(build+31423)+Release+Notes).

<seealso>
        <category ref="concepts">
            <a href="first-failure.md">First Failure</a>
        </category>
</seealso>
