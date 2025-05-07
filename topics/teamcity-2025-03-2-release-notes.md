[//]: # (title: TeamCity 2025.03.2 Release Notes)
[//]: # (help-id: TeamCity 2025.03.2 Release Notes)


**Build 0, 8 May 2025**

### Bug

* [**project-config.xml file not created for large project copy**](https://youtrack.jetbrains.com/issue/project-config.xml file not created for large project copy) — TW-92994
* [**Builds get stuck in the queue with \"no idle compatible agents\" due to an undiscoverable cloud profile error**](https://youtrack.jetbrains.com/issue/Builds get stuck in the queue with \"no idle compatible agents\" due to an undiscoverable cloud profile error) — TW-93187
* [**Repeating attempts to stop the build are too fast (20 secs)**](https://youtrack.jetbrains.com/issue/Repeating attempts to stop the build are too fast (20 secs)) — TW-93101
* [**Regression: cannot upgrade to 2025.03.1, enqueues thousands of builds**](https://youtrack.jetbrains.com/issue/Regression: cannot upgrade to 2025.03.1, enqueues thousands of builds) — TW-92997
* [**Artifacts Migration tool: a lot of messages like \"failed to copy '34' artifacts\" in migration tool output can be shown even if artifacts were copied**](https://youtrack.jetbrains.com/issue/Artifacts Migration tool: a lot of messages like \"failed to copy '34' artifacts\" in migration tool output can be shown even if artifacts were copied) — TW-92878
* [**Cloud agents don't start if plugin with agent part is enabled or upgrade of TeamCity took place**](https://youtrack.jetbrains.com/issue/Cloud agents don't start if plugin with agent part is enabled or upgrade of TeamCity took place) — TW-92653
* [**Agent git mirror is cleaned if remote server hangs up**](https://youtrack.jetbrains.com/issue/Agent git mirror is cleaned if remote server hangs up) — TW-92917
* [**Kubernetes Executor: cross-platform msbuild is stuck **](https://youtrack.jetbrains.com/issue/Kubernetes Executor: cross-platform msbuild is stuck ) — TW-90677
* [**No coverage results, \"Failed to find dotCover\"**](https://youtrack.jetbrains.com/issue/No coverage results, \"Failed to find dotCover\") — TW-89671
* [**Unaligned buttons in Project Settings -> Untrusted builds**](https://youtrack.jetbrains.com/issue/Unaligned buttons in Project Settings -> Untrusted builds) — TW-92586
* [**Region from the last created AWS EC2 profile is used instead of the opened one**](https://youtrack.jetbrains.com/issue/Region from the last created AWS EC2 profile is used instead of the opened one) — TW-91646
* [**Add Image dialog shows old list of AMIs after changing Region in AWS Cloud Profile**](https://youtrack.jetbrains.com/issue/Add Image dialog shows old list of AMIs after changing Region in AWS Cloud Profile) — TW-92404
* [**BuildAgent may hang on a faulty artifact publishing command**](https://youtrack.jetbrains.com/issue/BuildAgent may hang on a faulty artifact publishing command) — TW-92498
* [**Hard to follow documentation on nested test reporting with service messages**](https://youtrack.jetbrains.com/issue/Hard to follow documentation on nested test reporting with service messages) — TW-93055
* [**Update the description of the \"Project Administrator\" role by adding information about the visibility of the Root project**](https://youtrack.jetbrains.com/issue/Update the description of the \"Project Administrator\" role by adding information about the visibility of the Root project) — TW-91323
* [**Error \"Container wrapper is not supported\" during usage of \"Run in Docker\" build feature if \"Image platform\" was chosen in the build steps**](https://youtrack.jetbrains.com/issue/Error \"Container wrapper is not supported\" during usage of \"Run in Docker\" build feature if \"Image platform\" was chosen in the build steps) — TW-92895
* [**RequiredToolInstalledPrecondition pollutes logs when build type is removed**](https://youtrack.jetbrains.com/issue/RequiredToolInstalledPrecondition pollutes logs when build type is removed) — TW-92939
* [**EC2 Cloud Profile: When there aren't enough addresses in one subnet, another should be tried**](https://youtrack.jetbrains.com/issue/EC2 Cloud Profile: When there aren't enough addresses in one subnet, another should be tried) — TW-88707

### Security

Four security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2025.03.2). Security bulletins for new versions are typically published within the next few days after the release date.