[//]: # (title: TeamCity 2024.07.1 Release Notes)
[//]: # (auxiliary-id: TeamCity 2024.07.1 Release Notes)


**Build 160635, 6 August 2024**


<!--project: TeamCity Fix versions: 2024.07.1  #Fixed #Testing visible to: {All Users} -{Trunk issue}-->

### Bug

**[TW-88719](https://youtrack.jetbrains.com/issue/TW-88719/JB-license-Offline-activation-and-Licenses-page-loading-may-take-several-seconds-if-request-to-JBA-is-waiting-for-response)** — JB license: Offline activation and Licenses page loading may take several seconds, if request to JBA is waiting for response

**[TW-89184](https://youtrack.jetbrains.com/issue/TW-89184/Running-Maven-in-docker-using-Custom-maven-version-fails)** — Running Maven in docker using &lt;Custom> maven version fails

**[TW-89131](https://youtrack.jetbrains.com/issue/TW-89131/Custom-build-dialog-triggers-a-build-with-original-artifact-dependency-even-if-dependency-is-set-to-rebuild)** — Custom build dialog triggers a build with original artifact dependency even if dependency is set to rebuild

**[TW-88942](https://youtrack.jetbrains.com/issue/TW-88942/JB-license-Add-separate-help-link-to-the-documentation-page-that-describes-activation-process)** — JB license: Add separate help link to the documentation page that describes activation process

**[TW-89060](https://youtrack.jetbrains.com/issue/TW-89060/Mavens-releaseprepare-goal-is-failing-in-TC-build-step-with-ProvisionException)** — Maven's release:prepare goal is failing in TC build step with ProvisionException

**[TW-89186](https://youtrack.jetbrains.com/issue/TW-89186/Excessive-memory-usage-in-CheckingForChangesPrecondition)** — Excessive memory usage in CheckingForChangesPrecondition

**[TW-86534](https://youtrack.jetbrains.com/issue/TW-86534/No-information-about-the-revoked-JB-license-is-logged-into-the-audit)** — No information about the revoked JB license is logged into the audit

**[TW-88770](https://youtrack.jetbrains.com/issue/TW-88770/Remove-queued-builds-produced-by-this-trigger-in-GitHub-Checks-Webhook-Trigger-doesnt-work)** — "Remove queued builds produced by this trigger" in GitHub Checks Webhook Trigger doesn't work

**[TW-89093](https://youtrack.jetbrains.com/issue/TW-89093/JB-License-Warnings-Could-not-parse-agent-maintenance-due-date-in-TeamCity-License-unlimited-in-teamcity-server.log-if-a)** — JB License: Warnings "Could not parse agent maintenance due date in TeamCity License: unlimited" in teamcity-server.log, if a Professional license is activated

**[TW-89120](https://youtrack.jetbrains.com/issue/TW-89120/Edit-build-configuration-Internal-runner-ID-instead-of-name-is-shown-in-the-sidebar)** — Edit build configuration: Internal runner ID instead of name is shown in the sidebar

**[TW-88800](https://youtrack.jetbrains.com/issue/TW-88800/JB-license-No-warnings-about-expiring-licenses-if-their-maintenance-date-is-in-the-past-but-there-are-no-updates-from-update.xml)** — JB license: No warnings about expiring licenses, if their maintenance date is in the past, but there are no updates from update.xml

**[TW-88956](https://youtrack.jetbrains.com/issue/TW-88956/VCS-root-is-saved-with-obsolete-params-in-DSL)** — VCS root is saved with obsolete params in DSL

**[TW-88101](https://youtrack.jetbrains.com/issue/TW-88101/Failed-to-log-in-to-TeamCity-from-Visual-Studio-DeserializeResponse)** — Failed to log in to TeamCity from Visual Studio: DeserializeResponse

**[TW-89083](https://youtrack.jetbrains.com/issue/TW-89083/Use-last-known-revision-for-the-versioned-settings-VCS-root-if-its-revision-is-not-specified-in-REST-API-revisions-payload)** — Use last known revision for the versioned settings VCS root if it's revision is not specified in REST API revisions payload

**[TW-88142](https://youtrack.jetbrains.com/issue/TW-88142/Perforce-Stream-ChangeView-with-blanks-in-paths-leads-to-VCS-root-errors)** — Perforce: Stream ChangeView with blanks in paths leads to VCS root errors

**[TW-88768](https://youtrack.jetbrains.com/issue/TW-88768/Rerun-a-build-triggered-by-GitHub-Checks-Trigger-in-TeamCity-doesnt-update-the-status-in-GitHub)** — Rerun a build triggered by "GitHub Checks Trigger" in TeamCity doesn't update the status in GitHub

**[TW-87693](https://youtrack.jetbrains.com/issue/TW-87693/Agent-service-under-Windows-does-not-use-bundled-jre-and-fails-to-start-if-JAVAHOME-is-not-defined)** — Agent service under Windows does not use bundled jre, and fails to start if JAVA_HOME is not defined

**[TW-89036](https://youtrack.jetbrains.com/issue/TW-89036/When-a-part-of-a-composite-build-is-cancelled-due-to-agent-timeout-on-agent-start-a-build-may-be-stuck-in-a-Not-defined-state)** — When a part of a composite build is cancelled due to agent timeout on agent start, a build may be stuck in a "Not defined" state

**[TW-88830](https://youtrack.jetbrains.com/issue/TW-88830/A-lot-of-warnings-Unknown-GitHub-App-permission-in-teamcity-connections.log)** — A lot of warnings "Unknown GitHub App permission" in teamcity-connections.log

**[TW-88923](https://youtrack.jetbrains.com/issue/TW-88923/JB-license-Do-not-show-agents-table-if-Open-Source-license-is-activated)** — JB license: Do not show agents table, if Open Source license is activated

**[TW-88935](https://youtrack.jetbrains.com/issue/TW-88935/Problems-tab-no-pop-up-window-for-subprojects-when-tests-problems-are-selected-via-checkboxes)** — Problems tab: no pop-up window for subprojects when tests/problems are selected via checkboxes

**[TW-88962](https://youtrack.jetbrains.com/issue/TW-88962/Agents-running-from-Windows-2024.07-1809-docker-images-become-incompatible-with-some-runners-after-restart)** — Agents running from Windows 2024.07-1809 docker images become incompatible with some runners after restart

**[TW-87881](https://youtrack.jetbrains.com/issue/TW-87881/Build-status-can-be-failed-but-no-problems-in-the-overview-probably-because-of-recently-muted-tests)** — Build status can be failed but no problems in the overview (probably because of recently muted tests)

**[TW-88843](https://youtrack.jetbrains.com/issue/TW-88843/JB-license-Confirm-button-in-Deactivation-dialog-is-disabled-if-there-is-trailing-or-leading-space-in-Server-URL)** — JB license: Confirm button in Deactivation dialog is disabled, if there is trailing or leading space in Server URL

**[TW-88755](https://youtrack.jetbrains.com/issue/TW-88755/JB-license-Warning-Agent-licenses-do-not-support-the-latest-version-of-TeamCity-server-is-not-shown-if-server-license-has)** — JB license: Warning "Agent licenses do not support the latest version of TeamCity server" is not shown, if server license has expired

**[TW-88698](https://youtrack.jetbrains.com/issue/TW-88698/Add-support-for-org.opentest4j.FIleInfo-for-Gradle-runner)** — Add support for org.opentest4j.FIleInfo for Gradle runner


### Performance Problem

**[TW-89058](https://youtrack.jetbrains.com/issue/TW-89058/Build-chain-modifier-produces-a-project-persisting-task-per-each-new-virtual-build-configuration)** — Build chain modifier produces a project persisting task per each new virtual build configuration

**[TW-57528](https://youtrack.jetbrains.com/issue/TW-57528/Global-health-item-can-slowdown-web-UI)** — Global health item can slowdown web UI


### Task

**[TW-88729](https://youtrack.jetbrains.com/issue/TW-88729/Add-a-responsible-node-id-configuration-parameter)** — Add a responsible node id configuration parameter

**[TW-22179](https://youtrack.jetbrains.com/issue/TW-22179/Show-warning-if-artifacts-dependency-resolving-will-cause-checkout-directory-cleanup)** — Show warning if artifacts dependency resolving will cause checkout directory cleanup





<!--project: TeamCity Fix versions: 2024.07.1  #Fixed #Testing #{Security Problem} -{Trunk issue}-->



### Security

6 security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2024.03). Security bulletins for new versions are typically published within the next few days after the release date.