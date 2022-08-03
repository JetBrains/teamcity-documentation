[//]: # (title: TeamCity Release Notes: Build 115573)
[//]: # (auxiliary-id: TeamCity Release Notes: Build 115573;TeamCity 2022.08 Release Notes)

__Build: 115573__

__3 August 2022__

### Feature

**[TW-76678](https://youtrack.jetbrains.com/issue/TW-76678/Add-new-project-selector-to-all-dialogs-in-the-administration-area)** — Add new project selector to all dialogs in the administration area

**[TW-76649](https://youtrack.jetbrains.com/issue/TW-76649/Flatten-dropdown-lists-in-Add-build-step-Admin-UI)** — Flatten dropdown lists in 'Add build step' Admin UI

**[TW-76805](https://youtrack.jetbrains.com/issue/TW-76805/Introduce-new-permission-CHANGEVCSUSERNAMEINPROJECT)** — Introduce new permission CHANGE_VCS_USERNAME_IN_PROJECT

**[TW-75683](https://youtrack.jetbrains.com/issue/TW-75683/Cloud-agent-maintenance-mode)** — Cloud agent maintenance mode

**[TW-76756](https://youtrack.jetbrains.com/issue/TW-76756/Allow-login-using-a-restricted-access-token)** — Allow login using a restricted access token

**[TW-75796](https://youtrack.jetbrains.com/issue/TW-75796/P4-is-unable-to-establish-trust-after-a-certificate-update)** — P4 is unable to establish "trust" after a certificate update

**[TW-76242](https://youtrack.jetbrains.com/issue/TW-76242/Cloud-agent-instance-page-add-stop-button)** — Cloud agent instance page: add "stop" button

**[TW-70952](https://youtrack.jetbrains.com/issue/TW-70952/Support-refresh-token-workflow-for-existing-oAuth-connections)** — Support refresh token workflow for existing oAuth connections

**[TW-17122](https://youtrack.jetbrains.com/issue/TW-17122/Categoriesgrouping-for-build-runners)** — Categories/grouping for build runners

**[TW-24375](https://youtrack.jetbrains.com/issue/TW-24375/Allow-to-zip-big-artifacts-more-than-4Gb)** — Allow to zip big artifacts (more than 4Gb)

**[TW-73520](https://youtrack.jetbrains.com/issue/TW-73520/Add-avatars-to-various-places-in-Classic-UI)** — Add avatars to various places in Classic UI

### Usability Problem

**[TW-75570](https://youtrack.jetbrains.com/issue/TW-75570/S3-Artifacts-Storage-the-default-expiration-time-for-pre-signed-URLs-may-not-be-enough-in-certain-cases)** — S3 Artifacts Storage: the default expiration time for pre-signed URLs may not be enough in certain cases

**[TW-73934](https://youtrack.jetbrains.com/issue/TW-73934/Return-the-success-rate-in-percentages-to-its-place)** — Return the success rate in percentages to its place

**[TW-75474](https://youtrack.jetbrains.com/issue/TW-75474/Changes-page-provide-information-about-VCS-root-where-the-change-was-commited)** — Changes page: provide information about VCS root where the change was commited.

**[TW-73655](https://youtrack.jetbrains.com/issue/TW-73655/Add-a-loader-for-tabs)** — Add a loader for tabs

**[TW-69729](https://youtrack.jetbrains.com/issue/TW-69729/Shared-Resource-Usages-Report-should-be-alpha-sorted)** — Shared Resource Usages Report should be alpha-sorted

**[TW-76674](https://youtrack.jetbrains.com/issue/TW-76674/No-enabled-compatible-agents-warning-is-shown-to-a-user-when-build-is-going-to-be-run-on-an-cloud-agent-defined-in-project-where)** — 'No enabled compatible agents' warning is shown to a user when build is going to be run on an cloud agent defined in project where user doesn't have VIEW_AGENT_CLOUDS permission

**[TW-76482](https://youtrack.jetbrains.com/issue/TW-76482/Add-a-link-to-running-build-to-the-instances-popup)** — Add a link to running build to the instances popup

**[TW-76481](https://youtrack.jetbrains.com/issue/TW-76481/Instances-popup-can-be-not-very-informative-as-can-display-the-same-instances-names)** — Instances popup can be not very informative as can display the same instances names

### Bug

**[TW-75999](https://youtrack.jetbrains.com/issue/TW-75999/Qodana-plugin-build-failure-conditions-should-be-sorted-alphabetically)** — Qodana plugin: build failure conditions should be sorted alphabetically

**[TW-76880](https://youtrack.jetbrains.com/issue/TW-76880/Stopping-a-composite-build-may-stop-dependencies-which-are-parts-of-another-running-composite-build)** — Stopping a composite build may stop dependencies which are parts of another running composite build

**[TW-72638](https://youtrack.jetbrains.com/issue/TW-72638/Calculate-minimal-range-for-statistic-charts-basing-on-all-branch-builds-instead-of-just-default-branch)** — Calculate minimal range for statistic charts basing on all branch builds instead of just default branch

**[TW-77016](https://youtrack.jetbrains.com/issue/TW-77016/An-expanded-block-with-information-about-the-build-is-collapsed-immediately-after-opening)** — An expanded block with information about the build is collapsed immediately after opening

**[TW-76258](https://youtrack.jetbrains.com/issue/TW-76258/Commit-Status-Publisher-may-be-sending-too-many-requests-to-GitHub-and-possibly-other-VCS-hostings)** — Commit Status Publisher may be sending too many requests to GitHub (and possibly other VCS hostings)

**[TW-76660](https://youtrack.jetbrains.com/issue/TW-76660/NET-builds-with-whitespace-inside-of-quotes-in-the-config-name-fail-with-error-Only-one-project-can-be-specified)** — .NET builds with whitespace inside of quotes in the config name fail with error "Only one project can be specified"

**[TW-62829](https://youtrack.jetbrains.com/issue/TW-62829/Confusing-counter-of-personal-builds-in-sidebar)** — Confusing counter of personal builds in sidebar

**[TW-76770](https://youtrack.jetbrains.com/issue/TW-76770/teamcitycommitStatusPublishergithubContext-incorrectly-resolves-buildnumber-parameter)** — teamcity.commitStatusPublisher.githubContext incorrectly resolves build.number parameter

**[TW-76997](https://youtrack.jetbrains.com/issue/TW-76997/Create-New-project-from-Projects-page-ROOT-project-is-not-selected-in-Project-Selector)** — Create New project from Projects page - ROOT project is not selected in Project Selector

**[TW-76604](https://youtrack.jetbrains.com/issue/TW-76604/Builds-are-waiting-with-no-more-cloud-agents-availableno-fully-running-cloud-agents-reason-while-profile-allows-more-cloud)** — Builds are waiting with "no more cloud agents available"/"no fully-running cloud agents" reason, while profile allows more cloud agents

**[TW-76338](https://youtrack.jetbrains.com/issue/TW-76338/Duplication-of-the-commit-status-on-GitHub)** — Duplication of the commit status on GitHub

**[TW-76682](https://youtrack.jetbrains.com/issue/TW-76682/Dependencies-Chain-view-sort-builds-in-groups-when-grouping-by-projects-is-enabled)** — Dependencies - Chain view: sort builds in groups when grouping by projects is enabled

**[TW-76570](https://youtrack.jetbrains.com/issue/TW-76570/Commit-Status-Publisher-may-send-excessive-status-updates-for-builds-in-build-chains)** — Commit Status Publisher may send excessive status updates for builds in build chains

**[TW-69564](https://youtrack.jetbrains.com/issue/TW-69564/Parts-of-tokens-are-considered-as-the-issue-tracking-link-on-the-Tokens-page-in-versioned-settings)** — Parts of tokens are considered as the issue tracking link on the Tokens page in versioned settings

**[TW-76753](https://youtrack.jetbrains.com/issue/TW-76753/Bad-alignment-on-the-Changes-page-in-the-Classic-UI-Author-field)** — Bad alignment on the Changes page in the Classic UI (Author field)

**[TW-70750](https://youtrack.jetbrains.com/issue/TW-70750/Investigation-history-popup-hides-unexpectedly)** — Investigation history popup hides unexpectedly

**[TW-76909](https://youtrack.jetbrains.com/issue/TW-76909/Error-Cannot-read-properties-of-undefined-reading-id-in-attemp-to-open-cloud-agent-page-without-permission-View-project-and-all)** — Error "Cannot read properties of undefined (reading 'id')" in attemp to open cloud agent page without permission "View project and all parent projects"

**[TW-76847](https://youtrack.jetbrains.com/issue/TW-76847/Something-went-wrong-TypeError-rproject-is-undefined-in-attempt-to-open-cloud-agent-page-using-user-with-Agent-manager-role)** — "Something went wrong: TypeError: r.project is undefined" in attempt to open cloud agent page using user with Agent manager role

**[TW-76737](https://youtrack.jetbrains.com/issue/TW-76737/Documentation-lack-of-an-example-IAM-policy-with-permissions-required-to-use-S3CloudFront)** — Documentation: lack of an example IAM policy with permissions required to use S3+CloudFront

**[TW-76819](https://youtrack.jetbrains.com/issue/TW-76819/The-Show-Archived-toggle-has-disappeared-from-the-search-pop-up-the-one-you-bring-up-by-pressing-P)** — The "Show Archived"-toggle has disappeared from the search pop-up (the one you bring up by pressing "P")

**[TW-71942](https://youtrack.jetbrains.com/issue/TW-71942/Search-AND-and-do-not-work-between-keywords)** — Search: AND and "+" do not work between keywords

**[TW-76527](https://youtrack.jetbrains.com/issue/TW-76527/Running-a-NET-builder-task-on-DotNet-6-SDK-solution-generates-a-build-error-MSBUILD-error-MSB1006-Property-is-not-valid)** — Running a .NET builder task on DotNet 6 SDK solution generates a build error "MSBUILD : error MSB1006: Property is not valid."

**[TW-51519](https://youtrack.jetbrains.com/issue/TW-51519/Issue-fetcher-plugins-occasionally-fail-to-initialize-with-netsfehcacheCacheException-issuesCache-Could-not-create-disk-store)** — Issue fetcher plugins occasionally fail to initialize with net.sf.ehcache.CacheException: issuesCache: Could not create disk store (system\caches\ehcache\issues.index could not deleted)

**[TW-60198](https://youtrack.jetbrains.com/issue/TW-60198/View-users-popup-should-sort-by-name)** — "View users" popup should sort by name

**[TW-76738](https://youtrack.jetbrains.com/issue/TW-76738/Perforce-Shelve-trigger-skips-some-new-changelists-if-there-are-hundreds-of-them)** — Perforce Shelve trigger skips some new changelists, if there are hundreds of them

**[TW-76243](https://youtrack.jetbrains.com/issue/TW-76243/Cloud-agent-instance-page-add-clickable-cloud-image-and-agent-pool-labels)** — Cloud agent instance page: add clickable cloud image and agent pool labels

**[TW-76840](https://youtrack.jetbrains.com/issue/TW-76840/EC2-plugin-accelerate-check-connectionfetch-properties-by-fetching-properties-in-parallel)** — EC2 plugin: accelerate "check connection/fetch properties" by fetching properties in parallel

**[TW-72501](https://youtrack.jetbrains.com/issue/TW-72501/Restore-builds-re-indexing-logging-in-teamcity-serverlog)** — Restore builds re-indexing logging in teamcity-server.log

**[TW-76626](https://youtrack.jetbrains.com/issue/TW-76626/Commit-status-is-not-published-when-build-is-started)** — Commit status is not published when build is started

**[TW-76478](https://youtrack.jetbrains.com/issue/TW-76478/REST-API-request-with-shelved-changelist-from-excluded-stream-starts-a-build-in-the-main-stream)** — REST API request with shelved changelist from excluded stream starts a build in the main stream

**[TW-76407](https://youtrack.jetbrains.com/issue/TW-76407/Experimental-UI-shows-one-pending-change-multiple-times-after-filtering-by-user)** — Experimental UI shows one pending change multiple times after filtering by user

**[TW-76205](https://youtrack.jetbrains.com/issue/TW-76205/Missing-failed-tests-section-on-overview-of-a-personal-build)** — Missing failed tests section on overview of a personal build

**[TW-76493](https://youtrack.jetbrains.com/issue/TW-76493/Duplicated-NET-test-invocations-after-adding-Parallel-tests-build-feature)** — Duplicated .NET test invocations after adding Parallel tests build feature

**[TW-76652](https://youtrack.jetbrains.com/issue/TW-76652/teamcitycommitStatusPublishergithubContext-no-longer-works-in-TeamCity-202241)** — teamcity.commitStatusPublisher.githubContext no longer works in TeamCity 2022.4.1

**[TW-76007](https://youtrack.jetbrains.com/issue/TW-76007/Perforce-Shelve-Trigger-Java-Heap-Space-while-processing-a-big-changelist)** — Perforce Shelve Trigger: Java Heap Space while processing a big changelist

**[TW-76636](https://youtrack.jetbrains.com/issue/TW-76636/DSL-pre-compilation-not-working-for-some-plugins)** — DSL pre-compilation not working for some plugins

**[TW-76683](https://youtrack.jetbrains.com/issue/TW-76683/Perforce-mismatched-streams-after-updating-sources-is-interrupted)** — Perforce mismatched streams after updating sources is interrupted

**[TW-76409](https://youtrack.jetbrains.com/issue/TW-76409/Filter-changes-by-Users-Me-does-not-work-for-Pending-and-built-changes-in-Experimental-UI)** — Filter changes by Users = "Me" does not work for Pending and built changes in Experimental UI

**[TW-76689](https://youtrack.jetbrains.com/issue/TW-76689/Service-message-requires-comparisonFailure)** — Service message requires comparisonFailure

**[TW-76637](https://youtrack.jetbrains.com/issue/TW-76637/Warning-No-agents-satisfy-this-requirement-for-a-user-without-View-cloud-images-and-instances-permission)** — Warning "No agents satisfy this requirement" for a user without "View cloud images and instances" permission

**[TW-76034](https://youtrack.jetbrains.com/issue/TW-76034/NET-parallel-tests-with-a-Test-Case-filter-test-could-be-split-unevenly)** — .NET parallel tests: with a Test Case filter test could be split unevenly

**[TW-76643](https://youtrack.jetbrains.com/issue/TW-76643/A-page-in-the-browser-is-often-hanging-for-a-lot-of-time-on-attempt-to-show-queued-builds)** — A page in the browser is often hanging for a lot of time on attempt to show queued builds

**[TW-61298](https://youtrack.jetbrains.com/issue/TW-61298/Missing-queued-build-on-Branches-tab-for-build-configuration)** — Missing queued build on Branches tab for build configuration

**[TW-76687](https://youtrack.jetbrains.com/issue/TW-76687/Empty-build-log-for-some-builds-due-to-incorrect-offsets-recorded-into-the-build-log-index-file)** — Empty build log for some builds due to incorrect offsets recorded into the build log index file

**[TW-76504](https://youtrack.jetbrains.com/issue/TW-76504/The-field-must-not-be-empty-error-when-Default-version-for-Lauch-Template-is-selected)** — "The field must not be empty" error when Default version for Lauch Template is selected.

**[TW-76519](https://youtrack.jetbrains.com/issue/TW-76519/Custom-Image-Name-field-should-not-be-marked-as-mandatory)** — Custom Image Name field should not be marked as mandatory.

**[TW-75107](https://youtrack.jetbrains.com/issue/TW-75107/dotnet-nuget-push-build-step-is-failing-due-to-regression-change-in-latest-version-of-nuget-executable)** — dotnet nuget push build step is failing due to regression change in latest version of nuget executable

**[TW-61504](https://youtrack.jetbrains.com/issue/TW-61504/EC2-support-Reset-template-version-when-another-launch-template-is-selected-in-Edit-Image-dialog)** — EC2 support. Reset template version when another launch template is selected in Edit Image dialog,

**[TW-76408](https://youtrack.jetbrains.com/issue/TW-76408/Excessive-cloud-integration-logging)** — Excessive cloud integration logging

**[TW-76533](https://youtrack.jetbrains.com/issue/TW-76533/Absent-validation-for-the-custom-image-name-field)** — Absent validation for the "custom image name" field.

**[TW-76490](https://youtrack.jetbrains.com/issue/TW-76490/Bulk-triggering-from-Perforce-Shelve-trigger-after-enabling-personal-builds)** — Bulk triggering from Perforce Shelve trigger after enabling personal builds

**[TW-70560](https://youtrack.jetbrains.com/issue/TW-70560/User-may-see-agents-pools-to-which-they-do-not-have-access)** — User may see agents pools to which they do not have access

**[TW-76465](https://youtrack.jetbrains.com/issue/TW-76465/Disconnected-cloud-agents-are-displayed-as-idle-on-Agents-Overview-page-during-the-update)** — Disconnected cloud agents are displayed as idle on Agents Overview page during the update

**[TW-75182](https://youtrack.jetbrains.com/issue/TW-75182/Clear-selection-after-a-batch-operation-completes)** — Clear selection after a batch operation completes

**[TW-76448](https://youtrack.jetbrains.com/issue/TW-76448/Build-log-scroll-to-top-and-scroll-to-bottom-buttons-do-not-work)** — Build log 'scroll to top' and 'scroll to bottom' buttons do not work

**[TW-76532](https://youtrack.jetbrains.com/issue/TW-76532/Build-cannot-be-stopped-multi-node-setup)** — Build cannot be stopped (multi-node setup)

### Performance Problem

**[TW-76512](https://youtrack.jetbrains.com/issue/TW-76512/More-than-expected-CPU-usage-possibly-because-of-BuildPTRIndexer-threads)** — More than expected CPU usage possibly because of BuildPTRIndexer threads

**[TW-76517](https://youtrack.jetbrains.com/issue/TW-76517/CommitStatusPublisherListenerlambdainitModificationsProcessing-does-not-finish-within-14-hours)** — CommitStatusPublisherListener.lambda$initModificationsProcessing does not finish within 14 hours

**[TW-76366](https://youtrack.jetbrains.com/issue/TW-76366/Slow-artifact-upload-during-the-build-using-the-publishArtifacts-servicemessage)** — Slow artifact upload during the build using the publishArtifacts servicemessage

**[TW-76596](https://youtrack.jetbrains.com/issue/TW-76596/perforceShelve-Trigger-checking-too-aggressively)** — perforceShelve Trigger checking too aggressively

**[TW-76571](https://youtrack.jetbrains.com/issue/TW-76571/Poor-performance-after-changing-of-an-id-of-a-cloud-profile-if-the-profile-had-some-agents)** — Poor performance after changing of an id of a cloud profile if the profile had some agents

### Task

**[TW-74426](https://youtrack.jetbrains.com/issue/TW-74426/Maintenance-mode-for-AWS-EC2-agents)** — Maintenance mode for AWS EC2 agents

**[TW-76747](https://youtrack.jetbrains.com/issue/TW-76747/Add-REST-endpoints-to-Add-build-step-controller)** — Add REST endpoints to 'Add build step' controller

**[TW-76281](https://youtrack.jetbrains.com/issue/TW-76281/Use-incremental-compilation-for-plugins-DSL)** — Use incremental compilation for plugins DSL

**[TW-76790](https://youtrack.jetbrains.com/issue/TW-76790/Use-Remote-Artifacts-for-pushed-images-in-Docker-plugin)** — Use Remote Artifacts for pushed images in Docker plugin

**[TW-73669](https://youtrack.jetbrains.com/issue/TW-73669/Change-REST-API-response-text-for-POST-requests-with-basic-auth-CSRF-errors)** — Change REST API response text for POST requests with basic auth (CSRF errors)

**[TW-76844](https://youtrack.jetbrains.com/issue/TW-76844/Make-the-envTEAMCITYBUILDPROPERTIESFILE-filename-the-same-for-all-the-builds-on-the-agent)** — Make the env.TEAMCITY_BUILD_PROPERTIES_FILE filename the same for all the builds on the agent

**[TW-76402](https://youtrack.jetbrains.com/issue/TW-76402/Introduce-VPC-selector-for-the-Cloud-Image-form-of-EC2-agents)** — Introduce VPC selector for the Cloud Image form of EC2 agents

**[TW-75523](https://youtrack.jetbrains.com/issue/TW-75523/AWS-Connection-Ensure-that-session-credentials-will-be-valid-for-desired-time)** — AWS Connection: Ensure that session credentials will be valid for desired time

**[TW-76521](https://youtrack.jetbrains.com/issue/TW-76521/Display-customized-parameters-for-launch-template-in-the-Images-table-on-EC2-cloud-profile-page)** — Display customized parameters for launch template in the Images table on EC2 cloud profile page.

**[TW-75554](https://youtrack.jetbrains.com/issue/TW-75554/Remove-Availability-zone-from-the-form)** — Remove "Availability zone" from the form

**[TW-75553](https://youtrack.jetbrains.com/issue/TW-75553/Allow-specifying-of-EBS-Optimized-only-for-specific-instance-types)** — Allow specifying of "EBS Optimized" only for specific instance types

**[TW-76580](https://youtrack.jetbrains.com/issue/TW-76580/ImageBuilder-Provide-information-about-how-to-cache-VCS-roots)** — ImageBuilder: Provide information about how to cache VCS roots

**[TW-71560](https://youtrack.jetbrains.com/issue/TW-71560/Add-the-ability-to-get-the-first-build-which-had-all-the-changes-according-to-a-certain-locator-in-a-specific-build)** — Add the ability to get the first build, which had all the changes according to a certain locator in a specific build configuration

**[TW-76488](https://youtrack.jetbrains.com/issue/TW-76488/Move-Jira-plugin-to-Jira-Cloud-plugin)** — Move Jira plugin to Jira Cloud plugin

**[TW-75942](https://youtrack.jetbrains.com/issue/TW-75942/Open-some-DSL-classes-to-make-them-inheritable)** — Open some DSL classes to make them inheritable


