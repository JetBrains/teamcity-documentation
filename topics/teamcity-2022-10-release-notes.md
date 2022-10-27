[//]: # (title: TeamCity 2022.10 Release Notes)
[//]: # (auxiliary-id: TeamCity 2022.10 Release Notes)

__Build: 116751__

__27 October 2022__

### Feature

**[TW-77661](https://youtrack.jetbrains.com/issue/TW-77661/AWS-Connection)** — AWS Connection

**[TW-77377](https://youtrack.jetbrains.com/issue/TW-77377/Enable-PerfMon-for-every-new-build-configuration)** — Enable PerfMon for every new build configuration

**[TW-78152](https://youtrack.jetbrains.com/issue/TW-78152/Support-HTTPS-certificate-configuration-on-TeamCity-server)** — Support HTTPS certificate configuration on TeamCity server

**[TW-75047](https://youtrack.jetbrains.com/issue/TW-75047/Bundle-recent-version-of-Maven-38x)** — Bundle recent version of Maven (3.8.x)

**[TW-77400](https://youtrack.jetbrains.com/issue/TW-77400/IntelliJ-inspections-support-IntelliJ-20223-read-classpath-from-product-infojson)** — IntelliJ inspections: support IntelliJ 2022.3 (read classpath from product-info.json)

**[TW-74855](https://youtrack.jetbrains.com/issue/TW-74855/Move-artifactsjson-to-the-database-custom-data-storage)** — Move artifacts.json to the database (custom data storage)

**[TW-75128](https://youtrack.jetbrains.com/issue/TW-75128/Allow-connecting-to-AWS-EC2-based-agents-in-one-click-TTY-Linux)** — Allow connecting to AWS EC2-based agents in one click (TTY, Linux)

**[TW-70795](https://youtrack.jetbrains.com/issue/TW-70795/Rest-API-to-manage-secondary-nodes-responsibilities)** — Rest API to manage secondary nodes responsibilities

**[TW-71013](https://youtrack.jetbrains.com/issue/TW-71013/Allow-customizing-port-for-HTTPS-connections)** — Allow customizing port for HTTPS connections

**[TW-13916](https://youtrack.jetbrains.com/issue/TW-13916/Promotion-of-the-personal-build-must-create-personal-dependent-build-allow-promotion-of-personal-builds)** — Promotion of the personal build must create personal dependent build (+ allow promotion of personal builds)

**[TW-21953](https://youtrack.jetbrains.com/issue/TW-21953/Ability-to-add-a-link-and-link-name-to-the-description-of-a-project)** — Ability to add a link and link name to the description of a project.

**[TW-74941](https://youtrack.jetbrains.com/issue/TW-74941/Expose-the-AWS-credentials-into-a-build-step)** — Expose the AWS credentials into a build step

**[TW-77217](https://youtrack.jetbrains.com/issue/TW-77217/Allow-starting-and-stopping-cloud-instances-from-the-secondary-node)** — Allow starting and stopping cloud instances from the secondary node

**[TW-73262](https://youtrack.jetbrains.com/issue/TW-73262/Allow-navigating-from-TeamCity-build-to-the-corresponding-Perforce-Helix-Swarm-review)** — Allow navigating from TeamCity build to the corresponding Perforce Helix Swarm review

**[TW-76385](https://youtrack.jetbrains.com/issue/TW-76385/Allow-specifying-of-storage-ID)** — Allow specifying of storage ID

**[TW-77087](https://youtrack.jetbrains.com/issue/TW-77087/Add-ability-to-provide-DSL-snippets-for-plugins)** — Add ability to provide DSL snippets for plugins

**[TW-70032](https://youtrack.jetbrains.com/issue/TW-70032/Authentication-via-Google-OAuth)** — Authentication via Google OAuth

**[TW-70491](https://youtrack.jetbrains.com/issue/TW-70491/Stop-relying-on-the-pull-request-specific-branches-in-Bitbucket-Server-pull-requests-support)** — Stop relying on the pull request specific branches in Bitbucket Server pull requests support

**[TW-76994](https://youtrack.jetbrains.com/issue/TW-76994/Add-token-scope-field-to-Azure-DevOps-OAuth-20-connection)** — Add token scope field to Azure DevOps OAuth 2.0 connection

**[TW-74303](https://youtrack.jetbrains.com/issue/TW-74303/TeamCity-Agent-should-cleanup-required-disk-space-for-artifacts-automatically)** — TeamCity Agent should cleanup required disk space for artifacts automatically

**[TW-77506](https://youtrack.jetbrains.com/issue/TW-77506/Add-server-metrics-with-timestamps-for-server-cleanup)** — Add server metrics with timestamps for server cleanup

**[TW-76789](https://youtrack.jetbrains.com/issue/TW-76789/ImageBuilder-should-support-remote-artifacts)** — ImageBuilder should support remote artifacts

**[TW-76848](https://youtrack.jetbrains.com/issue/TW-76848/Add-metrics-for-the-versioned-settings-application-on-the-server)** — Add metrics for the versioned settings application on the server

### Bug

**[TW-77457](https://youtrack.jetbrains.com/issue/TW-77457/Its-not-possible-to-change-the-main-node-responsibility-on-the-nodes-configuration-page-if-the-main-node-was-stopped-gracefully)** — It's not possible to change the main node responsibility on the nodes configuration page if the main node was stopped gracefully

**[TW-77492](https://youtrack.jetbrains.com/issue/TW-77492/Remote-run-Adding-personal-builds-to-queue-takes-long-time-and-fails-on-the-first-attempt)** — Remote run: "Adding personal builds to queue" takes long time and fails on the first attempt

**[TW-76487](https://youtrack.jetbrains.com/issue/TW-76487/Its-not-possible-to-set-the-maximum-number-of-instances-in-a-cloud-image)** — It's not possible to set the maximum number of instances in a cloud image

**[TW-77075](https://youtrack.jetbrains.com/issue/TW-77075/Updating-search-index-progress-starts-with-50)** — Updating search index progress starts with 50%

**[TW-78231](https://youtrack.jetbrains.com/issue/TW-78231/Incorrect-DSL-is-generated-for-NuGet-Installer-update-section)** — Incorrect DSL is generated for NuGet Installer update section

**[TW-78185](https://youtrack.jetbrains.com/issue/TW-78185/NullPointerException-in-SearchIndexerDataCleaner)** — NullPointerException in SearchIndexerDataCleaner

**[TW-78196](https://youtrack.jetbrains.com/issue/TW-78196/No-typed-DSL-for-Use-VCS-root-credentials-authentication-type-in-Pull-Requests-build-feature-for-GitLab)** — No typed DSL for "Use VCS root credentials" authentication type in Pull Requests build feature for GitLab

**[TW-77884](https://youtrack.jetbrains.com/issue/TW-77884/javautilNoSuchElementException-in-SplitLimit-class)** — java.util.NoSuchElementException in SplitLimit class

**[TW-78105](https://youtrack.jetbrains.com/issue/TW-78105/The-agents-page-is-fully-reloaded-on-focus)** — The agents page is fully reloaded on focus

**[TW-72318](https://youtrack.jetbrains.com/issue/TW-72318/The-very-long-user-name-is-trimmed-incorrectly-in-changes-popup-on-the-Shanges-and-Pending-Changes-tabs-in-Experimantal-UI)** — The very long user name is trimmed incorrectly in changes popup, on the Сhanges and Pending Changes tabs in Experimantal UI.

**[TW-78173](https://youtrack.jetbrains.com/issue/TW-78173/Users-default-UI-switched-after-opening-the-link-from-another-UI)** — User's default UI switched after opening the link from another UI

**[TW-78135](https://youtrack.jetbrains.com/issue/TW-78135/Native-Git-operations-may-fail-if-they-cause-subsequent-local-repository-clone-when-Git-version-2381-is-used)** — Native Git operations may fail if they cause subsequent local repository clone when Git version 2.38.1 is used

**[TW-77990](https://youtrack.jetbrains.com/issue/TW-77990/Build-queue-optimizer-can-reuse-a-failed-build-in-a-build-chain-even-if-settings-prohibit-it)** — Build queue optimizer can reuse a failed build in a build chain even if settings prohibit it

**[TW-77442](https://youtrack.jetbrains.com/issue/TW-77442/Agents-overview-freezes-in-Safari-if-therere-1000-builds-rendered)** — Agents overview freezes in Safari if there're 1000+ builds rendered

**[TW-77957](https://youtrack.jetbrains.com/issue/TW-77957/Fix-description-and-other-texts-in-AWS-Core-plugin)** — Fix description and other texts in AWS Core plugin

**[TW-77443](https://youtrack.jetbrains.com/issue/TW-77443/Double-start-of-EC2-cloud-instance-for-a-build)** — Double start of EC2 cloud instance for a build

**[TW-78175](https://youtrack.jetbrains.com/issue/TW-78175/Huge-memory-usage-by-search-plugin)** — Huge memory usage by search plugin

**[TW-72704](https://youtrack.jetbrains.com/issue/TW-72704/Maven-info-maven-plugin-failing-on-Teamcity-version-202111)** — Maven info-maven-plugin failing on Teamcity version 2021.1.1

**[TW-77517](https://youtrack.jetbrains.com/issue/TW-77517/Build-dependency-cannot-start-because-of-a-maximum-number-of-dependent-composite-builds-limitation)** — Build dependency cannot start because of a "maximum number of dependent composite builds" limitation

**[TW-77818](https://youtrack.jetbrains.com/issue/TW-77818/Dont-ask-Discord-your-changes-when-user-clicks-on-link-with-targetblank)** — Don't ask 'Discord your changes?' when user clicks on link with target="_blank"

**[TW-77069](https://youtrack.jetbrains.com/issue/TW-77069/Search-page-has-no-Found-in-field-anymore)** — Search page has no "Found in" field anymore

**[TW-78096](https://youtrack.jetbrains.com/issue/TW-78096/TeamCity-202244-crashes-on-startup-due-to-wrong-handling-of-env-variable-in-teamcity-internalbat)** — TeamCity 2022.4.4 crashes on startup due to wrong handling of env variable in teamcity-internal.bat

**[TW-77189](https://youtrack.jetbrains.com/issue/TW-77189/Not-all-depbtId-parameter-references-are-updated-in-id-is-changed-in-a-build-configuration-which-is-accessible-transitively-via)** — Not all dep.&lt;btId> parameter references are updated in id is changed in a build configuration which is accessible transitively via an artifact dependency

**[TW-67132](https://youtrack.jetbrains.com/issue/TW-67132/Old-build-investigation-is-shown-in-builds-details-until-page-reload)** — Old build investigation is shown in builds details until page reload

**[TW-65821](https://youtrack.jetbrains.com/issue/TW-65821/Sometimes-the-boldness-of-pending-changes-is-not-updated-properly-in-the-experimental-UI)** — Sometimes the boldness of pending changes is not updated properly in the experimental UI

**[TW-77609](https://youtrack.jetbrains.com/issue/TW-77609/Additional-InspectCode-parameters-appears-to-ignore-caches-home)** — "Additional InspectCode parameters" appears to ignore "--caches-home"

**[TW-72353](https://youtrack.jetbrains.com/issue/TW-72353/Sakura-does-not-show-changes-on-build-overview-until-page-refresh)** — Sakura does not show changes on build overview until page refresh

**[TW-77365](https://youtrack.jetbrains.com/issue/TW-77365/SSH-Deployer-feature-doesnt-set-a-connection-timeout-nor-respect-the-build-cancel-properly)** — SSH Deployer feature doesn't set a connection timeout nor respect the build cancel properly

**[TW-76898](https://youtrack.jetbrains.com/issue/TW-76898/Artifacts-path-tempbuildTmpteamcitybuildXeventjson-not-found-for-docker-related-steps-in-a-build-log)** — "Artifacts path temp/buildTmp/.teamcity/build_X/event.json not found" for docker related steps in a build log

**[TW-75966](https://youtrack.jetbrains.com/issue/TW-75966/Parallel-build-take-settings-from-master-instead-of-the-corresponding-branch)** — Parallel build take settings from master instead of the corresponding branch.

**[TW-76925](https://youtrack.jetbrains.com/issue/TW-76925/Cannot-Unathorize-agent-if-the-maximum-limit-of-agents-is-reached)** — Cannot Unathorize agent, if the maximum limit of agents is reached

**[TW-77589](https://youtrack.jetbrains.com/issue/TW-77589/Broken-health-report-in-the-latest-Safari-160)** — Broken health report in the latest Safari (16.0)

**[TW-76467](https://youtrack.jetbrains.com/issue/TW-76467/VCS-Labeling-cant-overwrite-an-existing-tag-using-native-git)** — VCS Labeling can't overwrite an existing tag using native git

**[TW-78035](https://youtrack.jetbrains.com/issue/TW-78035/Broken-layout-on-Change-page-when-Show-project-hierarchy-option-is-enabled)** — Broken layout on Change page when "Show project hierarchy" option is enabled

**[TW-72331](https://youtrack.jetbrains.com/issue/TW-72331/Provide-ability-to-view-pending-changes-from-dependencies)** — Provide ability to view pending changes from dependencies

**[TW-77655](https://youtrack.jetbrains.com/issue/TW-77655/Filtered-success-builds-returns-not-only-the-finished-but-the-running-builds)** — Filtered 'success builds' returns not only the finished, but the running builds

**[TW-77744](https://youtrack.jetbrains.com/issue/TW-77744/Inefficient-processing-of-an-artifact-download-if-artifact-is-an-archive-and-is-published-to-an-external-storage-S3)** — Inefficient processing of an artifact download if artifact is an archive and is published to an external storage (S3)

**[TW-68785](https://youtrack.jetbrains.com/issue/TW-68785/NoClassDefFoundError-in-reading-maven-project-data)** — NoClassDefFoundError in reading maven project data

**[TW-78028](https://youtrack.jetbrains.com/issue/TW-78028/Do-not-show-Sakura-is-ready-dialog-and-banner-for-a-user-with-enabled-Sakura-UI)** — Do not show "Sakura is ready!" dialog and banner for a user with enabled Sakura UI

**[TW-77253](https://youtrack.jetbrains.com/issue/TW-77253/GZipping-content-for-some-static-resources-may-not-work-correctly)** — GZipping content for some static resources may not work correctly

**[TW-71010](https://youtrack.jetbrains.com/issue/TW-71010/Dont-allow-enabling-HTTP-redirect-without-a-valid-certificatekey-uploaded)** — Don't allow enabling HTTP redirect without a valid certificate/key uploaded

**[TW-76800](https://youtrack.jetbrains.com/issue/TW-76800/Issue-with-screen-drawing)** — Issue with screen drawing

**[TW-77813](https://youtrack.jetbrains.com/issue/TW-77813/Not-all-VCS-roots-are-displayed-on-the-build-changes-tab)** — Not all VCS-roots are displayed on the build changes tab

**[TW-76717](https://youtrack.jetbrains.com/issue/TW-76717/Use-OAuthProvidergetDefaultProperties-method-to-avoid-saving-defaults-to-Kotlin-DSL-code-for-various-connection-related-project)** — Use OAuthProvider.getDefaultProperties() method to avoid saving defaults to Kotlin DSL code for various connection related project features

**[TW-77078](https://youtrack.jetbrains.com/issue/TW-77078/Search-with-AND-operator-may-show-not-all-the-relevant-results)** — Search with AND operator may show not all the relevant results

**[TW-78063](https://youtrack.jetbrains.com/issue/TW-78063/Exception-in-custom-ParametersProvidergetParametersAvailableOnAgent-causes-inability-to-process-build-queue)** — Exception in custom ParametersProvider.getParametersAvailableOnAgent causes inability to process build queue

**[TW-78050](https://youtrack.jetbrains.com/issue/TW-78050/AWS-Connection-Error-Connection-is-prohibited-by-TeamCity-node-restrictions-during-test-connection-or-key-rotation-on-secondary)** — AWS Connection: Error "Connection is prohibited by TeamCity node restrictions" during test connection or key rotation on secondary node

**[TW-78053](https://youtrack.jetbrains.com/issue/TW-78053/Composite-build-cant-start-because-of-constraint-violation-on-attempt-to-insert-a-row-into-the-running-table)** — Composite build can't start because of constraint violation on attempt to insert a row into the running table

**[TW-77681](https://youtrack.jetbrains.com/issue/TW-77681/Large-files-upload-can-fail-with-read-timeout-if-upload-is-landed-on-the-secondary-node)** — Large files upload can fail with read timeout if upload is landed on the secondary node

**[TW-77883](https://youtrack.jetbrains.com/issue/TW-77883/Unable-to-mark-a-build-as-a-successful)** — Unable to mark a build as a successful

**[TW-77929](https://youtrack.jetbrains.com/issue/TW-77929/Cant-assign-notification-rules-on-the-secondary-node-if-some-rules-were-created-on-other-nodes-before)** — Can't assign notification rules on the secondary node (if some rules were created on other nodes before)

**[TW-77930](https://youtrack.jetbrains.com/issue/TW-77930/Cant-create-agent-pool-on-a-secondary-node-if-some-pool-was-created-on-another-node-before)** — Can't create agent pool on a secondary node if some pool was created on another node before

**[TW-78025](https://youtrack.jetbrains.com/issue/TW-78025/R-CLT-tools-versions-are-not-listed-in-Install-JetBrains-ReSharper-Command-Line-Tools-window)** — R# CLT tools versions are not listed in Install JetBrains ReSharper Command Line Tools window

**[TW-74845](https://youtrack.jetbrains.com/issue/TW-74845/Parallel-tests-cannot-be-muted-in-selected-build-configuration)** — Parallel tests cannot be muted in selected build configuration

**[TW-77484](https://youtrack.jetbrains.com/issue/TW-77484/Parallel-test-batches-are-not-reused-after-changing-internal-property)** — Parallel test batches are not reused after changing internal property

**[TW-77167](https://youtrack.jetbrains.com/issue/TW-77167/Changes-in-REST-API-count-1-not-supported)** — Changes in REST API - count:-1 not supported

**[TW-77399](https://youtrack.jetbrains.com/issue/TW-77399/Parallel-tests-successful-batches-should-be-not-reused-by-default)** — Parallel tests: successful batches should be not reused by default

**[TW-77978](https://youtrack.jetbrains.com/issue/TW-77978/User-without-enabledisable-versioned-settings-permission-is-able-to-change-settings-status-via-REST)** — User without `enable/disable versioned settings` permission is able to change settings status via REST

**[TW-76950](https://youtrack.jetbrains.com/issue/TW-76950/EC2-plugin-Spot-instance-termination-checker-doesnt-work-with-launch-templates-and-Spot-Fleets)** — EC2 plugin: Spot instance termination checker doesn't work with launch templates and Spot Fleets

**[TW-77272](https://youtrack.jetbrains.com/issue/TW-77272/Expose-AWS-creds-into-a-build-step-warnings-in-logs-when-there-is-no-AWS-connection-and-build-feautre)** — Expose AWS creds into a build step: warnings in logs when there is no AWS connection and build feautre

**[TW-77872](https://youtrack.jetbrains.com/issue/TW-77872/Too-many-Publishing-to-local-artifacts-cache-is-disabled-skipping-messages-in-build-log)** — Too many "Publishing to local artifacts cache is disabled, skipping" messages in build log

**[TW-77091](https://youtrack.jetbrains.com/issue/TW-77091/S3-Storage-throws-ArtifactPublishingFailedException-when-file-cannot-be-found)** — S3 Storage throws ArtifactPublishingFailedException when file cannot be found

**[TW-77951](https://youtrack.jetbrains.com/issue/TW-77951/Trying-to-connect-a-user-to-Space-integration-fails-with-an-Unexpected-error)** — Trying to connect a user to Space integration fails with an Unexpected error

**[TW-33642](https://youtrack.jetbrains.com/issue/TW-33642/Illegal-mix-of-collations-for-operation-UNION)** — Illegal mix of collations for operation 'UNION'

**[TW-77931](https://youtrack.jetbrains.com/issue/TW-77931/TeamCity-main-node-hangs-on-startup-possible-deadlock-with-other-nodes-because-of-truncate-table-buildtypevcschange-SQL)** — TeamCity main node hangs on startup (possible deadlock with other nodes because of truncate table build_type_vcs_change SQL statement)

**[TW-76873](https://youtrack.jetbrains.com/issue/TW-76873/Chain-dependencies-fail-when-canceled)** — Chain dependencies fail when canceled

**[TW-77142](https://youtrack.jetbrains.com/issue/TW-77142/Not-possible-to-re-enable-Cloud-Integration-on-subprojects-after-integration-has-been-disabled)** — Not possible to re-enable Cloud Integration on subprojects after integration has been disabled

**[TW-70417](https://youtrack.jetbrains.com/issue/TW-70417/Wrong-number-of-changes-is-shown-on-the-build-overview-page)** — Wrong number of changes is shown on the build overview page

**[TW-77184](https://youtrack.jetbrains.com/issue/TW-77184/Commit-Status-Publisher-spams-the-same-commit-200-times-per-second-filling-up-memory-fast)** — Commit Status Publisher spams the same commit 200 times per second filling up memory fast

**[TW-71545](https://youtrack.jetbrains.com/issue/TW-71545/Allow-loading-Kubernetes-plugin-on-secondary-nodes)** — Allow loading Kubernetes plugin on secondary nodes

**[TW-77917](https://youtrack.jetbrains.com/issue/TW-77917/Unhandled-exception-in-RandomSecureAuthenticationTokenCreatorparseToken-in-case-of-a-bad-token)** — Unhandled exception in RandomSecureAuthenticationTokenCreator.parseToken in case of a bad token

**[TW-72943](https://youtrack.jetbrains.com/issue/TW-72943/Align-test-names-to-column-for-retried-test-invocations)** — Align test names to column for retried test invocations

**[TW-72666](https://youtrack.jetbrains.com/issue/TW-72666/Tests-list-may-blink-if-Current-Scope-is-specified)** — Tests list may blink if Current Scope is specified

**[TW-77611](https://youtrack.jetbrains.com/issue/TW-77611/Improve-wording-in-the-Investigations-dialog-regarding-Mark-as-fixed)** — Improve wording in the Investigations dialog regarding "Mark as fixed"

**[TW-77816](https://youtrack.jetbrains.com/issue/TW-77816/Cant-delete-the-artifacts-storage-with-name-containing-the-quotes)** — Can't delete the artifacts storage with name containing the quotes

**[TW-77871](https://youtrack.jetbrains.com/issue/TW-77871/Extra-cloud-agents-can-be-started-ignoring-licensing-and-pool-limits)** — Extra cloud agents can be started ignoring licensing and pool limits

**[TW-77066](https://youtrack.jetbrains.com/issue/TW-77066/Search-only-shows-10-builds-per-page-showed-30-previously)** — Search only shows 10 builds per page (showed 30 previously)

**[TW-75234](https://youtrack.jetbrains.com/issue/TW-75234/Splitted-tests-are-marked-as-new-after-assigning-to-another-build-configuration)** — Splitted tests are marked as new after assigning to another build configuration

**[TW-65523](https://youtrack.jetbrains.com/issue/TW-65523/Long-build-comment-full-text-is-hidden-under-Show-more-link-on-build-overview-page)** — Long build comment full text is hidden under "Show more" link on build overview page

**[TW-74550](https://youtrack.jetbrains.com/issue/TW-74550/Empty-lines-are-removed-from-the-failed-test-overview)** — Empty lines are removed from the failed test overview

**[TW-76206](https://youtrack.jetbrains.com/issue/TW-76206/Expanded-change-presentation-on-a-build-overview-has-weird-margins-and-paddings)** — Expanded change presentation on a build overview has weird margins and paddings

**[TW-63078](https://youtrack.jetbrains.com/issue/TW-63078/Changes-counter-on-the-build-overview-page-may-sometime-show-incorrect-information)** — Changes counter on the build overview page may sometime show incorrect information

**[TW-65669](https://youtrack.jetbrains.com/issue/TW-65669/Changes-popup-doesnt-display-all-information-about-change-for-history-builds)** — Changes popup doesn't display all information about change for history builds

**[TW-76638](https://youtrack.jetbrains.com/issue/TW-76638/Its-not-possible-to-find-an-existing-build-configuration-from-an-archived-project-even-if-Show-archived-is-turned-on)** — It’s not possible to find an existing build configuration from an archived project even if ‘Show archived’ is turned on

**[TW-76983](https://youtrack.jetbrains.com/issue/TW-76983/Changes-page-Project-name-is-not-displayed-in-the-filter-in-Firefox)** — Changes page: Project name is not displayed in the filter in Firefox

**[TW-77624](https://youtrack.jetbrains.com/issue/TW-77624/Artifact-dependency-on-itself-has-empty-configuration-on-the-Dependencies-Downloaded-artifacts-tab)** — Artifact dependency on itself has empty configuration on the Dependencies->Downloaded artifacts tab.

**[TW-63750](https://youtrack.jetbrains.com/issue/TW-63750/TeamCity-fails-to-collect-changes-with-error-Malformed-input-or-input-contains-unmappable-characters)** — TeamCity fails to collect changes with error: Malformed input or input contains unmappable characters

**[TW-77302](https://youtrack.jetbrains.com/issue/TW-77302/Restored-build-may-not-be-replaced-in-the-original-build-chain-when-several-nodes-have-Processing-data-produced-by-running)** — Restored build may not be replaced in the original build chain when several nodes have "Processing data produced by running builds" responsibility

**[TW-77225](https://youtrack.jetbrains.com/issue/TW-77225/Confusing-projects-selector-in-Assign-roles-dialog)** — Confusing projects selector in Assign roles dialog

**[TW-77499](https://youtrack.jetbrains.com/issue/TW-77499/Azure-DevOps-Issue-Tracker-404-Not-Found-if-you-try-to-open-an-issue)** — Azure DevOps Issue Tracker - "404 Not Found" if you try to open an issue.

**[TW-76647](https://youtrack.jetbrains.com/issue/TW-76647/Improve-hints-for-connections-which-support-refreshable-tokens)** — Improve hints for connections which support refreshable tokens

**[TW-76715](https://youtrack.jetbrains.com/issue/TW-76715/No-way-to-get-parameter-value-vcsrooturl-without-VCSrootID-if-it-was-inherited-from-the-template-without-VCS-root)** — No way to get parameter value "vcsroot.url" without VCS_root_ID if it was inherited from the template without VCS root

**[TW-77621](https://youtrack.jetbrains.com/issue/TW-77621/Perforce-workspace-cannot-be-deleted-failing-a-build)** — Perforce workspace cannot be deleted, failing a build

**[TW-47425](https://youtrack.jetbrains.com/issue/TW-47425/500-Internal-Server-Error-status-code-returned-when-trying-to-add-build-configuration-via-REST-API-when-server-build)** — 500 (Internal Server Error) status code returned when trying to add build configuration via REST API when server build configuration limit is reached

**[TW-77615](https://youtrack.jetbrains.com/issue/TW-77615/When-adding-a-comment-on-Perforce-Swarm-review-for-a-regular-build-TeamCity-should-do-this-without-sending-a-notification-from)** — When adding a comment on Perforce Swarm review for a regular build, TeamCity should do this without sending a notification from Swarm

**[TW-76830](https://youtrack.jetbrains.com/issue/TW-76830/Add-a-possibility-to-search-by-runners-description-in-the-grid-view-of-Add-build-step-dialog)** — Add a possibility to search by runner's description in the grid view of 'Add build step' dialog

**[TW-76370](https://youtrack.jetbrains.com/issue/TW-76370/Improve-build-status-text-in-case-when-there-is-a-failed-build-step-and-some-failure-conditions)** — Improve build status text in case when there is a failed build step and some failure conditions

**[TW-76947](https://youtrack.jetbrains.com/issue/TW-76947/Parameter-based-Condition-matches-Regex-does-not-work-if-parameter-includes-n)** — Parameter-based Condition "matches" (Regex) does not work if parameter includes "|n"

**[TW-72317](https://youtrack.jetbrains.com/issue/TW-72317/Build-configuration-overview-page-layout-can-be-broken-when-investigation-is-assigned-to-user-with-too-long-name)** — Build configuration overview page layout can be broken when investigation is assigned to user with too long name

**[TW-73042](https://youtrack.jetbrains.com/issue/TW-73042/Tests-with-very-long-names-are-not-presented-fully-and-hide-the-suite-name-in-the-new-UI)** — Tests with very long names are not presented fully and hide the suite name in the new UI

**[TW-75751](https://youtrack.jetbrains.com/issue/TW-75751/Changes-page-Display-the-username-of-current-user-in-the-users-popup)** — Changes page. Display the username of current user in the users popup.

**[TW-73623](https://youtrack.jetbrains.com/issue/TW-73623/There-is-no-way-to-find-out-which-test-suite-has-no-failed-tests)** — There is no way to find out which test suite has no failed tests

**[TW-71875](https://youtrack.jetbrains.com/issue/TW-71875/Tests-UI-is-confusing)** — Tests UI is confusing

**[TW-77355](https://youtrack.jetbrains.com/issue/TW-77355/Show-initial-error-in-Teamcity-UI-if-Restore-from-backup-on-server-start-has-failed)** — Show initial error in Teamcity UI, if Restore from backup on server start has failed

**[TW-72941](https://youtrack.jetbrains.com/issue/TW-72941/Incorrect-encoding-for-the-S3-artifacts-with-Cyrillic-content-opened-in-browser)** — Incorrect encoding for the S3 artifacts with Cyrillic content opened in browser

**[TW-76897](https://youtrack.jetbrains.com/issue/TW-76897/S3-Storage-configuration-errors-appear-at-a-strange-place)** — S3 Storage configuration errors appear at a strange place

**[TW-70989](https://youtrack.jetbrains.com/issue/TW-70989/Passed-test-name-looks-clickable-in-Sakura)** — Passed test name looks clickable in Sakura

**[TW-77576](https://youtrack.jetbrains.com/issue/TW-77576/Agent-is-not-re-assigned-to-the-main-node-if-the-current-node-does-not-have-any-responsibilities)** — Agent is not re-assigned to the main node if the current node does not have any responsibilities

**[TW-77454](https://youtrack.jetbrains.com/issue/TW-77454/Critical-error-with-parsing-build-configuration-xml-file-Error-on-line-107-Invalid-byte-2-of-4-byte-UTF-8-sequence)** — Critical error with parsing build configuration xml file: Error on line 107: Invalid byte 2 of 4-byte UTF-8 sequence.

**[TW-65550](https://youtrack.jetbrains.com/issue/TW-65550/Display-cloud-profiles-project-settings-on-a-secondary-node)** — Display cloud profiles project settings on a secondary node.

**[TW-77342](https://youtrack.jetbrains.com/issue/TW-77342/Error-during-read-from-BuildLog-for-a-BuildProblem)** — Error during read from BuildLog for a BuildProblem

**[TW-71849](https://youtrack.jetbrains.com/issue/TW-71849/Builds-request-from-the-project-overview-page-does-not-take-into-account-Show-all-personal-builds-checkbox)** — Builds request from the project overview page does not take into account 'Show all personal builds' checkbox

**[TW-77157](https://youtrack.jetbrains.com/issue/TW-77157/Parallel-tests-feature-ignores-new-internal-properties)** — Parallel tests feature ignores new internal properties

**[TW-72613](https://youtrack.jetbrains.com/issue/TW-72613/Build-line-is-misaligned-with-a-path-on-a-Test-History-page)** — Build line is misaligned with a path on a Test History page

**[TW-69675](https://youtrack.jetbrains.com/issue/TW-69675/Test-history-page-build-status-can-be-hidden-for-builds-with-long-paths)** — Test history page: build status can be hidden for builds with long paths

**[TW-75131](https://youtrack.jetbrains.com/issue/TW-75131/Teamcity-sometimes-test-history-is-not-folded)** — Teamcity: sometimes test history is not folded

**[TW-73313](https://youtrack.jetbrains.com/issue/TW-73313/Overlapping-text-on-build-history-page)** — Overlapping text on build history page

**[TW-69320](https://youtrack.jetbrains.com/issue/TW-69320/Cancelled-builds-NA-agent-is-shown-as-a-link-invalid)** — Cancelled build's N/A agent is shown as a link (invalid)

**[TW-70562](https://youtrack.jetbrains.com/issue/TW-70562/Do-not-show-link-to-build-agent-details-on-build-overview-page-to-a-user-without-View-agent-details-permission)** — Do not show link to build agent details on build overview page to a user without View agent details permission.

**[TW-66228](https://youtrack.jetbrains.com/issue/TW-66228/Incorrect-data-is-displayed-on-Compatible-Agents-tab-for-the-canceled-build)** — Incorrect data is displayed on Compatible Agents tab for the canceled build.

**[TW-77212](https://youtrack.jetbrains.com/issue/TW-77212/Teamcity-does-not-support-JDK17-based-IntelliJ-versions-eg-20222-as-inspection-engine)** — Teamcity does not support JDK17 based IntelliJ versions (e.g. 2022.2) as inspection engine

**[TW-77134](https://youtrack.jetbrains.com/issue/TW-77134/svnssh-connections-on-the-TeamCity-server-may-generate-threads-overtime)** — svn+ssh connections on the TeamCity server may generate threads overtime

**[TW-68901](https://youtrack.jetbrains.com/issue/TW-68901/Test-history-page-date-on-average-chart-is-cropped)** — Test history page: date on average chart is cropped

**[TW-77283](https://youtrack.jetbrains.com/issue/TW-77283/DSL-documentation-generation-fails-on-Windows)** — DSL documentation generation fails on Windows

**[TW-77233](https://youtrack.jetbrains.com/issue/TW-77233/Server-doesnt-provide-tools-into-teamcity-agentxml)** — Server doesn't provide tools into teamcity-agent.xml

**[TW-76794](https://youtrack.jetbrains.com/issue/TW-76794/StartStop-cloud-instance-is-not-authorized-if-started-immediately-after-stopping)** — Start/Stop cloud instance is not authorized if started immediately after stopping

**[TW-77191](https://youtrack.jetbrains.com/issue/TW-77191/Vcs-trigger-doesnt-trigger-build-settings-VCS-root-is-attached-as-a-regular-one-in-a-dependency)** — Vcs trigger doesn't trigger build (settings VCS root is attached as a regular one in a dependency)

**[TW-76677](https://youtrack.jetbrains.com/issue/TW-76677/Refreshable-tokens-can-be-used-in-the-context-of-project-without-needed-connection)** — Refreshable tokens can be used in the context of project without needed connection

**[TW-68177](https://youtrack.jetbrains.com/issue/TW-68177/Change-make-it-configurable-goal-for-installing-info-maven3-plugin)** — Change (make it configurable) goal for installing info-maven3-plugin

**[TW-77077](https://youtrack.jetbrains.com/issue/TW-77077/BuildLogClosedException-on-main-node-from-BuildFailureOnMetricCondition)** — BuildLogClosedException on main node from BuildFailureOnMetricCondition

**[TW-77104](https://youtrack.jetbrains.com/issue/TW-77104/Pull-Requests-build-feature-logs-errors-with-debug-level-except-for-the-first-time-an-error-is-reported-for-a-build-type)** — Pull Requests build feature logs errors with debug level except for the first time an error is reported for a build type

**[TW-76984](https://youtrack.jetbrains.com/issue/TW-76984/Too-small-minimal-width-for-selected-Project-Path-on-Create-Project-Page)** — Too small minimal width for selected Project Path on Create Project Page

**[TW-77043](https://youtrack.jetbrains.com/issue/TW-77043/Long-project-path-in-Project-Selector-is-not-shortened-at-some-places)** — Long project path in Project Selector is not shortened at some places

**[TW-77049](https://youtrack.jetbrains.com/issue/TW-77049/Classic-UI-Change-Page-Files-Project-selector-may-not-allow-to-select-configurations-with-long-path)** — Classic UI -> Change Page -> Files -> Project selector may not allow to select configurations with long path

**[TW-76875](https://youtrack.jetbrains.com/issue/TW-76875/Heath-report-is-missing-if-some-plugin-DSL-has-compilation-error)** — Heath report is missing if some plugin DSL has compilation error

**[TW-77042](https://youtrack.jetbrains.com/issue/TW-77042/Change-permission-for-Log-out-all-users-action)** — Change permission for Log out all users action

**[TW-66071](https://youtrack.jetbrains.com/issue/TW-66071/Composite-build-tests-mutes-dont-work-as-expected)** — Composite build: tests mutes don't work as expected

**[TW-77046](https://youtrack.jetbrains.com/issue/TW-77046/Uncaught-ADE-when-retrieving-build-type-of-the-build-promotion)** — Uncaught ADE when retrieving build type of the build promotion

**[TW-76987](https://youtrack.jetbrains.com/issue/TW-76987/Generate-DSL-documentation-fails-if-server-is-started-from-a-directory-with-spaces)** — Generate DSL documentation fails, if server is started from a directory with spaces

**[TW-77047](https://youtrack.jetbrains.com/issue/TW-77047/Avoid-making-additional-commits-if-an-id-of-a-build-configuration-is-changed-via-a-VCS-commit-or-on-disk)** — Avoid making additional commits if an id of a build configuration is changed via a VCS commit or on disk

**[TW-76836](https://youtrack.jetbrains.com/issue/TW-76836/HTTP-proxy-does-not-switch-to-another-node-if-the-main-node-was-killed)** — HTTP proxy does not switch to another node if the main node was killed

**[TW-76985](https://youtrack.jetbrains.com/issue/TW-76985/NullPointerException-when-trying-to-create-a-server-thread-dump-with-token-authentication)** — NullPointerException when trying to create a server thread dump with token authentication

**[TW-78072](https://youtrack.jetbrains.com/issue/TW-78072/Update-commons-text-in-Python-plugin-to-1100)** — Update commons-text in Python plugin to 1.10.0

**[TW-78168](https://youtrack.jetbrains.com/issue/TW-78168/Update-commons-text-in-qodana-plugin-to-1100)** — Update commons-text in qodana plugin to 1.10.0

### Performance Problem

**[TW-77799](https://youtrack.jetbrains.com/issue/TW-77799/Builds-indexer-occupies-1Gb-of-memory)** — Builds indexer occupies ~1Gb of memory

**[TW-78051](https://youtrack.jetbrains.com/issue/TW-78051/Use-own-executor-for-the-multi-node-event-threads)** — Use own executor for the multi node event threads

**[TW-77927](https://youtrack.jetbrains.com/issue/TW-77927/Relatively-large-chunk-of-memory-is-occupied-by-cleanup-and-probably-kept-for-the-entire-cleanup-time)** — Relatively large chunk of memory is occupied by cleanup and probably kept for the entire cleanup time

**[TW-77257](https://youtrack.jetbrains.com/issue/TW-77257/Significant-performance-degradation-of-calls-for-builds-with-non-default-branch)** — Significant performance degradation of calls for builds with non default branch

**[TW-77398](https://youtrack.jetbrains.com/issue/TW-77398/Several-expensive-overviewstatusestrue-POST-requests-are-sent-by-the-same-user)** — Several expensive /overview?statuses=true POST requests are sent by the same user

**[TW-77485](https://youtrack.jetbrains.com/issue/TW-77485/ObsoleteBuildProblemResponsibilitiesCleanupExtensionafterCleanup-is-too-slow-takes-hours-to-complete)** — ObsoleteBuildProblemResponsibilitiesCleanupExtension.afterCleanup is too slow (takes hours to complete)

**[TW-77234](https://youtrack.jetbrains.com/issue/TW-77234/Tool-uploaded-on-secondary-node-is-not-visible-on-the-main-one)** — Tool uploaded on secondary node is not visible on the main one

**[TW-76907](https://youtrack.jetbrains.com/issue/TW-76907/Slow-user-groups-page-slow-containsAllPermissionsOf-method)** — Slow user groups page (slow containsAllPermissionsOf method)

### Security

6 security problems have been fixed.















