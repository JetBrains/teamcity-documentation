[//]: # (title: TeamCity 2025.07 Release Notes)
[//]: # (auxiliary-id: TeamCity 2025.07 Release Notes)

## Build 9999, 04 April 2025

### Bug

* [TW-92785](https://youtrack.jetbrains.com/issue/TW-92785) — Possible deadlock when enabling MainNode, reproduced in tests
* [TW-92667](https://youtrack.jetbrains.com/issue/TW-92667) — TeamCity artifact excluding rule no longer works in version 2025.03
* [TW-92645](https://youtrack.jetbrains.com/issue/TW-92645) — Always show "Tokens" tab in Versioned Settings menu
* [TW-92139](https://youtrack.jetbrains.com/issue/TW-92139) — jetbrains.buildServer.metrics.ServerMetrics should support metric removal
* [TW-81859](https://youtrack.jetbrains.com/issue/TW-81859) — Retry failed agent side git checkout
* [TW-92450](https://youtrack.jetbrains.com/issue/TW-92450) — S3 artifact cleanup failure may cause NoSuchElementException
* [TW-92401](https://youtrack.jetbrains.com/issue/TW-92401) — GitHub App connection doesn't support a large number of installations
* [TW-91938](https://youtrack.jetbrains.com/issue/TW-91938) — Artifacts are not split per batch when running Parallel Tests which causes confusion
* [TW-91034](https://youtrack.jetbrains.com/issue/TW-91034) — Incorrrect git_custom_certificates.crt is not recreated after error "Problem with the SSL CA cert (path? access rights?)" 
* [TW-88309](https://youtrack.jetbrains.com/issue/TW-88309) — No way to configure Dependency Cache build feature using Kotlin DSL

### Feature

* [TW-90527](https://youtrack.jetbrains.com/issue/TW-90527) — Support for multiple Perforce Shelve Triggers
* [TW-87033](https://youtrack.jetbrains.com/issue/TW-87033) — Implement dependency cache for .NET runner
* [TW-87032](https://youtrack.jetbrains.com/issue/TW-87032) — Implement dependency cache for Gradle runner

### Task

* [TW-90918](https://youtrack.jetbrains.com/issue/TW-90918) — Specifying a custom build file location is deprecated
* [TW-89113](https://youtrack.jetbrains.com/issue/TW-89113) — HTTPS: Support custom maxHttpHeaderSize for connector


### Security

Two security problems have been fixed.
To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2025.07).

Security bulletins are typically published few days after the release date.


