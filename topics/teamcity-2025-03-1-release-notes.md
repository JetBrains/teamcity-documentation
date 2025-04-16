[//]: # (title: TeamCity 2025.03.1 Release Notes)
[//]: # (help-id: TeamCity 2025.03.1 Release Notes)


**Build 0, 16 April 2025**

### Bug

* [**Custom report tab scrolls infinitely**](https://youtrack.jetbrains.com/issue/Custom report tab scrolls infinitely) — TW-92881
* [**Argument for @NotNull parameter 'template' of jetbrains/buildServer/web/functions/admin/AdminFunctions.getTemplateSettingsId must not be null**](https://youtrack.jetbrains.com/issue/Argument for @NotNull parameter 'template' of jetbrains/buildServer/web/functions/admin/AdminFunctions.getTemplateSettingsId must not be null) — TW-92731
* [**Test reports for package names starting with a digit**](https://youtrack.jetbrains.com/issue/Test reports for package names starting with a digit) — TW-92133
* [**Warnings in triggers log - \"lastProcessedModId has become unreachable, will reset it\"**](https://youtrack.jetbrains.com/issue/Warnings in triggers log - \"lastProcessedModId has become unreachable, will reset it\") — TW-74523
* [**Cached parameters after removing PullRequest (BitBucker Server)**](https://youtrack.jetbrains.com/issue/Cached parameters after removing PullRequest (BitBucker Server)) — TW-90256
* [**externalStatus.html requires authenticated session to show the information**](https://youtrack.jetbrains.com/issue/externalStatus.html requires authenticated session to show the information) — TW-90714
* [**git-lfs does not work for me on tc agent version 2025.03**](https://youtrack.jetbrains.com/issue/git-lfs does not work for me on tc agent version 2025.03) — TW-92715
* [**Upgrade to 2025.03 might fail in PipelineNameToProjectNameConverter when build type XML config is corrupted**](https://youtrack.jetbrains.com/issue/Upgrade to 2025.03 might fail in PipelineNameToProjectNameConverter when build type XML config is corrupted) — TW-92842
* [**TeamCity artifact excluding rule no longer works in version 2025.03**](https://youtrack.jetbrains.com/issue/TeamCity artifact excluding rule no longer works in version 2025.03) — TW-92667
* [**Agent Pool is not removed when cloud profile is deleted**](https://youtrack.jetbrains.com/issue/Agent Pool is not removed when cloud profile is deleted) — TW-85160
* [**Unhandled errors in Artifact Migration Tool in case of unavailability of Artifact Storage**](https://youtrack.jetbrains.com/issue/Unhandled errors in Artifact Migration Tool in case of unavailability of Artifact Storage) — TW-89648
* [**Build approved by a deleted user shows \"Approval request timed out\"**](https://youtrack.jetbrains.com/issue/Build approved by a deleted user shows \"Approval request timed out\") — TW-91548
* [**Public Recipes: add more information to the \"Add recipe\" popup**](https://youtrack.jetbrains.com/issue/Public Recipes: add more information to the \"Add recipe\" popup) — TW-92013
* [**Cloud agent instance name truncated (1st letter missing) in cloud profile group**](https://youtrack.jetbrains.com/issue/Cloud agent instance name truncated (1st letter missing) in cloud profile group) — TW-78929
* [**Tests from usual dependencies reported to Parallel tests build after Re-run**](https://youtrack.jetbrains.com/issue/Tests from usual dependencies reported to Parallel tests build after Re-run) — TW-92470
* [**Project lists in the sidebar is collapsed after page refresh**](https://youtrack.jetbrains.com/issue/Project lists in the sidebar is collapsed after page refresh) — TW-92456
* [**Parallel tests and build chain optimizer race leads to more than one build of the same build type in the build chain**](https://youtrack.jetbrains.com/issue/Parallel tests and build chain optimizer race leads to more than one build of the same build type in the build chain) — TW-92642
* [**Obsolete branches are sometimes not being removed from the repository state in the database**](https://youtrack.jetbrains.com/issue/Obsolete branches are sometimes not being removed from the repository state in the database) — TW-92794
* [**Wrong item us selected in Sidebar**](https://youtrack.jetbrains.com/issue/Wrong item us selected in Sidebar) — TW-91977
* [**Expand/collapse test/build details scrolls the page up**](https://youtrack.jetbrains.com/issue/Expand/collapse test/build details scrolls the page up) — TW-92200
* [**Impossible to see inspection type description**](https://youtrack.jetbrains.com/issue/Impossible to see inspection type description) — TW-71811
* [**Build overview in Experimental UI. Code inspection -> \"View report\" link leads to Classic UI**](https://youtrack.jetbrains.com/issue/Build overview in Experimental UI. Code inspection -> \"View report\" link leads to Classic UI) — TW-68246
* [**Unexpected error when creating a token on the VCS Auth Tokens page**](https://youtrack.jetbrains.com/issue/Unexpected error when creating a token on the VCS Auth Tokens page) — TW-92296
* [**Docker Info tab is not shown due to a com.google.gson.JsonParseException**](https://youtrack.jetbrains.com/issue/Docker Info tab is not shown due to a com.google.gson.JsonParseException) — TW-91890
* [**Broken link in help menu**](https://youtrack.jetbrains.com/issue/Broken link in help menu) — TW-92240
* [**Reset cloud profile: Buttons \"Reset\" and \"Delete\" are misaligned between project and subproject cloud profiles **](https://youtrack.jetbrains.com/issue/Reset cloud profile: Buttons \"Reset\" and \"Delete\" are misaligned between project and subproject cloud profiles ) — TW-92575
* [**Reset cloud profile: no Reset button on parent project level for subproject, if the project is read only**](https://youtrack.jetbrains.com/issue/Reset cloud profile: no Reset button on parent project level for subproject, if the project is read only) — TW-92576
* [**GitHub App connection doesn't support a large number of installations**](https://youtrack.jetbrains.com/issue/GitHub App connection doesn't support a large number of installations) — TW-92401

### Performance Problem

* [**Persisting of the build runtime state takes too much time**](https://youtrack.jetbrains.com/issue/Persisting of the build runtime state takes too much time) — TW-92578
* [**The content on the project page appears and disappears when loaded**](https://youtrack.jetbrains.com/issue/The content on the project page appears and disappears when loaded) — TW-89953

### Security

Five security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2025.03.1). Security bulletins for new versions are typically published within the next few days after the release date.