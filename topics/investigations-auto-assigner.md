[//]: # (title: Investigations Auto Assigner)
[//]: # (auxiliary-id: Investigations Auto Assigner)

TeamCity can analyse build problems (for example, compilation errors) and test failures, and try to find a committer to blame for the problem using a number of heuristics.

For test failures, there is a suggestion to assign an investigation to this user: you can review the suggestion and assign the user to investigate the failure.

In addition to suggestions, you can now configure the _Investigations Auto Assigner_ [build feature](adding-build-features.md). It will use the same heuristics to enable automatic assignment of investigations, and not only for a test failure but also for build problems.

When configuring the build feature, you can specify:
* the TeamCity username of the default user whom investigations will be assigned to in case it is not clear whose changes actually broke the build
* the list of usernames to exclude from assigning investigations (for example, system users or the users no longer working on the project)

After the build feature is added to a build configuration, the user is assigned to investigate a failure on the basis of the following heuristics:
* If a user is the only committer to the build.
* If a user is the only one who changed the suspicious file. The suspicious file is the one which probably caused this failure, i.e. its name appears in the test or build problem error text.
* If a user was responsible for this problem the previous time.
* If a user is set as the default responsible user.

If you have not configured the build feature, TeamCity will show a suggestion for investigation assignment with the reason for the possible assignment.

<anchor name="delay-auto-assign"/>

You can delay auto-assignment of investigations.   
TeamCity needs time after the build finish to detect [flaky tests](viewing-tests-and-configuration-problems.md#Flaky+Tests), and in some cases Investigations Auto Assigner may assign an investigation to a user even if the build fails without any user involvement. When there are many flaky tests in a project, this may be distracting.   
To prevent this scenario, configure the _Investigations Auto Assigner_ build feature to delay the investigation assignment until the failure repeats itself in the build configuration (in the default branch): select __Assign__: "_On second failure_" when configuring this build feature.

__ __