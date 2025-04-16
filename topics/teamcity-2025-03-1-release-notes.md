[//]: # (title: TeamCity 2025.03.1 Release Notes)
[//]: # (help-id: TeamCity 2025.03.1 Release Notes)


**Build 186125, 16 April 2025**

### Bug

* [**TW-92881**](https://youtrack.jetbrains.com/issue/TW-92881) — Custom report tab scrolls infinitely
* [**TW-92731**](https://youtrack.jetbrains.com/issue/TW-92731) — Argument for @NotNull parameter 'template' of jetbrains/buildServer/web/functions/admin/AdminFunctions.getTemplateSettingsId must not be null
* [**TW-92133**](https://youtrack.jetbrains.com/issue/TW-92133) — Test reports for package names starting with a digit
* [**TW-74523**](https://youtrack.jetbrains.com/issue/TW-74523) — Warnings in triggers log - "lastProcessedModId has become unreachable, will reset it"
* [**TW-90256**](https://youtrack.jetbrains.com/issue/TW-90256) — Cached parameters after removing PullRequest (BitBucker Server)
* [**TW-90714**](https://youtrack.jetbrains.com/issue/TW-90714) — externalStatus.html requires authenticated session to show the information
* [**TW-92715**](https://youtrack.jetbrains.com/issue/TW-92715) — git-lfs does not work for me on tc agent version 2025.03
* [**TW-92842**](https://youtrack.jetbrains.com/issue/TW-92842) — Upgrade to 2025.03 might fail in PipelineNameToProjectNameConverter when build type XML config is corrupted
* [**TW-92667**](https://youtrack.jetbrains.com/issue/TW-92667) — TeamCity artifact excluding rule no longer works in version 2025.03
* [**TW-85160**](https://youtrack.jetbrains.com/issue/TW-85160) — Agent Pool is not removed when cloud profile is deleted
* [**TW-89648**](https://youtrack.jetbrains.com/issue/TW-89648) — Unhandled errors in Artifact Migration Tool in case of unavailability of Artifact Storage
* [**TW-91548**](https://youtrack.jetbrains.com/issue/TW-91548) — Build approved by a deleted user shows "Approval request timed out"
* [**TW-92013**](https://youtrack.jetbrains.com/issue/TW-92013) — Public Recipes: add more information to the "Add recipe" popup
* [**TW-78929**](https://youtrack.jetbrains.com/issue/TW-78929) — Cloud agent instance name truncated (1st letter missing) in cloud profile group
* [**TW-92470**](https://youtrack.jetbrains.com/issue/TW-92470) — Tests from usual dependencies reported to Parallel tests build after Re-run
* [**TW-92456**](https://youtrack.jetbrains.com/issue/TW-92456) — Project lists in the sidebar is collapsed after page refresh
* [**TW-92642**](https://youtrack.jetbrains.com/issue/TW-92642) — Parallel tests and build chain optimizer race leads to more than one build of the same build type in the build chain
* [**TW-92794**](https://youtrack.jetbrains.com/issue/TW-92794) — Obsolete branches are sometimes not being removed from the repository state in the database
* [**TW-91977**](https://youtrack.jetbrains.com/issue/TW-91977) — Wrong item us selected in Sidebar
* [**TW-92200**](https://youtrack.jetbrains.com/issue/TW-92200) — Expand/collapse test/build details scrolls the page up
* [**TW-71811**](https://youtrack.jetbrains.com/issue/TW-71811) — Impossible to see inspection type description
* [**TW-68246**](https://youtrack.jetbrains.com/issue/TW-68246) — Build overview in Experimental UI. Code inspection -> "View report" link leads to Classic UI
* [**TW-92296**](https://youtrack.jetbrains.com/issue/TW-92296) — Unexpected error when creating a token on the VCS Auth Tokens page
* [**TW-91890**](https://youtrack.jetbrains.com/issue/TW-91890) — Docker Info tab is not shown due to a com.google.gson.JsonParseException
* [**TW-92240**](https://youtrack.jetbrains.com/issue/TW-92240) — Broken link in help menu
* [**TW-92575**](https://youtrack.jetbrains.com/issue/TW-92575) — Reset cloud profile: Buttons "Reset" and "Delete" are misaligned between project and subproject cloud profiles 
* [**TW-92576**](https://youtrack.jetbrains.com/issue/TW-92576) — Reset cloud profile: no Reset button on parent project level for subproject, if the project is read only
* [**TW-92401**](https://youtrack.jetbrains.com/issue/TW-92401) — GitHub App connection doesn't support a large number of installations

### Performance Problem

* [**TW-92578**](https://youtrack.jetbrains.com/issue/TW-92578) — Persisting of the build runtime state takes too much time
* [**TW-89953**](https://youtrack.jetbrains.com/issue/TW-89953) — The content on the project page appears and disappears when loaded

### Security

Five security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2025.03.1). Security bulletins for new versions are typically published within the next few days after the release date.