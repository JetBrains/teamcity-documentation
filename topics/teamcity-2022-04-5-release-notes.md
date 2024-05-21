[//]: # (title: TeamCity 2022.04.5 Release Notes)
[//]: # (auxiliary-id: TeamCity 2022.04.5 Release Notes)

__Build: 108886__

__8 June 2023__

<!--Project: TeamCity Fix versions: {2022.10.4 (117134)}  visible to: {All Users} #Fixed -{Trunk issue}-->


### Bug

**[TW-77589](https://youtrack.jetbrains.com/issue/TW-77589/Broken-health-report-in-the-latest-Safari-16.0)** — Broken health report in the latest Safari (16.0)

**[TW-77698](https://youtrack.jetbrains.com/issue/TW-77698/Clicking-link-split-of-tests-for-parallel-execution-in-a-Template-causes-an-error.)** — Clicking link "split of tests for parallel execution" in a Template causes an error.

**[TW-75820](https://youtrack.jetbrains.com/issue/TW-75820/Parallel-tests-do-not-export-generated-projects-configurations)** — Parallel tests: do not export generated projects/configurations

**[TW-78096](https://youtrack.jetbrains.com/issue/TW-78096/TeamCity-2022.4.4-crashes-on-startup-due-to-wrong-handling-of-env-variable-in-teamcity-internal.bat)** — TeamCity 2022.4.4 crashes on startup due to wrong handling of env variable in teamcity-internal.bat

**[TW-78690](https://youtrack.jetbrains.com/issue/TW-78690/Avoid-removing-data-from-buildproject-table-in-parallel)** — Avoid removing data from build_project table in parallel

**[TW-77621](https://youtrack.jetbrains.com/issue/TW-77621/Perforce-workspace-cannot-be-deleted-failing-a-build)** — Perforce workspace cannot be deleted, failing a build

**[TW-77167](https://youtrack.jetbrains.com/issue/TW-77167/Changes-in-REST-API-count-1-not-supported)** — Changes in REST API - count:-1 not supported


### Performance Problem

**[TW-80963](https://youtrack.jetbrains.com/issue/TW-80963/Build-status-recalculation-queue-can-become-overflown-if-many-builds-are-finishing-in-the-same-configuration)** — Build status recalculation queue can become overflown if many builds are finishing in the same configuration

**[TW-76292](https://youtrack.jetbrains.com/issue/TW-76292/Slow-LinearRevisionCalculator.findLatestRevisionAffectingBuildType-delays-finish-build-trigger-and-checking-for-changes)** — Slow LinearRevisionCalculator.findLatestRevisionAffectingBuildType delays finish build trigger and checking for changes

**[TW-78186](https://youtrack.jetbrains.com/issue/TW-78186/Too-much-memory-used-by-SyncTask-thread-in-the-CustomDataStorageManager)** — Too much memory used by SyncTask thread in the CustomDataStorageManager

**[TW-78161](https://youtrack.jetbrains.com/issue/TW-78161/Slow-handling-of-test-mutes-when-build-is-finishing-probably-because-of-too-broad-scope-of-analyzed-projects-and-inefficient)** — Slow handling of test mutes when build is finishing probably because of too broad scope of analyzed projects and inefficient locking

**[TW-77257](https://youtrack.jetbrains.com/issue/TW-77257/Significant-performance-degradation-of-calls-for-builds-with-non-default-branch)** — Significant performance degradation of calls for builds with non default branch


### Security Problems

Fixed 1 security problem.