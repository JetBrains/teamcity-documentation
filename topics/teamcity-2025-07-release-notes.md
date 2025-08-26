[//]: # (title: TeamCity 2025.07 Release Notes)
[//]: # (help-id: TeamCity 2025.07 Release Notes)


**Build 197242, 23 July 2025**

### Epic

* [**TW-91461**](https://youtrack.jetbrains.com/issue/TW-91461/Kubernetes-Executors-Stage-1) — Kubernetes Executors: Stage 1

### Feature

* [**TW-93591**](https://youtrack.jetbrains.com/issue/TW-93591/Recipes-Support-3rd-party-recipes-in-TeamCity) — [Recipes] Support 3rd-party recipes in TeamCity
* [**TW-91222**](https://youtrack.jetbrains.com/issue/TW-91222/Merge-TeamCity-and-Pipelines-UI-UX-improvements-2025.07) — Merge TeamCity and Pipelines: UI/UX improvements 2025.07
* [**TW-72223**](https://youtrack.jetbrains.com/issue/TW-72223/TeamCity-Whats-new) — TeamCity: What's new
* [**TW-93230**](https://youtrack.jetbrains.com/issue/TW-93230/Allow-enabling-incremental-compilation-in-TeamCity-Kotlin-DSL-Maven-plugin) — Allow enabling incremental compilation in TeamCity Kotlin DSL Maven plugin
* [**TW-91218**](https://youtrack.jetbrains.com/issue/TW-91218/Merge-TeamCity-Enterprise-and-Pipelines-Early-Access) — Merge TeamCity Enterprise and Pipelines: Early Access
* [**TW-90527**](https://youtrack.jetbrains.com/issue/TW-90527/Support-for-multiple-Perforce-Shelve-Triggers) — Support for multiple Perforce Shelve Triggers
* [**TW-91492**](https://youtrack.jetbrains.com/issue/TW-91492/Allow-builds-to-be-approved-by-one-of-several-users-or-groups) — Allow builds to be approved by one of several users or groups
* [**TW-44990**](https://youtrack.jetbrains.com/issue/TW-44990/Automatic-deletion-of-TeamCity-created-Perforce-workspaces-for-the-TeamCity-agents-which-are-no-longer-actual) — Automatic deletion of TeamCity-created Perforce workspaces for the TeamCity agents which are no longer actual
* [**TW-93468**](https://youtrack.jetbrains.com/issue/TW-93468/Custom-callback-URL-for-GitHub-App-connection) — Custom callback URL for GitHub App connection
* [**TW-92419**](https://youtrack.jetbrains.com/issue/TW-92419/Add-an-overlay-mode-for-the-sidebar-pin-unpin) — Add an overlay mode for the sidebar (pin/unpin)
* [**TW-88989**](https://youtrack.jetbrains.com/issue/TW-88989/Sidebar-sub-navigation-redesign) — Sidebar (sub-navigation) redesign

### Task

* [**TW-94696**](https://youtrack.jetbrains.com/issue/TW-94696/Update-JDBC-drivers-to-newer-versions) — Update JDBC drivers to newer versions
* [**TW-87915**](https://youtrack.jetbrains.com/issue/TW-87915/Show-pipelines-in-TeamCity-UI) — Show pipelines in TeamCity UI
* [**TW-73883**](https://youtrack.jetbrains.com/issue/TW-73883/Upload-Generate-new-SSH-keys-in-encrypted-form) — Upload/Generate new SSH keys in encrypted form
* [**TW-91953**](https://youtrack.jetbrains.com/issue/TW-91953/Show-longer-or-untruncated-VCS-errors-in-build-log) — Show longer or untruncated VCS errors in build log
* [**TW-94561**](https://youtrack.jetbrains.com/issue/TW-94561/Docker-Images-update-bundled-Git-to-2.50.1) — Docker Images: update bundled Git to 2.50.1
* [**TW-94140**](https://youtrack.jetbrains.com/issue/TW-94140/Add-support-for-custom-secret-detection-handlers-in-AgentRunningBuild) — Add support for custom secret detection handlers in AgentRunningBuild
* [**TW-90176**](https://youtrack.jetbrains.com/issue/TW-90176/Recipes-Implement-a-way-to-examine-recipes-used-on-a-TeamCity-instance-or-project) — Recipes: Implement a way to examine recipes used on a TeamCity instance or project
* [**TW-93768**](https://youtrack.jetbrains.com/issue/TW-93768/Set-networkaddress.cache.negative.ttl0-by-default-on-the-agent) — Set networkaddress.cache.negative.ttl=0 by default on the agent
* [**TW-86183**](https://youtrack.jetbrains.com/issue/TW-86183/Define-Executors-as-a-project-level-model) — Define Executors as a project-level model
* [**TW-92595**](https://youtrack.jetbrains.com/issue/TW-92595/Kubernetes-plugin-Update-plugin-version-from-1.0-SNAPSHOT) — Kubernetes plugin: Update plugin version from 1.0-SNAPSHOT
* [**TW-90282**](https://youtrack.jetbrains.com/issue/TW-90282/Recipes-add-a-way-to-view-a-recipes-content-in-the-TeamCity-UI-during-installation) — Recipes: add a way to view a recipe's content in the TeamCity UI during installation
* [**TW-93961**](https://youtrack.jetbrains.com/issue/TW-93961/Docker-update-Git-within-Windows-Docker-images-to-Git-2.49.0) — Docker: update Git within Windows Docker images to Git 2.49.0
* [**TW-93958**](https://youtrack.jetbrains.com/issue/TW-93958/Docker-handle-Git-2.49.0-package-removal-from-the-Ubuntu-registry) — Docker: handle Git 2.49.0 package removal from the Ubuntu registry
* [**TW-92484**](https://youtrack.jetbrains.com/issue/TW-92484/Public-Recipes-Add-a-way-to-choose-the-recipe-version-in-the-Add-recipe-popup) — Public Recipes: Add a way to choose the recipe version in the "Add recipe" popup
* [**TW-87916**](https://youtrack.jetbrains.com/issue/TW-87916/Allow-creating-a-pipeline-in-a-project) — Allow creating a pipeline in a project
* [**TW-90918**](https://youtrack.jetbrains.com/issue/TW-90918/Specifying-a-custom-build-file-location-is-deprecated) — Specifying a custom build file location is deprecated
* [**TW-89113**](https://youtrack.jetbrains.com/issue/TW-89113/HTTPS-Support-custom-maxHttpHeaderSize-for-connector) — HTTPS: Support custom maxHttpHeaderSize for connector

### Bug

* [**TW-92034**](https://youtrack.jetbrains.com/issue/TW-92034/Double-trigger-from-Perforce-Shelve-Trigger-feature-when-stream-is-imported-into-another) — Double trigger from "Perforce Shelve Trigger" feature when stream is imported into another
* [**TW-91453**](https://youtrack.jetbrains.com/issue/TW-91453/Artifact-migration-tool-deleted-artifacts-migrated-to-Azure-Storage-can-be-shown-as-available-in-TeamCity) — Artifact migration tool: deleted artifacts migrated to Azure Storage, can be shown as available in TeamCity
* [**TW-94185**](https://youtrack.jetbrains.com/issue/TW-94185/TCP-Merge-Add-description-to-the-Path-field-explaining-what-to-input-and-what-to-expect-with-the-link-to-documentation-part-two) — TCP Merge: Add description to the Path field, explaining what to input and what to expect, with the link to documentation, part two
* [**TW-93465**](https://youtrack.jetbrains.com/issue/TW-93465/Build-fails-with-docker-not-found-for-install-recipes-with-wrapper-or-with-Run-in-Docker-feature) — Build fails with "docker: not found" for install recipes with wrapper or with "Run in Docker" feature
* [**TW-88195**](https://youtrack.jetbrains.com/issue/TW-88195/Versioned-settings-in-perforce-fail-to-commit-if-some-of-the-changes-contain-two-s-within-normal-text) — Versioned settings in perforce fail to commit if some of the changes contain two 's within normal text
* [**TW-93489**](https://youtrack.jetbrains.com/issue/TW-93489/Artifacts-Storage-Transfer-Speed-up-UI-Defaults-to-None-Despite-Transfer-Acceleration-Being-Active) — Artifacts Storage “Transfer Speed-up” UI Defaults to “None” Despite Transfer Acceleration Being Active
* [**TW-92997**](https://youtrack.jetbrains.com/issue/TW-92997/Regression-cannot-upgrade-to-2025.03.1-enqueues-thousands-of-builds) — Regression: cannot upgrade to 2025.03.1, enqueues thousands of builds
* [**TW-92422**](https://youtrack.jetbrains.com/issue/TW-92422/Docker-wrapper-for-Kotlin-runner-with-Linux-platform-doesnt-work-on-Windows-agents-with-LCOW-support) — Docker wrapper for Kotlin runner with Linux platform doesn't work on Windows agents with LCOW support
* [**TW-90723**](https://youtrack.jetbrains.com/issue/TW-90723/Recipes-Its-possible-to-edit-upload-and-delete-the-recipes-in-Read-only-mode) — Recipes: It's possible to edit/upload and delete the recipes in Read-only mode
* [**TW-90533**](https://youtrack.jetbrains.com/issue/TW-90533/Misaligned-View-usages-link-on-Edit-project-Meta-runners-Recipes-tab) — Misaligned View usages link on Edit project -> Meta-runners (Recipes) tab
* [**TW-91938**](https://youtrack.jetbrains.com/issue/TW-91938/Artifacts-are-not-split-per-batch-when-running-Parallel-Tests-which-causes-confusion) — Artifacts are not split per batch when running Parallel Tests which causes confusion
* [**TW-93281**](https://youtrack.jetbrains.com/issue/TW-93281/Build-Log-Horizontal-scroll-is-overlapped-by-TeamCity-version) — Build Log: Horizontal scroll is overlapped by TeamCity version
* [**TW-89996**](https://youtrack.jetbrains.com/issue/TW-89996/Warning-Cannot-edit-properties-of-an-inherited-parameter-when-overriding-inherited-password-parameter-value) — Warning "Cannot edit properties of an inherited parameter" when overriding inherited password parameter value
* [**TW-94592**](https://youtrack.jetbrains.com/issue/TW-94592/Backup-does-not-start-because-of-duplicate-build-ids-in-history-and-lighthistory-tables) — Backup does not start because of duplicate build ids in history and light_history tables
* [**TW-93175**](https://youtrack.jetbrains.com/issue/TW-93175/Search-input-off-spacing) — Search input off spacing
* [**TW-94266**](https://youtrack.jetbrains.com/issue/TW-94266/Incorrect-Build-Queue-Status-with-Approval-Rules-Shared-Resources) — Incorrect Build Queue Status with Approval Rules + Shared Resources
* [**TW-92552**](https://youtrack.jetbrains.com/issue/TW-92552/metricsCounterAppender-log4j-appender-ServerMetricsImpl-may-be-removed-or-ignored) — metricsCounterAppender log4j appender (ServerMetricsImpl) may be removed or ignored
* [**TW-92314**](https://youtrack.jetbrains.com/issue/TW-92314/Dependencies-Timeline-Scroll-down-then-click-on-build-doesnt-show-build-info-popup) — Dependencies Timeline: Scroll down, then click on build doesn't show build info popup
* [**TW-94398**](https://youtrack.jetbrains.com/issue/TW-94398/Template-cannot-be-moved-because-of-Projects-collection-cannot-be-empty-error) — Template cannot be moved because of 'Projects collection cannot be empty' error
* [**TW-94565**](https://youtrack.jetbrains.com/issue/TW-94565/Worksace-removal-miliseconds-instead-of-days-are-reported-to-log) — Worksace removal: miliseconds instead of days are reported to log
* [**TW-94233**](https://youtrack.jetbrains.com/issue/TW-94233/Improve-error-messages-shown-in-TeamCity-when-incorrect-callback-url-in-OAuth-connection-is-using) — Improve error messages shown in TeamCity when incorrect callback url in OAuth connection is using
* [**TW-88821**](https://youtrack.jetbrains.com/issue/TW-88821/Changes-collection-fails-if-pull-request-branch-names-contain-brackets-and-non-default-escape-symbol-is-defined-in-the-VCS-root) — Changes collection fails if pull request branch names contain brackets and non-default escape symbol is defined in the VCS root branch specification
* [**TW-94365**](https://youtrack.jetbrains.com/issue/TW-94365/TCP-Merge-Global-Number-of-Build-Configurations-is-Negative-When-Server-Only-contains-Pipelines) — TCP Merge: Global Number of Build Configurations is Negative When Server Only contains Pipelines
* [**TW-93025**](https://youtrack.jetbrains.com/issue/TW-93025/org.apache.xmlrpc.XmlRpcClientException-Error-decoding-XML-RPC-response-in-teamcity-agent.log) — org.apache.xmlrpc.XmlRpcClientException: Error decoding XML-RPC response in teamcity-agent.log
* [**TW-48794**](https://youtrack.jetbrains.com/issue/TW-48794/Builds-are-not-reused-after-irrelevant-changes-in-Kotlin-DSL-the-same-VCS-root-for-settings-and-project-source-code) — Builds are not reused after irrelevant changes in Kotlin DSL (the same VCS root for settings and project source code)
* [**TW-94418**](https://youtrack.jetbrains.com/issue/TW-94418/Project-isolation-Its-not-possible-to-manually-remove-deleted-project-from-trusted) — [Project isolation] It's not possible to manually remove deleted project from trusted
* [**TW-93862**](https://youtrack.jetbrains.com/issue/TW-93862/Do-not-show-Editing-of-the-project-settings-is-disabled-on-Usages-Report-tab) — Do not show "Editing of the project settings is disabled" on Usages Report tab
* [**TW-93691**](https://youtrack.jetbrains.com/issue/TW-93691/The-scroll-doesnt-work-on-the-white-area-on-dependencies-tab) — The scroll doesn't work on the "white area" on dependencies tab
* [**TW-94079**](https://youtrack.jetbrains.com/issue/TW-94079/Lots-of-logging-from-the-pull-requests-plugin-with-unclear-purpose) — Lots of logging from the pull requests plugin with unclear purpose
* [**TW-93566**](https://youtrack.jetbrains.com/issue/TW-93566/Perforce-Automatic-merge-unable-to-find-the-source-branch) — Perforce Automatic merge: unable to find the source branch
* [**TW-93423**](https://youtrack.jetbrains.com/issue/TW-93423/Token-management-Do-not-show-user-filtering-and-related-errors-if-there-are-no-personal-tokens-to-show) — Token management: Do not show user filtering and related errors, if there are no personal tokens to show
* [**TW-92683**](https://youtrack.jetbrains.com/issue/TW-92683/S3-artifact-upload-when-using-multipart-upload-sets-Content-Type-to-application-octet-stream) — S3 artifact upload, when using multipart upload, sets Content-Type to application/octet-stream
* [**TW-92940**](https://youtrack.jetbrains.com/issue/TW-92940/Create-git-fsck-command-for-the-server-and-agents) — Create git fsck command for the server and agents
* [**TW-91920**](https://youtrack.jetbrains.com/issue/TW-91920/Tools-reported-as-not-used-when-only-referenced-by-templates.) — Tools reported as not used, when only referenced by templates.
* [**TW-91842**](https://youtrack.jetbrains.com/issue/TW-91842/Unknown-data-processor-type-dotNetCoverage-in-service-message-importData-after-upgrade-to-version-2024.12) — Unknown data processor type 'dotNetCoverage' in service message 'importData' after upgrade to version 2024.12
* [**TW-93163**](https://youtrack.jetbrains.com/issue/TW-93163/Problems-with-the-sidebar-if-it-was-opened-using-the-Q-button) — Problems with the sidebar if it was opened using the "Q" button
* [**TW-93028**](https://youtrack.jetbrains.com/issue/TW-93028/Token-management-too-small-margin-between-paginator-and-10-per-page-dropdown) — Token management: too small margin between paginator and "10 per page" dropdown
* [**TW-93420**](https://youtrack.jetbrains.com/issue/TW-93420/Token-management-ROOT-project-looks-strange-in-the-Project-scope-dropdown) — Token management: ROOT project looks strange in the Project scope dropdown
* [**TW-93436**](https://youtrack.jetbrains.com/issue/TW-93436/VMWare-cloud-agents-fail-to-auth-then-enter-a-restart-loop) — VMWare cloud agents fail to auth, then enter a restart loop
* [**TW-94010**](https://youtrack.jetbrains.com/issue/TW-94010/Token-management-cant-delete-personal-token-of-another-user) — Token management: can't delete personal token of another user
* [**TW-94194**](https://youtrack.jetbrains.com/issue/TW-94194/VCS-repository-state-in-the-database-can-have-duplicate-branch-names) — VCS repository state in the database can have duplicate branch names
* [**TW-93075**](https://youtrack.jetbrains.com/issue/TW-93075/Confusing-agent-disconnect-reason-Unregistered-because-of-inactivity) — Confusing agent disconnect reason "Unregistered because of inactivity"
* [**TW-93221**](https://youtrack.jetbrains.com/issue/TW-93221/PowerShell-runner-doesnt-work-in-Kubernetes-Executor-builds) — PowerShell runner doesn't work in Kubernetes Executor builds
* [**TW-94153**](https://youtrack.jetbrains.com/issue/TW-94153/Agent-incorrectly-interprets-503-status-in-upgradeAgentParameters-method-call) — Agent incorrectly interprets 503 status in upgradeAgentParameters method call
* [**TW-94098**](https://youtrack.jetbrains.com/issue/TW-94098/Agent-executes-the-same-command-twice) — Agent executes the same command twice
* [**TW-94105**](https://youtrack.jetbrains.com/issue/TW-94105/Wrong-triggering-in-case-of-VCS-root-change-in-a-build-configuration) — Wrong triggering in case of VCS root change in a build configuration
* [**TW-93434**](https://youtrack.jetbrains.com/issue/TW-93434/Kotlin-Script-Runner-and-TeamCity-recipes-do-not-work-in-container-without-java) — Kotlin Script Runner and TeamCity recipes do not work in container without java
* [**TW-94130**](https://youtrack.jetbrains.com/issue/TW-94130/NullPointerException-in-VCS-trigger) — NullPointerException in VCS trigger
* [**TW-93464**](https://youtrack.jetbrains.com/issue/TW-93464/Under-certain-circumstances-VCS-trigger-starts-a-build-when-there-are-no-relevant-changes) — Under certain circumstances, VCS trigger starts a build when there are no relevant changes
* [**TW-93654**](https://youtrack.jetbrains.com/issue/TW-93654/VCS-trigger-resets-its-own-state) — VCS trigger resets its own state
* [**TW-92403**](https://youtrack.jetbrains.com/issue/TW-92403/Agents-using-proxy-for-outgoing-connections-cannot-retrieve-wrapped-Hashicorp-Vault-tokens) — Agents using proxy for outgoing connections cannot retrieve wrapped Hashicorp Vault tokens
* [**TW-90687**](https://youtrack.jetbrains.com/issue/TW-90687/globalTmp-directory-is-not-mapped-when-running-Parallel-Tests-in-a-Container-Wrapper) — globalTmp directory is not mapped when running Parallel Tests in a Container Wrapper
* [**TW-94078**](https://youtrack.jetbrains.com/issue/TW-94078/The-failedtests-table-is-not-cleaned-up) — The failed_tests table is not cleaned up
* [**TW-94077**](https://youtrack.jetbrains.com/issue/TW-94077/Cannot-calculate-incompatible-builds-Error-adding-build-problem-build-problem-ignored-identity-mustnt-be-longer-than-60) — Cannot calculate incompatible builds: Error adding build problem, build problem ignored: identity mustn't be longer than 60 characters
* [**TW-90742**](https://youtrack.jetbrains.com/issue/TW-90742/Kubernetes-Executor-failed-to-get-template-error-is-instantly-logging-but-executor-works) — Kubernetes Executor: "failed to get template" error is instantly logging but executor works
* [**TW-62950**](https://youtrack.jetbrains.com/issue/TW-62950/Sidebar-running-builds-counter-shows-builds-from-default-branch-only) — Sidebar running builds counter shows builds from default branch only
* [**TW-92217**](https://youtrack.jetbrains.com/issue/TW-92217/Kubernetes-executor-doesnt-fail-the-queued-build-with-missing-connection) — Kubernetes executor doesn't fail the queued build with missing connection
* [**TW-90021**](https://youtrack.jetbrains.com/issue/TW-90021/Kubernetes-Executor-builds-wont-be-started-warning-is-shown-in-agent-requirements) — Kubernetes Executor: "builds won't be started" warning is shown in agent requirements
* [**TW-81477**](https://youtrack.jetbrains.com/issue/TW-81477/Dark-theme-the-Perfmon-tab-is-hard-to-read) — Dark theme: the Perfmon tab is hard to read
* [**TW-35935**](https://youtrack.jetbrains.com/issue/TW-35935/Resource-lock-doesnt-prevent-start-of-cloud-images) — Resource lock doesn't prevent start of cloud images
* [**TW-92729**](https://youtrack.jetbrains.com/issue/TW-92729/Commit-Status-publisher-fires-for-skipped-tags) — Commit Status publisher fires for skipped tags
* [**TW-91542**](https://youtrack.jetbrains.com/issue/TW-91542/NuGet-Publish-plugin-depends-on-deprecated-.net-runners-mono-discovery) — NuGet Publish plugin depends on deprecated .net runners mono discovery
* [**TW-93252**](https://youtrack.jetbrains.com/issue/TW-93252/Symbol-server-plugin-XML-metadata-files-are-not-published) — Symbol server plugin: XML metadata files are not published
* [**TW-93539**](https://youtrack.jetbrains.com/issue/TW-93539/tools-subsystem-detects-the-same-tool-twice) — tools subsystem detects the same tool twice
* [**TW-92744**](https://youtrack.jetbrains.com/issue/TW-92744/Shared-resources-lock-added-to-the-build-configuration-should-not-affect-already-queued-builds) — Shared resources lock added to the build configuration should not affect already queued builds
* [**TW-88964**](https://youtrack.jetbrains.com/issue/TW-88964/VCS-with-Refreshable-Access-Token-for-GitHub.com-requires-manual-token-re-acquisition-every-few-hours) — VCS with Refreshable Access Token for GitHub.com requires manual token re-acquisition every few hours
* [**TW-90036**](https://youtrack.jetbrains.com/issue/TW-90036/On-return-from-user-details-users-list-looses-styling-header-and-footer) — On return from user details, users list looses styling, header and footer
* [**TW-80014**](https://youtrack.jetbrains.com/issue/TW-80014/Docker-Build-Feature-retry-failed-login-attempts) — Docker Build Feature: retry failed login attempts
* [**TW-93804**](https://youtrack.jetbrains.com/issue/TW-93804/Return-recipe-format-in-overview-response) — Return recipe format in overview response
* [**TW-93656**](https://youtrack.jetbrains.com/issue/TW-93656/Ghost-builds-on-external-storage-s3-cannot-be-find) — Ghost builds on external storage (s3) cannot be find
* [**TW-92465**](https://youtrack.jetbrains.com/issue/TW-92465/Public-Recipes-Improve-better-build-failure-message-for-the-cases-when-Marketplace-synchronisation-is-disabled) — Public Recipes: Improve better build failure message for the cases when Marketplace synchronisation is disabled
* [**TW-93628**](https://youtrack.jetbrains.com/issue/TW-93628/PluginStandaloneClassLoader-attempted-duplicate-class-definition-for-kotlin.ResultKt) — PluginStandaloneClassLoader attempted duplicate class definition for kotlin.ResultKt
* [**TW-90164**](https://youtrack.jetbrains.com/issue/TW-90164/Kubernetes-Executor-no-other-compatible-agents-can-start-a-build-if-executor-reaches-its-builds-limit) — Kubernetes Executor: no other compatible agents can start a build if executor reaches its builds limit
* [**TW-93699**](https://youtrack.jetbrains.com/issue/TW-93699/CME-in-TokenAuthenticationModelImpl-deleteAllUserTokensByType) — CME in TokenAuthenticationModelImpl deleteAllUserTokensByType
* [**TW-93256**](https://youtrack.jetbrains.com/issue/TW-93256/Token-Management-The-project-scope-component-doesnt-render-anymore) — Token Management: The project scope component doesn't render anymore
* [**TW-93533**](https://youtrack.jetbrains.com/issue/TW-93533/Investigation-history-Configuration-page-Investigation-History-action-doesnt-display-dialog) — [Investigation history] Configuration page -> Investigation History action doesn't display dialog
* [**TW-93514**](https://youtrack.jetbrains.com/issue/TW-93514/No-Need-to-Install-SAML-Plugin-on-TeamCity-Cloud) — No Need to Install SAML Plugin on TeamCity Cloud
* [**TW-93235**](https://youtrack.jetbrains.com/issue/TW-93235/Build-Cache-discrepancy-in-the-order-of-operations-between-docs-and-behavior) — Build Cache - discrepancy in the order of operations between docs and behavior
* [**TW-92482**](https://youtrack.jetbrains.com/issue/TW-92482/Public-Recipes-add-a-way-to-reset-the-recipes-cache-in-the-UI) — Public Recipes: add a way to reset the recipes cache in the UI
* [**TW-93164**](https://youtrack.jetbrains.com/issue/TW-93164/Too-many-scrolls-and-unaligned-Search-field-in-the-Configure-Favorites-dialog) — Too many scrolls and unaligned Search field in the "Configure Favorites" dialog
* [**TW-91389**](https://youtrack.jetbrains.com/issue/TW-91389/Presigned-URL-TTL-for-downloads) — Presigned URL TTL for downloads
* [**TW-91690**](https://youtrack.jetbrains.com/issue/TW-91690/Agent-side-git-mirror-symlink-can-be-deleted-in-some-cases) — Agent-side git mirror symlink can be deleted in some cases
* [**TW-84369**](https://youtrack.jetbrains.com/issue/TW-84369/DSL-subProject-is-ignored-if-used-along-with-subProjects) — [DSL] subProject(…) is ignored if used along with subProjects(…)
* [**TW-93340**](https://youtrack.jetbrains.com/issue/TW-93340/Edit-Mode-health-items-overlaps-other-controllers-for-a-build-configuration-with-long-name) — Edit Mode: health items overlaps other controllers for a build configuration with long name
* [**TW-93339**](https://youtrack.jetbrains.com/issue/TW-93339/Edit-mode-long-template-name-overlaps-the-UI-View-as-code-controller) — Edit mode: long template name overlaps the UI/View as code controller
* [**TW-93449**](https://youtrack.jetbrains.com/issue/TW-93449/Virtual-subproject-is-not-archived-when-parent-project-is-archived) — Virtual subproject is not archived, when parent project is archived
* [**TW-74828**](https://youtrack.jetbrains.com/issue/TW-74828/No-easy-way-to-copy-a-branch-name) — No easy way to copy a branch name
* [**TW-91477**](https://youtrack.jetbrains.com/issue/TW-91477/Subnavigation-inconsistent-behavior) — Subnavigation: inconsistent behavior
* [**TW-93036**](https://youtrack.jetbrains.com/issue/TW-93036/Make-the-VCS-Auth-token-project-tab-available-to-project-admins-only) — Make the VCS Auth token project tab available to project admins only
* [**TW-83760**](https://youtrack.jetbrains.com/issue/TW-83760/DSL-Context-Parameters-update-on-secondary-node-has-no-effect) — DSL Context Parameters update on secondary node has no effect
* [**TW-89535**](https://youtrack.jetbrains.com/issue/TW-89535/Agent-status-Agent-termination-postponed-due-to-1-active-remote-terminal-session.-is-shown-even-if-no-terminal-conditions-were) — Agent status "Agent termination postponed due to 1 active remote terminal session." is shown even if no terminal conditions were met
* [**TW-92982**](https://youtrack.jetbrains.com/issue/TW-92982/Build-Queue-Optimization-by-TeamCity-doesnt-work-with-parallelTests) — Build Queue Optimization by TeamCity doesnt work with parallelTests
* [**TW-88866**](https://youtrack.jetbrains.com/issue/TW-88866/Pinning-build-with-Apply-to-all-snapshot-dependencies-doesnt-pin-some-dependency-builds) — Pinning build with 'Apply to all snapshot dependencies' doesn't pin some dependency builds
* [**TW-73478**](https://youtrack.jetbrains.com/issue/TW-73478/Build-branch-width-in-custom-run-dialog) — Build branch width in custom run dialog
* [**TW-92786**](https://youtrack.jetbrains.com/issue/TW-92786/Memory-leak-in-case-of-active-usage-of-requirements-with-regexp-pattern-for-the-cloud-agent-name) — Memory leak in case of active usage of requirements with regexp pattern for the cloud agent name
* [**TW-91182**](https://youtrack.jetbrains.com/issue/TW-91182/next-major-TeamCity-version-TeamCity-2024.11-should-be-updated-in-the-help-document) — next major TeamCity version (TeamCity 2024.11) should be updated in the help document
* [**TW-92133**](https://youtrack.jetbrains.com/issue/TW-92133/Test-reports-for-package-names-starting-with-a-digit) — Test reports for package names starting with a digit
* [**TW-92667**](https://youtrack.jetbrains.com/issue/TW-92667/TeamCity-artifact-excluding-rule-no-longer-works-in-version-2025.03) — TeamCity artifact excluding rule no longer works in version 2025.03
* [**TW-78520**](https://youtrack.jetbrains.com/issue/TW-78520/Dead-agent-in-the-builds-history-table-instead-of-an-actual-agent-name) — "Dead agent" in the builds history table instead of an actual agent name
* [**TW-92704**](https://youtrack.jetbrains.com/issue/TW-92704/gitHubChecks.-Names-of-the-checks-are-too-long) — gitHubChecks. Names of the checks are too long
* [**TW-92785**](https://youtrack.jetbrains.com/issue/TW-92785/Possible-deadlock-when-enabling-MainNode-reproduced-in-tests) — Possible deadlock when enabling MainNode, reproduced in tests
* [**TW-92645**](https://youtrack.jetbrains.com/issue/TW-92645/Always-show-Tokens-tab-in-Versioned-Settings-menu) — Always show "Tokens" tab in Versioned Settings menu
* [**TW-92139**](https://youtrack.jetbrains.com/issue/TW-92139/jetbrains.buildServer.metrics.ServerMetrics-should-support-metric-removal) — jetbrains.buildServer.metrics.ServerMetrics should support metric removal
* [**TW-81859**](https://youtrack.jetbrains.com/issue/TW-81859/Retry-failed-agent-side-git-checkout) — Retry failed agent side git checkout
* [**TW-92450**](https://youtrack.jetbrains.com/issue/TW-92450/S3-artifact-cleanup-failure-may-cause-NoSuchElementException) — S3 artifact cleanup failure may cause NoSuchElementException
* [**TW-92401**](https://youtrack.jetbrains.com/issue/TW-92401/GitHub-App-connection-doesnt-support-a-large-number-of-installations) — GitHub App connection doesn't support a large number of installations
* [**TW-91034**](https://youtrack.jetbrains.com/issue/TW-91034/Incorrrect-gitcustomcertificates.crt-is-not-recreated-after-error-Problem-with-the-SSL-CA-cert-path-access-rights) — Incorrrect git_custom_certificates.crt is not recreated after error "Problem with the SSL CA cert (path? access rights?)"
* [**TW-88309**](https://youtrack.jetbrains.com/issue/TW-88309/No-way-to-configure-Dependency-Cache-build-feature-using-Kotlin-DSL) — No way to configure Dependency Cache build feature using Kotlin DSL

### Performance Problem

* [**TW-94825**](https://youtrack.jetbrains.com/issue/TW-94825/Slow-loading-of-the-composite-build-tabs-if-composite-build-has-artifact-dependencies-and-there-are-several-report-tabs) — Slow loading of the composite build tabs if composite build has artifact dependencies and there are several report tabs configured
* [**TW-52336**](https://youtrack.jetbrains.com/issue/TW-52336/Loading-result-of-artifact-tab-for-composite-builds-is-not-reused) — Loading result of artifact tab for composite builds is not reused
* [**TW-72773**](https://youtrack.jetbrains.com/issue/TW-72773/Potential-memory-leak-in-InMemoryTokenContextDescriptionStorage) — Potential memory leak in InMemoryTokenContextDescriptionStorage
* [**TW-73703**](https://youtrack.jetbrains.com/issue/TW-73703/Potential-memory-leak-in-HealthStatusReportBean) — Potential memory leak in HealthStatusReportBean
* [**TW-94456**](https://youtrack.jetbrains.com/issue/TW-94456/Large-Build-settings-have-not-been-finalized-wait-reason-in-case-when-several-queued-build-chains-reuse-the-same-finished-build) — Large "Build settings have not been finalized" wait reason in case when several queued build chains reuse the same finished build
* [**TW-91020**](https://youtrack.jetbrains.com/issue/TW-91020/Do-not-request-ancestor-projects-for-the-Sidebar-Tree) — Do not request ancestor projects for the Sidebar Tree
* [**TW-94139**](https://youtrack.jetbrains.com/issue/TW-94139/Excessive-number-of-template-usages-calculation-can-slowdown-project-editing-page-of-a-large-project) — Excessive number of template usages calculation can slowdown project editing page of a large project
* [**TW-92738**](https://youtrack.jetbrains.com/issue/TW-92738/Slow-changes-collecting-for-configurations-with-many-VCS-Roots-in-chain) — Slow changes collecting for configurations with many VCS Roots in chain
* [**TW-93536**](https://youtrack.jetbrains.com/issue/TW-93536/Inefficient-tokens-storage-for-Google-authentication-can-lead-to-non-functional-TeamCity-server) — Inefficient tokens storage for Google authentication can lead to non functional TeamCity server
* [**TW-93540**](https://youtrack.jetbrains.com/issue/TW-93540/Slow-trigger-activation-when-project-is-reloaded-from-disk-can-cause-multi-node-events-lag) — Slow trigger activation when project is reloaded from disk can cause multi node events lag
* [**TW-92223**](https://youtrack.jetbrains.com/issue/TW-92223/Test-occurrences-request-can-cause-an-out-of-memory-error) — Test occurrences request can cause an out of memory error
* [**TW-89588**](https://youtrack.jetbrains.com/issue/TW-89588/Inefficient-search-of-agents-by-agent-pool-in-REST-API) — Inefficient search of agents by agent pool in REST API
* [**TW-80339**](https://youtrack.jetbrains.com/issue/TW-80339/Slow-loading-of-the-agent-types-from-the-database-affects-classic-agent-pools-page-and-build-queue-processing) — Slow loading of the agent types from the database (affects classic agent pools page and build queue processing)
* [**TW-93415**](https://youtrack.jetbrains.com/issue/TW-93415/Slow-processing-of-build-triggers-on-a-server-with-large-number-of-build-configurations) — Slow processing of build triggers on a server with large number of build configurations

### Security

16 security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [**Security Bulletin**](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2025.07). Security bulletins for new versions are typically published within the next few days after the release date.