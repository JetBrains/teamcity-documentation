[//]: # (title: TeamCity 2026.1.2 Release Notes)
[//]: # (help-id: TeamCity 2026.1.2 Release Notes)

**Build 222619, 24 June 2026**

### Bug

* [**TW-101333**](https://youtrack.jetbrains.com/issue/TW-101333) — Comparison method violates its general contract! in BuildPromotionProblems.getBuildProblemsFromDB
* [**TW-101373**](https://youtrack.jetbrains.com/issue/TW-101373) — Slash ("/") in S3 path prefix leads to signature mismatch
* [**TW-100758**](https://youtrack.jetbrains.com/issue/TW-100758) — Credentials provided via provideAwsCredentials appear to expire sooner than sessionDuration after upgrade to 2026.1
* [**TW-79701**](https://youtrack.jetbrains.com/issue/TW-79701) — Build hangs without any message ("InvalidRunningBuildException: Unauthorized access to build with id" in the agent log)
* [**TW-99693**](https://youtrack.jetbrains.com/issue/TW-99693) — Broken build if reusing a pipeline with failed jobs in a build chain
* [**TW-97638**](https://youtrack.jetbrains.com/issue/TW-97638) — Stop build command from the server may cancel the step set to be always executed
* [**TW-100860**](https://youtrack.jetbrains.com/issue/TW-100860) — Personal builds fail during agent-side checkout when unshelving a Perforce changelist that contains files opened exclusively
* [**TW-100745**](https://youtrack.jetbrains.com/issue/TW-100745) — Cloud Image can't be moved to a pool not containing the project that contains this image. 
* [**TW-101089**](https://youtrack.jetbrains.com/issue/TW-101089) — Lots of duplicate entry errors on attempt to store a metric id to the dictionary table
* [**TW-99340**](https://youtrack.jetbrains.com/issue/TW-99340) — Messages "Agent will upgrade" and "Agent can't be upgraded automatically" can be shown at the same time

### Performance Problem

* [**TW-100647**](https://youtrack.jetbrains.com/issue/TW-100647) — Server cleanup duration ~3x regression on SQL Server after 2025.11.3 upgrade


### Security

Two security problems have been fixed.
To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2026.1.2).

Security bulletins are typically published few days after the release date.


