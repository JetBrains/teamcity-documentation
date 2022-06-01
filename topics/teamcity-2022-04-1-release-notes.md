[//]: # (title: TeamCity 2022.04.1 Release Notes)
[//]: # (auxiliary-id: TeamCity 2022.04.1 Release Notes)

__Build: 108575__

__1 June 2022__


### Feature

**[TW-76346](https://youtrack.jetbrains.com/issue/TW-76346/Include-buildId-of-the-scheduled-personal-build-when-running-Perforce-build-on-shelve-via-REST-API)** — Include buildId of the scheduled personal build when running Perforce build on shelve via REST API

### Usability Problem

**[TW-75809](https://youtrack.jetbrains.com/issue/TW-75809/Add-link-to-the-parent-build-it-belongs-to-Build-Page)** — Add link to the parent build it belongs to (Build Page)

**[TW-76121](https://youtrack.jetbrains.com/issue/TW-76121/Versioned-settings-are-globally-disabled-after-upgrade-to-202204-even-though-there-were-no-changes-in-XML-files)** — Versioned settings are globally disabled after upgrade to 2022.04 even though there were no changes in XML files

**[TW-75254](https://youtrack.jetbrains.com/issue/TW-75254/Commit-Status-Publisher-health-report-doesnt-detect-when-no-VCS-roots-are-defined-in-a-build-configuration)** — Commit Status Publisher health report doesn't detect when no VCS roots are defined in a build configuration

### Bug

**[TW-74583](https://youtrack.jetbrains.com/issue/TW-74583/Long-checking-for-changes-time-of-VCS-root)** — Long "checking for changes" time of VCS root

**[TW-76189](https://youtrack.jetbrains.com/issue/TW-76189/Tests-error-with-The-argument-noconsolelogger-is-invalid-with-NET-SDK-60300-on-Windows-agents)** — Tests error with "The argument /noconsolelogger is invalid" with .NET SDK 6.0.300 (on Windows agents)

**[TW-76185](https://youtrack.jetbrains.com/issue/TW-76185/Latest-Windows-TeamCity-docker-images-missing)** — Latest Windows TeamCity docker images missing

**[TW-76139](https://youtrack.jetbrains.com/issue/TW-76139/Unable-to-assign-agents-or-projects-to-an-Agent-Pool-Classic-UI)** — Unable to assign agents or projects to an Agent Pool (Classic UI)

**[TW-76171](https://youtrack.jetbrains.com/issue/TW-76171/Error-Cannot-read-properties-of-undefined-reading-typeId-in-attempt-to-open-disconnected-cloud-agent)** — Error "Cannot read properties of undefined (reading 'typeId')" in attempt to open disconnected cloud agent

**[TW-76316](https://youtrack.jetbrains.com/issue/TW-76316/JS-error-on-attempt-to-select-a-build-configuration-in-Watched-build-section-of-a-schedule-trigger)** — JS error on attempt to select a build configuration in "Watched build" section of a schedule trigger

**[TW-76056](https://youtrack.jetbrains.com/issue/TW-76056/Changes-page-Changes-in-the-VCS-root-that-belongs-to-parent-project-are-not-displayed-when-changes-list-is-filtered-by-project)** — Changes page. Changes in the VCS root that belongs to parent project are not displayed when changes list is filtered by project.

**[TW-76255](https://youtrack.jetbrains.com/issue/TW-76255/Subversion-ssl-error-after-update-to-202204-when-Java-11015-is-used)** — Subversion ssl error after update to 2022.04 (when Java 11.0.15 is used)

**[TW-76304](https://youtrack.jetbrains.com/issue/TW-76304/Search-result-wtih-builds-with-no-access)** — Search result wtih builds with no access

**[TW-76218](https://youtrack.jetbrains.com/issue/TW-76218/Commit-versioned-settings-fails-when-the-server-is-switched-to-native-Git)** — Commit versioned settings fails when the server is switched to native Git

**[TW-76201](https://youtrack.jetbrains.com/issue/TW-76201/Build-is-hanging-for-a-long-period-of-time-during-the-artifacts-publishing-phase-if-artifacts-size-limit-is-exceeded)** — Build is hanging for a long period of time during the artifacts publishing phase if artifacts size limit is exceeded

**[TW-76340](https://youtrack.jetbrains.com/issue/TW-76340/Do-not-fail-a-build-due-to-timeout-errors)** — Do not fail a build due to timeout errors

**[TW-74795](https://youtrack.jetbrains.com/issue/TW-74795/Agents-pages-the-agent-being-removed-in-the-past-disappears-from-the-UI)** — Agents' pages: the agent being removed in the past disappears from the UI

**[TW-76100](https://youtrack.jetbrains.com/issue/TW-76100/Not-all-of-the-paths-in-the-unified-diff-patch-in-Git-format-are-modified-properly-before-sending-the-patch-to-an-agent)** — Not all of the paths in the unified diff patch in Git format are modified properly before sending the patch to an agent

**[TW-76097](https://youtrack.jetbrains.com/issue/TW-76097/Cannot-enable-Native-Git-operations)** — Cannot enable Native Git operations

**[TW-75905](https://youtrack.jetbrains.com/issue/TW-75905/Information-about-build-statuses-for-pull-request-branches-isnt-displayed-in-the-Azure-Commits-and-Branches-tabs)** — Information about build statuses for pull request branches isn't displayed in the Azure Commits and Branches tabs

**[TW-76025](https://youtrack.jetbrains.com/issue/TW-76025/Parallel-tests-wrong-tests-count-is-reported-in-the-build-status)** — Parallel tests: wrong tests count is reported in the build status

**[TW-76207](https://youtrack.jetbrains.com/issue/TW-76207/Cant-close-the-build-log-popup)** — Can't close the build log popup

**[TW-74466](https://youtrack.jetbrains.com/issue/TW-74466/JDKx64-parameters-is-reported-for-aarch64-Java-installed-on-agents)** — JDK_*_x64 parameters is reported for aarch64 Java installed on agents

**[TW-75150](https://youtrack.jetbrains.com/issue/TW-75150/Queued-build-status-is-sent-to-a-commit-outside-of-checkout-rules)** — Queued build status is sent to a commit outside of checkout rules

**[TW-75734](https://youtrack.jetbrains.com/issue/TW-75734/PerforceSVN-build-does-not-update-changes-after-building-not-the-latest-changelist)** — Perforce/SVN build does not update changes after building not the latest changelist

**[TW-74863](https://youtrack.jetbrains.com/issue/TW-74863/Failed-to-send-artifact-upload-information-to-digest-consumer-javalangNullPointerException)** — Failed to send artifact upload information to digest consumer: java.lang.NullPointerException

**[TW-76169](https://youtrack.jetbrains.com/issue/TW-76169/NullPointerException-in-ArtifactsCachePublisherImplpublishFile)** — NullPointerException in ArtifactsCachePublisherImpl.publishFile

**[TW-64935](https://youtrack.jetbrains.com/issue/TW-64935/Composite-builds-artifacts-are-missing-still-present-in-dependent-builds-build-configuration-for-one-artifact-dependency-no)** — Composite builds artifacts are missing, still present in dependent builds (build configuration for one artifact dependency no longer exists)

**[TW-76119](https://youtrack.jetbrains.com/issue/TW-76119/Artifact-upload-to-Backblaze-B2s-S3-compatible-API-fails-with-TeamCity-202204)** — Artifact upload to Backblaze B2's S3-compatible API fails with TeamCity 2022.04

**[TW-75986](https://youtrack.jetbrains.com/issue/TW-75986/Display-all-batches-statuses-in-build-log-for-parallel-tests)** — Display all batches statuses in build log for parallel tests.

**[TW-75975](https://youtrack.jetbrains.com/issue/TW-75975/Build-log-for-parallel-tests-Inconsistent-presentation-and-behavior-if-multiple-batches-are-used-in-the-build)** — Build log for parallel tests. Inconsistent presentation and behavior if multiple batches are used in the build.

**[TW-75509](https://youtrack.jetbrains.com/issue/TW-75509/Cant-switch-off-the-option-Show-failed-on-statistics-page)** — Can't switch off the option "Show failed" on statistics page

**[TW-76098](https://youtrack.jetbrains.com/issue/TW-76098/teamcityagentname-parameter-is-not-updated-in-DB-for-cloud-agent-types)** — teamcity.agent.name parameter is not updated in DB for cloud agent types

**[TW-75841](https://youtrack.jetbrains.com/issue/TW-75841/Excessive-logging-for-DSL-dependencies-resolving-in-teamcity-serverlog)** — Excessive logging for DSL dependencies resolving in teamcity-server.log

**[TW-74327](https://youtrack.jetbrains.com/issue/TW-74327/Refresh-pull-request-button-does-not-work-for-second-Pull-Request)** — Refresh pull request button does not work for second Pull Request

**[TW-75961](https://youtrack.jetbrains.com/issue/TW-75961/Config-persisting-tasks-processing-can-be-stuck-if-there-is-a-task-in-preparation-state-which-is-never-marked-as-ready)** — Config persisting tasks processing can be stuck if there is a task in preparation state which is never marked as ready

**[TW-76088](https://youtrack.jetbrains.com/issue/TW-76088/Gradle-plugin-cannot-be-updated-without-restarting-the-server)** — Gradle plugin cannot be updated without restarting the server.

**[TW-76030](https://youtrack.jetbrains.com/issue/TW-76030/parallel-tests-tests-arent-launched-when-triggered-from-a-secondary-node)** — parallel tests: tests aren't launched when triggered from a secondary node

**[TW-75991](https://youtrack.jetbrains.com/issue/TW-75991/S3-migration-tool-copyright-should-be-printed-right-under-an-executed-command)** — S3 migration tool: copyright should be printed right under an executed command

**[TW-75987](https://youtrack.jetbrains.com/issue/TW-75987/Current-build-is-not-highlighted-when-it-is-selected-in-Build-Log-Dependencies-Navigation-bar)** — Current build is not highlighted when it is selected in Build Log Dependencies Navigation bar.

**[TW-74382](https://youtrack.jetbrains.com/issue/TW-74382/No-way-to-view-artifacts-in-archive-uploaded-using-CloudFront)** — No way to view artifacts in archive uploaded using CloudFront

### Performance Problem

**[TW-76305](https://youtrack.jetbrains.com/issue/TW-76305/TeamCity-tries-to-invoke-loading-changes-even-if-repository-state-did-not-change-if-Pull-Requests-build-feature-is-configured)** — TeamCity tries to invoke loading changes even if repository state did not change if Pull Requests build feature is configured and some pull request branches are present

**[TW-76216](https://youtrack.jetbrains.com/issue/TW-76216/Query-used-for-assignable-projects-dialog-on-a-Agent-pool-projects-tab-is-very-slow)** — Query used for assignable projects dialog on a Agent pool -> projects tab is very slow

**[TW-76167](https://youtrack.jetbrains.com/issue/TW-76167/Cleanup-has-been-running-for-6-days-MySQL-57-delete-with-sub-query)** — Cleanup has been running for 6 days (MySQL 5.7, delete with sub query)

**[TW-76236](https://youtrack.jetbrains.com/issue/TW-76236/High-memory-usage-during-cleanup-because-of-re-creating-pooled-groups-of-build-configurations)** — High memory usage during cleanup because of re-creating pooled groups of build configurations

**[TW-76204](https://youtrack.jetbrains.com/issue/TW-76204/Browser-hangs-when-Root-project-is-selected-while-editing-notification-rules)** — Browser hangs when Root project is selected while editing notification rules

**[TW-76070](https://youtrack.jetbrains.com/issue/TW-76070/Slow-processing-of-repositoryStateChanged-event-if-it-happens-for-a-VCS-root-instance-used-in-the-versioned-settings-of-a-large)** — Slow processing of 'repositoryStateChanged' event if it happens for a VCS root instance used in the versioned settings of a large project (with lots of subprojects)

**[TW-76122](https://youtrack.jetbrains.com/issue/TW-76122/Endless-loop-on-attempt-to-create-a-flow-aware-index-file-for-a-build-log)** — Endless loop on attempt to create a flow aware index file for a build log

**[TW-76095](https://youtrack.jetbrains.com/issue/TW-76095/High-memory-usage-in-case-of-a-REST-API-query-filtering-resulting-properties-of-many-builds)** — High memory usage in case of a REST API query filtering resulting properties of many builds

**[TW-76068](https://youtrack.jetbrains.com/issue/TW-76068/Slow-persisting-of-newly-detected-VCS-commits-for-a-non-DAG-VCS-repository)** — Slow persisting of newly detected VCS commits for a non DAG VCS repository

**[TW-76031](https://youtrack.jetbrains.com/issue/TW-76031/Main-node-becomes-unresponsible-after-a-restart-of-one-of-the-secondary-nodes)** — Main node becomes unresponsible after a restart of one of the secondary nodes

### Task

**[TW-76017](https://youtrack.jetbrains.com/issue/TW-76017/Publish-docker-images-with-Windows-2004-base-image)** — Publish docker images with Windows 2004 base image

**[TW-75998](https://youtrack.jetbrains.com/issue/TW-75998/Update-the-list-of-downloadable-IDEA-tool-to-include-latest-versions-20221)** — Update the list of downloadable IDEA tool to include latest versions (2022.1)

**[TW-76052](https://youtrack.jetbrains.com/issue/TW-76052/Build-approval-publish-a-list-of-approvers-as-a-system-build-parameter)** — Build approval: publish a list of approvers as a system build parameter

**[TW-75702](https://youtrack.jetbrains.com/issue/TW-75702/Handle-SBuildServerListenerchangesLoadedSBuildQueued-event-in-Commit-Status-Publisher)** — Handle SBuildServerListener::changesLoaded(SBuildQueued) event in Commit Status Publisher


