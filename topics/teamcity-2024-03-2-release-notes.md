[//]: # (title: TeamCity 2024.03.2 Release Notes)
[//]: # (auxiliary-id: TeamCity 2024.03.2 Release Notes)


**Build ???, ??? May 2024**


<!--Project: TeamCity Fix versions: 2024.03.2 -{2024.03.1 (156270)} visible to: {All Users}   #Fixed #Testing -{Trunk issue}-->

### Bug

**[TW-87798](https://youtrack.jetbrains.com/issue/TW-87798/Agent-side-NuGet-Cache-cleanup-is-interrupted-if-the-process-cannot-clean-it-in-under-1-minute)** — Agent-side NuGet Cache cleanup is interrupted if the process cannot clean it in under 1 minute

**[TW-87765](https://youtrack.jetbrains.com/issue/TW-87765/Subproject-administrator-cannot-view-NuGet-feed-and-Artifact-storage-settings-of-a-parent-project)** — Subproject administrator cannot view NuGet feed and Artifact storage settings of a parent project

**[TW-63400](https://youtrack.jetbrains.com/issue/TW-63400/Some-links-opens-the-href-pages-in-new-UI-even-if-user-has-not-checked-option-use-experimental-UI)** — Some links opens the href pages in new UI even if user has not checked option 'use experimental UI'

**[TW-87657](https://youtrack.jetbrains.com/issue/TW-87657/PullRequest-build-feature-not-available-for-composite-configurations-anymore)** — PullRequest build feature not available for composite configurations anymore

**[TW-87750](https://youtrack.jetbrains.com/issue/TW-87750/The-user-with-the-Project-Administrator-role-cant-see-the-NuGet-feed)** — The user with the Project Administrator role can't see the NuGet feed

**[TW-87470](https://youtrack.jetbrains.com/issue/TW-87470/Vault-Remote-parameters-Vault-query-change-from-the-Run-custom-build-dialog-is-not-applied)** — [Vault Remote parameters] Vault query change from the Run custom build dialog is not applied

**[TW-86820](https://youtrack.jetbrains.com/issue/TW-86820/Redesign-the-Add-new-parameter-dialog-disable-the-button-Delete-appearance-settings-when-parameter-is-uneditable)** — Redesign the "Add new parameter" dialog: disable the button "Delete appearance settings" when parameter is uneditable 

**[TW-86702](https://youtrack.jetbrains.com/issue/TW-86702/Checkout-rules-dont-work-with-p4-task-streams-in-a-different-depot)** — Checkout rules don't work with p4 task streams in a different depot

**[TW-87530](https://youtrack.jetbrains.com/issue/TW-87530/Support-quotes-for-additional-arguments-in-Docker-wrapper)** — Support quotes for additional arguments in Docker wrapper

**[TW-87205](https://youtrack.jetbrains.com/issue/TW-87205/dotCover-runner-working-dir-is-duplicated-in-Linux-container-on-Windows-agent)** — dotCover runner: working dir is duplicated in Linux container on Windows agent

**[TW-86663](https://youtrack.jetbrains.com/issue/TW-86663/dotCover-runner-unable-to-start-profiling-in-Linux-container-on-Windows-agent)** — dotCover runner: unable to start profiling in Linux container on Windows agent

**[TW-67454](https://youtrack.jetbrains.com/issue/TW-67454/Web-feedback-from-https-www.jetbrains.com-help-teamcity-configuring-finish-build-trigger.html)** — Web feedback from "https://www.jetbrains.com/help/teamcity/configuring-finish-build-trigger.html"

**[TW-86764](https://youtrack.jetbrains.com/issue/TW-86764/Versioned-settings-on-TeamCity-throw-java.lang.SecurityException-Registering-shutdown-hooks-is-not-permitted)** — Versioned settings on TeamCity throw java.lang.SecurityException: Registering shutdown hooks is not permitted

**[TW-87668](https://youtrack.jetbrains.com/issue/TW-87668/Visual-Studio-Tests-runner-is-broken)** — Visual Studio Tests runner is broken

**[TW-87436](https://youtrack.jetbrains.com/issue/TW-87436/Deadlock-in-TeamCity-server-jetbrains.buildServer.serverSide.impl.history.DBBuildHistory.add2Cache)** — Deadlock in TeamCity server, jetbrains.buildServer.serverSide.impl.history.DBBuildHistory.add2Cache

**[TW-85593](https://youtrack.jetbrains.com/issue/TW-85593/Unclear-warning-Unable-to-determine-DSL-API-packages-type-if-pom.xml-is-absent-in-one-of-DSL-repositories-without)** — Unclear warning "Unable to determine DSL API packages type", if pom.xml is absent in one of DSL repositories without IncrementalMode

**[TW-87330](https://youtrack.jetbrains.com/issue/TW-87330/If-upload-custom-tool-archive-failed-with-error-do-not-show-the-To-fix-this-installation-error-upload-an-archive-with-the-tool)** — If upload custom tool archive failed with error, do not show the "To fix this installation error, upload an archive with the tool from local storage" hint

**[TW-87465](https://youtrack.jetbrains.com/issue/TW-87465/UnsupportedOperationException-when-stopping-a-build)** — UnsupportedOperationException when stopping a build


### Performance Problem

**[TW-87434](https://youtrack.jetbrains.com/issue/TW-87434/Optimize-data-loading-from-ignoredtests-table-for-MSSQL-database)** — Optimize data loading from ignored_tests table for MSSQL database

<!--Project: TeamCity Fix versions: 2024.03.2 -{2024.03.1 (156270)} #{Security Problem}  #Fixed #Testing -{Trunk issue}-->

### Security

??? security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2024.03.2). Security bulletins for new versions are typically published within the next few days after the release date.
