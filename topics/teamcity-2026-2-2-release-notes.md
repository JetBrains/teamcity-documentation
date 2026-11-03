[//]: # (title: TeamCity 2026.2.2 Release Notes)
[//]: # (help-id: TeamCity 2026.2.2 Release Notes)

**Build 239308, 3 November 2026**

### Feature

* [**TW-102444**](https://youtrack.jetbrains.com/issue/TW-102444) — Allow selecting agent pool or executor for DSL compilation
* [**TW-102418**](https://youtrack.jetbrains.com/issue/TW-102418) — Automatic security patch installation

## Task

* [**TW-105214**](https://youtrack.jetbrains.com/issue/TW-105214) — Update bundled Java to Corretto 21.0.13.0.11 in TeamCity distributions and Docker images

### Bug

* [**TW-98419**](https://youtrack.jetbrains.com/issue/TW-98419) — TeamCity provides artifact file with incorrect Content-disposition header format
* [**TW-103595**](https://youtrack.jetbrains.com/issue/TW-103595) — Dynamic Build Chains: revisions are not synchronised between the Final and the Generator builds
* [**TW-104499**](https://youtrack.jetbrains.com/issue/TW-104499) — A VCS root with too big body causes errors during the changes collection which block processing of other VCS roots
* [**TW-104529**](https://youtrack.jetbrains.com/issue/TW-104529) — Load settings from VCS and auto-sync do nothing for a project created by its parent's versioned settings
* [**TW-104602**](https://youtrack.jetbrains.com/issue/TW-104602) — "Show dev panel" button appears when  creating a Kubernetes cloud profile
* [**TW-104767**](https://youtrack.jetbrains.com/issue/TW-104767) — Click "Check for updates" on a secondary node doesn't load new security patches
* [**TW-104813**](https://youtrack.jetbrains.com/issue/TW-104813) — After server restart, connected cloud agents are not matched back to their instances, so instances with running builds are terminated as idle
* [**TW-104981**](https://youtrack.jetbrains.com/issue/TW-104981) — Unnecessary triggering in unmodified pull request branches after the server restart
* [**TW-100479**](https://youtrack.jetbrains.com/issue/TW-100479) — TeamCity builds sporadically fail to perform checkout with Git 2.54.0: unable to find all commit-graph files
* [**TW-100871**](https://youtrack.jetbrains.com/issue/TW-100871) — Builds fail due to irrelevant git warnings
* [**TW-104556**](https://youtrack.jetbrains.com/issue/TW-104556) — What's new popup shows outdates information for TeamCity Cloud
* [**TW-104816**](https://youtrack.jetbrains.com/issue/TW-104816) — Artifact publishing from Windows share fails
* [**TW-105338**](https://youtrack.jetbrains.com/issue/TW-105338) — Deprecate classic (legacy) queries for Vault parameters
* [**TW-103608**](https://youtrack.jetbrains.com/issue/TW-103608) — AI Assistant: OpenAI-compatible provider drops the last URL path segment when the URL has no trailing slash

### Performance Problem

* [**TW-103847**](https://youtrack.jetbrains.com/issue/TW-103847) — Agent-side ls-remote is not scoped to the needed refs, unlike the server side
* [**TW-104295**](https://youtrack.jetbrains.com/issue/TW-104295) — Slow revision calculation for checkout rules affecting rarely changed files
* [**TW-104785**](https://youtrack.jetbrains.com/issue/TW-104785) — Loading notification rules might exhaust database connection pool leading to TeamCity outage
* [**TW-103231**](https://youtrack.jetbrains.com/issue/TW-103231) — Build triggers processing is slowed down by the pipeline dependencies parsing


### Security

40 security problems have been fixed.
To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2026.2.2).

Security bulletins are typically published few days after the release date.


