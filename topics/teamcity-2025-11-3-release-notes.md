[//]: # (title: TeamCity 2025.11.3 Release Notes)
[//]: # (help-id: TeamCity 2025.11.3 Release Notes)

**Build 000001, 19 February 2026**

### Feature

* [**TW-68556**](https://youtrack.jetbrains.com/issue/TW-68556) — Support quick search in remote builds in My Changes tab

### Task

* [**TW-98512**](https://youtrack.jetbrains.com/issue/TW-98512) — Docker Images, DinD support: make Docker GID fixed to align accross the releases
* [**TW-96441**](https://youtrack.jetbrains.com/issue/TW-96441) — Add retries for PowerShell detection command
* [**TW-98780**](https://youtrack.jetbrains.com/issue/TW-98780) — Docker: update bundled Git version: 2.52.0 -> 2.53.0

### Bug

* [**TW-98803**](https://youtrack.jetbrains.com/issue/TW-98803) — SSH Agent-side checkout fails for VCS root created with new creation flow
* [**TW-98201**](https://youtrack.jetbrains.com/issue/TW-98201) — Incorrect Test Count when Selecting Failed Tests
* [**TW-96304**](https://youtrack.jetbrains.com/issue/TW-96304) — Connection settings are cleaned up after changing the parent project for the connection
* [**TW-96434**](https://youtrack.jetbrains.com/issue/TW-96434) — Fix PowerShell detection on Windows for PowerShell Core edition
* [**TW-96429**](https://youtrack.jetbrains.com/issue/TW-96429) — Optimize PowerShell detection on Windows by avoiding launching the PowerShell executable when possible
* [**TW-98168**](https://youtrack.jetbrains.com/issue/TW-98168) — "Restart server" on the Plugins page does nothing
* [**TW-98143**](https://youtrack.jetbrains.com/issue/TW-98143) — long beforeBuildFinish makes build invisible on some pages
* [**TW-96602**](https://youtrack.jetbrains.com/issue/TW-96602) — "Stop build" button partially obscured in classic UI
* [**TW-98361**](https://youtrack.jetbrains.com/issue/TW-98361) — Deadlock during logging in from the login dialog while the auto-login feature is enabled
* [**TW-91143**](https://youtrack.jetbrains.com/issue/TW-91143) — After upgrading to IDEA 2024.3 teamcity "open in IDE" dialog doesn't detect running IDEA
* [**TW-91447**](https://youtrack.jetbrains.com/issue/TW-91447) — Login fails with NPE
* [**TW-98289**](https://youtrack.jetbrains.com/issue/TW-98289) — Project things leaking through the TC plugin
* [**TW-98251**](https://youtrack.jetbrains.com/issue/TW-98251) — Don't show the re-encryption health report for after the update

### Performance Problem

* [**TW-98726**](https://youtrack.jetbrains.com/issue/TW-98726) — Edit roles pages for a user or group as well as user list page are slow if there are many projects available to the current administrator
* [**TW-98037**](https://youtrack.jetbrains.com/issue/TW-98037) — Locking in AI assistant plugin (DBBackedCustomDataStorage.refresh)
* [**TW-98296**](https://youtrack.jetbrains.com/issue/TW-98296) — Queuing many builds with the same VCS Root Instance and Checkout Rules can lead to builds hanging on the "Finalize build settings" stage 
* [**TW-98259**](https://youtrack.jetbrains.com/issue/TW-98259) — Unnecessary computations are performed in the CommonBranchSpec probably because of the different order of the pull requests returned by the PullRequests build feature
* [**TW-98287**](https://youtrack.jetbrains.com/issue/TW-98287) — TeamCity performs fetch with ^refs/tags/*,+refs/*:refs/* refspec as soon as the number of branches in repository reaches 200
* [**TW-98390**](https://youtrack.jetbrains.com/issue/TW-98390) — JdkTypedValueSet slows TeamCity server startup
* [**TW-98185**](https://youtrack.jetbrains.com/issue/TW-98185) — Should preload build promotions from DB in the SecuredBuildHistory::findEntries method
* [**TW-98083**](https://youtrack.jetbrains.com/issue/TW-98083) — Slow removal of a large project tree (slow ProblemMutingServiceImpl.projectRemoved listener)


### Security

Five security problems have been fixed.
To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2025.11.3).

Security bulletins are typically published few days after the release date.


