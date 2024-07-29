[//]: # (title: TeamCity 2024.03 Release Notes)
[//]: # (auxiliary-id: TeamCity 2024.03 Release Notes)


**Build 156166, 27 March 2024**


<!--project: TeamCity Fix versions: {2024.03 (156166)} , {2024.01 Cloud} , -{2023.11 (147331)} , -{2023.11.1 (147412)} , -{2023.11.2 (147486)} , -{2023.11.3 (147512)} , -{2023.11.4 (147586)} #Fixed #Testing visible to: {All Users} -{Trunk issue}-->


### Feature

**[TW-81623](https://youtrack.jetbrains.com/issue/TW-81623/Approval-of-untrusted-builds-on-pull-requests-from-forks-for-GitHub-and-GitLab)** — Approval of "untrusted" builds on pull requests from forks for GitHub and GitLab

**[TW-84116](https://youtrack.jetbrains.com/issue/TW-84116/Open-the-Agent-Terminal-in-checkout-directory-if-it-is-opened-from-the-build-page)** — Open the Agent Terminal in checkout directory if it is opened from the build page

**[TW-85424](https://youtrack.jetbrains.com/issue/TW-85424/Calculate-and-store-optimized-time-as-build-statistics-in-composite-builds)** — Calculate and store optimized time as build statistics in composite builds

**[TW-21673](https://youtrack.jetbrains.com/issue/TW-21673/Support-plugins-tools-packing-for-executable-bits-support)** — Support plugins/tools packing for executable bits support

**[TW-71916](https://youtrack.jetbrains.com/issue/TW-71916/Support-configuration-cache-in-the-Gradle-runner)** — Support configuration cache in the Gradle runner

**[TW-79582](https://youtrack.jetbrains.com/issue/TW-79582/Extending-vstest-command-of-the-.NET-runner-to-allow-retry-of-failing-tests)** — Extending vstest command of the .NET runner to allow retry of failing tests

**[TW-85030](https://youtrack.jetbrains.com/issue/TW-85030/Add-dotCover-runner)** — Add dotCover runner

**[TW-84686](https://youtrack.jetbrains.com/issue/TW-84686/Support-kotlin-1.9.x)** — Support kotlin 1.9.x

**[TW-21761](https://youtrack.jetbrains.com/issue/TW-21761/Ability-to-configure-submodule-authentication-different-from-the-main-repository-one)** — Ability to configure submodule authentication different from the main repository one

**[TW-79525](https://youtrack.jetbrains.com/issue/TW-79525/Redesign-the-add-new-parameter-dialog)** — Redesign the "add new parameter" dialog

**[TW-85783](https://youtrack.jetbrains.com/issue/TW-85783/Bundle-Hashicorp-Vault-plugin)** — Bundle Hashicorp Vault plugin

**[TW-19132](https://youtrack.jetbrains.com/issue/TW-19132/Option-to-run-the-build-even-if-artifact-dependency-failed-to-download-optional-dependency)** — Option to run the build even if artifact dependency failed to download (optional dependency)

**[TW-71308](https://youtrack.jetbrains.com/issue/TW-71308/Support-custom-lfs-URL-and-credentials)** — Support custom lfs URL and credentials

**[TW-77455](https://youtrack.jetbrains.com/issue/TW-77455/More-control-over-helix-swarm-review-comments-from-commit-status-publisher)** — More control over helix swarm review comments from Commit Status Publisher

**[TW-80947](https://youtrack.jetbrains.com/issue/TW-80947/Allow-disabling-Commit-Status-Publisher-comments-in-Perforce-Swarm)** — Allow disabling Commit Status Publisher comments in Perforce Swarm

**[TW-84952](https://youtrack.jetbrains.com/issue/TW-84952/Make-it-possible-to-remap-VCS-root-fetch-URL-on-the-agent)** — Make it possible to remap VCS root fetch URL on the agent

**[TW-83261](https://youtrack.jetbrains.com/issue/TW-83261/Expose-information-about-the-tests-that-were-running-when-the-build-was-stopped)** — Expose information about the tests that were running when the build was stopped

**[TW-82632](https://youtrack.jetbrains.com/issue/TW-82632/Telegraf-as-PerfMon-backend-to-collect-system-metrics)** — Telegraf as PerfMon backend to collect system metrics

**[TW-59046](https://youtrack.jetbrains.com/issue/TW-59046/Extend-PerfMon-to-track-free-space-on-agent-during-build)** — Extend PerfMon to track free space on agent during build

**[TW-84537](https://youtrack.jetbrains.com/issue/TW-84537/Allow-configuring-a-signal-to-terminate-the-build-processes)** — Allow configuring a signal to terminate the build processes

### Bug

**[TW-85960](https://youtrack.jetbrains.com/issue/TW-85960)** — Matrix builds overview: all status filters become enabled when current filter is not relevant anymore

**[TW-86526](https://youtrack.jetbrains.com/issue/TW-86526/Project-whose-uuid-was-changed-cannot-be-loaded-if-it-has-a-sub-project)** — Project whose uuid was changed cannot be loaded if it has a sub project

**[TW-80329](https://youtrack.jetbrains.com/issue/TW-80329/Docker-support-build-feature-The-Docker-events-log-shows-image-ID-instead-of-image-digest)** — Docker support build feature: The Docker events log shows image ID instead of image digest

**[TW-86040](https://youtrack.jetbrains.com/issue/TW-86040/IntelliJ-IDEA-tool-is-added-to-the-backup-file)** — IntelliJ IDEA tool is added to the backup file

**[TW-84904](https://youtrack.jetbrains.com/issue/TW-84904/Slack-notifier-health-report-works-minutes-and-slows-down-generation-of-all-other-reports)** — Slack notifier health report works minutes and slows down generation of all other reports

**[TW-86896](https://youtrack.jetbrains.com/issue/TW-86896/Docker-Build-Agent-Upgrade-Fail-Error-deleting-file-CBuildAgentBUILD147586)** — Docker Build Agent Upgrade Fail - Error deleting file: C:\BuildAgent\BUILD_147586

**[TW-85284](https://youtrack.jetbrains.com/issue/TW-85284/Unable-to-log-in-from-the-IntelliJ-IDEA-TeamCity-plugin)** — Unable to log in from the IntelliJ IDEA TeamCity plugin

**[TW-86046](https://youtrack.jetbrains.com/issue/TW-86046/NuGet-feed-does-not-work-because-of-interlocking-inside-HSQLDB)** — NuGet feed does not work because of interlocking inside HSQLDB

**[TW-86078](https://youtrack.jetbrains.com/issue/TW-86078/From-the-Agent-Page-the-Agent-Terminal-should-be-opened-in-the-home-directory)** — From the Agent Page the Agent Terminal should be opened in the home directory

**[TW-67253](https://youtrack.jetbrains.com/issue/TW-67253/Update-TeamCity-version-in-Add-Remove-Programs-Windows-installations)** — Update TeamCity version in Add/Remove Programs (Windows installations)

**[TW-81659](https://youtrack.jetbrains.com/issue/TW-81659/Service-messages-notification-to-slack-generates-a-link-to-the-classic-UI)** — Service messages notification to slack generates a link to the classic UI

**[TW-84732](https://youtrack.jetbrains.com/issue/TW-84732/Build-history-page-shows-404-request-when-select-filter-by-tag)** — Build history page shows 404 request when select filter by tag

**[TW-86649](https://youtrack.jetbrains.com/issue/TW-86649/Warning-Replying-with-403-status-for-the-unauthorized-request-GET-clouds-extensions-..-cloud-list-image.jsp-in-teamcity)** — Warning " Replying with 403 status for the unauthorized request: GET '/clouds/extensions/../cloud-list-image.jsp" in teamcity-server.log on server startup

**[TW-86401](https://youtrack.jetbrains.com/issue/TW-86401/Project-secret-token-generated-via-REST-API-can-be-lost-after-the-server-restart)** — Project secret token generated via REST API can be lost after the server restart

**[TW-86735](https://youtrack.jetbrains.com/issue/TW-86735/Post-code-review-comments-permission-of-a-Space-connection-is-falsely-required-to-satisfy-Can-publish-build-statuses-capability)** — "Post code review comments" permission of a Space connection is falsely required to satisfy "Can publish build statuses" capability

**[TW-85829](https://youtrack.jetbrains.com/issue/TW-85829/Agent-gets-OOM-on-reading-server-command)** — Agent gets OOM on reading server command

**[TW-86271](https://youtrack.jetbrains.com/issue/TW-86271/Refreshable-tokens-can-not-be-used-in-build-features-if-the-attached-VCS-Root-is-defined-in-a-parent-project)** — Refreshable tokens can not be used in build features if the attached VCS Root is defined in a parent project

**[TW-85344](https://youtrack.jetbrains.com/issue/TW-85344/.NET-steps-running-vstest-may-produce-directories-going-over-the-length-limit)** — .NET steps running vstest may produce directories going over the length limit

**[TW-84903](https://youtrack.jetbrains.com/issue/TW-84903/Using-deprecated-dotCover-on-macOS-leads-to-error-about-Windows-without-running-dotCover.sh)** — Using deprecated dotCover on macOS leads to error about Windows without running dotCover.sh

**[TW-86764](https://youtrack.jetbrains.com/issue/TW-86764/Versioned-settings-on-TeamCity-throw-java.lang.SecurityException-Registering-shutdown-hooks-is-not-permitted)** — Versioned settings on TeamCity throw java.lang.SecurityException: Registering shutdown hooks is not permitted

**[TW-86732](https://youtrack.jetbrains.com/issue/TW-86732/Support-paths-with-backslashes-in-executable-files-section-in-teamcity-plugin.xml)** — Support paths with backslashes in executable-files section in teamcity-plugin.xml

**[TW-78649](https://youtrack.jetbrains.com/issue/TW-78649/Make-links-in-the-notifications-point-to-a-new-UI-if-user-does-not-use-a-classic-one)** — Make links in the notifications point to a new UI if user does not use a classic one

**[TW-86917](https://youtrack.jetbrains.com/issue/TW-86917/Gradle-build-step-does-not-pick-up-wrapper-from-non-default-locations)** — Gradle build step does not pick up wrapper from non-default locations

**[TW-86481](https://youtrack.jetbrains.com/issue/TW-86481/Pull-request-build-feature-does-not-provide-parameters-to-matrix-builds)** — Pull request build feature does not provide parameters to matrix builds

**[TW-81675](https://youtrack.jetbrains.com/issue/TW-81675/Improve-agent-configuration-parameter-for-defining-which-container-engine-to-use)** — Improve agent configuration parameter for defining which container engine to use

**[TW-75291](https://youtrack.jetbrains.com/issue/TW-75291/Noisy-comments-from-Commit-status-publisher-Helix-Swarm-when-Composite-build-with-retried-tests-changes-its-status)** — Noisy comments from Commit status publisher (Helix Swarm) when Composite build with retried tests changes its status

**[TW-84858](https://youtrack.jetbrains.com/issue/TW-84858/Change-Username-password-authentication-type-for-Bitbucket-Cloud-Commit-Status-Publisher-and-Pull-Request)** — Change `Username/password` authentication type for Bitbucket Cloud Commit Status Publisher and Pull Request

**[TW-86863](https://youtrack.jetbrains.com/issue/TW-86863/GitHub-Enterprise-OAuth-Authentication-error-isnt-shown-due-to-JSP-error)** — GitHub Enterprise OAuth Authentication error isn't shown due to JSP error

**[TW-75682](https://youtrack.jetbrains.com/issue/TW-75682/S3-migration-tool.-Do-not-throw-exception-when-incorrect-path-to-artifacts-is-used.)** — S3 migration tool. Do not throw exception when incorrect path to artifacts is used.

**[TW-86708](https://youtrack.jetbrains.com/issue/TW-86708/.NET-vstest-command-with-Test-names-filtration-runs-all-tests-in-test-retries-and-Parallel-test-batches)** — .NET vstest command with Test names filtration runs all tests in test retries and Parallel test batches

**[TW-85611](https://youtrack.jetbrains.com/issue/TW-85611/Warning-about-incorrect-webhook-will-be-shown-during-GitHub-App-Test-Connection-after-the-update-of-the-Webhook-Secret)** — Warning about incorrect webhook will be shown during GitHub App Test Connection after the update of the Webhook Secret

**[TW-85612](https://youtrack.jetbrains.com/issue/TW-85612/GitHub-App-test-connection-can-show-warning-about-Webhook-secret-for-new-GitHub-Apps)** — GitHub App test connection can show warning about Webhook secret for new GitHub Apps

**[TW-86728](https://youtrack.jetbrains.com/issue/TW-86728/dotCover-runner-fails-to-run-on-macOS-x64)** — dotCover runner fails to run on macOS x64

**[TW-86740](https://youtrack.jetbrains.com/issue/TW-86740/Remove-ntlmAuth-path-from-our-documentation)** — Remove /ntlmAuth/&lt;path> from our documentation

**[TW-86458](https://youtrack.jetbrains.com/issue/TW-86458/Improve-error-message-during-tool-installation-if-TeamCity-server-doesnt-have-internet-access)** — Improve error message during tool installation if TeamCity server doesn't have internet access

**[TW-85898](https://youtrack.jetbrains.com/issue/TW-85898/Warning-for-the-authentication-module-is-shown-without-real-reason)** — Warning for the authentication module is shown without real reason

**[TW-18674](https://youtrack.jetbrains.com/issue/TW-18674/Function-Run-selected-tests-locally-using-JUnit-only-runs-one-test-and-not-all-selected-tests.)** — Function "Run selected tests locally using JUnit" only runs one test and not all selected tests.

**[TW-86063](https://youtrack.jetbrains.com/issue/TW-86063/Pull-Requests-build-feature-fails-on-source-branch-names-containing-brackets)** — Pull Requests build feature fails on source branch names containing brackets

**[TW-86041](https://youtrack.jetbrains.com/issue/TW-86041/Root-projects-NuGet-Feed-page-is-unresponsive)** — Root project's NuGet Feed page is unresponsive

**[TW-84394](https://youtrack.jetbrains.com/issue/TW-84394/Matrix-builds-Allow-to-view-build-logs-from-virtual-dependencies-from-parent-Build-log-tab)** — Matrix builds: Allow to view build logs from virtual dependencies from parent Build log tab

**[TW-85026](https://youtrack.jetbrains.com/issue/TW-85026/Failure-Conditions.-Unlimited-execution-timeout-not-unlimited)** — Failure Conditions. Unlimited execution timeout not unlimited

**[TW-86635](https://youtrack.jetbrains.com/issue/TW-86635/Node-may-lose-just-persisted-attributes)** — Node may lose just persisted attributes

**[TW-85023](https://youtrack.jetbrains.com/issue/TW-85023/Ensure-there-always-is-a-default-version-if-at-least-one-version-of-the-tool-is-installed)** — Ensure there always is a default version if at least one version of the tool is installed

**[TW-85970](https://youtrack.jetbrains.com/issue/TW-85970/Show-progress-when-analysing-tool-usages)** — Show progress when analysing tool usages

**[TW-86576](https://youtrack.jetbrains.com/issue/TW-86576/Remove-failed-to-start-builds-limit-from-the-retry-build-trigger)** — Remove failed to start builds limit from the retry build trigger

**[TW-86493](https://youtrack.jetbrains.com/issue/TW-86493/Missing-coverage-when-both-.NET-and-NUnit-steps-with-dotCover-are-added)** — Missing coverage when both .NET and NUnit steps with dotCover are added

**[TW-85246](https://youtrack.jetbrains.com/issue/TW-85246/COPYRIGHT-JBA-Login)** — [COPYRIGHT] JBA Login

**[TW-86304](https://youtrack.jetbrains.com/issue/TW-86304/Allow-to-install-bundled-tool-with-modified-ID-to-data-directory)** — Allow to install bundled tool with modified ID to data directory

**[TW-85984](https://youtrack.jetbrains.com/issue/TW-85984/Add-possibility-to-clean-up-failed-tool-installation)** — Add possibility to clean up failed tool installation

**[TW-86097](https://youtrack.jetbrains.com/issue/TW-86097/Publish-patched-kubernetes-client-as-a-proper-pom)** — Publish patched kubernetes-client as a proper pom

**[TW-86430](https://youtrack.jetbrains.com/issue/TW-86430/No-bullets-for-unordered-lists-in-server-health-header-from-markdownMessage)** — No bullets for unordered lists in server health header from markdownMessage

**[TW-86428](https://youtrack.jetbrains.com/issue/TW-86428/Analyse-usages-of-old-bundled-Maven-tools-only-on-first-server-startup)** — Analyse usages of old bundled Maven tools only on first server startup

**[TW-80926](https://youtrack.jetbrains.com/issue/TW-80926/Improve-error-when-pull-request-number-is-used-for-testing-connection-with-GitHub-Issue-Tracker)** — Improve error when pull request number is used for testing connection with GitHub Issue Tracker

**[TW-86119](https://youtrack.jetbrains.com/issue/TW-86119/Kotlin-DSL-external-process-output-is-missing-for-local-runs)** — Kotlin DSL external process output is missing for local runs

**[TW-86470](https://youtrack.jetbrains.com/issue/TW-86470/dotCover-runner-command-line-doesnt-use-a-command-line-shell-to-run-the-covering-process)** — dotCover runner: command line doesn't use a command-line shell to run the covering process

**[TW-85520](https://youtrack.jetbrains.com/issue/TW-85520/UI-Notifications-center-server-health-items)** — UI: Notifications center - server health items

**[TW-86301](https://youtrack.jetbrains.com/issue/TW-86301/Maven-tool-version-is-not-shown-in-tool-list-if-the-server-failed-to-install-maven-distribution)** — Maven tool version is not shown in tool list, if the server failed to install maven distribution

**[TW-86349](https://youtrack.jetbrains.com/issue/TW-86349/Possibility-of-an-NPE-during-agent-side-checkout-in-Git-submodule-misconfiguration)** — Possibility of an NPE during agent side checkout in Git submodule misconfiguration

**[TW-86283](https://youtrack.jetbrains.com/issue/TW-86283/Config-persisting-task-created-right-before-server-shutdown-can-reset-versioned-settings-project-revision-and-disable)** — Config persisting task created right before server shutdown can reset versioned settings project revision and disable synchronization

**[TW-86327](https://youtrack.jetbrains.com/issue/TW-86327/Inspections-ReSharper-runner-and-Duplicates-finder-ReSharper-runner-do-not-work-with-R-command-line-tools-that-are-manually)** — Inspections (ReSharper) runner and Duplicates finder (ReSharper) runner do not work with R# command line tools that are manually installed via zip packages

**[TW-81386](https://youtrack.jetbrains.com/issue/TW-81386/S3-artifacts-upload-Total-upload-time-in-build-log-became-strangely-small)** — S3 artifacts upload: "Total upload time" in build log became strangely small

**[TW-85998](https://youtrack.jetbrains.com/issue/TW-85998/Add-new-parameter-dialog-Do-not-close-Run-custom-build-dialog-after-the-reset-of-appearance-settings)** — Add new parameter dialog: Do not close "Run custom build" dialog after the reset of appearance settings

**[TW-79943](https://youtrack.jetbrains.com/issue/TW-79943/Agent-instances-are-not-terminated-if-Cloud-Profile-was-deleted-from-a-secondary-node)** — Agent instances are not terminated, if Cloud Profile was deleted from a secondary node

**[TW-85838](https://youtrack.jetbrains.com/issue/TW-85838/AWS-Cloud-Profile-Agent-pool-defined-from-DSL-isnt-displayed-in-the-UI)** — [AWS Cloud Profile] Agent pool defined from DSL isn't displayed in the UI

**[TW-85052](https://youtrack.jetbrains.com/issue/TW-85052/EC2-UI-User-cannot-switch-AMI-source-if-AMI-tags-option-was-specified)** — EC2 UI: User cannot switch AMI source if "AMI tags" option was specified

**[TW-84664](https://youtrack.jetbrains.com/issue/TW-84664/JetBrains-Space-Users-with-no-access-to-the-repository-from-VCS-root-can-acquire-new-token-for-this-repository)** — (JetBrains Space) Users with no access to the repository from VCS root, can acquire new token for this repository

**[TW-80180](https://youtrack.jetbrains.com/issue/TW-80180/No-way-to-choose-if-only-one-project-from-the-pool-or-all-subprojects-will-be-unassigned-if-the-project-was-associated-only-with)** — No way to choose if only one project from the pool or all subprojects will be unassigned if the project was associated only with one pool

**[TW-85954](https://youtrack.jetbrains.com/issue/TW-85954/Branch-label-background-is-misplaced-on-My-investigations-page)** — Branch label background is misplaced on My investigations page

**[TW-81801](https://youtrack.jetbrains.com/issue/TW-81801/Add-role-dialog-shows-a-hint-that-says-that-the-role-could-be-added-with-a-build-configuration-scope)** — Add role dialog shows a hint that says that the role could be added with a build configuration scope

**[TW-80434](https://youtrack.jetbrains.com/issue/TW-80434/Manual-Label-sources-action-creates-git-tag-with-tagging-message-automatically-created-by-VCS-labeling-build-feature)** — Manual "Label sources" action creates git tag with tagging message "automatically created by VCS labeling build feature"

**[TW-81318](https://youtrack.jetbrains.com/issue/TW-81318/Kotlin-DSL-does-not-use-parameters-if-they-have-been-deprecated)** — Kotlin DSL does not use parameters if they have been deprecated

**[TW-85978](https://youtrack.jetbrains.com/issue/TW-85978/Fix-installation-of-dotCover-2023.3.-tools)** — Fix installation of dotCover 2023.3.* tools

**[TW-84271](https://youtrack.jetbrains.com/issue/TW-84271/Copy-to-clipboard-in-custom-report)** — Copy to clipboard in custom report

**[TW-83154](https://youtrack.jetbrains.com/issue/TW-83154/Free-disk-space-feature-cannot-clean-huge-temp-directory-with-OutOfMemory-error)** — Free disk space feature cannot clean huge temp directory with OutOfMemory error

**[TW-85613](https://youtrack.jetbrains.com/issue/TW-85613/Agent-not-found-page-for-no-longer-registered-cloud-agent-should-have-a-link-to-cloud-image)** — Agent not found page for no longer registered cloud agent should have a link to cloud image

**[TW-85979](https://youtrack.jetbrains.com/issue/TW-85979/Cannot-expand-folder-in-Artifact-dependency-change-Changes-popup-closes-on-every-click)** — Cannot expand folder in Artifact dependency change: Changes popup closes on every click

**[TW-84411](https://youtrack.jetbrains.com/issue/TW-84411/GitHub-App-Opening-a-Pull-Request-or-Commit-Status-Publisher-build-feature-takes-a-lot-of-time-when-GitHub-Server-is-not)** — GitHub App: Opening a Pull Request or Commit Status Publisher build feature takes a lot of time when GitHub Server is not available

**[TW-61829](https://youtrack.jetbrains.com/issue/TW-61829/Investigation-mute-can-be-not-removed-when-it-fixed-just-after-server-start-up)** — Investigation (mute) can be not removed when it fixed just after server start-up

**[TW-85517](https://youtrack.jetbrains.com/issue/TW-85517/UI-Collapse-health-reports)** — UI: Collapse health reports

**[TW-85748](https://youtrack.jetbrains.com/issue/TW-85748/Jerky-build-configuration-UI-when-there-is-a-server-health-item-to-show)** — Jerky build configuration UI when there is a server health item to show

**[TW-85754](https://youtrack.jetbrains.com/issue/TW-85754/Project-export-puts-credentials.json.x-files-into-the-exported-archive)** — Project export puts credentials.json.x files into the exported archive

**[TW-67058](https://youtrack.jetbrains.com/issue/TW-67058/Out-of-memory-error-on-agent-during-DependencyResolverImpl.cleanupDestinationFolders-java.lang.OutOfMemoryError-GC-overhead)** — Out of memory error on agent during DependencyResolverImpl.cleanupDestinationFolders: java.lang.OutOfMemoryError: GC overhead limit exceeded

**[TW-56080](https://youtrack.jetbrains.com/issue/TW-56080/Incorrectly-parsed-surefire-XML-reports-when-flakyFailure-is-present)** — Incorrectly parsed surefire XML reports when flakyFailure is present

**[TW-5816](https://youtrack.jetbrains.com/issue/TW-5816/Test-can-be-still-regarded-as-running-when-build-is-finished-on-execution-timeout)** — Test can be still regarded as running when build is finished on execution timeout

**[TW-85019](https://youtrack.jetbrains.com/issue/TW-85019/Build-on-TeamCity-old-temporary-connection-gets-picked-up)** — Build on TeamCity: old temporary connection gets picked up

**[TW-85594](https://youtrack.jetbrains.com/issue/TW-85594/TC-Demo-Project-Make-import-work-for-projects-with-custom-default-branch-in-VCS-Root)** — [TC Demo Project] Make import work for projects with custom default branch in VCS Root

**[TW-85062](https://youtrack.jetbrains.com/issue/TW-85062/Build-configuration-history-page-shows-infinity-loader)** — Build configuration history page shows infinity loader

**[TW-82970](https://youtrack.jetbrains.com/issue/TW-82970/Unable-to-set-any-value-for-configuration-parameters-with-prefix-dep-using-a-service-message)** — Unable to set any value for configuration parameters with prefix "dep" using a service message

**[TW-75353](https://youtrack.jetbrains.com/issue/TW-75353/Failed-tests-status-is-reported-for-canceled-builds-using-Helix-Swarm-status-API.)** — Failed tests status is reported for canceled builds using Helix Swarm status API.

**[TW-69407](https://youtrack.jetbrains.com/issue/TW-69407/Provide-better-DSL-for-GitHub-Commit-Status-Publisher-build-feature-with-Access-Token-authentication)** — Provide better DSL for GitHub Commit Status Publisher build feature with Access Token authentication


### Performance Problem

**[TW-86743](https://youtrack.jetbrains.com/issue/TW-86743/Perforce-slow-collecting-changes-many-streams-large-changelist)** — Perforce: slow collecting changes, many streams + large changelist

**[TW-67312](https://youtrack.jetbrains.com/issue/TW-67312/Artifacts-storage-admin-tab-is-slow)** — Artifacts storage admin tab is slow

**[TW-86012](https://youtrack.jetbrains.com/issue/TW-86012/Potentially-slow-Maven-dependencies-resolution-while-generating-settings-from-DSL-SNAPSHOT-versions)** — Potentially slow Maven dependencies resolution while generating settings from DSL (SNAPSHOT versions)

**[TW-83341](https://youtrack.jetbrains.com/issue/TW-83341/TeamCity-plugin-consumes-a-lot-of-memory-even-if-I-dont-use-any-of-its-features)** — TeamCity plugin consumes a lot of memory even if I don't use any of its features



<!--project: TeamCity Fix versions: {2024.03 (156166)} , {2024.01 Cloud} , -{2023.11 (147331)} , -{2023.11.1 (147412)} , -{2023.11.2 (147486)} , -{2023.11.3 (147512)} , -{2023.11.4 (147586)} #Fixed #Testing #{Security Problem} -{Trunk issue}-->

### Security

26 security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2024.03). Security bulletins for new versions are typically published within the next few days after the release date.

> Getting timely security updates is now easier than ever! Starting with version 2024.03, TeamCity can auto-download lightweight security patches for crucial security issues. See this section for more information: [](upgrading-teamcity-server-and-agents.md#Security+Patches).