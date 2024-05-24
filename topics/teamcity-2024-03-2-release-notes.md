[//]: # (title: TeamCity 2024.03.2 Release Notes)
[//]: # (auxiliary-id: TeamCity 2024.03.2 Release Notes)


**Build 156319, 24 May 2024**


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

<!--Project: TeamCity Fix versions: {2024.03.2 (156319)}  #{Security Problem}  #Fixed #Testing -{Trunk issue} -bulletin-exclude -->

### Security

8 security issues were fixed. To protect customers who have not yet updated their servers, we typically withhold details about these fixes. Instead, we encourage you to review our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity) a few days after each bugfix release for more information.

In our effort to enhance transparency and due to potential delays in publishing new security bulletins (stemming from the simultaneous release of the 2022.04.6, 2022.10.5, 2023.05.5, 2023.11.5, and 2024.03.2 bug-fix updates), we have decided to provide a summary of both new and backported fixes. You can still expect details on these issues to be published in our Security Bulletin shortly.

* Path traversal allowing to read files from the server was possible
* Several stored XSS in untrusted builds settings were possible
* A third-party agent could impersonate a cloud agent
* Stored XSS via build step settings was possible
* Technical information regarding the TeamCity server could be exposed
* TeamCity users could perform actions that should not be available to them based on their permissions
* Certain TeamCity API endpoints did not check user permissions 
* TeamCity server was susceptible to DoS attacks with incorrect auth tokens
