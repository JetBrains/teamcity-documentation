[//]: # (title: TeamCity 2025.03.1 Release Notes)
[//]: # (auxiliary-id: TeamCity 2025.03.1 Release Notes)

## Build 999, 07 April 2025

### Bug

* [TW-92794](https://youtrack.jetbrains.com/issue/TW-92794) — Obsolete branches are sometimes not being removed from the repository state in the database
* [TW-91977](https://youtrack.jetbrains.com/issue/TW-91977) — Wrong item us selected in Sidebar
* [TW-92415](https://youtrack.jetbrains.com/issue/TW-92415) — UX optimization of the Projects sidebar
* [TW-92200](https://youtrack.jetbrains.com/issue/TW-92200) — Expand/collapse test/build details scrolls the page up
* [TW-92642](https://youtrack.jetbrains.com/issue/TW-92642) — Parallel tests and build chain optimizer race leads to more than one build of the same build type in the build chain
* [TW-92667](https://youtrack.jetbrains.com/issue/TW-92667) — TeamCity artifact excluding rule no longer works in version 2025.03
* [TW-71811](https://youtrack.jetbrains.com/issue/TW-71811) — Impossible to see inspection type description
* [TW-68246](https://youtrack.jetbrains.com/issue/TW-68246) — Build overview in Experimental UI. Code inspection -> "View report" link leads to Classic UI
* [TW-92715](https://youtrack.jetbrains.com/issue/TW-92715) — git-lfs does not work for me on tc agent version 2025.03
* [TW-92296](https://youtrack.jetbrains.com/issue/TW-92296) — Unexpected error when creating a token on the VCS Auth Tokens page
* [TW-89577](https://youtrack.jetbrains.com/issue/TW-89577) — Wrong Test Count for DynamicData Tests with TeamCity's Test Adapter
* [TW-91548](https://youtrack.jetbrains.com/issue/TW-91548) — Build approved by a deleted user shows "Approval request timed out"
* [TW-74523](https://youtrack.jetbrains.com/issue/TW-74523) — Warnings in triggers log - "lastProcessedModId has become unreachable, will reset it"
* [TW-90677](https://youtrack.jetbrains.com/issue/TW-90677) — Kubernetes Executor: cross-platform msbuild is stuck 
* [TW-89648](https://youtrack.jetbrains.com/issue/TW-89648) — Unhandled errors in Artifact Migration Tool in case of unavailability of Artifact Storage
* [TW-90256](https://youtrack.jetbrains.com/issue/TW-90256) — Cached parameters after removing PullRequest (BitBucker Server)
* [TW-92013](https://youtrack.jetbrains.com/issue/TW-92013) — Public Recipes: add more information to the "Add recipe" popup
* [TW-91890](https://youtrack.jetbrains.com/issue/TW-91890) — Docker Info tab is not shown due to a com.google.gson.JsonParseException
* [TW-90714](https://youtrack.jetbrains.com/issue/TW-90714) — externalStatus.html requires authenticated session to show the information
* [TW-92240](https://youtrack.jetbrains.com/issue/TW-92240) — Broken link in help menu
* [TW-92575](https://youtrack.jetbrains.com/issue/TW-92575) — Reset cloud profile: Buttons "Reset" and "Delete" are misaligned between project and subproject cloud profiles 
* [TW-92576](https://youtrack.jetbrains.com/issue/TW-92576) — Reset cloud profile: no Reset button on parent project level for subproject, if the project is read only
* [TW-92401](https://youtrack.jetbrains.com/issue/TW-92401) — GitHub App connection doesn't support a large number of installations

### Performance Problem

* [TW-92578](https://youtrack.jetbrains.com/issue/TW-92578) — Persisting of the build runtime state takes too much time
* [TW-89953](https://youtrack.jetbrains.com/issue/TW-89953) — The content on the project page appears and disappears when loaded


### Security

Four security problems have been fixed.
To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2025.03.1).

Security bulletins are typically published few days after the release date.


