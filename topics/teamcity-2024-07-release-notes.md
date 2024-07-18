[//]: # (title: TeamCity 2024.07 Release Notes)
[//]: # (auxiliary-id: TeamCity 2024.07 Release Notes)


**Build 160569, 18 July 2024**


<!--project: TeamCity Fix versions: {2024.07 (160569)}  #Fixed #Testing visible to: {All Users} -{Trunk issue}-->

### Feature

**[TW-75072](https://youtrack.jetbrains.com/issue/TW-75072/Commit-all-configuration-changes-in-a-Git-repository)** — Commit all configuration changes in a Git repository

**[TW-71572](https://youtrack.jetbrains.com/issue/TW-71572/Sakura-UI-Project-Build-Configuration-and-Failed-test-reports-tab)** — Sakura UI: Project, Build Configuration and Failed test reports tab

**[TW-70264](https://youtrack.jetbrains.com/issue/TW-70264/Allow-uploading-SSH-keys-on-create-project-VCS-root-from-URL-page)** — Allow uploading SSH keys on create project/VCS root from URL page

**[TW-84463](https://youtrack.jetbrains.com/issue/TW-84463/Create-GitHub-Checks-API-trigger)** — Create GitHub Checks API trigger

**[TW-32570](https://youtrack.jetbrains.com/issue/TW-32570/Add-ability-to-turn-off-recursive-git-submodule-init)** — Add ability to turn off recursive git submodule init

**[TW-58812](https://youtrack.jetbrains.com/issue/TW-58812/Custom-path-in-the-repository-for-versioned-settings)** — Custom path in the repository for versioned settings

**[TW-6141](https://youtrack.jetbrains.com/issue/TW-6141/Ability-to-change-VCS-label-description-for-Perforce-P4)** — Ability to change VCS label description for Perforce/P4

**[TW-86474](https://youtrack.jetbrains.com/issue/TW-86474/Add-prometheus-counter-for-unassignable-builds-in-queue)** — Add prometheus counter for unassignable builds in queue

**[TW-14646](https://youtrack.jetbrains.com/issue/TW-14646/Ability-to-run-custom-task-before-a-branch-is-checked-out-on-agent-bootstrap-steps)** — Ability to run custom task before a branch is checked out on agent (bootstrap steps)

**[TW-49274](https://youtrack.jetbrains.com/issue/TW-49274/REST-Allow-to-map-passwords-to-settings-usable-strings)** — REST: Allow to map passwords to settings-usable strings

**[TW-85645](https://youtrack.jetbrains.com/issue/TW-85645/Add-prometheus-metric-for-versioned-settings-executor-threads)** — Add prometheus metric for versioned settings executor threads

**[TW-88290](https://youtrack.jetbrains.com/issue/TW-88290/Allow-triggering-build-with-checkout-rules-with-custom-upper-limit-revision-via-REST-API)** — Allow triggering build with checkout rules with custom upper limit revision via REST API



### Bug

**[TW-88410](https://youtrack.jetbrains.com/issue/TW-88410/Always-even-if-build-stop-command-was-issued-step-is-not-executed-as-expected)** — "Always, even if build stop command was issued" step is not executed as expected

**[TW-87486](https://youtrack.jetbrains.com/issue/TW-87486/Inability-to-Override-hashiCorpVaultParameter-in-TeamCity-When-Declared-in-Templates)** — Inability to Override hashiCorpVaultParameter in TeamCity When Declared in Templates

**[TW-85376](https://youtrack.jetbrains.com/issue/TW-85376/Copy-of-project-does-not-retain-archived-state-for-subproject)** — Copy of project does not retain archived state for subproject

**[TW-86876](https://youtrack.jetbrains.com/issue/TW-86876/builds-arent-reusing-when-triggered-on-non-default-branch-and-with-parallel-tests-build-feature)** — builds aren't reusing when triggered on non-default branch and with parallel tests build feature

**[TW-74238](https://youtrack.jetbrains.com/issue/TW-74238/Commit-Status-Publisher-does-not-change-commit-status-when-successfully-finished-build-is-marked-as-failed)** — Commit Status Publisher does not change commit status when successfully finished build is marked as failed

**[TW-54361](https://youtrack.jetbrains.com/issue/TW-54361/Confusing-pending-changes-and-history-builds-after-force-push-in-git)** — Confusing pending changes and history builds after force push in git

**[TW-59643](https://youtrack.jetbrains.com/issue/TW-59643/Support-Perforce-Ditto-Mappings)** — Support Perforce Ditto Mappings

**[TW-87246](https://youtrack.jetbrains.com/issue/TW-87246/Build-requires-my-approval-notification-email-is-not-sent-for-builds-from-Retry-trigger-and-Re-run-action)** — "Build requires my approval" notification email is not sent for builds from Retry trigger and Re-run action

**[TW-87670](https://youtrack.jetbrains.com/issue/TW-87670/Create-new-project-from-repository-URL-with-token-and-without-username-fails-with-Anonymous-authentication-has-failed)** — Create new project from repository URL with token and without username fails with "Anonymous authentication has failed"

**[TW-88864](https://youtrack.jetbrains.com/issue/TW-88864/Deadlock-while-applying-settings-from-VCS)** — Deadlock while applying settings from VCS

**[TW-88200](https://youtrack.jetbrains.com/issue/TW-88200/Artifact-dependency-for-filenames-with-symbol)** — Artifact dependency for filenames with '%' symbol

**[TW-85982](https://youtrack.jetbrains.com/issue/TW-85982/Edition-of-the-existing-credential-after-custom-encryption-key-regeneration-does-not-re-encrypt-current-values)** — Edition of the existing credential after custom encryption key regeneration does not re-encrypt current values

**[TW-88820](https://youtrack.jetbrains.com/issue/TW-88820/JB-license-Server-cannot-start-if-it-couldnt-parse-corrupted-JWT-token)** — JB license: Server cannot start, if it couldn't parse corrupted JWT token

**[TW-88240](https://youtrack.jetbrains.com/issue/TW-88240/Failed-to-run-telegraf-with-config-file)** — Failed to run telegraf with config file

**[TW-88845](https://youtrack.jetbrains.com/issue/TW-88845/Make-Build.isChangesCollectingInProgress-lazy-by-default)** — Make Build.isChangesCollectingInProgress lazy by default

**[TW-88728](https://youtrack.jetbrains.com/issue/TW-88728/Build-incorrectly-marked-as-History-in-case-of-a-fast-forward-merge)** — Build incorrectly marked as History in case of a fast forward merge

**[TW-85221](https://youtrack.jetbrains.com/issue/TW-85221/java.lang.ArrayIndexOutOfBoundsException-from-PerfMon)** — java.lang.ArrayIndexOutOfBoundsException from PerfMon

**[TW-87723](https://youtrack.jetbrains.com/issue/TW-87723/Changes-are-not-collected-if-the-Pull-Request-source-branch-contains-brackets-and-the-VCS-Root-branch-spec-is-not-empty)** — Changes are not collected if the Pull Request source branch contains brackets and the VCS Root branch spec is not empty

**[TW-88772](https://youtrack.jetbrains.com/issue/TW-88772/Matrix-builds-cannot-start-because-of-disabled-agent-requirements-from-parent-build)** — Matrix builds cannot start because of disabled agent requirements from parent build

**[TW-88131](https://youtrack.jetbrains.com/issue/TW-88131/TeamCity-can-produce-excess-load-to-a-cloud-provider-because-of-multiple-non-cached-requests)** — TeamCity can produce excess load to a cloud provider because of multiple non-cached requests

**[TW-88309](https://youtrack.jetbrains.com/issue/TW-88309/No-way-to-configure-Dependency-Cache-build-feature-using-Kotlin-DSL)** — No way to configure Dependency Cache build feature using Kotlin DSL

**[TW-84028](https://youtrack.jetbrains.com/issue/TW-84028/Improve-the-error-message-for-GitHub-App-Test-Connection-in-case-if-incorrect-settings-are-used)** — Improve the error message for GitHub App Test Connection in case if incorrect settings are used

**[TW-86092](https://youtrack.jetbrains.com/issue/TW-86092/Access-token-is-not-saved-if-the-project-is-created-from-scratch)** — Access token is not saved if the project is created from scratch

**[TW-74537](https://youtrack.jetbrains.com/issue/TW-74537/Exception-on-first-login-via-GitLab-Auth-for-a-user-without-public-email)** — Exception on first login via GitLab Auth for a user without public email

**[TW-82002](https://youtrack.jetbrains.com/issue/TW-82002/Infinite-loader-on-the-non-existent-test-page.)** — Infinite loader on the non-existent test page.

**[TW-85803](https://youtrack.jetbrains.com/issue/TW-85803/Perforce-Helix-Swarm-Commit-Status-Publisher-only-publishes-comments-on-reviews-in-an-open-state)** — Perforce Helix Swarm: Commit Status Publisher only publishes comments on reviews in an open state

**[TW-85490](https://youtrack.jetbrains.com/issue/TW-85490/SSH-Exec-step-should-fail-if-we-got-non-zero-exit-code)** — SSH Exec step should fail if we got non-zero exit code

**[TW-87278](https://youtrack.jetbrains.com/issue/TW-87278/Sending-verification-e-mail-does-not-work-on-secondary-nodes)** — Sending verification e-mail does not work on secondary nodes

**[TW-87892](https://youtrack.jetbrains.com/issue/TW-87892/Converter-DownloadedArtifactsIndexesConverter-fails-during-the-update-from-TeamCity-7.x-to-2024.03.x)** — Converter DownloadedArtifactsIndexesConverter fails during the update from TeamCity 7.x to 2024.03.x

**[TW-88359](https://youtrack.jetbrains.com/issue/TW-88359/Do-not-merge-test-outputs-for-different-flows-in-TestOutputMergingIterator)** — Do not merge test outputs for different flows in TestOutputMergingIterator

**[TW-88358](https://youtrack.jetbrains.com/issue/TW-88358/Align-multiline-messages-when-downloading-buildlogs)** — Align multiline messages when downloading buildlogs

**[TW-88357](https://youtrack.jetbrains.com/issue/TW-88357/Support-flowAware-mode-when-downloading-buildlogs)** — Support flowAware mode when downloading buildlogs

**[TW-88534](https://youtrack.jetbrains.com/issue/TW-88534/Unable-to-collect-changes-error-in-case-of-parametrized-settings-VCS-root)** — Unable to collect changes error in case of parametrized settings VCS root

**[TW-83884](https://youtrack.jetbrains.com/issue/TW-83884/Improve-the-dialog-of-the-Manual-Creation-of-the-GitHub-App-Connection-in-case-of-long-TeamCity-Server-URL)** — Improve the dialog of the Manual Creation of the GitHub App Connection in case of long TeamCity Server URL

**[TW-84326](https://youtrack.jetbrains.com/issue/TW-84326/Mercurial-plugin-doesnt-support-updated-share-extension-behavior)** — Mercurial plugin doesn't support updated share extension behavior

**[TW-87108](https://youtrack.jetbrains.com/issue/TW-87108/Container-info-tab-change-column-name-from-Image-to-Digest)** — Container info tab: change column name from Image to Digest

**[TW-83901](https://youtrack.jetbrains.com/issue/TW-83901/Vertically-resized-Value-field-in-Add-new-parameter-dialog-jumps-to-smaller-size-when-moving-cursor-inside-the-field)** — Vertically resized Value field in "Add new parameter dialog" jumps to smaller size when moving cursor inside the field

**[TW-88299](https://youtrack.jetbrains.com/issue/TW-88299/NPE-parsing-invalid-Space-init-payload-state)** — NPE parsing invalid Space init payload state

**[TW-88441](https://youtrack.jetbrains.com/issue/TW-88441/BuildLog-is-not-displayed-correctly-for-some-of-the-builds-produced-by-older-servers)** — BuildLog is not displayed correctly for some of the builds produced by older servers

**[TW-87489](https://youtrack.jetbrains.com/issue/TW-87489/Build-settings-have-not-been-finalized-for-hours)** — "Build settings have not been finalized" for hours

**[TW-87244](https://youtrack.jetbrains.com/issue/TW-87244/Retried-re-run-builds-do-not-Log-untrusted-builds-to-build-log)** — Retried/re-run builds do not Log untrusted builds to build log

**[TW-88036](https://youtrack.jetbrains.com/issue/TW-88036/Agent-shutdown-in-a-multi-node-environment-may-result-in-a-failed-to-start-parent)** — Agent shutdown in a multi-node environment may result in a failed to start parent

**[TW-84536](https://youtrack.jetbrains.com/issue/TW-84536/No-Build-problem-or-error-message-in-the-log-if-commit-status-publisher-failed-to-publish-the-status-because-the-incorrect)** — No Build problem or error message in the log if commit status publisher failed to publish the status because the incorrect Server URL

**[TW-84271](https://youtrack.jetbrains.com/issue/TW-84271/Copy-to-clipboard-in-custom-report)** — Copy to clipboard in custom report

**[TW-88133](https://youtrack.jetbrains.com/issue/TW-88133/Git-credential-helper-cant-handle-missing-passwords)** — Git credential helper can't handle missing passwords

**[TW-88251](https://youtrack.jetbrains.com/issue/TW-88251/java.lang.InstantiationException-bean-historyPager-not-found-within-scope)** — java.lang.InstantiationException: bean historyPager not found within scope

**[TW-80103](https://youtrack.jetbrains.com/issue/TW-80103/Agent-Terminal-doesnt-support-reloading-without-server-restart)** — Agent Terminal doesn't support reloading without server restart

**[TW-86594](https://youtrack.jetbrains.com/issue/TW-86594/Docker-compose-runner-doesnt-work-with-a-podman-compose)** — Docker-compose runner doesn't work with a podman-compose

**[TW-87293](https://youtrack.jetbrains.com/issue/TW-87293/Internal-error-occurred-in-Docker-Compose-builds-running-on-podman-agents)** — "Internal error occurred" in Docker Compose builds running on podman agents

**[TW-86820](https://youtrack.jetbrains.com/issue/TW-86820/Redesign-the-Add-new-parameter-dialog-disable-the-button-Delete-appearance-settings-when-parameter-is-uneditable)** — Redesign the "Add new parameter" dialog: disable the button "Delete appearance settings" when parameter is uneditable

**[TW-88041](https://youtrack.jetbrains.com/issue/TW-88041/Fix-white-lists-property-delimiter-in-teamcity-caches-cleanup-plugin)** — Fix white lists property delimiter in teamcity-caches-cleanup-plugin

**[TW-88252](https://youtrack.jetbrains.com/issue/TW-88252/Token-names-seem-to-vanish)** — Token names seem to vanish

**[TW-85187](https://youtrack.jetbrains.com/issue/TW-85187/Use-previous-upper-limit-revisions-for-the-checking-for-changes-after-revisions-reset)** — Use previous upper limit revisions for the checking for changes after revisions reset

**[TW-88075](https://youtrack.jetbrains.com/issue/TW-88075/Space-authentication-module-can-choose-unsuitable-connection)** — Space authentication module can choose unsuitable connection

**[TW-87387](https://youtrack.jetbrains.com/issue/TW-87387/BuildTypeImpl.getAdditionalBranchSpecs-can-send-an-HTTP-request)** — BuildTypeImpl.getAdditionalBranchSpecs can send an HTTP request

**[TW-87432](https://youtrack.jetbrains.com/issue/TW-87432/Lens-plugin-does-not-re-establish-new-connections-in-case-of-broken-connection-with-a-OTEL-collector)** — Lens plugin does not re-establish new connections in case of broken connection with a OTEL collector

**[TW-88096](https://youtrack.jetbrains.com/issue/TW-88096/Token-table-doesnt-allow-to-filter-by-all-available-connections)** — Token table doesn't allow to filter by all available connections

**[TW-87896](https://youtrack.jetbrains.com/issue/TW-87896/Checking-for-changes-task-will-not-finish-if-build-chain-has-a-build-whose-build-configuration-does-not-exist)** — Checking for changes task will not finish if build chain has a build whose build configuration does not exist

**[TW-63400](https://youtrack.jetbrains.com/issue/TW-63400/Some-links-opens-the-href-pages-in-new-UI-even-if-user-has-not-checked-option-use-experimental-UI)** — Some links opens the href pages in new UI even if user has not checked option 'use experimental UI'

**[TW-71871](https://youtrack.jetbrains.com/issue/TW-71871/Build-log-excessive-white-space-while-scrolling-in-Safari)** — Build log: excessive white space while scrolling in Safari

**[TW-85340](https://youtrack.jetbrains.com/issue/TW-85340/Upgraded-agents-can-run-builds-with-the-deleted-version-of-the-tool)** — Upgraded agents can run builds with the deleted version of the tool

**[TW-87859](https://youtrack.jetbrains.com/issue/TW-87859/Branch-name-does-not-fit-into-its-element)** — Branch name does not fit into its element

**[TW-87614](https://youtrack.jetbrains.com/issue/TW-87614/Taking-build-cache-from-the-default-branch-does-not-work)** — Taking build cache from the default branch does not work

**[TW-86570](https://youtrack.jetbrains.com/issue/TW-86570/Archived-projects-are-not-shown-in-the-new-Agent-pool-UI)** — Archived projects are not shown in the new Agent pool UI

**[TW-87264](https://youtrack.jetbrains.com/issue/TW-87264/Lens-plugin-unhandled-exception)** — Lens plugin: unhandled exception

**[TW-86647](https://youtrack.jetbrains.com/issue/TW-86647/Description-error-of-getBuildResultingProperties-Rest-API)** — Description error of getBuildResultingProperties Rest API

**[TW-87777](https://youtrack.jetbrains.com/issue/TW-87777/Incorrect-test-artifacts-in-the-test-metadata-in-Classic-UI-same-test-name-different-metadata)** — Incorrect test artifacts in the test metadata in Classic UI (same test name, different metadata)

**[TW-87497](https://youtrack.jetbrains.com/issue/TW-87497/Difficulties-finding-documentation-for-JetBrains-hosted-agents-in-TeamCity-Cloud)** — Difficulties finding documentation for JetBrains-hosted agents in TeamCity Cloud

**[TW-87274](https://youtrack.jetbrains.com/issue/TW-87274/Bitbucket-server-OAuth-sign-in-can-fail-to-fetch-current-user)** — Bitbucket server: OAuth sign-in can fail to fetch current user

**[TW-87134](https://youtrack.jetbrains.com/issue/TW-87134/Changes-collection-failures-due-to-massive-refspecs-being-passed-to-git-fetch-operation)** — Changes collection failures due to massive refspecs being passed to git fetch operation

**[TW-63051](https://youtrack.jetbrains.com/issue/TW-63051/Tests-tab-should-include-total-counter-of-tests-and-their-summary-duration)** — Tests tab should include total counter of tests and their summary duration

**[TW-87084](https://youtrack.jetbrains.com/issue/TW-87084/Multiple-warnings-from-eApplicationInformationManager-if-Space-connection-application-has-Invalid-client-service-secret)** — Multiple warnings from eApplicationInformationManager if Space connection application has Invalid client service secret

**[TW-87182](https://youtrack.jetbrains.com/issue/TW-87182/Charts-have-black-lines-significantly-obscuring-visibility-in-statistics)** — Charts have black lines significantly obscuring visibility in statistics

**[TW-87413](https://youtrack.jetbrains.com/issue/TW-87413/Reset-password-page-doesnt-work-on-secondary-server-node)** — Reset password page doesn't work on secondary server node

**[TW-82543](https://youtrack.jetbrains.com/issue/TW-82543/Broken-UI-in-the-Promote-Build-dialog-when-teamcity.ui.runButton.caption-is-set-to-an-empty-value)** — Broken UI in the Promote Build dialog when "teamcity.ui.runButton.caption" is set to an empty value

**[TW-85720](https://youtrack.jetbrains.com/issue/TW-85720/A-lot-of-NPEs-may-be-logged-if-caching-estimator-was-unable-to-initialize)** — A lot of NPEs may be logged if caching estimator was unable to initialize

**[TW-82895](https://youtrack.jetbrains.com/issue/TW-82895/Using-incompatible-fetch-and-push-URL-in-a-Git-VCS-root-results-in-a-confusing-error)** — Using incompatible fetch and push URL in a Git VCS root results in a confusing error

**[TW-87360](https://youtrack.jetbrains.com/issue/TW-87360/Checkout-rules-are-not-taken-into-account-when-revision-is-calculated-for-overridden-VCS-root)** — Checkout rules are not taken into account when revision is calculated for overridden VCS root

**[TW-86315](https://youtrack.jetbrains.com/issue/TW-86315/Failed-to-perform-checkout-on-agent-Problem-while-checkout-on-agent-java.lang.IllegalStateException-NotNull-method-jetbrains)** — Failed to perform checkout on agent: Problem while checkout on agent: java.lang.IllegalStateException: @NotNull method jetbrains/buildServer/vcs/perforce/ClientNameBuilder.getWorkspaceName must not return null

**[TW-85768](https://youtrack.jetbrains.com/issue/TW-85768/Inconsistent-capitalisation-in-the-tests-actions-menu)** — Inconsistent capitalisation in the test's actions menu

**[TW-85777](https://youtrack.jetbrains.com/issue/TW-85777/Test-action-menu-Show-in-Build-Log-shouldnt-be-a-link)** — Test action menu: "Show in Build Log" shouldn't be a link

**[TW-85021](https://youtrack.jetbrains.com/issue/TW-85021/Clean-up-Settings-Periodical-Periodic)** — Clean-up Settings: "Periodical" -> "Periodic"

**[TW-85837](https://youtrack.jetbrains.com/issue/TW-85837/Show-changes-from-dependencies-checkbox-is-not-shown-on-the-Build-Changes-tab)** — "Show changes from dependencies" checkbox is not shown on the Build Changes tab

**[TW-87131](https://youtrack.jetbrains.com/issue/TW-87131/Space-character-in-checkout-rules-defined-in-DSL-may-lead-to-false-detection-of-change-in-version-control-settings-of-a-build)** — Space character in checkout rules defined in DSL may lead to false detection of change in version control settings of a build configuration

**[TW-85991](https://youtrack.jetbrains.com/issue/TW-85991/Build-configuration-does-not-fill-in-current-project-information.)** — Build configuration does not fill in current project information.

**[TW-84998](https://youtrack.jetbrains.com/issue/TW-84998/settings-tab-prompt-Change-Show-more-to-Show-all)** — settings tab prompt: Change "Show more >>" to "Show all >>"

**[TW-87115](https://youtrack.jetbrains.com/issue/TW-87115/Avoid-generating-automatic-branch-label-if-build-has-a-non-empty-branch-specification)** — Avoid generating automatic branch label if build has a non empty branch specification

**[TW-86200](https://youtrack.jetbrains.com/issue/TW-86200/Agents-look-incompatible-after-installation-of-missing-non-default-tool-version-until-re-save-of-build-step-settings)** — Agents look incompatible after installation of missing non-default tool version until re-save of build step settings

**[TW-86793](https://youtrack.jetbrains.com/issue/TW-86793/Lens-plugin-ignores-test-data-event-limits)** — Lens plugin ignores test data event limits

**[TW-86258](https://youtrack.jetbrains.com/issue/TW-86258/Len-plugin-S3-event-names-are-not-aligned-with-OTEL-convention)** — Len plugin S3 event names are not aligned with OTEL convention

**[TW-86576](https://youtrack.jetbrains.com/issue/TW-86576/Remove-failed-to-start-builds-limit-from-the-retry-build-trigger)** — Remove failed to start builds limit from the retry build trigger

**[TW-79776](https://youtrack.jetbrains.com/issue/TW-79776/No-escaping-of-values-what-used-during-labelling-build-sources)** — No escaping of values what used during labelling build sources

**[TW-84589](https://youtrack.jetbrains.com/issue/TW-84589/authentication-modules-shows-alert-discard-your-changes-on-profile-page)** — authentication modules shows alert "discard your changes" on profile page


### Task

**[TW-39885](https://youtrack.jetbrains.com/issue/TW-39885/Add-tests-total-duration-statistics-metric)** — Add tests total duration statistics metric

**[TW-86140](https://youtrack.jetbrains.com/issue/TW-86140/Determine-the-visibility-of-the-repository-and-cache-the-information)** — Determine the visibility of the repository and cache the information

**[TW-87305](https://youtrack.jetbrains.com/issue/TW-87305/R-inspections-since-2024.1-its-necessary-to-specify-output-format-explicitly)** — R# inspections: since 2024.1 it's necessary to specify output format explicitly

**[TW-88521](https://youtrack.jetbrains.com/issue/TW-88521/Support-back-slash-as-a-default-escape-sequence-for-branch-specifications)** — Support back slash as a default escape sequence for branch specifications

**[TW-86894](https://youtrack.jetbrains.com/issue/TW-86894/Remove-the-passwords-deprecated-health-report-for-VCS-roots-created-from-GitHub)** — Remove the passwords deprecated health report for VCS roots created from GitHub

**[TW-86496](https://youtrack.jetbrains.com/issue/TW-86496/Update-JDBC-drivers-to-newer-versions)** — Update JDBC drivers to newer versions

**[TW-87172](https://youtrack.jetbrains.com/issue/TW-87172/Provide-the-list-of-allowed-values-for-fields-in-the-auto-generated-REST-API-documentation)** — Provide the list of allowed values for fields in the auto-generated REST API documentation

**[TW-84382](https://youtrack.jetbrains.com/issue/TW-84382/Remove-ReSharper-CLT-bundled-tool-and-install-it-after-TeamCity-server-startup)** — Remove ReSharper CLT bundled tool and install it after TeamCity server startup

**[TW-78795](https://youtrack.jetbrains.com/issue/TW-78795/Improve-page-personal-build.html)** — Improve page "personal-build.html"

**[TW-86185](https://youtrack.jetbrains.com/issue/TW-86185/Support-testRetry-in-the-Gradle-Runner-with-gradle-enterprise-plugin)** — Support testRetry in the Gradle Runner with gradle enterprise plugin

**[TW-84380](https://youtrack.jetbrains.com/issue/TW-84380/Remove-dotCover-bundled-tool-and-install-it-after-TeamCity-server-startup)** — Remove dotCover bundled tool and install it after TeamCity server startup

**[TW-86054](https://youtrack.jetbrains.com/issue/TW-86054/Update-Gradle-Tooling-API-in-Gradle-plugin-to-the-latest-version)** — Update Gradle Tooling API in Gradle plugin to the latest version



### Performance Problem

**[TW-88527](https://youtrack.jetbrains.com/issue/TW-88527/Automatic-thread-dumps-frequently-have-multiple-app-perforce-commitHook-threads)** — Automatic thread dumps frequently have multiple /app/perforce/commitHook threads

**[TW-88306](https://youtrack.jetbrains.com/issue/TW-88306/A-page-opened-as-a-background-tab-is-not-rendered-until-visit)** — A page opened as a background tab is not rendered until visit

**[TW-87138](https://youtrack.jetbrains.com/issue/TW-87138/Speedup-start-of-the-builds-having-.teamcity-directory-in-the-main-repository)** — Speedup start of the builds having .teamcity directory in the main repository

**[TW-73505](https://youtrack.jetbrains.com/issue/TW-73505/Very-slow-loading-of-the-newly-generated-build-types-because-of-disk-usage-intializing-data-for-each-newly-registered-build)** — Very slow loading of the newly generated build types because of disk usage intializing data for each newly registered build configuration

**[TW-84245](https://youtrack.jetbrains.com/issue/TW-84245/Slow-REST-API-request-fetching-deployment-builds)** — Slow REST API request fetching deployment builds

**[TW-88178](https://youtrack.jetbrains.com/issue/TW-88178/Changes-page-in-Sakura-takes-a-while-to-load-without-progress)** — Changes page in Sakura takes a while to load without progress

**[TW-88123](https://youtrack.jetbrains.com/issue/TW-88123/Inefficient-DBVcsModificationHistory.getModificationsInVersionsRange-slows-down-REST-API-call)** — Inefficient DBVcsModificationHistory.getModificationsInVersionsRange() slows down REST API call

**[TW-88113](https://youtrack.jetbrains.com/issue/TW-88113/Unloading-of-too-many-commits-at-once-as-a-result-of-a-cleanup-can-greatly-slow-down-events-processing)** — Unloading of too many commits at once as a result of a cleanup can greatly slow down events processing

**[TW-87805](https://youtrack.jetbrains.com/issue/TW-87805/Processing-of-settings-persist-queue-may-be-very-slow)** — Processing of settings persist queue may be very slow

**[TW-78253](https://youtrack.jetbrains.com/issue/TW-78253/Slow-triggers-processing-possibly-because-some-of-the-VCS-commits-are-unloaded-from-the-cache-because-they-are-too-old)** — Slow triggers processing possibly because some of the VCS commits are unloaded from the cache because they are too old

**[TW-84069](https://youtrack.jetbrains.com/issue/TW-84069/Favorite-builds-page-is-slow-if-there-are-many-favorite-builds-found-for-a-user)** — Favorite builds page is slow if there are many favorite builds found for a user

**[TW-87477](https://youtrack.jetbrains.com/issue/TW-87477/RawParameterImpl-and-ParameterUtil2-can-occupy-significant-memory-if-there-is-a-select-parameter-types-with-lots-of-options)** — RawParameterImpl and ParameterUtil$2 can occupy significant memory if there is a "select" parameter types with lots of options

**[TW-84141](https://youtrack.jetbrains.com/issue/TW-84141/TestFailureRateCollector-threads-occupying-Normal-executor-thread-pool)** — TestFailureRateCollector threads occupying Normal executor thread pool

**[TW-63877](https://youtrack.jetbrains.com/issue/TW-63877/Single-slow-trigger-can-prevent-other-build-configurations-from-triggering-even-when-multiple-trigger-texecutor-threads-are)** — Single slow trigger can prevent other build configurations from triggering even when multiple trigger texecutor threads are configured

**[TW-87192](https://youtrack.jetbrains.com/issue/TW-87192/Improve-performance-of-multi-node-tasks-processing)** — Improve performance of multi node tasks processing

**[TW-86911](https://youtrack.jetbrains.com/issue/TW-86911/Inefficient-code-in-Change.isVersionedSettings-possibly-leading-to-higher-CPU-usage)** — Inefficient code in Change.isVersionedSettings possibly leading to higher CPU usage






<!--project: TeamCity Fix versions: {2024.07 (160569)}  #Fixed #Testing #{Security Problem} -{Trunk issue}-->



### Security

19 security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2024.03). Security bulletins for new versions are typically published within the next few days after the release date.