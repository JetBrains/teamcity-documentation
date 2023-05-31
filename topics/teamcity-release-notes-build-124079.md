[//]: # (title: TeamCity Release Notes: Build 124079)
[//]: # (auxiliary-id: TeamCity Release Notes: Build 124079;TeamCity 2021.12 Release Notes)

__Build: 124079__

__14 December 2022__

### Feature

**[TW-77597](https://youtrack.jetbrains.com/issue/TW-77597/Use-the-Sakura-Chains-component-on-the-Build-Configuration-Page-Build-Chains-Tab)** — Use the Sakura Chains component on the Build Configuration Page -> Build Chains Tab

**[TW-69641](https://youtrack.jetbrains.com/issue/TW-69641/Add-an-ability-to-expand-collapse-subtree-on-new-build-log)** — Add an ability to expand / collapse subtree on new build log

**[TW-78722](https://youtrack.jetbrains.com/issue/TW-78722/Personal-builds-shelvedChangelist-parameter-is-inaccessible-in-templated-build-steps)** — Personal build's shelvedChangelist parameter is inaccessible in templated build steps

**[TW-69754](https://youtrack.jetbrains.com/issue/TW-69754/Build-Queue-Reordering-builds-in-queue-in-Sakura-UI)** — Build Queue: Reordering builds in queue in Sakura UI

**[TW-78069](https://youtrack.jetbrains.com/issue/TW-78069/Indicate-draft-pull-requests-on-the-build-page)** — Indicate draft pull requests on the build page

**[TW-64444](https://youtrack.jetbrains.com/issue/TW-64444/Pull-Requests-Plugin-should-support-ignoring-draft-pull-requests-for-GitHub)** — Pull Requests Plugin should support ignoring draft pull requests for GitHub

**[TW-75551](https://youtrack.jetbrains.com/issue/TW-75551/Launch-Template-as-a-source-of-params-value-instead-of-separate-launch-type)** — Launch Template as a source of params value instead of separate launch type

**[TW-77436](https://youtrack.jetbrains.com/issue/TW-77436/Make-prometheus-metrics-adhere-to-OpenMetrics-specification)** — Make prometheus metrics adhere to OpenMetrics specification

### Bug

**[TW-78291](https://youtrack.jetbrains.com/issue/TW-78291/number-suffix-contradicts-to-metric-name-Prometheus-convention)** — number suffix contradicts to metric name Prometheus convention

**[TW-62983](https://youtrack.jetbrains.com/issue/TW-62983/Reason-of-build-failure-is-collapsed-in-build-log-in-the-experimental-UI)** — Reason of build failure is collapsed in build log in the experimental UI

**[TW-78806](https://youtrack.jetbrains.com/issue/TW-78806/TeamCity-reports-inactive-main-node-despite-there-is-an-active-one)** — TeamCity reports inactive main node despite there is an active one

**[TW-34639](https://youtrack.jetbrains.com/issue/TW-34639/Symbolic-links-are-not-preserved-when-.tgz-or-.zip-artifact-is-expanded-by-agent)** — Symbolic links are not preserved when .tgz or .zip artifact is expanded by agent

**[TW-77887](https://youtrack.jetbrains.com/issue/TW-77887/Raise-error-in-builds-with-ab-a-c-checkout-rules-if-agent-side-checkout-mode-is-set)** — Raise error in builds with a=>[b/]a/c checkout rules if agent-side checkout mode is set

**[TW-71538](https://youtrack.jetbrains.com/issue/TW-71538/Builds-with-allure-steps-dont-start-after-the-main-node-responsibility-is-assigned-to-secondary-node)** — Builds with allure steps don't start after the main node responsibility is assigned to secondary node

**[TW-78044](https://youtrack.jetbrains.com/issue/TW-78044/No-comment-about-Marking-build-as-successful-is-shown-in-Chains-views)** — No comment about Marking build as successful is shown in Chains views

**[TW-78802](https://youtrack.jetbrains.com/issue/TW-78802/Wait-reasons-for-a-build-sitting-in-the-queue-are-not-available-on-the-secondary-node)** — Wait reasons for a build sitting in the queue are not available on the secondary node

**[TW-78618](https://youtrack.jetbrains.com/issue/TW-78618/Disk-space-watcher-does-not-watch-for-a-disk-space-in-the-custom-caches-directory)** — Disk space watcher does not watch for a disk space in the custom caches directory

**[TW-78864](https://youtrack.jetbrains.com/issue/TW-78864/The-Ignore-Drafts-option-behaves-differently-for-GitHub.com-and-GitHub-Enterprise-repos)** — The "Ignore Drafts" option behaves differently for GitHub.com and GitHub Enterprise repos

**[TW-78890](https://youtrack.jetbrains.com/issue/TW-78890/TeamCity-S3-Storage-requires-excessive-GetAccelerateConfiguration-permission)** — TeamCity S3 Storage requires excessive GetAccelerateConfiguration permission

**[TW-78413](https://youtrack.jetbrains.com/issue/TW-78413/PerfMon-tab-should-not-be-shown-for-a-composite-build)** — PerfMon tab should not be shown for a composite build

**[TW-77300](https://youtrack.jetbrains.com/issue/TW-77300/Test-History-Information-about-new-builds-is-not-displayed-without-reloading-of-the-page)** — Test History: Information about new builds is not displayed without reloading of the page

**[TW-78207](https://youtrack.jetbrains.com/issue/TW-78207/param-for-projectName-inside-versioned-settings-vcs-root)** — param for projectName inside versioned settings' vcs root

**[TW-78905](https://youtrack.jetbrains.com/issue/TW-78905/Confusing-messages-in-the-build-log-of-the-parallel-tests-umbrella-build)** — Confusing messages in the build log of the parallel tests umbrella build

**[TW-77777](https://youtrack.jetbrains.com/issue/TW-77777/Kotlin-DSL-transitive-maven-dependencies-arent-resolved-during-server-execution)** — Kotlin DSL transitive maven dependencies aren't resolved during server execution

**[TW-78610](https://youtrack.jetbrains.com/issue/TW-78610/ECS-agent-plugin-spawns-more-instances-than-configuration-permitts)** — ECS agent plugin spawns more instances than configuration permitts

**[TW-75156](https://youtrack.jetbrains.com/issue/TW-75156/Add-ability-to-sanely-sort-test-runs-by-test-name)** — Add ability to sanely sort test runs by test name

**[TW-78294](https://youtrack.jetbrains.com/issue/TW-78294/Signed-APKs-fail-to-expand-in-the-UI)** — Signed APKs fail to expand in the UI

**[TW-78458](https://youtrack.jetbrains.com/issue/TW-78458/Usage-of-branchpolicyXXXX-locator-dimensions-causes-HTTP-400)** — Usage of branch:(policy:XXXX) locator dimensions causes HTTP 400

**[TW-78727](https://youtrack.jetbrains.com/issue/TW-78727/Wrong-behavior-of-BuildServerListener.projectCreated-event)** — Wrong behavior of BuildServerListener.projectCreated event

**[TW-77814](https://youtrack.jetbrains.com/issue/TW-77814/Trailing-space-is-ReSharper-Inspections-runner-fails-the-step)** — Trailing space is ReSharper Inspections runner fails the step

**[TW-74139](https://youtrack.jetbrains.com/issue/TW-74139/.NET-custom-step-exiting-with-1-doesnt-fail-the-build)** — .NET custom step exiting with 1 doesn't fail the build

**[TW-78045](https://youtrack.jetbrains.com/issue/TW-78045/Build-status-is-not-displayed-in-the-chain-header-on-the-Chains-tab)** — Build status is not displayed in the chain header on the Chains tab

**[TW-63042](https://youtrack.jetbrains.com/issue/TW-63042/Provide-Switch-to-Sakura-UI-icon-on-the-Disconnected-Agents-tab.)** — Provide Switch to Sakura UI icon on the Disconnected Agents tab.

**[TW-78550](https://youtrack.jetbrains.com/issue/TW-78550/RejectedExecutionException-with-the-entire-stacktrace-is-logged-in)** — RejectedExecutionException with the entire stacktrace is logged in BuildProblemInvestigationsAndMutesManagerImpl.submitRemoveBuildConfigurationScopedBuildProblemMutes

**[TW-78403](https://youtrack.jetbrains.com/issue/TW-78403/Maven-with-TestNG-and-XML-report-processing-feature-report-BeforeTest-and-AfterTest-methods-as-separate-tests-only-when-using)** — Maven with TestNG and 'XML report processing' feature report @BeforeTest and @AfterTest methods as separate tests (only when using TestNG report type)

**[TW-78463](https://youtrack.jetbrains.com/issue/TW-78463/timestamp-option-of-maintainDB-shown-as-invalid)** — --timestamp option of maintainDB shown as invalid

**[TW-78047](https://youtrack.jetbrains.com/issue/TW-78047/No-way-to-see-more-when-20-build-chains-on-Build-Configuration-Chains-tab)** — No way to see more when 20 build chains on Build Configuration Chains tab

**[TW-78046](https://youtrack.jetbrains.com/issue/TW-78046/Expanding-collapsing-of-build-chains-doesnt-work-when-one-of-the-build-was-highlighted)** — Expanding/collapsing of build chains doesn't work when one of the build was highlighted

**[TW-78001](https://youtrack.jetbrains.com/issue/TW-78001/Incorrect-number-of-builds-can-be-shown-in-the-charts-on-Chains-tab-if-some-builds-in-the-chain-were-cancelled)** — Incorrect number of builds can be shown in the charts on Chains tab, if some builds in the chain were cancelled

**[TW-74933](https://youtrack.jetbrains.com/issue/TW-74933/Unobvious-InspectCode-Platform-option-names-in-ReSharper-Inspections-runner-settings)** — Unobvious InspectCode Platform option names in ReSharper Inspections runner settings

**[TW-71547](https://youtrack.jetbrains.com/issue/TW-71547/Allow-loading-webhooks-plugin-on-secondary-nodes)** — Allow loading webhooks plugin on secondary nodes

**[TW-71546](https://youtrack.jetbrains.com/issue/TW-71546/Allow-loading-jira-cloud-integration-plugin-on-secondary-nodes)** — Allow loading jira-cloud integration plugin on secondary nodes

**[TW-78004](https://youtrack.jetbrains.com/issue/TW-78004/No-checkboxes-Show-details-and-Group-by-project-on-Chains-tab)** — No checkboxes "Show details" and "Group by project" on Chains tab

**[TW-77991](https://youtrack.jetbrains.com/issue/TW-77991/No-information-about-reusing-builds-shown-on-the-Chains-tab)** — No information about reusing builds shown on the Chains tab

**[TW-67037](https://youtrack.jetbrains.com/issue/TW-67037/Slack-Notifier-checkmark-in-successful-build-notification-is-gray-in-dark-theme-Slack)** — Slack Notifier - checkmark in successful build notification is gray in dark theme Slack

**[TW-76549](https://youtrack.jetbrains.com/issue/TW-76549/Metric-httprequestsdurationmillisecondsbucket-does-not-have-leInf-bucket)** — Metric http_requests_duration_milliseconds_bucket does not have le=+Inf bucket

**[TW-71777](https://youtrack.jetbrains.com/issue/TW-71777/Add-a-description-for-Uploaded-key-and-Custom-private-key-for-Git-VCS-root)** — Add a description for "Uploaded key" and "Custom private key" for Git VCS root

### Performance Problem

**[TW-78399](https://youtrack.jetbrains.com/issue/TW-78399/Change-the-way-of-how-experimental-metrics-introduced)** — Change the way of how `experimental` metrics introduced

**[TW-78314](https://youtrack.jetbrains.com/issue/TW-78314/Extracting-tar-artifact-with-large-number-of-small-files-is-extremely-slow)** — Extracting tar artifact with large number of small files is extremely slow

### Task

**[TW-77562](https://youtrack.jetbrains.com/issue/TW-77562/Enable-agent-related-actions-on-the-secondary-nodes-view-logs-take-thread-dumps-etc)** — Enable agent related actions on the secondary nodes: view logs, take thread dumps, etc

**[TW-78341](https://youtrack.jetbrains.com/issue/TW-78341/Increase-Perforce-communication-timeouts-for-perforce-commands-executed-on-the-agent)** — Increase Perforce communication timeouts for perforce commands executed on the agent

**[TW-77657](https://youtrack.jetbrains.com/issue/TW-77657/Build-Dependencies-Timeline-should-have-search)** — Build Dependencies Timeline should have search

**[TW-78068](https://youtrack.jetbrains.com/issue/TW-78068/Add-filterDrafts-field-to-Pull-Requests-build-feature-DSL-extension)** — Add filterDrafts field to Pull Requests build feature DSL extension

**[TW-77554](https://youtrack.jetbrains.com/issue/TW-77554/Improve-DSL-Dokka-documentation-navigation-tree)** — Improve DSL Dokka documentation navigation tree

**[TW-78803](https://youtrack.jetbrains.com/issue/TW-78803/Improve-DSL-documentation-search-results)** — Improve DSL documentation search results

**[TW-78233](https://youtrack.jetbrains.com/issue/TW-78233/Expose-reason-for-instance-terminating)** — Expose reason for instance terminating

**[TW-78200](https://youtrack.jetbrains.com/issue/TW-78200/Better-wording-for-filterDrafts-setting)** — Better wording for "filterDrafts" setting

**[TW-64392](https://youtrack.jetbrains.com/issue/TW-64392/Support-some-notion-of-pull-requests-in-TeamCity-core)** — Support some notion of pull requests in TeamCity core

### Security Problem

6 security problems have been fixed.