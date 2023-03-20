[//]: # (title: TeamCity 2022.10.3 Release Notes)
[//]: # (auxiliary-id: TeamCity 2022.10.3 Release Notes)

__Build: 117072__

__21 March 2023__

<!---
project: TeamCity Fix versions: 2022.10.3 visible to: {All Users} State: Fixed -{Trunk issue}
-->

### Bug

**[TW-78129](https://youtrack.jetbrains.com/issue/TW-78129/Improve-the-log-message-when-a-port-for-the-HTTPS-connection-is-busy)** — Improve the log message when a port for the HTTPS connection is busy

**[TW-74139](https://youtrack.jetbrains.com/issue/TW-74139/.NET-custom-step-exiting-with-1-doesnt-fail-the-build)** — .NET custom step exiting with 1 doesn't fail the build

**[TW-79860](https://youtrack.jetbrains.com/issue/TW-79860/Nuget-feed-cleared-after-upgrading-from-2022.10.1-to-2022.10.2)** — Nuget feed cleared after upgrading from 2022.10.1 to 2022.10.2

**[TW-79984](https://youtrack.jetbrains.com/issue/TW-79984/Update-Git-version-within-TeamCity-Docker-Images-2.39.1-2.39.2)** — Update Git version within TeamCity Docker Images: 2.39.1 -> 2.39.2

**[TW-80169](https://youtrack.jetbrains.com/issue/TW-80169/Kotlin-DSL-generates-excessive-UI-Patch-for-branchFilter-VCS-option-parameter)** — Kotlin DSL generates excessive UI Patch for 'branchFilter' VCS option parameter

**[TW-80029](https://youtrack.jetbrains.com/issue/TW-80029/Build-can-fail-with-Unable-to-collect-changes-error-if-VCS-generic-executor-pool-queue-is-full)** — Build can fail with "Unable to collect changes" error if VCS generic executor pool queue is full

**[TW-77954](https://youtrack.jetbrains.com/issue/TW-77954/Set-focus-on-first-found-runner-when-searching-for-build-runners)** — Set focus on first found runner when searching for build runners

**[TW-79154](https://youtrack.jetbrains.com/issue/TW-79154/Failed-to-upload-certificate-Invalid-key.)** — Failed to upload certificate: Invalid key.

**[TW-80059](https://youtrack.jetbrains.com/issue/TW-80059/Vacuum-of-customdatabody-table-can-take-too-much-time-if-there-is-a-huge-number-of-dead-tuples)** — Vacuum of custom_data_body table can take too much time if there is a huge number of dead tuples

**[TW-78858](https://youtrack.jetbrains.com/issue/TW-78858/Lots-of-logs-Failed-to-find-agent-type-for-agent-...-in-teamcity-server.log-after-removal-of-a-non-cloud-agent)** — Lots of logs "Failed to find agent type for agent ..." in teamcity-server.log after removal of a non cloud agent

**[TW-79859](https://youtrack.jetbrains.com/issue/TW-79859/Null-pointer-exception-calling-buildQueue-API-since-upgrade-to-build-117025)** — Null pointer exception calling buildQueue API since upgrade to build 117025

**[TW-56009](https://youtrack.jetbrains.com/issue/TW-56009/Docker-processes-left-after-builds)** — Docker processes left after builds

**[TW-79046](https://youtrack.jetbrains.com/issue/TW-79046/Cannot-login-to-docker-repository-due-to-a-file-lock-on-windows.)** — Cannot login to docker repository due to a file lock on windows.

**[TW-79512](https://youtrack.jetbrains.com/issue/TW-79512/Dockerindocker-does-not-restart-reliable)** — Docker_in_docker does not restart reliable

**[TW-79848](https://youtrack.jetbrains.com/issue/TW-79848/Test-metadata-is-not-reported-for-finished-tests-in-different-flow-id-with-test-batching-enabled)** — Test metadata is not reported for finished tests in different flow id with test batching enabled

**[TW-76823](https://youtrack.jetbrains.com/issue/TW-76823/Add-build-step-button-looks-excess-in-New-Build-Step-dialog)** — "Add build step" button looks excess in "New Build Step" dialog

**[TW-76815](https://youtrack.jetbrains.com/issue/TW-76815/Not-all-symbols-in-runner-descriptions-are-processed-correctly-in-grid-view)** — Not all symbols in runner descriptions are processed correctly in grid view

**[TW-75877](https://youtrack.jetbrains.com/issue/TW-75877/Build-approval-improvements-for-the-Approval-rules-field-validation)** — Build approval: improvements for the "Approval rules" field validation

**[TW-78878](https://youtrack.jetbrains.com/issue/TW-78878/HTTPS-display-a-dedicated-error-message-when-a-key-has-a-valid-format-but-is-encrypted)** — HTTPS: display a dedicated error message when a key has a valid format but is encrypted


### Performance Problem

**[TW-79703](https://youtrack.jetbrains.com/issue/TW-79703/A-lot-of-memory-is-consumed-by-FlowAwareIndexFileOptimizedBuilder)** — A lot of memory is consumed by FlowAwareIndexFileOptimizedBuilder

**[TW-78800](https://youtrack.jetbrains.com/issue/TW-78800/Contention-on-storing-newly-loaded-VCS-commits-from-the-database-can-slow-down-startup-of-the-server)** — Contention on storing newly loaded VCS commits from the database can slow down startup of the server

### Security

4 security problems have been fixed.