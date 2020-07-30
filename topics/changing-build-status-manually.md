[//]: # (title: Changing Build Status Manually)
[//]: # (auxiliary-id: Changing Build Status Manually)

A user with appropriate permissions can change the status of a build manually, i.e. make it either failed or successful (issue [TW-2529](http://youtrack.jetbrains.com/issue/TW-2529)).

The corresponding action is available in the __Actions__ menu on the [__Build Results__](working-with-build-results.md) page.

## Marking build as successful

You may want to make build successful to:
	
* Change the _last successful build_ anchor when using [build failure conditions](build-failure-conditions.md), i.e. if your last build failed because of an incorrect value of a metric, and this new value is valid, you may mark this build with a successful anchor.
* Allow using an incorrectly failed build with good artifacts in the [Artifact Dependencies](artifact-dependencies.md#Configuring+Artifact+Dependencies+Using+Web+UI).
* For a running [Personal Build](personal-build.md), you can mark the current failures as non-relevant to allow the pretested commit to pass (if the user has permission to do this).

The "_Mark as successful_" action is not available for [builds that failed to start](build-state.md#Failed+to+Start+Builds).

## Marking build as failed

You may want to mark a build as failed when:

* The build has some problem which didn't affect the final [build status](build-state.md).
* There is a known problem with the build, and it should be ignored by your QA team.
* You've mistakenly marked the build as successful manually.

## Permissions

By default, the permission to change the build status is granted to a Project Administrator. 
