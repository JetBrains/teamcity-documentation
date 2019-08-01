[//]: # (title: Investigating and Muting Build Problems)
[//]: # (auxiliary-id: Investigating and Muting Build Problems)
When for some reason a build fails, TeamCity provides handy means of figuring out which changes might have caused the build failure.

You can __assign__ some of your team members __to investigate__ what caused the failure of this build or to start the investigation yourself. When an investigator is set, he/she receives the corresponding notification.

TeamCity also provides a way to mute a build problem (similarly to [muting failed tests](muting-test-failures.md)) so that it not affect build status for future builds.

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## Investigating or Muting Build Problems

__To assign an investigation of a failed build__:
1. Navigate to the __Overview__ tab of the __Build Configuration Home__ page, or the __Overview__ tab of the [Build Results page](working-with-build-results.md), click the _Start investigation..._ link.
2. Select a team member's name from the __User__ drop-down menu.
3. Select the condition to remove the investigation: when the build configuration becomes green or the test passes (default) or specify that an investigation should be resolved manually.   
Manual resolution is useful with so called "flickering tests", i.e. when a test fails from time to time and the "green" status of a build is not an indicator of the problem resolution.
4. Save your investigation.

To investigate a problem in more than one build configuration, click __more__ and select the desired build configurations.


__To assign an investigation / mute a particular build problem__:
1. Navigate to the __Overview__ tab of the [Build Results page](working-with-build-results.md), click the arrow next to the build problem, click the _Investigate_ link.
2. The _Investigate / Mute Build problem_ dialog opens. Select the investigation or muting options and save them:   

   <img src="investigating-muting-build-problem.png" width="1166" alt="Investigate/mute a build problem"/>

__To assign an investigation / mute a particular test__, navigate to the __[Tests](working-with-build-results.md#Tests)__ tab, __[Current Problems](viewing-tests-and-configuration-problems.md)__ tab or __Problematic Tests__ of the __Build Results__ page, click the arrow next to the test name and select __Investigate__.

## Viewing Investigations and Mutes

A subject for investigating/ muting is not necessarily the whole build, it can also be a build problem, a particular test. For each project you can find out what problems are currently being investigated and by whom using the dedicated __Investigations__ page. All mutes are listed on the __Muted Problems__ tab:

<img src="investigations-muted-tabs.png" width="1304" alt="Investigations and Muted Problems tabs"/>

### My Investigations

To view all problems assigned to you to investigate, click the box with a number next to your name on the TeamCity top panel:

<img src="new_investigations.png" width="200" alt="New investigations"/>

On the __My Investigations__ page you can see the investigations assigned to you in different projects and monitor the problems in the build log. You can manage the problems directly on this page: mark them as fixed, reassign, mute, or give up the investigations.

For each failed test, you can monitor a lot of information instantly, without leaving this page. For example:
* all build configurations where the test is currently failing
* current stack trace and the information about the build where the test is currently failing
* information about the first failure of the test, with the stack trace and the build

The investigations assigned to you are also highlighted in the Web UI if you enable the "_Highlight my changes and investigations_" option in your [profile settings](managing-your-user-account.md).




__  __

__See also:__



__Concepts__: [Build Configuration Status](build-configuration.md) | [Muting Test Failures](muting-test-failures.md)

__User's Guide__: [Viewing Tests and Configuration Problems](viewing-tests-and-configuration-problems.md)
