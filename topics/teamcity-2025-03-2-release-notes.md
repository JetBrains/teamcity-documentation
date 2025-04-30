[//]: # (title: TeamCity 2025.03.2 Release Notes)
[//]: # (auxiliary-id: TeamCity 2025.03.2 Release Notes)

## Build , 30 April 2025

### Bug

* [TW-89671](https://youtrack.jetbrains.com/issue/TW-89671) — No coverage results, "Failed to find dotCover"
* [TW-92997](https://youtrack.jetbrains.com/issue/TW-92997) — Regression: cannot upgrade to 2025.03.1, enqueues thousands of builds
* [TW-92586](https://youtrack.jetbrains.com/issue/TW-92586) — Unaligned buttons in Project Settings -> Untrusted builds
* [TW-92994](https://youtrack.jetbrains.com/issue/TW-92994) — project-config.xml file not created for large project copy
* [TW-91646](https://youtrack.jetbrains.com/issue/TW-91646) — Region from the last created AWS EC2 profile is used instead of the opened one
* [TW-92404](https://youtrack.jetbrains.com/issue/TW-92404) — Add Image dialog shows old list of AMIs after changing Region in AWS Cloud Profile
* [TW-92498](https://youtrack.jetbrains.com/issue/TW-92498) — BuildAgent may hang on a faulty artifact publishing command
* [TW-93055](https://youtrack.jetbrains.com/issue/TW-93055) — Hard to follow documentation on nested test reporting with service messages
* [TW-93101](https://youtrack.jetbrains.com/issue/TW-93101) — Repeating attempts to stop the build are too fast (20 secs)
* [TW-91323](https://youtrack.jetbrains.com/issue/TW-91323) — Update the description of the "Project Administrator" role by adding information about the visibility of the Root project
* [TW-92895](https://youtrack.jetbrains.com/issue/TW-92895) — Error "Container wrapper is not supported" during usage of "Run in Docker" build feature if "Image platform" was chosen in the build steps
* [TW-92939](https://youtrack.jetbrains.com/issue/TW-92939) — RequiredToolInstalledPrecondition pollutes logs when build type is removed
* [TW-92653](https://youtrack.jetbrains.com/issue/TW-92653) — Cloud agents don't start if plugin with agent part is enabled or upgrade of TeamCity took place
* [TW-92917](https://youtrack.jetbrains.com/issue/TW-92917) — Agent git mirror is cleaned if remote server hangs up
* [TW-92878](https://youtrack.jetbrains.com/issue/TW-92878) — Artifacts Migration tool: a lot of messages like "failed to copy '34' artifacts" in migration tool output can be shown even if artifacts were copied
* [TW-88707](https://youtrack.jetbrains.com/issue/TW-88707) — EC2 Cloud Profile: When there aren't enough addresses in one subnet, another should be tried
* [TW-90677](https://youtrack.jetbrains.com/issue/TW-90677) — Kubernetes Executor: cross-platform msbuild is stuck 
* [TW-91440](https://youtrack.jetbrains.com/issue/TW-91440) — Hide stacktraces from the default teamcity-cleanup.log if TeamCity can't delete the artifacts from Azure

### Task

* [TW-79379](https://youtrack.jetbrains.com/issue/TW-79379) — Update bundled JaCoCo to latest version (0.8.8)


### Security

One security problem has been fixed.
To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2025.03.2).

Security bulletins are typically published few days after the release date.


