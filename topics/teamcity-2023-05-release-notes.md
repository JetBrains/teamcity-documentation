[//]: # (title: TeamCity 2023.05 Release Notes)
[//]: # (auxiliary-id: TeamCity 2023.05 Release Notes)

__Build: 129069__

__12 May 2023__

### Feature

**[TW-79970](https://youtrack.jetbrains.com/issue/TW-79970/Add-a-simple-way-for-a-user-to-call-for-the-support-for-an-enterprise-license)** — Add a simple way for a user to call for the support for an enterprise license

**[TW-80388](https://youtrack.jetbrains.com/issue/TW-80388/Allow-Project-Developer-role-to-view-buildSettings.xml-artifact)** — Allow Project Developer role to view buildSettings.xml artifact

**[TW-11382](https://youtrack.jetbrains.com/issue/TW-11382/Include-empty-directories-into-packed-artifacts)** — Include empty directories into packed artifacts

**[TW-79426](https://youtrack.jetbrains.com/issue/TW-79426/Add-more-details-to-a-build-log-about-upload-download-artifacts)** — Add more details to a build log about upload/download artifacts

### Bug

**[TW-80943](https://youtrack.jetbrains.com/issue/TW-80943/Copy-project-action-doesnt-work-if-AWS-connection-is-configred-in-project-tree)** — Copy project action doesn't work if AWS connection is configred in project tree

**[TW-81170](https://youtrack.jetbrains.com/issue/TW-81170/Confusing-message-on-the-nodes-configuration-page-when-the-main-node-is-down)** — Confusing message on the nodes configuration page when the main node is down

**[TW-81172](https://youtrack.jetbrains.com/issue/TW-81172/Nodes-configuration-the-Main-node-checkbox-doesnt-remove-the-responsibility-from-the-previous-node)** — Nodes configuration: the "Main node" checkbox doesn't remove the responsibility from the previous node

**[TW-80698](https://youtrack.jetbrains.com/issue/TW-80698/How-to-set-timeout-for-auto-generated-build-configs-with-Parallel-tests-build-feature)** — How to set timeout for auto-generated build configs with Parallel tests build feature?

**[TW-81076](https://youtrack.jetbrains.com/issue/TW-81076/Bitbucket-Cloud-connections-arent-shown-in-commit-status-publisher-settings)** — Bitbucket Cloud connections aren't shown in commit status publisher settings

**[TW-80872](https://youtrack.jetbrains.com/issue/TW-80872/Dark-Theme-sync-iframe-and-snippet-theme-with-the-global-theme)** — Dark Theme: sync iframe and snippet theme with the global theme

**[TW-80949](https://youtrack.jetbrains.com/issue/TW-80949/Update-Git-version-within-TeamCity-Docker-Images-2.40.0-2.40.1)** — Update Git version within TeamCity Docker Images: 2.40.0 -> 2.40.1

**[TW-75155](https://youtrack.jetbrains.com/issue/TW-75155/Concurrency-issue-on-attempt-to-manually-remove-a-build-from-the-queue-which-is-already-optimized-to-some-other-build)** — Concurrency issue on attempt to manually remove a build from the queue which is already optimized to some other build

**[TW-81117](https://youtrack.jetbrains.com/issue/TW-81117/Versioned-Settings-Import-decision-parameter-is-not-set-with-REST)** — Versioned Settings Import decision parameter is not set with REST

**[TW-81035](https://youtrack.jetbrains.com/issue/TW-81035/TeamCity-should-provide-P4CLIENT-P4HOST-P4USER-environment-variables-in-bootstrap-steps-of-builds-with-Perforce-VCS)** — TeamCity should provide P4CLIENT, P4HOST, P4USER environment variables in bootstrap steps of builds with Perforce VCS

**[TW-80798](https://youtrack.jetbrains.com/issue/TW-80798/IntelliJ-IDEA-Inspections-runner-adds-incorrect-Xbootclasspath-a-option-to-the-command-line-of-the-IDE-process)** — IntelliJ IDEA Inspections runner adds incorrect -Xbootclasspath/a option to the command line of the IDE process

**[TW-80229](https://youtrack.jetbrains.com/issue/TW-80229/TeamCity-does-not-continue-startup-if-postgres-database-has-been-started-while-the-server-startup-was-in-progress)** — TeamCity does not continue startup if postgres database has been started while the server startup was in progress

**[TW-80278](https://youtrack.jetbrains.com/issue/TW-80278/REST-API-BuildType-compatibleCloudImages-returns-images-from-incompatible-pools)** — REST API: BuildType compatibleCloudImages returns images from incompatible pools

**[TW-80987](https://youtrack.jetbrains.com/issue/TW-80987/teamcity-vcs.log-files-are-no-longer-available-on-the-build-agents-probably-since-migration-to-log4j2)** — teamcity-vcs.log files are no longer available on the build agents (probably since migration to log4j2)

**[TW-79351](https://youtrack.jetbrains.com/issue/TW-79351/Apply-the-same-revision-clarification-logic-when-a-revision-is-computed-by-a-finish-build-trigger-or-vcs-trigger-with-quiet)** — Apply the same revision clarification logic when a revision is computed by a finish build trigger or vcs trigger with quiet period

**[TW-81019](https://youtrack.jetbrains.com/issue/TW-81019/Revision-affected-by-checkout-rules-cannot-be-found-if-fetch-for-the-start-revision-fails)** — Revision affected by checkout rules cannot be found if fetch for the start revision fails

**[TW-80253](https://youtrack.jetbrains.com/issue/TW-80253/Default-Credentials-Provider-Chain-The-security-token-included-in-the-request-is-expired)** — Default Credentials Provider Chain: The security token included in the request is expired

**[TW-80983](https://youtrack.jetbrains.com/issue/TW-80983/Return-init-block-for-S3StorageSettings.xml)** — Return &lt;init> block for S3StorageSettings.xml

**[TW-78091](https://youtrack.jetbrains.com/issue/TW-78091/Error-Main-node-responsibilities-cannot-be-changed-in-attempt-to-set-CANCHECKFORCHANGES-for-the-main-node-using-rest-api)** — Error "Main node responsibilities cannot be changed" in attempt to set CAN_CHECK_FOR_CHANGES for the main node using rest api

**[TW-77453](https://youtrack.jetbrains.com/issue/TW-77453/Allow-browse-agents-working-directory-even-with-UI-edit-disabled)** — Allow browse agents working directory even with UI edit disabled

**[TW-80374](https://youtrack.jetbrains.com/issue/TW-80374/IdeaRunner-unable-to-use-JRT-protocol-with-JDK-Jar-Files-Patterns)** — IdeaRunner: unable to use JRT protocol with "JDK Jar Files Patterns"

**[TW-80454](https://youtrack.jetbrains.com/issue/TW-80454/Parallel-tests-batch-runs-no-tests-if-it-was-automatically-restarted-after-a-canceled-build)** — Parallel tests batch runs no tests, if it was automatically restarted after a canceled build

**[TW-75567](https://youtrack.jetbrains.com/issue/TW-75567/Builds-no-longer-run-after-recent-DST-time-change-Fail-to-peek-column-10-with-type-java.sql.Timestamp)** — Builds no longer run after recent DST time change (Fail to peek column 10 with type java.sql.Timestamp)

**[TW-80404](https://youtrack.jetbrains.com/issue/TW-80404/S3-Artifact-upload-Request-for-pre-signed-URLs-time-out-after-60-seconds)** — S3 Artifact upload - Request for pre-signed URLs time-out after 60 seconds

**[TW-80525](https://youtrack.jetbrains.com/issue/TW-80525/Unexpected-switch-from-All-branches-to-the-default-branch-on-the-projects-page)** — Unexpected switch from All branches to the default branch on the projects page

**[TW-80814](https://youtrack.jetbrains.com/issue/TW-80814/Something-went-wrong-appears-when-trying-to-see-changes-of-a-build)** — Something went wrong appears when trying to see changes of a build

**[TW-44987](https://youtrack.jetbrains.com/issue/TW-44987/Support-create-project-build-configuration-VCS-root-from-URL-for-Perforce-address-including-sslhostport-URLs)** — Support create project/build configuration/VCS root from URL for Perforce address, including ssl:host:port URLs

**[TW-80391](https://youtrack.jetbrains.com/issue/TW-80391/AdHoc-notifications-what-are-the-default-value-for-Allowed-hostnames-setting)** — AdHoc notifications: what are the default value for "Allowed hostnames" setting?

**[TW-80757](https://youtrack.jetbrains.com/issue/TW-80757/Exception-on-the-buildserver-removed-build-configuration-not-expected-in-parallels-test-code)** — Exception on the buildserver (removed build configuration not expected in parallels test code)

**[TW-80508](https://youtrack.jetbrains.com/issue/TW-80508/Cloud-agents-are-terminated-despite-Maintenance-mode-after-idle-time-10-minutes)** — Cloud agents are terminated despite Maintenance mode after idle time + 10 minutes

**[TW-79365](https://youtrack.jetbrains.com/issue/TW-79365/Commit-Status-Publisher-fails-to-publish-status-to-Azure-DevOps-pull-requests)** — Commit Status Publisher fails to publish status to Azure DevOps pull requests

**[TW-80579](https://youtrack.jetbrains.com/issue/TW-80579/Incorrect-branch-parameter-included-in-the-breadcrumb-URL-of-the-build-path)** — Incorrect branch parameter included in the breadcrumb URL of the build path

**[TW-80591](https://youtrack.jetbrains.com/issue/TW-80591/Show-all-build-problems-button-on-page-Build-overview-doesnt-work)** — "Show all" build problems button on page Build overview doesn't work

**[TW-80069](https://youtrack.jetbrains.com/issue/TW-80069/Build-configuration-filter-unavailable-on-the-Build-History-tab-of-the-agent-page)** — Build configuration filter unavailable on the Build History tab of the agent page

**[TW-79117](https://youtrack.jetbrains.com/issue/TW-79117/Agent-does-not-recognize-docker-compose-installation)** — Agent does not recognize docker compose installation

**[TW-80469](https://youtrack.jetbrains.com/issue/TW-80469/S3-Infinite-loop-in-fetch-resources-when-the-AWS-key-expired)** — [S3] Infinite loop in fetch resources when the AWS key expired

**[TW-55923](https://youtrack.jetbrains.com/issue/TW-55923/reverse.dep.-parameter-with-type-password-does-not-push-parameter-to-downstream-builds-properly)** — reverse.dep. parameter with type password does not push parameter to downstream builds properly

**[TW-79461](https://youtrack.jetbrains.com/issue/TW-79461/Error-sending-build-log-to-secondary-node)** — Error sending build log to secondary node

**[TW-66091](https://youtrack.jetbrains.com/issue/TW-66091/Build-log-Loading...-note-when-Errors-mode-is-selected)** — Build log: 'Loading...' note when 'Errors' mode is selected

**[TW-80419](https://youtrack.jetbrains.com/issue/TW-80419/Dependencies-chains-crashed-for-chains-with-70-builds)** — Dependencies chains crashed for chains with 70+ builds

**[TW-74031](https://youtrack.jetbrains.com/issue/TW-74031/Provide-an-ability-to-apply-changed-parameters-for-Amazon-EC2-Cloud-profile-without-Check-connection-Fetch-parameter-values)** — Provide an ability to apply changed parameters for Amazon EC2 Cloud profile without Check connection / Fetch parameter values

**[TW-76404](https://youtrack.jetbrains.com/issue/TW-76404/Merge-launch-types-shared-AMI-and-private-AMI-to-the-AMI-one.)** — Merge launch types "shared AMI" and "private AMI" to the "AMI" one.

**[TW-77768](https://youtrack.jetbrains.com/issue/TW-77768/Take-Parallel-tests-build-feature-settings-from-versioned-settings-feature-branches)** — Take Parallel tests build feature settings from versioned settings feature branches

**[TW-80021](https://youtrack.jetbrains.com/issue/TW-80021/Gradle-compilation-error-is-not-reported-as-a-build-problem-for-Kotlin-projects)** — Gradle compilation error is not reported as a build problem for Kotlin projects

**[TW-80301](https://youtrack.jetbrains.com/issue/TW-80301/Attempt-to-reload-REST-API-plugin-in-runtime-leads-to-endless-errors-in-the-teamcity-server.log)** — Attempt to reload REST API plugin in runtime leads to endless errors in the teamcity-server.log

**[TW-80416](https://youtrack.jetbrains.com/issue/TW-80416/Ensure-cloud-agents-are-removed-by-the-end-of-agentUnregistered)** — Ensure cloud agents are removed by the end of `agentUnregistered`

**[TW-80367](https://youtrack.jetbrains.com/issue/TW-80367/Wrong-changes-order-for-merged-changes-on-Versioned-Settings-Change-Log-page)** — Wrong changes order for merged changes on Versioned Settings Change Log page

**[TW-80399](https://youtrack.jetbrains.com/issue/TW-80399/Build-Configuration-Triggers-do-not-retain-custom-parameter-values-if-the-default-value-is-set)** — Build Configuration Triggers do not retain custom parameter values if the default value is set

**[TW-79361](https://youtrack.jetbrains.com/issue/TW-79361/Improve-tokens-information-for-the-case-when-user-doesnt-have-access-to-Connection-or-connection-was-deleted)** — Improve token's information for the case when user doesn't have access to Connection or connection was deleted

**[TW-79946](https://youtrack.jetbrains.com/issue/TW-79946/Improve-tokens-information-for-the-case-when-user-created-token-was-deleted)** — Improve token's information for the case when user created token was deleted

**[TW-79947](https://youtrack.jetbrains.com/issue/TW-79947/Small-improvements-for-Token-information-in-VCS-root-settings)** — Small improvements for "Token" information in VCS root settings

**[TW-79952](https://youtrack.jetbrains.com/issue/TW-79952/Dont-show-tokenid-as-hint-for-Token-field-in-VCS-root-settings)** — Don't show token_id as hint for Token field in VCS root settings

**[TW-79953](https://youtrack.jetbrains.com/issue/TW-79953/Information-about-token-looks-strangely-until-VCS-root-settings-were-saved)** — Information about token looks strangely until VCS root settings were saved

**[TW-79955](https://youtrack.jetbrains.com/issue/TW-79955/Acquire-new-token-for-Bitbucket-Server-Cloud-doesnt-change-username)** — Acquire new token for Bitbucket Server/Cloud doesn't change username

**[TW-79246](https://youtrack.jetbrains.com/issue/TW-79246/Update-Token-field-when-new-token-via-connection-buttons-was-acquired.)** — Update Token field when new token via connection buttons was acquired.

**[TW-79954](https://youtrack.jetbrains.com/issue/TW-79954/ReAcquire-new-token-by-not-owner-of-repository-can-break-VCS-root)** — ReAcquire new token by not owner of repository can break VCS root

**[TW-80279](https://youtrack.jetbrains.com/issue/TW-80279/REST-API-BuildType-compatibleCloudImages-returns-images-that-do-not-meet-agent-requirements)** — REST API: BuildType compatibleCloudImages returns images that do not meet agent requirements

**[TW-80355](https://youtrack.jetbrains.com/issue/TW-80355/Decrease-the-severity-of-log-records-about-the-Slack-notifier-problem-to-the-warning-level.)** — Decrease the severity of log records about the Slack notifier problem to the warning level.

**[TW-73489](https://youtrack.jetbrains.com/issue/TW-73489/Teamcity-Artifact-publishing-tar.gz-fails-with-large-group-id-error)** — Teamcity Artifact publishing tar.gz fails with large group id error

**[TW-77540](https://youtrack.jetbrains.com/issue/TW-77540/Restore-from-backup-may-hang-if-database-connection-is-lost)** — Restore from backup may hang if database connection is lost

**[TW-80252](https://youtrack.jetbrains.com/issue/TW-80252/REST-API-Non-consistent-count-response-for-resultingProperties)** — REST API: Non-consistent count response for resultingProperties

**[TW-79782](https://youtrack.jetbrains.com/issue/TW-79782/Exception-in-pluginsLoaded-event-handler-of-static-UI-extensions-plugin-on-secondary-node)** — Exception in pluginsLoaded event handler of static UI extensions plugin on secondary node

**[TW-80126](https://youtrack.jetbrains.com/issue/TW-80126/maintainDB.cmd-Class-Path-not-referencing-correct-version-of-JAR-files-and-missing-one)** — maintainDB.cmd Class Path not referencing correct version of JAR files and missing one

**[TW-77002](https://youtrack.jetbrains.com/issue/TW-77002/Commit-Status-Publisher-sends-an-excessive-status-update-for-optimized-builds-in-a-chain)** — Commit Status Publisher sends an excessive status update for optimized builds in a chain

**[TW-72751](https://youtrack.jetbrains.com/issue/TW-72751/Move-Azure-DevOps-obsolete-connection-to-the-end-of-connections-list)** — Move "Azure DevOps (obsolete)" connection to the end of connections list

**[TW-78221](https://youtrack.jetbrains.com/issue/TW-78221/Projects-export-doesnt-work-on-secondary-nodes)** — Projects export doesn't work on secondary nodes

**[TW-80162](https://youtrack.jetbrains.com/issue/TW-80162/Overview-tab-test-image-metadata-mouse-left-click-locked)** — Overview tab test image metadata mouse left click locked

**[TW-80168](https://youtrack.jetbrains.com/issue/TW-80168/Rename-button-Diassociate-to-Dissociate-on-Agents-pool-page)** — Rename button Diassociate to Dissociate on Agent's pool page

**[TW-79181](https://youtrack.jetbrains.com/issue/TW-79181/REST-API-Provide-a-way-to-delete-SSH-key-from-a-project)** — REST API: Provide a way to delete SSH key from a project

**[TW-80169](https://youtrack.jetbrains.com/issue/TW-80169/Kotlin-DSL-generates-excessive-UI-Patch-for-branchFilter-VCS-option-parameter)** — Kotlin DSL generates excessive UI Patch for 'branchFilter' VCS option parameter

**[TW-80029](https://youtrack.jetbrains.com/issue/TW-80029/Build-can-fail-with-Unable-to-collect-changes-error-if-VCS-generic-executor-pool-queue-is-full)** — Build can fail with "Unable to collect changes" error if VCS generic executor pool queue is full

**[TW-80059](https://youtrack.jetbrains.com/issue/TW-80059/Vacuum-of-customdatabody-table-can-take-too-much-time-if-there-is-a-huge-number-of-dead-tuples)** — Vacuum of custom_data_body table can take too much time if there is a huge number of dead tuples

**[TW-72938](https://youtrack.jetbrains.com/issue/TW-72938/Misprint-in-a-an-article-when-editing-Azure-DevOps-authentication-module)** — Misprint in "a"/"an" article when editing Azure DevOps authentication module

**[TW-80047](https://youtrack.jetbrains.com/issue/TW-80047/Text-fields-in-the-Create-Build-Configuration-from-template-have-broken-layout)** — Text fields in the Create Build Configuration from template have broken layout

**[TW-79023](https://youtrack.jetbrains.com/issue/TW-79023/No-Disk-Space-Watcher-health-report-on-a-secondary-node-without-Processing-user-requests-responsibility)** — No Disk Space Watcher health report on a secondary node without "Processing user requests" responsibility

**[TW-80039](https://youtrack.jetbrains.com/issue/TW-80039/Parallel-tests-The-number-of-batches-must-be-more-than-1-error-without-comma)** — Parallel tests: The number of batches must be more than 1 error without comma

**[TW-73108](https://youtrack.jetbrains.com/issue/TW-73108/Make-Two-Factor-authentication-page-similar-to-the-login-page)** — Make Two-Factor authentication page similar to the login page

**[TW-78625](https://youtrack.jetbrains.com/issue/TW-78625/Not-enough-clickable-space-on-the-build-line)** — Not enough clickable space on the build line

**[TW-79690](https://youtrack.jetbrains.com/issue/TW-79690/S3-storage-Cannot-set-Multipart-upload-part-size-to-the-minimum-5MB-mentioned-in-the-field-comment)** — S3 storage: Cannot set "Multipart upload part size" to the minimum (5MB) mentioned in the field comment

**[TW-79914](https://youtrack.jetbrains.com/issue/TW-79914/Kubernetes-Executor-Remove-mention-of-Kubernetes-job-from-build-steps)** — Kubernetes Executor: Remove mention of Kubernetes job from build steps

**[TW-78943](https://youtrack.jetbrains.com/issue/TW-78943/Not-all-variations-of-revisions-are-shown-by-the-new-UI-on-the-Changes-tab)** — Not all variations of revisions are shown by the new UI on the Changes tab

**[TW-78582](https://youtrack.jetbrains.com/issue/TW-78582/Build-status-for-Bitbucket-Server-Data-Center-does-not-contain-build-number-even-for-started-failed-succeeded-statuses)** — Build status for Bitbucket Server / Data Center does not contain build number even for started/failed/succeeded statuses

### Performance Problem

**[TW-75906](https://youtrack.jetbrains.com/issue/TW-75906/very-slow-zip-artifact-browsing-in-web-UI)** — very slow zip artifact browsing in web UI

**[TW-81136](https://youtrack.jetbrains.com/issue/TW-81136/Checking-for-VCS-changes-monitor-thread-causes-interlocking-and-a-large-queue-of-VCS-polling-tasks)** — Checking for VCS changes monitor thread causes interlocking and a large queue of VCS polling tasks

**[TW-80963](https://youtrack.jetbrains.com/issue/TW-80963/Build-status-recalculation-queue-can-become-overflown-if-many-builds-are-finishing-in-the-same-configuration)** — Build status recalculation queue can become overflown if many builds are finishing in the same configuration

**[TW-80602](https://youtrack.jetbrains.com/issue/TW-80602/Speedup-calculation-of-revision-affected-by-checkout-rules-if-checkout-rules-do-not-filter-files-inside-submodule-mountpoints)** — Speedup calculation of revision affected by checkout rules if checkout rules do not filter files inside submodule mountpoints

**[TW-79675](https://youtrack.jetbrains.com/issue/TW-79675/TeamCity-REST-API-computes-pending-changes-without-any-limits)** — TeamCity REST API computes pending changes without any limits

**[TW-45828](https://youtrack.jetbrains.com/issue/TW-45828/Slow-build-parameters-page-loading-30-seconds-rendering)** — Slow build parameters page loading (>30 seconds rendering)

### Security

9 security problems have been fixed.