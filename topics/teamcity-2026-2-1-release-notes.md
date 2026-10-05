[//]: # (title: TeamCity 2026.2.1 Release Notes)
[//]: # (help-id: TeamCity 2026.2.1 Release Notes)

**Build 239120, 05 October 2026**

### Bug

* [**TW-103395**](https://youtrack.jetbrains.com/issue/TW-103395) — When the pre-existing branch was removed respond with empty yaml
* [**TW-103737**](https://youtrack.jetbrains.com/issue/TW-103737) — Execution timeout should cause retry of a dependency
* [**TW-103446**](https://youtrack.jetbrains.com/issue/TW-103446) — YAML in branches: can't import yaml from the main branch
* [**TW-104024**](https://youtrack.jetbrains.com/issue/TW-104024) — New builds are triggered by +pr:* VCS trigger on closed PRs if PRs on source branches are used and covered by the branchspec
* [**TW-104290**](https://youtrack.jetbrains.com/issue/TW-104290) — TeamCity can skip updating the current project from the versioned settings claiming that the task with the same revision is scheduled
* [**TW-104098**](https://youtrack.jetbrains.com/issue/TW-104098) — Mercurial in pipelines cannot compute revision properly because there are no changes attached to the job
* [**TW-98285**](https://youtrack.jetbrains.com/issue/TW-98285) — "Expand" tooltip blocks the expand sidebar button
* [**TW-103024**](https://youtrack.jetbrains.com/issue/TW-103024) — True-Up license ignores standalone agent licenses when calculating the licensed agent count
* [**TW-103853**](https://youtrack.jetbrains.com/issue/TW-103853) — Make sure on agent DSL execution uses Maven 3.9 and not just a default one
* [**TW-100772**](https://youtrack.jetbrains.com/issue/TW-100772) — DiffView in teamcity UI stopped working on 2026.1 for TFS
* [**TW-103602**](https://youtrack.jetbrains.com/issue/TW-103602) — Improve contrast for Versioned Settings page when it's in read-only mode
* [**TW-102948**](https://youtrack.jetbrains.com/issue/TW-102948) — Pipeline delete button is unavailable if pipeline versioned settings are stored in yaml
* [**TW-103946**](https://youtrack.jetbrains.com/issue/TW-103946) — Not all Log4j appenders are started prior to adding them to the logging setups
* [**TW-102758**](https://youtrack.jetbrains.com/issue/TW-102758) — Param "teamcity.buildQueue.restartBuildAttempts" not respected on virtual builds.
* [**TW-103813**](https://youtrack.jetbrains.com/issue/TW-103813) — "Investigations Auto-Assigner" plugin may assign flaky test for investigation
* [**TW-94612**](https://youtrack.jetbrains.com/issue/TW-94612) — Log in to GitHub button not shown during creation a new pipeline after revoking OAuth token without typing the repository name
* [**TW-103778**](https://youtrack.jetbrains.com/issue/TW-103778) — Avatar upload fails with HTTP 400 after servlet multipart migration
* [**TW-104025**](https://youtrack.jetbrains.com/issue/TW-104025) — Visual Studio Build Tools 2026 September update : MSBuildTools not detected
* [**TW-100753**](https://youtrack.jetbrains.com/issue/TW-100753) — Heartbeat thread can't recover after DB communication failure
* [**TW-100466**](https://youtrack.jetbrains.com/issue/TW-100466) — Unexpected skipping of dependency builds in TeamCity build chains (skipQueuedBuild service message)
* [**TW-102376**](https://youtrack.jetbrains.com/issue/TW-102376) — YAML in branches: Branch selection dialog looses branch name when typed too fast
* [**TW-103535**](https://youtrack.jetbrains.com/issue/TW-103535) — YAML in branches: improve error dialogue in case the commit failed 
* [**TW-103537**](https://youtrack.jetbrains.com/issue/TW-103537) — YAML in branches: Wrong color of the branch icon on hover in error state
* [**TW-101975**](https://youtrack.jetbrains.com/issue/TW-101975) — Pipelines: numeric runner fields fail to render — "No applicable renderer found" (e.g. SSH Upload: Port, Timeout)

### Performance Problem

* [**TW-103895**](https://youtrack.jetbrains.com/issue/TW-103895) — FakeHttpSession objects leak in case of continuous authentication with help of Basic auth and auth token as a password
* [**TW-104021**](https://youtrack.jetbrains.com/issue/TW-104021) — Limit the number of changes processed by a VCS trigger for a newly detected branch


### Security

41 security problems have been fixed.
To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2026.2.1).

Security bulletins are typically published few days after the release date.


