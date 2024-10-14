[//]: # (title: TeamCity 2024.07.3 Release Notes)
[//]: # (auxiliary-id: TeamCity 2024.07.3 Release Notes)


**Build 160765, 1 October 2024**


<!--project: TeamCity Fix versions: 2024.07.3  #Fixed #Testing visible to: {All Users} -{Trunk issue}-->


### Bug

**[TW-89529](https://youtrack.jetbrains.com/issue/TW-89529/SSH-agent-fails-on-Win-with-2024.07.2-TeamCity-update)** — SSH agent fails on Win with 2024.07.2 TeamCity update

**[TW-89198](https://youtrack.jetbrains.com/issue/TW-89198/Failed-to-obtain-details-if-agent-is-local-for-cloud-agents)** — Failed to obtain details if agent is local for cloud agents

**[TW-89659](https://youtrack.jetbrains.com/issue/TW-89659/Wrong-number-of-pending-changes-in-build-configuration-tab-if-all-branches-is-selected)** — Wrong number of pending changes in build configuration tab if &lt;all branches> is selected

**[TW-76996](https://youtrack.jetbrains.com/issue/TW-76996/Builds-metadata-storage-is-corrupted-and-will-be-recreated-when-starting-secondary-node)** — "Builds metadata storage is corrupted and will be recreated" when starting secondary node

**[TW-89552](https://youtrack.jetbrains.com/issue/TW-89552/Cannot-connect-to-SVN-with-locally-stored-user-credentials-in-2024.07)** — Cannot connect to SVN with locally stored user credentials in 2024.07+

**[TW-89835](https://youtrack.jetbrains.com/issue/TW-89835/Build-may-be-cancelled-when-the-server-is-temporary-unavailable-repeating-failure-after-successful-registration)** — Build may be cancelled when the server is temporary unavailable (repeating failure after successful registration)

**[TW-89836](https://youtrack.jetbrains.com/issue/TW-89836/Detect-untrusted-builds-only-for-gitlab.com)** — Detect untrusted builds only for gitlab.com

**[TW-89571](https://youtrack.jetbrains.com/issue/TW-89571/429-response-code-for-repository-visibility-cache)** — 429 response code for repository visibility cache

**[TW-89803](https://youtrack.jetbrains.com/issue/TW-89803/TeamCity-tries-to-collect-changes-from-a-deleted-Perforce-Stream)** — TeamCity tries to collect changes from a deleted Perforce Stream

**[TW-80467](https://youtrack.jetbrains.com/issue/TW-80467/Cannot-find-a-node100479888-may-occur-when-collecting-VCS-changes-on-the-secondary-node)** — Cannot find a node:100479888 may occur when collecting VCS changes on the secondary node

**[TW-89052](https://youtrack.jetbrains.com/issue/TW-89052/Unable-to-specify-context-parameters-for-versioned-settings-Editing-of-the-project-settings-is-disabled)** — Unable to specify context parameters for versioned settings: Editing of the project settings is disabled

**[TW-89824](https://youtrack.jetbrains.com/issue/TW-89824/Project-creation-from-the-GitHub-App-fails-with-404)** — Project creation from the GitHub App fails with 404

**[TW-89538](https://youtrack.jetbrains.com/issue/TW-89538/Unexpected-response-from-the-server-can-stop-the-build-on-the-agent-if-the-server-cannot-accept-data-from-agent-for-long-period)** — Unexpected response from the server can stop the build on the agent if the server cannot accept data from agent for long period of time

**[TW-89553](https://youtrack.jetbrains.com/issue/TW-89553/Cannot-create-new-SVN-project-from-URL-error-Path-is-invalid-if-path-contains-upper-case-letters)** — Cannot create new SVN project from URL: error "Path is invalid", if path contains upper case letters

**[TW-89758](https://youtrack.jetbrains.com/issue/TW-89758/StackOverflowError-on-attempt-to-compute-build-artifacts-state-loop-via-composite-build-artifacts)** — StackOverflowError on attempt to compute build artifacts state (loop via composite build artifacts)

**[TW-89386](https://youtrack.jetbrains.com/issue/TW-89386/Error-class-unknown-class-in-the-DSL-documentation)** — &lt;Error class: unknown class> in the DSL documentation

**[TW-85762](https://youtrack.jetbrains.com/issue/TW-85762/Add-information-about-Matrix-build-feature-to-parent-Matrix-build-log)** — Add information about Matrix build feature to parent Matrix build log

**[TW-89470](https://youtrack.jetbrains.com/issue/TW-89470/Number-of-Agents-instead-of-Max-Number-of-Agents-is-shown-for-the-True-Up-license-on-the-new-page)** — Number of Agents instead of Max Number of Agents is shown for the True-Up license on the new page

**[TW-89417](https://youtrack.jetbrains.com/issue/TW-89417/GitHub-App-commit-hook-does-not-schedule-changes-collecting-in-versioned-settings-VCS-root)** — GitHub App commit hook does not schedule changes collecting in versioned settings VCS root

**[TW-86541](https://youtrack.jetbrains.com/issue/TW-86541/Build-dependencies-view-are-not-available-when-there-is-one-dependency-with-lack-of-access)** — Build dependencies view are not available when there is one dependency with lack of access

**[TW-89359](https://youtrack.jetbrains.com/issue/TW-89359/Automatic-plugin-update-proposes-versions-from-non-stable-channel)** — Automatic plugin update proposes versions from non-stable channel

**[TW-88023](https://youtrack.jetbrains.com/issue/TW-88023/Invalid-directory-for-version-settings-doesnt-process-correctly-no-validation-or-clear-error-message)** — Invalid directory for version settings doesn't process correctly (no validation or clear error message)

**[TW-88697](https://youtrack.jetbrains.com/issue/TW-88697/JB-License-Warning-on-the-Updates-page-shows-expired-server-license-as-incompatible)** — JB License: Warning on the Updates page shows expired server license as incompatible


### Performance Problem

**[TW-89992](https://youtrack.jetbrains.com/issue/TW-89992/Slow-checking-for-changes-operation-in-case-of-XML-versioned-settings-format)** — Slow checking for changes operation in case of XML versioned settings format

**[TW-89581](https://youtrack.jetbrains.com/issue/TW-89581/High-contention-on-a-write-lock-when-a-new-test-name-is-saved-in-DB-can-slow-down-processing-of-messages-from-the-builds)** — High contention on a write lock when a new test name is saved in DB can slow down processing of messages from the builds

**[TW-89544](https://youtrack.jetbrains.com/issue/TW-89544/Improve-performance-of-SQL-query-used-to-check-whether-VCS-root-has-a-DAG)** — Improve performance of SQL query used to check whether VCS root has a DAG


### Task

**[TW-89479](https://youtrack.jetbrains.com/issue/TW-89479/2FA-Make-disable-endpoint-also-refresh-grace-period-if-two-factor-auth-is-mandatory-for-the-user.)** — 2FA: Make /disable endpoint also refresh grace period, if two factor auth is mandatory for the user.






<!--project: TeamCity Fix versions: 2024.07.3  #Fixed #Testing #{Security Problem} -{Trunk issue}-->



### Security

5 security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2024.03). Security bulletins for new versions are typically published within the next few days after the release date.