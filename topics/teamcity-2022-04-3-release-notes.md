[//]: # (title: TeamCity 2022.04.3 Release Notes)
[//]: # (auxiliary-id: TeamCity 2022.04.3 Release Notes)

__Build: 108706__

__10 August 2022__


### Feature

**[TW-75796](https://youtrack.jetbrains.com/issue/TW-75796/P4-is-unable-to-establish-trust-after-a-certificate-update)** — P4 is unable to establish "trust" after a certificate update

### Bug

**[TW-76324](https://youtrack.jetbrains.com/issue/TW-76324/With-the-new-Parallel-Tests-Feature-always-one-build-is-failing-the-first-with-message-Cannot-run-process-file-not-found)** — With the new "Parallel Tests"-Feature always one build is failing (the first) with message: Cannot run process (file not found): C:\Program Files\dotnet\dotnet.exe

**[TW-76002](https://youtrack.jetbrains.com/issue/TW-76002/Use-successful-batches-when-running-a-new-build-with-Parallel-Tests-feature-on-a-previously-built-revision)** — Use successful batches when running a new build with Parallel Tests feature on a previously built revision

**[TW-75686](https://youtrack.jetbrains.com/issue/TW-75686/Builds-with-Parallel-tests-execution-build-feature-are-not-being-reused)** — Builds with "Parallel tests execution" build feature are not being reused

**[TW-76853](https://youtrack.jetbrains.com/issue/TW-76853/Re-run-parallel-tests-with-rebuild-all-failed-builds-rebuilds-successful-builds)** — Re-run parallel tests with "rebuild all failed builds" rebuilds successful builds

**[TW-77084](https://youtrack.jetbrains.com/issue/TW-77084/Change-of-the-project-id-via-Bulk-edit-IDs-dialog-leaves-duplicate-files-in-the-repository)** — Change of the project id via Bulk edit IDs dialog leaves duplicate files in the repository

**[TW-76993](https://youtrack.jetbrains.com/issue/TW-76993/TeamCity-Gitlab-Commit-Status-Publisher-Sometimes-Sends-Messages-Out-of-Order)** — TeamCity Gitlab Commit Status Publisher Sometimes Sends Messages Out of Order

**[TW-76626](https://youtrack.jetbrains.com/issue/TW-76626/Commit-status-is-not-published-when-build-is-started)** — Commit status is not published when build is started

**[TW-77058](https://youtrack.jetbrains.com/issue/TW-77058/maintainDBsh-scripts-throwing-exceptions)** — maintainDB.sh scripts throwing exceptions

**[TW-76545](https://youtrack.jetbrains.com/issue/TW-76545/Composite-build-does-not-update-artifact-dependency-references-in-case-when-one-of-its-dependencies-was-restarted)** — Composite build does not update artifact dependency references in case when one of its dependencies was restarted

**[TW-76304](https://youtrack.jetbrains.com/issue/TW-76304/Search-result-wtih-builds-with-no-access)** — Search result wtih builds with no access

**[TW-76837](https://youtrack.jetbrains.com/issue/TW-76837/Failed-to-copy-JDBC-drivers-when-doing-a-refresh-drivers-during-initial-setup)** — Failed to copy JDBC drivers when doing a refresh drivers during initial setup

**[TW-77046](https://youtrack.jetbrains.com/issue/TW-77046/Uncaught-ADE-when-retrieving-build-type-of-the-build-promotion)** — Uncaught ADE when retrieving build type of the build promotion

**[TW-76258](https://youtrack.jetbrains.com/issue/TW-76258/Commit-Status-Publisher-may-be-sending-too-many-requests-to-GitHub-and-possibly-other-VCS-hostings)** — Commit Status Publisher may be sending too many requests to GitHub (and possibly other VCS hostings)

**[TW-76770](https://youtrack.jetbrains.com/issue/TW-76770/teamcitycommitStatusPublishergithubContext-incorrectly-resolves-buildnumber-parameter)** — teamcity.commitStatusPublisher.githubContext incorrectly resolves build.number parameter

**[TW-76338](https://youtrack.jetbrains.com/issue/TW-76338/Duplication-of-the-commit-status-on-GitHub)** — Duplication of the commit status on GitHub

**[TW-76570](https://youtrack.jetbrains.com/issue/TW-76570/Commit-Status-Publisher-may-send-excessive-status-updates-for-builds-in-build-chains)** — Commit Status Publisher may send excessive status updates for builds in build chains

**[TW-76753](https://youtrack.jetbrains.com/issue/TW-76753/Bad-alignment-on-the-Changes-page-in-the-Classic-UI-Author-field)** — Bad alignment on the Changes page in the Classic UI (Author field)

**[TW-76574](https://youtrack.jetbrains.com/issue/TW-76574/Commit-status-publisher-for-Perforce-Swarm-posts-to-all-reviews-when-a-build-triggers-without-ShelvePersonal-build)** — Commit Status Publisher for Perforce Swarm posts to all reviews when a build triggers without Shelve/Personal build

**[TW-76899](https://youtrack.jetbrains.com/issue/TW-76899/Non-daemon-thread-started-by-the-search-plugin-prevents-proper-server-shutdown)** — Non daemon thread started by the search plugin prevents proper server shutdown

**[TW-76799](https://youtrack.jetbrains.com/issue/TW-76799/Project-added-to-Projects-IDs-after-upgrading-from-20202-to-202204)** — "_Project" added to Projects IDs after upgrading from 2020.2 to 2022.04

**[TW-76738](https://youtrack.jetbrains.com/issue/TW-76738/Perforce-Shelve-trigger-skips-some-new-changelists-if-there-are-hundreds-of-them)** — Perforce Shelve trigger skips some new changelists, if there are hundreds of them

**[TW-76812](https://youtrack.jetbrains.com/issue/TW-76812/Re-run-does-not-respect-selected-artifact-dependency-build)** — Re-run does not respect selected artifact dependency build

**[TW-74015](https://youtrack.jetbrains.com/issue/TW-74015/Select-a-build-configuration-popup-on-Single-change-page-may-be-misplaced)** — "Select a build configuration" popup on Single change page may be misplaced

**[TW-76735](https://youtrack.jetbrains.com/issue/TW-76735/Perforce-Shelve-trigger-triggers-later-changelists-first)** — Perforce Shelve trigger triggers later changelists first

**[TW-76679](https://youtrack.jetbrains.com/issue/TW-76679/perforce-client-cleanup-errors-on-large-stdout)** — perforce client cleanup errors on large stdout

**[TW-76381](https://youtrack.jetbrains.com/issue/TW-76381/)** — Parallel test error Stack overflow

### Performance Problem

**[TW-67910](https://youtrack.jetbrains.com/issue/TW-67910/Slowdown-in-TestName2IndexImplgetTestNames-caused-hanging-UI-and-finishing-of-builds)** — Slowdown in TestName2IndexImpl.getTestNames() caused hanging UI and finishing of builds

**[TW-73168](https://youtrack.jetbrains.com/issue/TW-73168/Single-change-page-does-not-respondhangs-when-loading-buildsproblems)** — Single change page does not respond/hangs when loading builds/problems

**[TW-76512](https://youtrack.jetbrains.com/issue/TW-76512/More-than-expected-CPU-usage-possibly-because-of-BuildPTRIndexer-threads)** — More than expected CPU usage possibly because of BuildPTRIndexer threads

**[TW-76517](https://youtrack.jetbrains.com/issue/TW-76517/CommitStatusPublisherListenerlambdainitModificationsProcessing-does-not-finish-within-14-hours)** — CommitStatusPublisherListener.lambda$initModificationsProcessing does not finish within 14 hours

**[TW-76802](https://youtrack.jetbrains.com/issue/TW-76802/Slow-BuildQueuePriorityOrderingaddBuilds-in-case-of-a-large-build-queue)** — Slow BuildQueuePriorityOrdering.addBuilds in case of a large build queue

### Task

**[TW-76053](https://youtrack.jetbrains.com/issue/TW-76053/Build-approval-collect-changes-as-soon-as-the-build-is-enqueued)** — Build approval: collect changes as soon as the build is enqueued
