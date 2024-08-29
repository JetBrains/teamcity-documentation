[//]: # (title: TeamCity 2024.07.2 Release Notes)
[//]: # (auxiliary-id: TeamCity 2024.07.2 Release Notes)


**Build 160695, 29 August 2024**


<!--project: TeamCity Fix versions: 2024.07.2 #Fixed #Testing visible to: {All Users} -{Trunk issue}-->


### Bug

**[TW-89259](https://youtrack.jetbrains.com/issue/TW-89259/Error-404-is-thrown-when-attempting-to-view-the-contents-of-the-content-directory-in-build-artifacts)** — Error 404 is thrown when attempting to view the contents of the 'content' directory in build artifacts

**[TW-85917](https://youtrack.jetbrains.com/issue/TW-85917/Matrix-build-or-parallel-tests-build-is-red-if-there-are-more-than-100-generated-builds.-While-there-is-a-limit-of-350-builds-in)** — Matrix build (or parallel tests build) is red if there are more than 100 generated builds. While there is a limit of 350 builds in the error message

**[TW-89477](https://youtrack.jetbrains.com/issue/TW-89477/Exception-in-changesLoaded-event-on-attempt-to-compute-changes-of-auto-generated-build-if-the-main-build-is-personal)** — Exception in changesLoaded event on attempt to compute changes of auto-generated build if the main build is personal

**[TW-84044](https://youtrack.jetbrains.com/issue/TW-84044/Incorrect-information-about-max-number-of-authorized-agents-in-case-of-per-usage-license-on-the-secondary-node)** — Incorrect information about max number of authorized agents in case of per-usage license on the secondary node

**[TW-89453](https://youtrack.jetbrains.com/issue/TW-89453/Cannot-start-cloud-agent-from-the-Agents-Page)** — Cannot start cloud agent from the Agents Page

**[TW-89210](https://youtrack.jetbrains.com/issue/TW-89210/Failed-connection-to-Amazon-after-choosing-type-of-a-cloud-profile)** — Failed connection to Amazon after choosing type of a cloud profile

**[TW-88676](https://youtrack.jetbrains.com/issue/TW-88676/GitHub-Checks-report-doesnt-contain-info-about-dependencies-for-composite-builds)** — GitHub Checks report doesn't contain info about dependencies for composite builds

**[TW-88654](https://youtrack.jetbrains.com/issue/TW-88654/There-are-no-separate-statuses-in-GitHub-Checks-for-failed-to-start-and-canceled-builds)** — There are no separate statuses in GitHub Checks for "failed to start" and "canceled" builds

**[TW-88774](https://youtrack.jetbrains.com/issue/TW-88774/Provide-a-better-status-for-queued-builds-in-GitHub-Checks)** — Provide a better status for queued builds in GitHub Checks

**[TW-89324](https://youtrack.jetbrains.com/issue/TW-89324/KeepArtifactsCleanerCache-consumes-a-lot-of-disk-space)** — KeepArtifactsCleanerCache consumes a lot of disk space

**[TW-89144](https://youtrack.jetbrains.com/issue/TW-89144/Perforce-ditto-mapping-server-side-chekout-not-able-to-build-correct-patsh)** — Perforce ditto mapping: server-side chekout not able to build correct patсh

**[TW-89117](https://youtrack.jetbrains.com/issue/TW-89117/Too-broad-problem-identity-if-a-problem-is-raised-by-SSH-agent)** — Too broad problem identity if a problem is raised by SSH agent

**[TW-85394](https://youtrack.jetbrains.com/issue/TW-85394/Nodes-do-not-synchronize-the-entire-queue-from-the-database)** — Nodes do not synchronize the entire queue from the database

**[TW-89267](https://youtrack.jetbrains.com/issue/TW-89267/Parameter-teamcity.build.triggeredBy-gets-broken-by-matrix-build)** — Parameter %teamcity.build.triggeredBy% gets "broken" by matrix build

**[TW-88291](https://youtrack.jetbrains.com/issue/TW-88291/Improve-error-messages-in-service-messages-from-teamcity-containing-URLs)** — Improve error messages in service messages from teamcity containing URLs

**[TW-88804](https://youtrack.jetbrains.com/issue/TW-88804/Add-Info-logging-about-events-related-to-GitHub-Checks-Webhook-Trigger)** — Add Info logging about events related to GitHub Checks Webhook Trigger

**[TW-89378](https://youtrack.jetbrains.com/issue/TW-89378/Already-optimized-build-is-being-added-to-the-build-queue-as-a-result-of-a-race-condition)** — Already optimized build is being added to the build queue as a result of a race condition

**[TW-89360](https://youtrack.jetbrains.com/issue/TW-89360/Enabling-the-Apply-changes-in-snapshot-dependencies-and-version-control-settings-option-in-Version-Settings-via-the-REST-API)** — Enabling the "Apply changes in snapshot dependencies and version control settings" option in Version Settings via the REST API doesn't work

**[TW-87740](https://youtrack.jetbrains.com/issue/TW-87740/Agent-status-does-not-change-if-the-agents-termination-is-blocked-by-the-terminal)** — Agent status does not change if the agent's termination is blocked by the terminal

**[TW-89094](https://youtrack.jetbrains.com/issue/TW-89094/NUnit-runner-doesnt-work-with-NUnit-Console-3.18.1)** — NUnit runner doesn't work with NUnit Console 3.18.1

**[TW-82025](https://youtrack.jetbrains.com/issue/TW-82025/VMWare-cloud-agents-may-fail-to-fetch-server-URL-from-GuestInfo-after-startup)** — VMWare cloud agents may fail to fetch server URL from GuestInfo after startup

**[TW-86276](https://youtrack.jetbrains.com/issue/TW-86276/SSH-Agent-build-feature-should-convert-EOL-in-ssh-keys)** — "SSH Agent" build feature should convert EOL in ssh keys

**[TW-89226](https://youtrack.jetbrains.com/issue/TW-89226/Allow-to-upload-dotCover-and-ReSharper-tools-in-tar.gz-format)** — Allow to upload dotCover and ReSharper tools in tar.gz format

**[TW-89234](https://youtrack.jetbrains.com/issue/TW-89234/Add-links-to-download-bundled-dotCover-and-ReSharper-tools-to-the-error-info)** — Add links to download bundled dotCover and ReSharper tools to the error info

**[TW-89133](https://youtrack.jetbrains.com/issue/TW-89133/Deleted-subproject-returns-after-changes-in-parent-project-if-Versioned-settings-are-stored-in-custom-path)** — Deleted subproject returns after changes in parent project, if Versioned settings are stored in custom path

**[TW-85319](https://youtrack.jetbrains.com/issue/TW-85319/TeamCity-refuses-to-connect-to-PostgreSQL-if-the-password-is-longer-than-60-characters)** — TeamCity refuses to connect to PostgreSQL, if the password is longer than 60 characters

**[TW-66741](https://youtrack.jetbrains.com/issue/TW-66741/Branch-filter-applied-but-not-editable-on-Favorite-Projects-view)** — Branch filter applied but not editable on Favorite Projects view

**[TW-57330](https://youtrack.jetbrains.com/issue/TW-57330/Gradle-build-always-runs-tests)** — Gradle build always runs tests


### Performance Problem

**[TW-89424](https://youtrack.jetbrains.com/issue/TW-89424/Reduce-contention-on-update-of-agent-parameters-for-cloud-agents-started-from-the-same-cloud-image)** — Reduce contention on update of agent parameters for cloud agents started from the same cloud image

**[TW-89330](https://youtrack.jetbrains.com/issue/TW-89330/Versioned-settings-VCS-root-defined-in-Root-project-can-slow-down-start-of-the-builds-even-if-they-are-configured-to-use-current)** — Versioned settings VCS root defined in Root project can slow down start of the builds even if they are configured to use current settings



<!--project: TeamCity Fix versions: 2024.07.2  #Fixed #Testing #{Security Problem} -{Trunk issue}-->



### Security

3 security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2024.03). Security bulletins for new versions are typically published within the next few days after the release date.