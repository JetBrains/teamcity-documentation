[//]: # (title: TeamCity 2023.11 Release Notes)
[//]: # (auxiliary-id: TeamCity 2023.11 Release Notes)


**Build 147318, 24 November 2023**


<!--project: TeamCity Fix versions: {2023.11 (147318)} #Fixed visible to: {All Users} -{Trunk issue}-->

### Feature

**[TW-72912](https://youtrack.jetbrains.com/issue/TW-72912/Ability-to-deserialize-a-json-to-an-object-in-Kotlin-DSL)** — Ability to deserialize a json to an object in Kotlin DSL

**[TW-84103](https://youtrack.jetbrains.com/issue/TW-84103/Agent-terminal-UI-rework)** — Agent terminal: UI rework

**[TW-3661](https://youtrack.jetbrains.com/issue/TW-3661/Matrix-build-configuration)** — Matrix build configuration

**[TW-76903](https://youtrack.jetbrains.com/issue/TW-76903/Agent-distributions-with-bundled-java)** — Agent distributions with bundled java

**[TW-42533](https://youtrack.jetbrains.com/issue/TW-42533/Versioned-Settings-for-a-build-Use-snapshot-dependencies-stored-in-VCS)** — Versioned Settings for a build - Use snapshot dependencies stored in VCS

**[TW-80527](https://youtrack.jetbrains.com/issue/TW-80527/.NET-parallel-tests-experimental-filtering-test-suppression)** — .NET parallel tests: experimental filtering (test suppression)

**[TW-80480](https://youtrack.jetbrains.com/issue/TW-80480/Intentionally-propagate-AWS-connection-to-child-projects)** — Intentionally propagate AWS connection to child projects

**[TW-80303](https://youtrack.jetbrains.com/issue/TW-80303/Project-administrators-should-be-able-to-forbid-using-AWS-connection-to-expose-credentials-to-build-steps.)** — Project administrators should be able to forbid using AWS connection to expose credentials to build steps.

**[TW-45503](https://youtrack.jetbrains.com/issue/TW-45503/A-way-to-run-history-build-on-non-current-revisions-from-previous-VCS-root-pointing-to-the-same-repository-after-VCS-root)** — A way to run history build on non-current revisions (from previous VCS root pointing to the same repository) after VCS root changes

**[TW-41671](https://youtrack.jetbrains.com/issue/TW-41671/Schedule-a-build-in-custom-run-dialog)** — Schedule a build in custom run dialog

**[TW-84809](https://youtrack.jetbrains.com/issue/TW-84809/Artifact-based-Build-caches-Build-feature-for-TeamCity-on-premise)** — Artifact-based Build caches Build feature for TeamCity on-premise

**[TW-66908](https://youtrack.jetbrains.com/issue/TW-66908/Favorite-Agent-Pools)** — Favorite Agent Pools

**[TW-55612](https://youtrack.jetbrains.com/issue/TW-55612/History-for-deleted-cloud-agent)** — History for deleted cloud agent

**[TW-80385](https://youtrack.jetbrains.com/issue/TW-80385/Enable-viewing-of-connection-IDs-in-the-admin-UI)** — Enable viewing of connection IDs in the admin UI

**[TW-82888](https://youtrack.jetbrains.com/issue/TW-82888/Build-agent-priorities-Amazon-EC2)** — Build agent priorities (Amazon EC2)

**[TW-82623](https://youtrack.jetbrains.com/issue/TW-82623/Allow-creating-custom-parameter-with-type-password-during-build-start)** — Allow creating custom parameter with type 'password' during build start

**[TW-69433](https://youtrack.jetbrains.com/issue/TW-69433/Support-Perforce-incremental-checkout-on-cloud-agents-when-the-VCS-Root-uses-streams-client-mapping)** — Support Perforce incremental checkout on cloud agents when the VCS Root uses streams/client mapping

**[TW-82141](https://youtrack.jetbrains.com/issue/TW-82141/Support-OAuth-tokens-in-configuration-of-Commit-Status-Publisher-for-Azure-DevOps)** — Support OAuth tokens in configuration of Commit Status Publisher for Azure DevOps

**[TW-82072](https://youtrack.jetbrains.com/issue/TW-82072/Add-Access-Tokens-support-to-Bitbucket-Cloud-integration-for-Pull-Requests-Build-Feature)** — Add Access Tokens support to Bitbucket Cloud integration for Pull Requests Build Feature

**[TW-72093](https://youtrack.jetbrains.com/issue/TW-72093/Allow-to-configure-P4-workspace-name-used-by-TeamCity-for-agent-side-checkout)** — Allow to configure P4 workspace name used by TeamCity for agent-side checkout

**[TW-70782](https://youtrack.jetbrains.com/issue/TW-70782/Provide-links-to-Space-merge-requests-from-the-TeamCity-web-UI)** — Provide links to Space merge requests from the TeamCity web UI

**[TW-64640](https://youtrack.jetbrains.com/issue/TW-64640/Support-ignoring-Draft-merge-requests-in-GitLab)** — Support ignoring Draft merge requests in GitLab

**[TW-64696](https://youtrack.jetbrains.com/issue/TW-64696/Pull-Requests-for-Azure-DevOps-choosing-access-token-from-connections)** — Pull Requests for Azure DevOps: choosing access token from connections

**[TW-80451](https://youtrack.jetbrains.com/issue/TW-80451/HashiCorp-Remote-Parameter)** — HashiCorp Remote Parameter

**[TW-81640](https://youtrack.jetbrains.com/issue/TW-81640/Allow-to-pass-TeamCity-server-responsibilities-via-command-line)** — Allow to pass TeamCity server responsibilities via command line

**[TW-82139](https://youtrack.jetbrains.com/issue/TW-82139/Support-OAuth-tokens-in-configuration-of-Pull-Request-Build-Feature-for-Bitbucket-Server-Data-Center)** — Support OAuth tokens in configuration of Pull Request Build Feature for Bitbucket Server / Data Center

**[TW-82143](https://youtrack.jetbrains.com/issue/TW-82143/Add-Use-VCS-Root-credentials-authentication-type-in-Commit-Status-Publisher-settings-for-Bitbucket-Server-Data-Center)** — Add "Use VCS Root credentials" authentication type in Commit Status Publisher settings for Bitbucket Server / Data Center

**[TW-82144](https://youtrack.jetbrains.com/issue/TW-82144/Add-Use-VCS-Root-credentials-authentication-type-in-Commit-Status-Publisher-settings-for-Bitbucket-Cloud)** — Add "Use VCS Root credentials" authentication type in Commit Status Publisher settings for Bitbucket Cloud

**[TW-82146](https://youtrack.jetbrains.com/issue/TW-82146/Add-Use-VCS-Root-credentials-authentication-type-in-Commit-Status-Publisher-settings-for-GitLab)** — Add "Use VCS Root credentials" authentication type in Commit Status Publisher settings for GitLab

**[TW-81073](https://youtrack.jetbrains.com/issue/TW-81073/Add-stop-instance-after-build-is-finished-REST-API-method)** — Add "stop instance after build is finished" REST API method

**[TW-82556](https://youtrack.jetbrains.com/issue/TW-82556/Add-prometheus-metric-for-agentless-use)** — Add prometheus metric for agentless use

**[TW-83182](https://youtrack.jetbrains.com/issue/TW-83182/Add-an-easy-way-to-save-existing-AWS-S3-Settings-from-old-S3-connections-to-AWS-Connection)** — Add an easy way to save existing AWS S3 Settings from old S3 connections to AWS Connection

**[TW-83288](https://youtrack.jetbrains.com/issue/TW-83288/Use-VCS-Root-fetch-url-host-for-Bitbucket-Data-Center-Commit-Status-publisher-if-Bitbucket-Data-Center-URL-isnt-defined)** — Use VCS Root fetch url host for Bitbucket Data Center Commit Status publisher if Bitbucket Data Center URL isn't defined

**[TW-83849](https://youtrack.jetbrains.com/issue/TW-83849/Provide-links-to-Space-code-reviews-from-TeamCity-web-UI)** — Provide links to Space code reviews from TeamCity web UI

**[TW-81980](https://youtrack.jetbrains.com/issue/TW-81980/Simplify-creation-of-GitHub-App-and-installation-via-manifest)** — Simplify creation of GitHub App and installation via manifest

**[TW-83399](https://youtrack.jetbrains.com/issue/TW-83399/Allow-to-start-TeamCity-with-database-settings-passed-via-environment-variables)** — Allow to start TeamCity with database settings passed via environment variables

**[TW-80648](https://youtrack.jetbrains.com/issue/TW-80648/S3-storage-Integrity-verification-check)** — S3 storage: Integrity verification check

**[TW-65866](https://youtrack.jetbrains.com/issue/TW-65866/Bold-changes-indicator-in-new-favorites-tree)** — Bold changes indicator in new favorites tree

**[TW-7852](https://youtrack.jetbrains.com/issue/TW-7852/Ability-to-display-relative-times-start-build-based-in-the-build-log)** — Ability to display relative times (start build based) in the build log


### Bug

**[TW-80488](https://youtrack.jetbrains.com/issue/TW-80488/Artifact-dependencies-setting-from-VCS-are-not-handled-correctly-when-modified-in-branch)** — Artifact dependencies setting from VCS are not handled correctly when modified in branch

**[TW-85218](https://youtrack.jetbrains.com/issue/TW-85218/Matrix-build-with-execution-timeout-in-DSL-feature-branch-fails-with-unclear-error-Failed-to-resolve-artifact-dependencies)** — Matrix build with execution timeout in DSL feature branch fails with unclear error "Failed to resolve artifact dependencies"

**[TW-84576](https://youtrack.jetbrains.com/issue/TW-84576/Matrix-builds-errors-not-showing-on-build-feature-form-when-saving)** — Matrix builds: errors not showing on build feature form when saving

**[TW-81525](https://youtrack.jetbrains.com/issue/TW-81525/cropped-expand-collapse-buttons-in-build-logs-in-Safari)** — cropped expand/collapse buttons in build logs in Safari

**[TW-84550](https://youtrack.jetbrains.com/issue/TW-84550/Unconditional-commit-status-publishing-is-executed-for-virtual-build-configurations)** — Unconditional commit status publishing is executed for virtual build configurations

**[TW-81524](https://youtrack.jetbrains.com/issue/TW-81524/Queued-Parallel-tests-Matrix-build-triggered-by-VCS-trigger-is-not-replaced-with-a-more-recent-queued-build)** — Queued Parallel tests/Matrix build triggered by VCS trigger is not replaced with a more recent queued build

**[TW-84843](https://youtrack.jetbrains.com/issue/TW-84843/Matrix-build-feature-is-present-after-changing-configuration-type-from-Regular-to-Composite)** — Matrix build feature is present after changing configuration type from Regular to Composite

**[TW-79948](https://youtrack.jetbrains.com/issue/TW-79948/Users-with-no-access-to-the-repository-from-VCS-root-can-acquire-new-token-for-this-repository-except-for-Azure-DevOps)** — Users with no access to the repository from VCS root, can acquire new token for this repository (except for Azure DevOps)

**[TW-84155](https://youtrack.jetbrains.com/issue/TW-84155/Vault-Test-Connection-button-returns-connection-successful-on-empty-fields)** — Vault: Test Connection button returns connection successful on empty fields

**[TW-84825](https://youtrack.jetbrains.com/issue/TW-84825/Build-triggered-on-a-branch-which-exists-only-in-the-settings-VCS-root-fails)** — Build triggered on a branch which exists only in the settings VCS root fails

**[TW-83303](https://youtrack.jetbrains.com/issue/TW-83303/EC2-Cloud-Profile-instantly-terminates-Spot-Fleet-instance-if-instance-types-were-not-manually-specified-in-the-config)** — EC2 Cloud Profile instantly terminates Spot Fleet instance if instance types were not manually specified in the config

**[TW-74905](https://youtrack.jetbrains.com/issue/TW-74905/Select-agent-in-Custom-run-dialog-for-Parallel-Tests-build-has-no-effect)** — Select agent in Custom run dialog for Parallel Tests build has no effect

**[TW-83841](https://youtrack.jetbrains.com/issue/TW-83841/ReSharper-Inspections-change-Download-File-and-Folder-options-in-the-R-CTL-Plugins-hint-to-the-new-ID-format)** — ReSharper Inspections: change "Download", "File" and "Folder" options in the R# CTL Plugins hint to the new ID format

**[TW-60493](https://youtrack.jetbrains.com/issue/TW-60493/Versioned-settings-allow-to-change-vcs-roots-in-feature-branches)** — Versioned settings: allow to change vcs roots in feature branches

**[TW-55164](https://youtrack.jetbrains.com/issue/TW-55164/Windows-agent-doesnt-support-windows-10-native-ssh-agent)** — Windows agent doesn't support windows 10 native ssh agent

**[TW-60923](https://youtrack.jetbrains.com/issue/TW-60923/Agents-restart-after-updating-tools-on-Server)** — Agents restart after updating tools on Server

**[TW-85137](https://youtrack.jetbrains.com/issue/TW-85137/UnexpectedDBException-in-teamcity-server.log)** — UnexpectedDBException in teamcity-server.log

**[TW-85103](https://youtrack.jetbrains.com/issue/TW-85103/Space-Application-creation-did-not-succeed)** — Space Application creation did not succeed

**[TW-84140](https://youtrack.jetbrains.com/issue/TW-84140/Slack-notifications-are-not-working-for-terminated-builds)** — Slack notifications are not working for terminated builds

**[TW-84317](https://youtrack.jetbrains.com/issue/TW-84317/System-problems-reported-by-pull-request-feature-are-not-cleared-if-multiple-features-use-the-same-vcs-url)** — System problems reported by pull request feature are not cleared if multiple features use the same vcs url

**[TW-84357](https://youtrack.jetbrains.com/issue/TW-84357/Matrix-builds-No-compatible-agents-message-on-manual-build-Run-if-agent-requirements-contain-matrix-parameters)** — Matrix builds: "No compatible agents" message on manual build Run, if agent requirements contain matrix parameters

**[TW-79776](https://youtrack.jetbrains.com/issue/TW-79776/No-escaping-of-values-what-used-during-labelling-build-sources)** — No escaping of values what used during labelling build sources

**[TW-84396](https://youtrack.jetbrains.com/issue/TW-84396/Deprecated-java-health-report-always-leads-to-the-recent-documentation-version)** — Deprecated java health report always leads to the recent documentation version

**[TW-55960](https://youtrack.jetbrains.com/issue/TW-55960/Secondary-node-shows-zero-build-agents-counter-when-main-server-starting-up-after-graceful-shutdown)** — Secondary node shows zero build agents counter when main server starting up after graceful shutdown

**[TW-84073](https://youtrack.jetbrains.com/issue/TW-84073/Page-content-should-not-be-aligned-centrally-on-the-changes-page)** — Page content should not be aligned centrally on the changes page

**[TW-85087](https://youtrack.jetbrains.com/issue/TW-85087/Server-may-send-agent-for-upgrade-before-it-is-fully-initialized)** — Server may send agent for upgrade before it is fully initialized

**[TW-85014](https://youtrack.jetbrains.com/issue/TW-85014/Gradle-runner-plugin-size-increased-20x-times)** — Gradle runner plugin size increased 20x times

**[TW-82564](https://youtrack.jetbrains.com/issue/TW-82564/TeamCity-REST-API-may-occasionally-provide-artifacts-of-another-build-of-the-same-build-configuration)** — TeamCity REST API may occasionally provide artifacts of another build of the same build configuration

**[TW-79307](https://youtrack.jetbrains.com/issue/TW-79307/What-is-the-wait-reason-Overflow)** — What is the wait reason "Overflow"?

**[TW-84049](https://youtrack.jetbrains.com/issue/TW-84049/JaCoCo-coverage-isnt-shown-without-any-error)** — JaCoCo coverage isn't shown without any error

**[TW-81527](https://youtrack.jetbrains.com/issue/TW-81527/Open-Terminal-button-is-shown-only-on-the-Agent-Summary-tab)** — Open Terminal button is shown only on the Agent Summary tab

**[TW-84910](https://youtrack.jetbrains.com/issue/TW-84910/S3-Upload-Signed-URL-TTL-expiration-causes-Upload-Failure)** — [S3 Upload] Signed URL TTL expiration causes Upload Failure

**[TW-83559](https://youtrack.jetbrains.com/issue/TW-83559/Change-Bitbucket-API-URL-from-https-bitbucket.org-api-2.0-to-https-api.bitbucket.org-2.0-in-Pull-Request-Plugin)** — Change Bitbucket API URL from https://bitbucket.org/api/2.0/ to https://api.bitbucket.org/2.0/ in Pull Request Plugin

**[TW-84268](https://youtrack.jetbrains.com/issue/TW-84268/New-EC2-UI-Show-image-and-instance-names-before-IDs-and-sort-the-lists-by-name)** — New EC2 UI: Show image and instance names before IDs and sort the lists by name

**[TW-84962](https://youtrack.jetbrains.com/issue/TW-84962/Kotlin-DSL-local-debugging-is-broken)** — Kotlin DSL local debugging is broken

**[TW-83158](https://youtrack.jetbrains.com/issue/TW-83158/Commit-Status-Publisher-doesnt-remove-Queued-status-for-personal-builds-from-Bitbucket-Cloud)** — Commit Status Publisher doesn't remove Queued status for personal builds from Bitbucket Cloud

**[TW-84177](https://youtrack.jetbrains.com/issue/TW-84177/The-checkbox-Allow-any-OAuth-user-to-log-in-in-Authentication-modules-settings-can-be-confusing)** — The checkbox "Allow any OAuth user to log in" in Authentication modules settings can be confusing

**[TW-77914](https://youtrack.jetbrains.com/issue/TW-77914/Maven-error-generating-configs-via-Kotlin-openjdk-18-The-Security-Manager-is-deprecated)** — Maven error generating configs via Kotlin + openjdk-18: "The Security Manager is deprecated"

**[TW-82848](https://youtrack.jetbrains.com/issue/TW-82848/S3-Storage-new-UI-Improve-error-message-shown-in-S3-storage-settings-when-broken-credentials-are-used)** — S3 Storage new UI: Improve error message shown in S3 storage settings when broken credentials are used

**[TW-83185](https://youtrack.jetbrains.com/issue/TW-83185/S3-Storage-new-UI-cant-save-updated-Custom-S3-Storage-settings-without-rechoosing-the-bucket-if-incorrect-Endpoint-was-used)** — S3 Storage new UI: can't save updated Custom S3 Storage settings without rechoosing the bucket if incorrect Endpoint was used before

**[TW-84932](https://youtrack.jetbrains.com/issue/TW-84932/Inefficient-caching-in-Perforce-changes-collecting)** — Inefficient caching in Perforce changes collecting

**[TW-76456](https://youtrack.jetbrains.com/issue/TW-76456/Do-not-allow-duplicate-cloud-profile-ids-in-the-Kotlin-DSL)** — Do not allow duplicate cloud profile ids in the Kotlin DSL

**[TW-84915](https://youtrack.jetbrains.com/issue/TW-84915/Artifacts-of-a-nested-composite-build-cant-be-downloaded-if-domain-isolation-feature-is-enabled)** — Artifacts of a nested composite build can't be downloaded if domain isolation feature is enabled

**[TW-60435](https://youtrack.jetbrains.com/issue/TW-60435/System-problem-reported-by-Pull-Requests-plugin-does-not-disappear-after-the-corresponding-build-feature-is-disabled-or-removed)** — System problem reported by Pull Requests plugin does not disappear after the corresponding build feature is disabled or removed in/from a build configuration

**[TW-71177](https://youtrack.jetbrains.com/issue/TW-71177/Exception-in-the-RunType.PropertiesProcessor-process-method-for-one-of-the-queued-builds-can-prevent-start-of-other-queued)** — Exception in the RunType.PropertiesProcessor process() method for one of the queued builds can prevent start of other queued builds

**[TW-83801](https://youtrack.jetbrains.com/issue/TW-83801/Test-connection-for-GitHub-App-connection-should-check-installation-functionality-not-application)** — Test connection for GitHub App connection should check installation functionality, not application

**[TW-83942](https://youtrack.jetbrains.com/issue/TW-83942/Project-disappears-and-Critical-error-in-configuration-file-is-shown-after-restarting-the-server-when-a-project-is-copied-with)** — Project disappears and "Critical error in configuration file" is shown after restarting the server when a project is copied with errors

**[TW-83763](https://youtrack.jetbrains.com/issue/TW-83763/Teamcity-failed-to-start-with-new-PSQL-16-ERROR-unrecognized-configuration-parameter-lccollate)** — Teamcity failed to start with new PSQL 16, ERROR: unrecognized configuration parameter "lc_collate"

**[TW-51774](https://youtrack.jetbrains.com/issue/TW-51774/Multiple-Test-Connection-buttons-on-any-of-the-Add-connection-dialog)** — Multiple 'Test Connection' buttons on any of the Add connection dialog

**[TW-82764](https://youtrack.jetbrains.com/issue/TW-82764/Agent-JDKs-add-download-progress)** — Agent JDKs: add download progress

**[TW-83850](https://youtrack.jetbrains.com/issue/TW-83850/Azure-DevOps-refreshable-token-info-isnt-shown-in-VCS-Root-page-when-token-was-issue-via-connection-icon)** — Azure DevOps refreshable token info isn't shown in VCS Root page when token was issue via connection icon

**[TW-83253](https://youtrack.jetbrains.com/issue/TW-83253/GitHub-App-Improve-error-messages-appearing-during-usage-of-the-GitHub-App-Connection.-Part3)** — GitHub App: Improve error messages appearing during usage of the GitHub App Connection. Part3

**[TW-84225](https://youtrack.jetbrains.com/issue/TW-84225/Unexpected-Error-in-attempt-to-install-the-GitHub-App-to-GitHub-Enterprise-if-SSL-certificate-wasnt-loaded-or-there-is-no)** — "Unexpected Error" in attempt to install the GitHub App to GitHub Enterprise if SSL certificate wasn't loaded or there is no connection to GtHub Enterprise Server

**[TW-84485](https://youtrack.jetbrains.com/issue/TW-84485/Build-Schedule-Canceled-scheduled-build-is-shown-as-History)** — Build Schedule: Canceled scheduled build is shown as History

**[TW-83208](https://youtrack.jetbrains.com/issue/TW-83208/Matrix-build-preferred-templates-parameters-instead-of-configurations-parameters)** — Matrix build preferred template's parameters instead of configurations parameters

**[TW-72890](https://youtrack.jetbrains.com/issue/TW-72890/VCS-root-and-build-log-should-warn-when-a-perforce-client-doesnt-exist)** — VCS root and build log should warn when a perforce client doesn't exist

**[TW-83628](https://youtrack.jetbrains.com/issue/TW-83628/Build-caches-Publish-only-if-changed-option-does-not-work-if-build-does-not-download-the-cache)** — Build caches: "Publish only if changed" option does not work if build does not download the cache

**[TW-84339](https://youtrack.jetbrains.com/issue/TW-84339/Cannot-open-artifacts-folder-with-square-brackets-from-the-UI)** — Cannot open artifacts folder with square brackets from the UI

**[TW-84375](https://youtrack.jetbrains.com/issue/TW-84375/Investigation-Auto-Assigner-doesnt-assign-anyone-when-composite-build-fails)** — Investigation Auto Assigner doesn't assign anyone when composite build fails

**[TW-84596](https://youtrack.jetbrains.com/issue/TW-84596/Failure-to-unzip-a-tool-on-an-agent-can-cause-permanent-failure-to-install-it)** — Failure to unzip a tool on an agent can cause permanent failure to install it

**[TW-53765](https://youtrack.jetbrains.com/issue/TW-53765/runAs-unnecessary-ERROR-in-teamcity-agent.log-when-runAs-plugin-is-installed)** — runAs: unnecessary ERROR in teamcity-agent.log when runAs plugin is installed

**[TW-83264](https://youtrack.jetbrains.com/issue/TW-83264/Configurations-with-disabled-cleanup-rules-prevent-artifact-dependencies-cleanup)** — Configurations with disabled cleanup rules prevent artifact dependencies cleanup

**[TW-83885](https://youtrack.jetbrains.com/issue/TW-83885/Avoid-using-Normal-executor-thread-pool-for-upgrade-isLocal-agent-commands)** — Avoid using Normal executor thread pool for upgrade/isLocal agent commands

**[TW-71098](https://youtrack.jetbrains.com/issue/TW-71098/Build-not-grabbing-latest-revisions-from-P4)** — Build not grabbing latest revisions from P4

**[TW-84065](https://youtrack.jetbrains.com/issue/TW-84065/Enable-testOnBorrow-and-validation-query-for-PostgreSQL-connections-by-default)** — Enable testOnBorrow and validation query for PostgreSQL connections by default

**[TW-83328](https://youtrack.jetbrains.com/issue/TW-83328/Inefficient-notifications-processing-for-user-groups)** — Inefficient notifications processing for user groups

**[TW-83312](https://youtrack.jetbrains.com/issue/TW-83312/Builds-fail-on-free-disk-space-stage)** — Builds fail on free disk space stage

**[TW-82939](https://youtrack.jetbrains.com/issue/TW-82939/vcsRoot.ID.shelvedChangelist-parameter-is-not-available)** — vcsRoot.&lt;ID>.shelvedChangelist parameter is not available

**[TW-72739](https://youtrack.jetbrains.com/issue/TW-72739/Branch-filter-should-not-be-available-on-Projects-Current-Problems-page.)** — Branch filter should not be available on Project's Current Problems page.

**[TW-83663](https://youtrack.jetbrains.com/issue/TW-83663/REST-API-builds-with-failedToStarttrue-dont-show-up-in-response-when-combined-with-propertynameXYZ)** — REST API - builds with "failedToStart:true" don't show up in response when combined with "property:(name:XYZ)"

**[TW-84278](https://youtrack.jetbrains.com/issue/TW-84278/S3-should-ensure-its-headers-will-only-be-fetched-on-AWS-agents)** — S3 should ensure its headers will only be fetched on AWS agents

**[TW-84125](https://youtrack.jetbrains.com/issue/TW-84125/Deadlock-in-HSQLMetadataStorage)** — Deadlock in HSQLMetadataStorage

**[TW-83971](https://youtrack.jetbrains.com/issue/TW-83971/Possibility-of-a-deadlock-in-OAuthTokensStorage)** — Possibility of a deadlock in OAuthTokensStorage

**[TW-83716](https://youtrack.jetbrains.com/issue/TW-83716/REST-API-plugin-does-not-see-ServerPaths-Spring-bean)** — REST API plugin does not see ServerPaths Spring bean

**[TW-84545](https://youtrack.jetbrains.com/issue/TW-84545/Unconditional-commit-status-publishing-is-executed-for-uninteresting-parts-of-build-chains)** — Unconditional commit status publishing is executed for uninteresting parts of build chains

**[TW-84583](https://youtrack.jetbrains.com/issue/TW-84583/Token-Authentication-is-not-supported-in-Use-VCS-root-s-credentials-setting-in-Pull-Requests-Build-feature-for-GitLab-if)** — Token Authentication is not supported in "Use VCS root(-s) credentials" setting in Pull Requests Build feature for GitLab if username is defined

**[TW-80096](https://youtrack.jetbrains.com/issue/TW-80096/Inconsistent-test-counts-when-using-dotnet-test-and-NUnit-adapter)** — Inconsistent test counts when using "dotnet test" and NUnit adapter

**[TW-84333](https://youtrack.jetbrains.com/issue/TW-84333/Matrix-builds-Empty-checkout-directory-if-checkout-rules-contain-matrix-parameter)** — Matrix builds: Empty checkout directory if checkout rules contain matrix parameter

**[TW-83181](https://youtrack.jetbrains.com/issue/TW-83181/S3-Storage-new-UI-Storage-settings-bucket-Transfer-speed-up-settings-reset-after-changing-Access-Key-details)** — S3 Storage new UI: Storage settings (bucket, Transfer speed-up settings) reset after changing Access Key details

**[TW-83709](https://youtrack.jetbrains.com/issue/TW-83709/Build.step.status-parameter-name-differs-from-step-ID-for-a-meta-runner-build-step)** — Build.step.status parameter name differs from step ID for a meta-runner build step

**[TW-83695](https://youtrack.jetbrains.com/issue/TW-83695/Copy-Build-Step-action-sets-old-style-Step-ID-RunnerX)** — Copy Build Step action sets old-style Step ID Runner_X

**[TW-84062](https://youtrack.jetbrains.com/issue/TW-84062/Auto-detected-build-steps-have-old-style-ID-RUNNERX)** — Auto-detected build steps have old style ID = RUNNER_X

**[TW-83463](https://youtrack.jetbrains.com/issue/TW-83463/All-public-repos-are-shown-during-new-Project-Build-Configuration-Creation-even-if-they-were-excluded-in-the-Repository-access)** — All public repos are shown during new Project/ Build Configuration Creation, even if they were excluded in the "Repository access" setting of GitHub App

**[TW-84399](https://youtrack.jetbrains.com/issue/TW-84399/IntelliJ-tool-installation-retry-check-if-the-distribution-is-already-downloaded)** — IntelliJ tool: installation retry, check if the distribution is already downloaded

**[TW-83451](https://youtrack.jetbrains.com/issue/TW-83451/REST-Builds-Count-1-gets-changed-to-Count-0-on-nexthref-resulting-in-internal-server-error)** — REST Builds Count -1 gets changed to Count 0 on nexthref resulting in internal server error

**[TW-84570](https://youtrack.jetbrains.com/issue/TW-84570/Newly-created-Space-organization-connections-always-start-as-pending)** — Newly created Space organization connections always start as pending

**[TW-82846](https://youtrack.jetbrains.com/issue/TW-82846/S3-Storage-new-UI-the-artifacts-published-using-Custom-S3-storages-cant-be-downloaded-from-TeamCity)** — S3 Storage new UI: the artifacts published using Custom S3 storages can't be downloaded from TeamCity

**[TW-84345](https://youtrack.jetbrains.com/issue/TW-84345/SpaceApplicationInformationManager.scheduleForConnection-is-invoked-from-projectRestored-and-fills-up-low-prio-executor-queue)** — SpaceApplicationInformationManager.scheduleForConnection is invoked from projectRestored and fills up low prio executor queue

**[TW-84180](https://youtrack.jetbrains.com/issue/TW-84180/Build-Schedule-scheduled-build-should-not-substitute-immediate-builds-from-the-queue)** — Build Schedule: scheduled build should not substitute immediate builds from the queue

**[TW-84186](https://youtrack.jetbrains.com/issue/TW-84186/StackOverflowError-when-trying-to-load-a-message-from-app-messages)** — StackOverflowError when trying to load a message from /app/messages

**[TW-84075](https://youtrack.jetbrains.com/issue/TW-84075/Nullable-href-in-Parameters-should-not-be-NotNull)** — Nullable href in Parameters should not be NotNull

**[TW-84323](https://youtrack.jetbrains.com/issue/TW-84323/Clear-Service-Worker-caches-on-Login-page)** — Clear Service Worker caches on Login page

**[TW-83626](https://youtrack.jetbrains.com/issue/TW-83626/S3-Storage-New-UI-Connection-field-can-be-broken-after-changing-the-Storage-Type)** — S3 Storage New UI: Connection field can be broken after changing the Storage Type

**[TW-83634](https://youtrack.jetbrains.com/issue/TW-83634/No-way-to-change-the-Secret-access-key-using-Edit-AWS-Connection-dialog-in-S3-Storage-Settings)** — No way to change the "Secret access key" using "Edit AWS Connection" dialog in S3 Storage Settings

**[TW-83234](https://youtrack.jetbrains.com/issue/TW-83234/Remote-parameters-DSL-generating-with-extra-empty-lines)** — Remote parameters DSL generating with extra empty lines

**[TW-83982](https://youtrack.jetbrains.com/issue/TW-83982/No-enum-constant-jetbrains.buildServer.clouds.kubernetes.connector.PodConditionType.podcondition)** — No enum constant jetbrains.buildServer.clouds.kubernetes.connector.PodConditionType.&lt;pod_condition>

**[TW-83829](https://youtrack.jetbrains.com/issue/TW-83829/Python-executable-could-be-found-but-not-reported-to-agent-parameter)** — Python executable could be found but not reported to agent parameter

**[TW-81847](https://youtrack.jetbrains.com/issue/TW-81847/Test-Connection-for-Azure-VCS-root-can-show-incorrect-status)** — Test Connection for Azure VCS root can show incorrect status

**[TW-83100](https://youtrack.jetbrains.com/issue/TW-83100/Add-a-hint-for-and-pencil-buttons-near-the-Connection-in-S3-Storage-page)** — Add a hint for `+` and pencil buttons near the Connection in S3 Storage page

**[TW-67979](https://youtrack.jetbrains.com/issue/TW-67979/Altering-dependency-options-in-non-default-branches-of-versioned-settings-may-not-take-any-effect)** — Altering dependency options in non-default branches of versioned settings may not take any effect

**[TW-54299](https://youtrack.jetbrains.com/issue/TW-54299/Wrong-agents-mentioned-on-Build-Duration-graph-with-range-All)** — Wrong agents mentioned on `Build Duration` graph with range `All`

**[TW-83178](https://youtrack.jetbrains.com/issue/TW-83178/S3-storage-new-UI-Error-popup-doesnt-close-automatically-after-fixing-the-settings)** — S3 storage new UI: Error popup doesn't close automatically after fixing the settings

**[TW-83101](https://youtrack.jetbrains.com/issue/TW-83101/New-Connection-popup-cant-be-closed-using-a-hotkey-if-the-cursor-is-in-one-of-the-fields)** — New Connection popup can't be closed using a hotkey if the cursor is in one of the fields

**[TW-84138](https://youtrack.jetbrains.com/issue/TW-84138/Undefined-build-log-filter-after-opening-another-build-log-with-Verbose-filter)** — "Undefined" build log filter after opening another build log with "Verbose" filter

**[TW-83179](https://youtrack.jetbrains.com/issue/TW-83179/Cant-save-AWS-Connection-with-incorrect-STS-endpoint-from-S3-Storage-even-if-Issue-temporary-credentials-by-request-was-disabled)** — Can't save AWS Connection with incorrect STS endpoint from S3 Storage even if "Issue temporary credentials by request" was disabled

**[TW-83806](https://youtrack.jetbrains.com/issue/TW-83806/AMI-template-AWS-name-in-Cloud-Images-list-is-loaded-with-visible-delay)** — AMI/template AWS name in Cloud Images list is loaded with visible delay

**[TW-83640](https://youtrack.jetbrains.com/issue/TW-83640/rest-api-java.lang.IllegalArgumentException-Comparison-method-violates-its-general-contract)** — [rest-api] java.lang.IllegalArgumentException: Comparison method violates its general contract!

**[TW-71473](https://youtrack.jetbrains.com/issue/TW-71473/Build-triggers-responsibility-sometimes-does-not-redistribute-triggers-among-nodes)** — Build triggers responsibility sometimes does not redistribute triggers among nodes

**[TW-83821](https://youtrack.jetbrains.com/issue/TW-83821/Refreshable-tokens-are-not-working-in-projects-if-they-were-originally-issued-in-other-non-parent-projects)** — Refreshable tokens are not working in projects if they were originally issued in other non-parent projects

**[TW-34249](https://youtrack.jetbrains.com/issue/TW-34249/Commit-Status-Publisher-Plugin-Retry-on-network-problems)** — Commit Status Publisher Plugin: Retry on network problems

**[TW-82881](https://youtrack.jetbrains.com/issue/TW-82881/Token-usage-after-application-permissions-were-extended)** — Token usage after application permissions were extended

**[TW-83992](https://youtrack.jetbrains.com/issue/TW-83992/Refreshable-access-token-option-is-missed-in-Pull-Requests-Build-Feature-when-corresponding-connections-arent-configured)** — «Refreshable access token» option is missed in Pull Requests Build Feature when corresponding connections aren't configured

**[TW-83664](https://youtrack.jetbrains.com/issue/TW-83664/Slow-rendering-of-build-dependensies-list-after-switch-between-tabs-or-filters)** — Slow rendering of build dependenсies list after switch between tabs or filters

**[TW-83988](https://youtrack.jetbrains.com/issue/TW-83988/Only-one-Bitbucket-Cloud-and-GitHub.com-connection-is-shown-on-Create-Project-page)** — Only one Bitbucket Cloud (and GitHub.com) connection is shown on Create Project page

**[TW-82978](https://youtrack.jetbrains.com/issue/TW-82978/DSL-Validation-is-not-processing-for-remote-parameters)** — DSL Validation is not processing for remote parameters

**[TW-83407](https://youtrack.jetbrains.com/issue/TW-83407/All-builds-stuck-with-build-settings-have-not-been-finalized-when-Kotlin-DSL-is-broken-in-a-large-project-with-many-queued)** — All builds stuck with build settings have not been finalized (when Kotlin DSL is broken in a large project with many queued builds)

**[TW-83340](https://youtrack.jetbrains.com/issue/TW-83340/TeamCity-IntelliJ-plugin-sometimes-does-a-lot-of-activity-even-if-I-dont-use-any-of-its-features)** — TeamCity IntelliJ plugin sometimes does a lot of activity even if I don't use any of its features

**[TW-83920](https://youtrack.jetbrains.com/issue/TW-83920/Kotlin-DSL-structure-of-Commit-status-publisher-configuration-for-Azure-DevOps-must-be-actualized)** — Kotlin DSL structure of Commit status publisher configuration for Azure DevOps must be actualized

**[TW-83926](https://youtrack.jetbrains.com/issue/TW-83926/Confusing-documentation-about-custom-checkout-directory-path)** — Confusing documentation about custom checkout directory path

**[TW-83752](https://youtrack.jetbrains.com/issue/TW-83752/Some-build-features-notifications-status-publishing-build-approval-dont-work-when-Parallel-Tests-are-enabled-and-execution)** — Some build features (notifications, status publishing, build approval) don't work when Parallel Tests are enabled and execution timeout is not zero

**[TW-83708](https://youtrack.jetbrains.com/issue/TW-83708/Assignment-of-an-uuid-to-a-build-configuration-which-previously-did-not-have-one-leads-to-lost-history)** — Assignment of an uuid to a build configuration which previously did not have one leads to lost history

**[TW-83635](https://youtrack.jetbrains.com/issue/TW-83635/The-dialog-with-the-header-Add-new-AWS-Connection-is-shown-during-editing-the-AWS-Connection-in-S3-Storage-page)** — The dialog with the header "Add new AWS Connection" is shown during editing the AWS Connection in S3 Storage page

**[TW-83681](https://youtrack.jetbrains.com/issue/TW-83681/Build-Step-ID-specified-in-the-UI-is-not-committed-to-DSL)** — Build Step ID specified in the UI is not committed to DSL

**[TW-83698](https://youtrack.jetbrains.com/issue/TW-83698/perforce-shelve-trigger-firing-for-an-already-ran-build)** — perforce shelve trigger firing for an already ran build

**[TW-83263](https://youtrack.jetbrains.com/issue/TW-83263/Parallel-tests-build-feature-can-disable-builds-reuse-if-the-build-with-this-build-feature-is-a-part-of-a-bigger-chain)** — Parallel tests build feature can disable builds reuse if the build with this build feature is a part of a bigger chain

**[TW-83569](https://youtrack.jetbrains.com/issue/TW-83569/There-is-no-extra-drop-down-menu-on-the-build-log-tab-section-for-the-builds-batch-filter)** — There is no extra drop-down menu on the build log tab section for the builds batch filter

**[TW-82843](https://youtrack.jetbrains.com/issue/TW-82843/Improve-wording-of-the-implicit-requirements-part-in-the-incompatible-agents-section)** — Improve wording of the implicit requirements part in the incompatible agents section

**[TW-83693](https://youtrack.jetbrains.com/issue/TW-83693/Space-Connection-Connect-button-is-not-active)** — Space Connection: Connect button is not active

**[TW-83759](https://youtrack.jetbrains.com/issue/TW-83759/Build-which-was-not-persisted-to-the-build-queue-can-prevent-a-start-of-another-queued-build)** — Build which was not persisted to the build queue can prevent a start of another queued build

**[TW-83187](https://youtrack.jetbrains.com/issue/TW-83187/Xml-test-reporting-doesnt-report-test-classes-from-NUnit-v3-report-made-by-Unity-in-Unity-runner)** — Xml-test-reporting doesn't report test classes from NUnit v3 report made by Unity in Unity runner

**[TW-83092](https://youtrack.jetbrains.com/issue/TW-83092/Inconsistent-test-reporting-in-test-runs-launched-from-teamcity-csharp-interactive)** — Inconsistent test reporting in test runs launched from teamcity-csharp-interactive

**[TW-83745](https://youtrack.jetbrains.com/issue/TW-83745/Detected-TeamCity-settings-errors-message-in-the-log-when-there-are-no-errors)** — "Detected TeamCity settings errors" message in the log when there are no errors

**[TW-83696](https://youtrack.jetbrains.com/issue/TW-83696/Clicking-on-a-link-in-Space-connection-causes-edit-mode-to-be-opened)** — Clicking on a link in Space connection causes edit mode to be opened

**[TW-81194](https://youtrack.jetbrains.com/issue/TW-81194/An-ability-to-see-copy-full-client-ID-for-JetBrains-Space-connection)** — An ability to see/copy full client ID for JetBrains Space connection

**[TW-68421](https://youtrack.jetbrains.com/issue/TW-68421/Report-tabs-in-composite-builds)** — Report tabs in composite builds

**[TW-80188](https://youtrack.jetbrains.com/issue/TW-80188/Commit-Status-Publisher-for-Bitbucket-Server-Base-URL-is-not-filled-from-the-connection-when-Access-Token-authentication-is-used)** — Commit Status Publisher for Bitbucket Server: Base URL is not filled from the connection when Access Token authentication is used

**[TW-83644](https://youtrack.jetbrains.com/issue/TW-83644/Build-appears-in-a-wrong-state-if-it-could-not-start-because-the-normal-executor-queue-is-full)** — Build appears in a wrong state if it could not start because the normal executor queue is full

**[TW-80246](https://youtrack.jetbrains.com/issue/TW-80246/Build-results-page-URL-containing-the-id-of-a-substituted-build-gives-error-404)** — Build results page URL containing the id of a substituted build gives error 404

**[TW-83497](https://youtrack.jetbrains.com/issue/TW-83497/Broken-DSL-data-file-on-disk-leads-to-non-working-Kotlin-DSL-versioned-settings-of-all-the-projects)** — Broken DSL data file on disk leads to non-working Kotlin DSL versioned settings of all the projects

**[TW-83534](https://youtrack.jetbrains.com/issue/TW-83534/There-is-no-extra-drop-down-menu-on-the-build-log-tab-section-for-the-builds-batch-filter)** — There is no extra drop-down menu on the build log tab section for the builds batch filter

**[TW-83437](https://youtrack.jetbrains.com/issue/TW-83437/Broken-layout-on-the-build-configuration-page-with-a-running-build-classical-UI)** — Broken layout on the build configuration page with a running build (classical UI)

**[TW-83225](https://youtrack.jetbrains.com/issue/TW-83225/Sakura-UI-Fails-to-Display-Certain-Old-Builds)** — Sakura UI Fails to Display Certain Old Builds

**[TW-83331](https://youtrack.jetbrains.com/issue/TW-83331/Table-testnames-can-grow-too-large-because-of-insufficient-cleanup)** — Table test_names can grow too large because of insufficient cleanup

**[TW-83307](https://youtrack.jetbrains.com/issue/TW-83307/Tests-are-not-always-correctly-matched-on-Compare-Builds-page)** — Tests are not always correctly matched on 'Compare Builds' page

**[TW-83439](https://youtrack.jetbrains.com/issue/TW-83439/Source-property-in-build-docker-step-should-be-mandatory-in-DSL)** — Source property in build docker step should be mandatory in DSL

**[TW-80294](https://youtrack.jetbrains.com/issue/TW-80294/ImageBuilder-plugin-does-not-allow-choosing-security-groups-for-target-instance.)** — ImageBuilder plugin does not allow choosing security groups for target instance.

**[TW-83354](https://youtrack.jetbrains.com/issue/TW-83354/TeamCity-2023.05.x-does-not-allow-to-start-a-build-with-a-parameter-with-null-value)** — TeamCity 2023.05.x does not allow to start a build with a parameter with null value

**[TW-83272](https://youtrack.jetbrains.com/issue/TW-83272/Cleanup-cloud-images-agent-types-from-the-database-if-they-belong-to-non-existing-cloud-profiles)** — Cleanup cloud images (agent types) from the database if they belong to non existing cloud profiles

**[TW-82560](https://youtrack.jetbrains.com/issue/TW-82560/Updates-made-on-the-Service-Worker-validation-phase-do-not-trigger-updates-to-main-reducers)** — Updates made on the Service Worker validation phase do not trigger updates to main reducers


### Performance Problem

**[TW-83062](https://youtrack.jetbrains.com/issue/TW-83062/Slow-checking-for-changes-because-of-slow-calculation-of-additional-branch-spec)** — Slow checking for changes because of slow calculation of additional branch spec

**[TW-78429](https://youtrack.jetbrains.com/issue/TW-78429/Several-queries-for-artifact-dependencies-contain-unnecessary-wide-filters)** — Several queries for artifact dependencies contain unnecessary wide filters

**[TW-82294](https://youtrack.jetbrains.com/issue/TW-82294/More-than-one-checking-for-changes-task-can-be-executed-for-the-same-build-chain)** — More than one checking for changes task can be executed for the same build chain

**[TW-82255](https://youtrack.jetbrains.com/issue/TW-82255/Lots-of-detected-commits-in-some-VCS-root-can-slow-down-delivering-of-the-checking-for-changes-result-in-unrelated-projects)** — Lots of detected commits in some VCS root can slow down delivering of the checking for changes result in unrelated projects

**[TW-84736](https://youtrack.jetbrains.com/issue/TW-84736/Pull-Requests-plugin-may-slow-down-handling-repositoryStateChanged-multinode-event)** — Pull Requests plugin may slow down handling repositoryStateChanged multinode event

**[TW-84618](https://youtrack.jetbrains.com/issue/TW-84618/Inefficient-checking-for-changes-in-Perforce-nested-doGetPath2LatestRevision-calls)** — Inefficient checking for changes in Perforce (nested doGetPath2LatestRevision calls)

**[TW-84279](https://youtrack.jetbrains.com/issue/TW-84279/Avoid-revision-calculation-for-the-pure-versioned-settings-VCS-root)** — Avoid revision calculation for the pure versioned settings VCS root

**[TW-83528](https://youtrack.jetbrains.com/issue/TW-83528/Avoid-running-vacuum-analyze-customdatabody-concurrently)** — Avoid running vacuum analyze custom_data_body concurrently

**[TW-83554](https://youtrack.jetbrains.com/issue/TW-83554/New-Test-history-page-requests-data-which-is-only-relevant-for-the-queued-builds)** — New Test history page requests data which is only relevant for the queued builds

**[TW-84243](https://youtrack.jetbrains.com/issue/TW-84243/Slow-custom-build-dialog-if-there-are-many-branches-in-dependencies)** — Slow custom build dialog if there are many branches in dependencies

**[TW-83917](https://youtrack.jetbrains.com/issue/TW-83917/Slow-processing-build-agent-messages)** — Slow processing build agent messages

**[TW-83337](https://youtrack.jetbrains.com/issue/TW-83337/Test-metadata-storage-dictionary-cleaner-stops-working-if-testmetadata-table-becomes-large)** — Test metadata storage dictionary cleaner stops working if test_metadata table becomes large

**[TW-81876](https://youtrack.jetbrains.com/issue/TW-81876/CoveragePageFragment.isAvailable-becomes-slow-for-composite-builds-with-hundreds-of-dependencies)** — CoveragePageFragment.isAvailable becomes slow for composite builds with hundreds of dependencies

**[TW-83657](https://youtrack.jetbrains.com/issue/TW-83657/Slow-change-details-page-if-the-number-of-affected-configurations-is-big)** — Slow change details page if the number of affected configurations is big

**[TW-83447](https://youtrack.jetbrains.com/issue/TW-83447/Slow-SQL-select-maxmuteid-projectintid-testnameid-from-mutetestinproj-SQL-query)** — Slow SQL select max(mute_id), project_int_id, test_name_id from mute_test_in_proj SQL query

**[TW-82994](https://youtrack.jetbrains.com/issue/TW-82994/Schedule-trigger-doesnt-have-branch-limit)** — Schedule trigger doesn't have branch limit





<!--project: TeamCity Fix versions: {2023.11 (147318)} #Fixed #{Security Problem}  -{Trunk issue}-->

### Security

55 security problems have been fixed. Note that the absolute majority of the fixed issues do not originate from the TeamCity codebase, and are related to the updated 3rd-party dependencies.

> We do not share the details of security-related issues to avoid compromising clients that keep using previous bugfix and/or major versions of TeamCity. Check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2023.11) for the list of disclosed vulnerability fixes. Security bulletins for new versions are typically published within the next few days after the release date.
>
{type="note"}

