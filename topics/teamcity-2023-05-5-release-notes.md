[//]: # (title: TeamCity 2023.05.5 Release Notes)
[//]: # (auxiliary-id: TeamCity 2023.05.5 Release Notes)

__Build: ???__

__??? May 2024__


<!--Project: TeamCity Fix versions: 2023.05.5 -{2023.05.4 (129421)}  visible to: {All Users} #Fixed #Testing -{Trunk issue}-->


### Bug

**[TW-87765](https://youtrack.jetbrains.com/issue/TW-87765/Subproject-administrator-cannot-view-NuGet-feed-and-Artifact-storage-settings-of-a-parent-project)** — Subproject administrator cannot view NuGet feed and Artifact storage settings of a parent project

**[TW-87750](https://youtrack.jetbrains.com/issue/TW-87750/The-user-with-the-Project-Administrator-role-cant-see-the-NuGet-feed)** — The user with the Project Administrator role can't see the NuGet feed

**[TW-86261](https://youtrack.jetbrains.com/issue/TW-86261/Regression-java.lang.InstantiationException-bean-bean-not-found-within-scope-while-processing-request-GET-admin)** — Regression: java.lang.InstantiationException: bean bean not found within scope; while processing request: GET '/admin/admin.html?item=diagnostics&tab=dataDir&file=..%2F..%2Fopt%2FTeamCity_Data%2Fconfig%2Finternal.properties'

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


<!--Project: TeamCity Fix versions: 2023.05.5 -{2023.05.4 (129421)} #{Security Problem}  #Fixed #Testing -{Trunk issue}-->

### Security

29 security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2023.05.5). Security bulletins for new versions are typically published within the next few days after the release date.

