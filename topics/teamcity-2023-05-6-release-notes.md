[//]: # (title: TeamCity 2023.05.6 Release Notes)
[//]: # (auxiliary-id: TeamCity 2023.05.6 Release Notes)

__Build: 129475__

__30 May 2024__


<!--Project: TeamCity Fix versions: 2023.05.5 -{2023.05.4 (129421)}  visible to: {All Users} #Fixed #Testing -{Trunk issue}-->


### Bug

**[TW-87765](https://youtrack.jetbrains.com/issue/TW-87765/Subproject-administrator-cannot-view-NuGet-feed-and-Artifact-storage-settings-of-a-parent-project)** — Subproject administrator cannot view NuGet feed and Artifact storage settings of a parent project

**[TW-87750](https://youtrack.jetbrains.com/issue/TW-87750/The-user-with-the-Project-Administrator-role-cant-see-the-NuGet-feed)** — The user with the Project Administrator role can't see the NuGet feed

**[TW-86261](https://youtrack.jetbrains.com/issue/TW-86261/Regression-java.lang.InstantiationException-bean-bean-not-found-within-scope-while-processing-request-GET-admin)** — Regression: java.lang.InstantiationException: bean bean not found within scope; while processing request

**[TW-83663](https://youtrack.jetbrains.com/issue/TW-83663/REST-API-builds-with-failedToStarttrue-dont-show-up-in-response-when-combined-with-propertynameXYZ)** — REST API - builds with "failedToStart:true" don't show up in response when combined with "property:(name:XYZ)"

**[TW-85625](https://youtrack.jetbrains.com/issue/TW-85625/Perforce-possible-race-condition-when-collecting-changes)** — Perforce: possible race condition when collecting changes

**[TW-85328](https://youtrack.jetbrains.com/issue/TW-85328/Perforce-build-continues-executing-even-if-there-are-problems-during-checkout)** — Perforce: build continues executing even if there are problems during checkout

**[TW-82564](https://youtrack.jetbrains.com/issue/TW-82564/TeamCity-REST-API-may-occasionally-provide-artifacts-of-another-build-of-the-same-build-configuration)** — TeamCity REST API may occasionally provide artifacts of another build of the same build configuration

**[TW-84339](https://youtrack.jetbrains.com/issue/TW-84339/Cannot-open-artifacts-folder-with-square-brackets-from-the-UI)** — Cannot open artifacts folder with square brackets from the UI

**[TW-84065](https://youtrack.jetbrains.com/issue/TW-84065/Enable-testOnBorrow-and-validation-query-for-PostgreSQL-connections-by-default)** — Enable testOnBorrow and validation query for PostgreSQL connections by default

**[TW-83312](https://youtrack.jetbrains.com/issue/TW-83312/Builds-fail-on-free-disk-space-stage)** — Builds fail on free disk space stage

**[TW-83829](https://youtrack.jetbrains.com/issue/TW-83829/Python-executable-could-be-found-but-not-reported-to-agent-parameter)** — Python executable could be found but not reported to agent parameter

**[TW-83698](https://youtrack.jetbrains.com/issue/TW-83698/perforce-shelve-trigger-firing-for-an-already-ran-build)** — perforce shelve trigger firing for an already ran build


### Performance Problem

**[TW-85640](https://youtrack.jetbrains.com/issue/TW-85640/Exclude-testOccurrencecount-from-request-on-a-build-overview-page)** — Exclude testOccurrence(count) from request on a build overview page

**[TW-85482](https://youtrack.jetbrains.com/issue/TW-85482/Too-slow-revision-computation-lots-of-time-spent-in-CheckoutRulesRevWalk.collectUninterestingCommits)** — Too slow revision computation (lots of time spent in CheckoutRulesRevWalk.collectUninterestingCommits)

**[TW-84618](https://youtrack.jetbrains.com/issue/TW-84618/Inefficient-checking-for-changes-in-Perforce-nested-doGetPath2LatestRevision-calls)** — Inefficient checking for changes in Perforce (nested doGetPath2LatestRevision calls)


<!--Project: TeamCity Fix versions: {2023.05.5 (129472)} #{Security Problem}  #Fixed #Testing -{Trunk issue} -bulletin-exclude -->

### Security

25 security issues were fixed. To protect customers who have not yet updated their servers, we typically withhold details about these fixes. Instead, we encourage you to review our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity) a few days after each bugfix release for more information.

In our effort to enhance transparency and due to potential delays in publishing new security bulletins (stemming from the simultaneous release of the 2022.04.6, 2022.10.5, 2023.05.5, 2023.11.5, and 2024.03.2 bug-fix updates), we have decided to provide a summary of both new and backported fixes.

#### Backported Fixes

These issues were resolved in newer major TeamCity versions and backported to this bug-fix update. You can find more info about them in our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity).

* Path traversal allowed reading data within JAR archives. Reported by Sndav Bai and Crispr Xiang from TianShu Dubhe Team (TW-86017)
* Stored XSS during restore from backup was possible (TW-82309)
* Authentication bypass allowing to perform admin actions was possible. Reported by Rapid7 team (TW-86500)
* Authentication bypass leading to RCE was possible. Reported by Sndav Bai and Crispr Xiang from TianShu Dubhe Team (TW-86005)
* Path traversal allowing to perform limited admin actions was possible. Reported by Rapid7 team (TW-86502)
* XXE was possible in the Maven build steps detector (TW-86300)
* Authenticated users without administrative permissions could register other users when self-registration was disabled (TW-87046)
* Server administrators could remove arbitrary files from the server by installing tools (TW-86039)
* Presigned URL generation requests in S3 Artifact Storage plugin were authorized improperly (TW-85562)
* XSS was possible via Agent Distribution settings (TW-86535)
* Open redirect was possible on the login page (TW-87062)
* Limited directory traversal was possible in the Kotlin DSL documentation (TW-85585)
* Stored XSS while viewing the build log was possible (TW-81777)

#### Recently Resolved Issues

Fixes for the following security issues are not immediately available in our Security Bulletin: newly discovered and fixed problems, issues from upstream libraries outside the TeamCity codebase, specific cases of previously reported vulnerabilities, and others.

You can expect details on most of these issues to be published in our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity) a few days after the official release.

* Path traversal allowing to read files from server was possible
* TeamCity server could be accessed without authorization during specific brief moments of its lifecycle
* Several Stored XSS in code inspection reports
* Improper access control in Pull Requests and Commit status publisher build features
* Stored XSS in Commit status publisher was possible
* A third-party agent could impersonate a cloud agent
* An XSS could be executed via certain report grouping and filtering operations
* Stored XSS via third-party reports was possible
* Reflected XSS via OAuth provider configuration was possible
* Stored XSS via issue tracker integration was possible
* Stored XSS via OAuth connection settings was possible
* Reflected XSS on the subscriptions page was possible

