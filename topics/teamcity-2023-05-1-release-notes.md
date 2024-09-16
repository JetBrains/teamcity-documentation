[//]: # (title: TeamCity 2023.05.1 Release Notes)
[//]: # (auxiliary-id: TeamCity 2023.05.1 Release Notes)

__Build: 129321__

__11 July 2023__


<!--project: TeamCity Fix versions: {2023.05.1 (129321)} #Fixed visible to: {All Users} -{Trunk issue}-->

### Feature

**[TW-81199](https://youtrack.jetbrains.com/issue/TW-81199/REST-Expose-global-server-settings)** — REST: Expose global server settings

**[TW-81590](https://youtrack.jetbrains.com/issue/TW-81590)** — HTTPS settings: support ECC keys

### Bug

**[TW-82234](https://youtrack.jetbrains.com/issue/TW-82234)** — S3 Upload: Multipart upload fails with java.net.SocketException

**[TW-81866](https://youtrack.jetbrains.com/issue/TW-81866)** — Failed to publish artifacts in AWS s3 after updating TeamCity to v2023.05

**[TW-82293](https://youtrack.jetbrains.com/issue/TW-82293/bean-currentUser-not-found-within-scope-error-in-the-logs-when-an-unauthorized-user-opens-a-non-existing-page)** — "bean currentUser not found within scope" error in the logs when an unauthorized user opens a non existing page

**[TW-82115](https://youtrack.jetbrains.com/issue/TW-82115/Symbolic-link-folders-no-longer-present-in-build-artifact-paths-since-2023.05-upgrade)** — Symbolic link folders no longer present in build artifact paths since 2023.05 upgrade

**[TW-80256](https://youtrack.jetbrains.com/issue/TW-80256/Perfmon-tab-does-not-work-for-a-running-build-on-a-secondary-node)** — Perfmon tab does not work for a running build on a secondary node

**[TW-74891](https://youtrack.jetbrains.com/issue/TW-74891/Build-with-parallel-tests-does-not-publish-artifacts-from-artifact-paths-sub-artifacts-are-not-implemented)** — Build with parallel tests does not publish artifacts from artifact paths (sub-artifacts are not implemented)

**[TW-82279](https://youtrack.jetbrains.com/issue/TW-82279/Broken-links-on-the-All-builds-page-in-TC-2023.05)** — Broken links on the 'All builds' page in TC 2023.05

**[TW-81871](https://youtrack.jetbrains.com/issue/TW-81871/Many-attempts-to-update-settings-from-VCS-constantly-repeating-Detected-new-revision-...-in-the-teamcity-versioned-settings.log)** — Many attempts to update settings from VCS (constantly repeating Detected new revision ... in the teamcity-versioned-settings.log)

**[TW-81913](https://youtrack.jetbrains.com/issue/TW-81913/Unexpected-error-during-build-messages-processing-in-TeamCity-2023.05)** — Unexpected error during build messages processing in TeamCity - 2023.05

**[TW-82088](https://youtrack.jetbrains.com/issue/TW-82088/JWT-issued-for-GitHub-App-can-be-unusable-because-of-incorrect-expiration-value)** — JWT issued for GitHub App can be unusable because of incorrect expiration value

**[TW-82021](https://youtrack.jetbrains.com/issue/TW-82021/Using-GitHub-App-connection-some-organisation-repositories-can-be-invisible-to-the-user)** — Using GitHub App connection, some organisation repositories can be invisible to the user

**[TW-82083](https://youtrack.jetbrains.com/issue/TW-82083/Improve-handling-of-the-case-when-the-user-has-access-to-no-repositories)** — Improve handling of the case when the user has access to no repositories

**[TW-79303](https://youtrack.jetbrains.com/issue/TW-79303/Authentication-to-TeamCity-via-Space-Connection-fails-with-an-undescriptive-error-if-connection-settings-are-wrong)** — Authentication to TeamCity via Space Connection fails with an undescriptive error if connection settings are wrong

**[TW-81380](https://youtrack.jetbrains.com/issue/TW-81380/The-Build-log-Pop-up-break-down)** — The Build log Pop-up break down

**[TW-81897](https://youtrack.jetbrains.com/issue/TW-81897/Unable-to-open-Dependencies-tab-Chain-mode-after-upgrading-to-2023.05)** — Unable to open Dependencies tab Chain mode after upgrading to 2023.05

**[TW-81645](https://youtrack.jetbrains.com/issue/TW-81645/Redundant-Parallel-tests-dependencies-are-added-to-queue-even-though-the-build-reuses-another-dependent-build)** — Redundant Parallel tests dependencies are added to queue even though the build reuses another dependent build

**[TW-81391](https://youtrack.jetbrains.com/issue/TW-81391/S3-artifacts-upload-Generating-URLs-time-is-not-summarized-by-files)** — S3 artifacts upload: Generating URL's time is not summarized by files

**[TW-81740](https://youtrack.jetbrains.com/issue/TW-81740/Artifacts-published-as-an-archive-contain-an-extraneous-directory)** — Artifacts published as an archive contain an extraneous directory

**[TW-81874](https://youtrack.jetbrains.com/issue/TW-81874/Podman-agent-unmet-requirement-docker.server.osType-on-RHEL-with-Docker-CLI-podman-emulation)** — Podman agent unmet requirement docker.server.osType on RHEL with Docker CLI podman emulation

**[TW-82056](https://youtrack.jetbrains.com/issue/TW-82056/Launch-Template-Run-may-cause-uncaught-exceptions)** — Launch Template Run may cause uncaught exceptions

**[TW-81959](https://youtrack.jetbrains.com/issue/TW-81959/Re-run-Parallel-tests-build-with-rebuild-failed-batch-does-not-run-any-tests)** — Re-run Parallel tests build with rebuild failed batch does not run any tests

**[TW-81617](https://youtrack.jetbrains.com/issue/TW-81617/Multiple-GitHub-App-connections-configured-for-the-same-app-do-not-handle-webhooks-correctly)** — Multiple GitHub App connections configured for the same app do not handle webhooks correctly

**[TW-81807](https://youtrack.jetbrains.com/issue/TW-81807/SMB-runner-is-unable-to-start-on-Java-17)** — SMB runner is unable to start on Java 17

**[TW-82038](https://youtrack.jetbrains.com/issue/TW-82038/Publish-S3-artifacts-with-Temporary-credentials-fails-without-AWSREGION-environment-variable)** — Publish S3 artifacts with Temporary credentials fails without AWS_REGION environment variable

**[TW-81591](https://youtrack.jetbrains.com/issue/TW-81591/TeamCity-displays-a-wrong-Callback-URL-hint.)** — TeamCity displays a wrong Callback URL hint.

**[TW-81709](https://youtrack.jetbrains.com/issue/TW-81709/Commit-Status-Publisher-shows-warning-when-using-Bitbucket-Server-with-user-password)** — Commit Status Publisher shows warning when using Bitbucket Server with user/password

**[TW-81293](https://youtrack.jetbrains.com/issue/TW-81293/Non-existing-project-is-being-shown-in-the-agent-pools-sidebar-of-the-build-queue-page)** — &lt;Non existing project> is being shown in the agent pools sidebar of the build queue page

**[TW-80585](https://youtrack.jetbrains.com/issue/TW-80585/GitHub-Commit-status-Publisher-with-Use-VCS-root-credentials-does-not-work-if-username-is-not-specified)** — GitHub Commit status Publisher with "Use VCS root credentials" does not work, if username is not specified

**[TW-73928](https://youtrack.jetbrains.com/issue/TW-73928/Undefined-tokenType-parameter-in-GitHub-OAuth-git-VCS-root)** — Undefined tokenType parameter in GitHub OAuth git VCS root

**[TW-81369](https://youtrack.jetbrains.com/issue/TW-81369/Open-Terminal-opens-a-link-to-connect-to-the-first-opened-agent-if-the-agent-overview-wasnt-refreshed-before-opening)** — "Open Terminal" opens a link to connect to the first opened agent (if the agent overview wasn't refreshed before opening)

**[TW-81850](https://youtrack.jetbrains.com/issue/TW-81850/Regression-EC2-Agent-security-group-assignment-is-broken-AGAIN.)** — Regression: EC2 Agent security group assignment is broken AGAIN.

**[TW-79610](https://youtrack.jetbrains.com/issue/TW-79610/Improve-readability-of-Timeline-chart-on-Dependencies-tab-when-displaying-parallel-test-executions)** — Improve readability of Timeline chart on Dependencies tab when displaying parallel test executions

**[TW-81869](https://youtrack.jetbrains.com/issue/TW-81869/S3-Storage-S3-artifact-publishing-requires-https-after-upgrading-to-2023.05)** — [S3 Storage] S3 artifact publishing requires https after upgrading to 2023.05

**[TW-81682](https://youtrack.jetbrains.com/issue/TW-81682/Cloud-provisioning-broken-java.util.ServiceConfigurationError-javax.mail.Provider-com.sun.mail.imap.IMAPProvider-not-a-subtype)** — Cloud provisioning broken (java.util.ServiceConfigurationError: javax.mail.Provider: com.sun.mail.imap.IMAPProvider not a subtype)

**[TW-81829](https://youtrack.jetbrains.com/issue/TW-81829/Cloud-Images-Source-changes-DSL-patches-are-applying-without-any-effect)** — Cloud Images Source changes DSL patches are applying without any effect

**[TW-81253](https://youtrack.jetbrains.com/issue/TW-81253/Dont-log-that-ID-will-not-be-generated-in-cloud-profiles)** — Don't log that ID will not be generated in cloud profiles

**[TW-80467](https://youtrack.jetbrains.com/issue/TW-80467/Cannot-find-a-node100479888-may-occur-when-collecting-VCS-changes-on-the-secondary-node)** — Cannot find a node:100479888 may occur when collecting VCS changes on the secondary node

**[TW-74197](https://youtrack.jetbrains.com/issue/TW-74197/Cannot-set-.NET-msbuild-OutDir-property-value-to-end-with-exactly-one-backslash-through-a-system-property)** — Cannot set .NET msbuild OutDir property value to end with exactly one backslash through a system property

**[TW-81775](https://youtrack.jetbrains.com/issue/TW-81775/Stop-and-Remove-agent-buttons-stick-to-each-other)** — Stop and Remove agent buttons stick to each other

**[TW-57046](https://youtrack.jetbrains.com/issue/TW-57046/Cannot-generate-TRX-file-for-test-run-dotnet-test)** — Cannot generate TRX file for test run (dotnet test)

**[TW-81727](https://youtrack.jetbrains.com/issue/TW-81727/Rename-Agent-actions-Connect-to-agent-audit-action-to-better-associate-it-with-the-Agent-Terminal-feature)** — Rename "Agent actions / Connect to agent" audit action to better associate it with the Agent Terminal feature

**[TW-81725](https://youtrack.jetbrains.com/issue/TW-81725/Rename-the-Open-an-interactive-session-to-the-agent-permission-to-better-associate-it-with-the-Open-terminal-button)** — Rename the "Open an interactive session to the agent" permission to better associate it with the 'Open terminal' button

**[TW-81747](https://youtrack.jetbrains.com/issue/TW-81747/Username-isnt-saved-in-Commit-Status-Publisher-settings-for-GitHub-if-password-auth-type-is-chosen)** — Username isn't saved in Commit Status Publisher settings for GitHub if password auth type is chosen

**[TW-80178](https://youtrack.jetbrains.com/issue/TW-80178/Projects-can-lose-compatible-agents-if-it-was-assigned-unassigned-from-the-different-nodes)** — Projects can lose compatible agents if it was assigned/unassigned from the different nodes

**[TW-81400](https://youtrack.jetbrains.com/issue/TW-81400/Space-Pull-Requests-feature-suggests-to-create-a-connection-for-a-user-without-Edit-project-permission)** — Space Pull Requests feature suggests to create a connection for a user without Edit project permission

**[TW-81593](https://youtrack.jetbrains.com/issue/TW-81593/GitHub-App-server-error-when-non-existing-organisation-user-is-specified-in-the-connection)** — GitHub App: server error when non-existing organisation/user is specified in the connection

**[TW-81279](https://youtrack.jetbrains.com/issue/TW-81279/Acquire-new-button-is-enabled-on-Edit-VCS-Root-page-for-users-who-have-no-permissions-to-edit-VCS-Root)** — Acquire new button is enabled on Edit VCS Root page for users who have no permissions to edit VCS Root

**[TW-81291](https://youtrack.jetbrains.com/issue/TW-81291/GitHub-App-app-permissions-are-not-checked-during-Test-Connection-in-Commit-Status-Publisher)** — GitHub App: app permissions are not checked during Test Connection in Commit Status Publisher

**[TW-81560](https://youtrack.jetbrains.com/issue/TW-81560/GitHub-App-In-some-cases-more-repositories-are-listed-than-authorized-for-the-app)** — GitHub App: In some cases, more repositories are listed than authorized for the app

**[TW-81687](https://youtrack.jetbrains.com/issue/TW-81687/Regression-Perforce-server-P4PORT-agent-override-not-propagated-into-build-steps)** — Regression: Perforce server P4PORT agent override not propagated into build steps

**[TW-81577](https://youtrack.jetbrains.com/issue/TW-81577/Podman-support-Broken-ownership-for-agent-directories-if-teamcity.docker.use.sudo-is-set-on-project-level)** — Podman support: Broken ownership for agent directories if teamcity.docker.use.sudo is set on project level

**[TW-81166](https://youtrack.jetbrains.com/issue/TW-81166/Podman-wrapper-Docker-rate-limit-warning-is-present-even-for-authorized-pulls-if-podman-runs-as-root)** — Podman wrapper: Docker rate limit warning is present even for authorized pulls, if podman runs as root

**[TW-81450](https://youtrack.jetbrains.com/issue/TW-81450/A-race-condition-during-build-log-creation)** — A race condition during build-log creation

**[TW-81680](https://youtrack.jetbrains.com/issue/TW-81680/Cleanup-rules-page-Disk-Usage-shows-irrelevant-data)** — Cleanup rules page: Disk Usage shows irrelevant data

**[TW-81655](https://youtrack.jetbrains.com/issue/TW-81655/java.util.ConcurrentModificationException-null)** — java.util.ConcurrentModificationException: null

**[TW-81465](https://youtrack.jetbrains.com/issue/TW-81465/Missing-builds.href-field-when-buildType-was-requested)** — Missing builds.href field when buildType was requested

**[TW-81634](https://youtrack.jetbrains.com/issue/TW-81634/Error-in-event-handler-Error-calling-method-RepositoryStateListener.repositoryStateChanged-for-listener)** — Error in event handler: Error calling method RepositoryStateListener.repositoryStateChanged for listener jetbrains.buildServer.buildTriggers.vcs.git.GitClonesUpdater$1: java.lang.NullPointerException: Cannot invoke "java.util.concurrent.ExecutorService.isShutdown()"

**[TW-80854](https://youtrack.jetbrains.com/issue/TW-80854/Do-not-explicitly-show-docker.io-as-default-registry-in-Docker-connection-because-podman-wrapper-wont-use-it-by-default)** — Do not explicitly show docker.io as default registry in Docker connection, because podman wrapper won't use it by default

**[TW-81578](https://youtrack.jetbrains.com/issue/TW-81578/BuildTypeNotFoundException-in-WeightedAverageBuildDurationCalculator.getStagesToRun-when-invoked-from)** — BuildTypeNotFoundException in WeightedAverageBuildDurationCalculator.getStagesToRun (when invoked from DefaultBuildEstimatesCalculator)

**[TW-80120](https://youtrack.jetbrains.com/issue/TW-80120/Build-page-content-doesnt-update-automatically)** — Build page content doesn't update automatically

**[TW-81542](https://youtrack.jetbrains.com/issue/TW-81542/S3-Plugin-Multipart-upload-retry-flow-issue)** — [S3 Plugin] Multipart upload retry flow issue

**[TW-81026](https://youtrack.jetbrains.com/issue/TW-81026/Git-plugin-does-not-restore-native-Git-flag-if-the-plugin-is-reloaded-in-runtime)** — Git plugin does not restore native Git flag if the plugin is reloaded in runtime

### Performance Problem

**[TW-81735](https://youtrack.jetbrains.com/issue/TW-81735/Agent-updates-local-mirror-several-times-the-number-of-times-correlates-with-the-number-of-include-rules-in-the-checkout-rules)** — Agent updates local mirror several times (the number of times correlates with the number of include rules in the checkout rules)

<!--project: TeamCity Fix versions: {2023.05.1 (129321)} #Fixed #{Security Problem}  -{Trunk issue}-->

### Security

10 security problems have been fixed.

> We do not share the details of security-related issues to avoid compromising clients that keep using previous bugfix and/or major versions of TeamCity. Check out our [Security Bulletin](https://blog.jetbrains.com/blog/tag/security-bulletin/) for the list of disclosed vulnerability fixes.
> 
{style="note"}