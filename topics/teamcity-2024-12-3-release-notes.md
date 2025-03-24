[//]: # (title: TeamCity 2024.12.3 Release Notes)
[//]: # (auxiliary-id: TeamCity 2024.12.3 Release Notes)


**Build 174541, 24 March 2025**

### Task

* [**TW-90668**](https://youtrack.jetbrains.com/issue/TW-90668) — Update Plugin for .NET Runtime Detection on Ubuntu 22.04/24.04

### Bug

* [**TW-92199**](https://youtrack.jetbrains.com/issue/TW-92199) — Container Deployer runner is broken since 2024.12.1
* [**TW-92394**](https://youtrack.jetbrains.com/issue/TW-92394) — S3 cleanup cannot delete artifacts : \"Project is not specified to get correct Connection\"
* [**TW-74812**](https://youtrack.jetbrains.com/issue/TW-74812) — False warning on unauthenticated docker pull
* [**TW-86250**](https://youtrack.jetbrains.com/issue/TW-86250) — Mute doesn't work for Unit 5 TestFactory generated tests
* [**TW-83825**](https://youtrack.jetbrains.com/issue/TW-83825) — Change agent requirement for cross-platform ReSharper CLT
* [**TW-91914**](https://youtrack.jetbrains.com/issue/TW-91914) — GitHub App always times out when loading repositories
* [**TW-91608**](https://youtrack.jetbrains.com/issue/TW-91608) — Unable to retrieve build info for build which was approved via Build Approval feature after feature removed from build type
* [**TW-91830**](https://youtrack.jetbrains.com/issue/TW-91830) — DSL Context parameters are removed when disabling Versioned Settings 

### Performance Problem

* [**TW-92098**](https://youtrack.jetbrains.com/issue/TW-92098) — Poor REST API /builds endpoint performance
* [**TW-92237**](https://youtrack.jetbrains.com/issue/TW-92237) — Inefficient code in VcsSettingsBean.getPopularVcsRoots

### Security

-1 security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2024.12.3). Security bulletins for new versions are typically published within the next few days after the release date.