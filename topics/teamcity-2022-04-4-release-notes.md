[//]: # (title: TeamCity 2022.04.4 Release Notes)
[//]: # (auxiliary-id: TeamCity 2022.04.4 Release Notes)

__Build: 108763__

__19 September 2022__


### Feature

**[TW-75759](hhttps://youtrack.jetbrains.com/issue/TW-75759/Build-approval-support-parameter-reference-for-approval-rules)** — Build approval: support parameter reference for approval rules

### Bug

**[TW-77454](https://youtrack.jetbrains.com/issue/TW-77454/Critical-error-with-parsing-build-configuration-xml-file-Error-on-line-107-Invalid-byte-2-of-4-byte-UTF-8-sequence)** — Critical error with parsing build configuration xml file: Error on line 107: Invalid byte 2 of 4-byte UTF-8 sequence

**[TW-77253](https://youtrack.jetbrains.com/issue/TW-77253/GZipping-content-for-some-static-resources-may-not-work-correctly)** — GZipping content for some static resources may not work correctly

**[TW-77445](https://youtrack.jetbrains.com/issue/TW-77445/Allow-providing-revisions-for-dependencies-when-a-build-chain-is-triggered-with-help-of-REST-API)** — Allow providing revisions for dependencies when a build chain is triggered with the help of REST API

**[TW-77184](https://youtrack.jetbrains.com/issue/TW-77184/Commit-Status-Publisher-spams-the-same-commit-200-times-per-second-filling-up-memory-fast)** — Commit Status Publisher spams the same commit 200 times per second filling up memory fast

**[TW-77471](https://youtrack.jetbrains.com/issue/TW-77471/IllegalStateException-Duplicate-key-while-processing-the-build-queue)** — IllegalStateException: Duplicate key while processing the build queue

**[TW-77355](https://youtrack.jetbrains.com/issue/TW-77355/Show-initial-error-in-Teamcity-UI-if-Restore-from-backup-on-server-start-has-failed)** — Show initial error in Teamcity UI, if Restore from backup on server start has failed

**[TW-76715](https://youtrack.jetbrains.com/issue/TW-76715/No-way-to-get-parameter-value-vcsrooturl-without-VCSrootID-if-it-was-inherited-from-the-template-without-VCS-root)** — No way to get parameter value "vcsroot.url" without VCS_root_ID if it was inherited from the template without VCS root

**[TW-77457](https://youtrack.jetbrains.com/issue/TW-77457/Its-not-possible-to-change-the-main-node-responsibility-on-the-nodes-configuration-page-if-the-main-node-was-stopped-gracefully)** — It's not possible to change the main node responsibility on the nodes configuration page if the main node was stopped gracefully

**[TW-77157](https://youtrack.jetbrains.com/issue/TW-77157/Parallel-tests-feature-ignores-new-internal-properties)** — Parallel tests feature ignores new internal properties

**[TW-50524](https://youtrack.jetbrains.com/issue/TW-50524/ERROR-database-is-not-accepting-commands-to-avoid-wraparound-data-loss-in-database-teamcitydev-with-PostgreSQL)** — ERROR: database is not accepting commands to avoid wraparound data loss in database "teamcity_dev" with PostgreSQL

**[TW-77105](https://youtrack.jetbrains.com/issue/TW-77105/No-way-to-specify-timeout-for-build-approval-feature-Timeout-should-be-a-number-error)** — No way to specify a timeout for build approval feature ("Timeout should be a number" error)

**[TW-76018](https://youtrack.jetbrains.com/issue/TW-76018/Git-Diagnostic-page-improve-message-for-cases-when-git-isnt-installed)** — Git Diagnostic page: improve message for cases when git isn't installed

**[TW-75757](https://youtrack.jetbrains.com/issue/TW-75757/Build-approval-better-handling-for-wrong-approval-rules)** — Build approval: better handling for the wrong "approval rules"

**[TW-76467](https://youtrack.jetbrains.com/issue/TW-76467/VCS-Labeling-cant-overwrite-an-existing-tag-using-native-git)** — VCS Labeling can't overwrite an existing tag using native git

**[TW-77338](https://youtrack.jetbrains.com/issue/TW-77338/A-delay-with-processing-of-repositoryStateChangedbeforeRepositoryStateUpdate-events-can-lead-to-new-branches-not-being-shown-in)** — A delay with the processing of repositoryStateChanged/beforeRepositoryStateUpdate events can lead to new branches not being shown in the UI of this node (multi-node setup)

**[TW-77134](https://youtrack.jetbrains.com/issue/TW-77134/svnssh-connections-on-the-TeamCity-server-may-generate-threads-overtime)** — SVN+SSH connections on the TeamCity server may generate threads overtime

**[TW-47685](https://youtrack.jetbrains.com/issue/TW-47685/LDAP-sync-can-hang-for-indefinite-time-always-fetching-data-from-LDAP-network-connection-hanging-case)** — LDAP sync can hang for an indefinite time: always "fetching data from LDAP" (network connection hanging case)

**[TW-76794](https://youtrack.jetbrains.com/issue/TW-76794/StartStop-cloud-instance-is-not-authorized-if-started-immediately-after-stopping)** — Start/Stop cloud instance is not authorized if started immediately after stopping

**[TW-77077](https://youtrack.jetbrains.com/issue/TW-77077/BuildLogClosedException-on-main-node-from-BuildFailureOnMetricCondition)** — BuildLogClosedException on main node from BuildFailureOnMetricCondition

**[TW-77104](https://youtrack.jetbrains.com/issue/TW-77104/Pull-Requests-build-feature-logs-errors-with-debug-level-except-for-the-first-time-an-error-is-reported-for-a-build-type)** — Pull Requests build feature logs errors with debug level except for the first time an error is reported for a building type

**[TW-77082](https://youtrack.jetbrains.com/issue/TW-77082/golang-feature-ignores-subtests-without-Package)** — Golang feature ignores subtests without "Package."


### Performance Problem

**[TW-77350](https://youtrack.jetbrains.com/issue/TW-77350)** — Too much memory occupied by IdOrderedBuildTypeComparator

**[TW-76238](https://youtrack.jetbrains.com/issue/TW-76238/High-CPU-usage-if-code-cache-is-filled-in)** — High CPU usage if the code cache is filled in

**[TW-77360](https://youtrack.jetbrains.com/issue/TW-77360/Cleanup-is-delayed-by-a-sleep-introduced-in-ExternalBuildArtifactsgetExternalArtifactsInfo)** — Cleanup is delayed by a sleep introduced in ExternalBuildArtifacts.getExternalArtifactsInfo


### Security

1 security issue has been fixed.


