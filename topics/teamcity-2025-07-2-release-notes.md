[//]: # (title: TeamCity 2025.07.2 Release Notes)
[//]: # (help-id: TeamCity 2025.07.2 Release Notes)

**Build 197379, 10 September 2025**

### Task

* [**TW-95223**](https://youtrack.jetbrains.com/issue/TW-95223) — Update Azure DevOps connection to support Entra ID OAuth apps
* [**TW-95714**](https://youtrack.jetbrains.com/issue/TW-95714) — Hide versions of the dotCover tool in the tool selector starting with 2025.2.1
* [**TW-94273**](https://youtrack.jetbrains.com/issue/TW-94273) — Docker: add legacy IPTables support to enable Docker-in-Docker on systems without nf_tables modules
* [**TW-94914**](https://youtrack.jetbrains.com/issue/TW-94914) — Validation of the data shown in Sub navigation
* [**TW-95342**](https://youtrack.jetbrains.com/issue/TW-95342) — Update Git within TeamCity Docker Images: 2.50.1 -> 2.51.0

### Bug

* [**TW-92660**](https://youtrack.jetbrains.com/issue/TW-92660) — Misleading statement in the documentation 
* [**TW-95588**](https://youtrack.jetbrains.com/issue/TW-95588) — VCS Roots are not ordered in the Run Custom Build dialog
* [**TW-94996**](https://youtrack.jetbrains.com/issue/TW-94996) — TCP Merge: Scheduled triggers don't have accurate description
* [**TW-95421**](https://youtrack.jetbrains.com/issue/TW-95421) — Jackson Upgrade Broke Kubernetes Plugins
* [**TW-95415**](https://youtrack.jetbrains.com/issue/TW-95415) — Resolution of nested parameters in build feature settings doesn't work for finished builds 
* [**TW-94444**](https://youtrack.jetbrains.com/issue/TW-94444) — TCP Merge: hide Pull requests control from triggers form, if it can't be used 
* [**TW-94613**](https://youtrack.jetbrains.com/issue/TW-94613) — TCP Merge: link to the project in breadcrumbs in Pipeline Settings redirects to Project Overview instead of Project Settings
* [**TW-95225**](https://youtrack.jetbrains.com/issue/TW-95225) — Cannot get rid of auto-generated project: they should not be available in health reports
* [**TW-95590**](https://youtrack.jetbrains.com/issue/TW-95590) — VCS trigger may start a build when there are no relevant changes
* [**TW-95474**](https://youtrack.jetbrains.com/issue/TW-95474) — Cannot disable pull requests in repository settings
* [**TW-95368**](https://youtrack.jetbrains.com/issue/TW-95368) — Build Queue Priority Menu missing
* [**TW-93146**](https://youtrack.jetbrains.com/issue/TW-93146) — Builds got stuck for 2+ hours (more than timeout)
* [**TW-95398**](https://youtrack.jetbrains.com/issue/TW-95398) — Job is executed in a pipeline even when an upstream job is failed
* [**TW-94707**](https://youtrack.jetbrains.com/issue/TW-94707) — Broken layout in the info popup on the build dependencies timeline view
* [**TW-95100**](https://youtrack.jetbrains.com/issue/TW-95100) — Build on Pull Request runs in default branch (secondary VCS root in Pipeline)
* [**TW-47593**](https://youtrack.jetbrains.com/issue/TW-47593) — Build without own VCS roots is marked as "History" after removal of a snapshot dependency
* [**TW-94909**](https://youtrack.jetbrains.com/issue/TW-94909) — TCP Merge: Irrelevant help link in Pipelines UI
* [**TW-95327**](https://youtrack.jetbrains.com/issue/TW-95327) — TCP Merge:  infinite loader when the pipeline is in queue until a page refresh  
* [**TW-94721**](https://youtrack.jetbrains.com/issue/TW-94721) — TCP Merge: Secondary VCS root added from Any Git URL looks like added from connection until Save
* [**TW-94782**](https://youtrack.jetbrains.com/issue/TW-94782) — TCP merge: Typo "repositorieses" in repository list when creating Pipeline from connection
* [**TW-94687**](https://youtrack.jetbrains.com/issue/TW-94687) — TCP Merge: can't access step edit action (the pen is hiding when cursor reaches the point)
* [**TW-91440**](https://youtrack.jetbrains.com/issue/TW-91440) — Hide stacktraces from the default teamcity-cleanup.log if TeamCity can't delete the artifacts from Azure
* [**TW-95298**](https://youtrack.jetbrains.com/issue/TW-95298) — TCP Merge: Removing Schedule Trigger Time Zone Crashes the Page
* [**TW-95350**](https://youtrack.jetbrains.com/issue/TW-95350) — Schedule trigger with "self dependency" adjusts build revisions based on the watched build
* [**TW-94849**](https://youtrack.jetbrains.com/issue/TW-94849) — Public recipes: fix the text in the delete dialog of a recipe with usages
* [**TW-95161**](https://youtrack.jetbrains.com/issue/TW-95161) — Link to private recipe from the build step is broken (opens empty recipes page)
* [**TW-94382**](https://youtrack.jetbrains.com/issue/TW-94382) — TCP Merge: Misaligned "Reorder" button in Edit project settings
* [**TW-94321**](https://youtrack.jetbrains.com/issue/TW-94321) — TCP Merge: 'Job reused' label is not added to all the reused jobs in the pipeline
* [**TW-93213**](https://youtrack.jetbrains.com/issue/TW-93213) — Can't download JDBC driver as <datadir> not writable
* [**TW-93403**](https://youtrack.jetbrains.com/issue/TW-93403) — TCP Merge: VCS root usages report leads to Versioned settings of virtual project created for Pipelines
* [**TW-94636**](https://youtrack.jetbrains.com/issue/TW-94636) — TCP Merge: YAML/Visual view is not preserved between pipelines


### Security

Three security problems have been fixed.
To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2025.07.2).

Security bulletins are typically published few days after the release date.


