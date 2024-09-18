[//]: # (title: TeamCity 2023.05.4 Release Notes)
[//]: # (auxiliary-id: TeamCity 2023.05.4 Release Notes)

__Build: 129421__

__18 September 2023__


<!--project: TeamCity Fix versions: {2023.05.4 (129421)} #Fixed visible to: {All Users} -{Trunk issue}-->


### Feature

**[TW-83079](https://youtrack.jetbrains.com/issue/TW-83079/Allow-to-download-build-log-with-message-timestamps-which-contain-date-in-addition-to-time)** — Allow to download build log with message timestamps which contain date in addition to time



### Bug

**[TW-82732](https://youtrack.jetbrains.com/issue/TW-82732/Unable-to-view-change-details-diffView-for-a-personal-patch-uploaded-via-REST-API-uploadDiffChanges)** — Unable to view change details/diffView for a personal patch uploaded via REST API uploadDiffChanges

**[TW-74406](https://youtrack.jetbrains.com/issue/TW-74406/Make-SSLInvestigator-use-proxy-settings-to-check-custom-certificates)** — Make SSLInvestigator use proxy settings to check custom certificates

**[TW-83353](https://youtrack.jetbrains.com/issue/TW-83353/The-incorrect-branch-is-used-for-re-run-for-a-build-finished-in-a-default-branch-if-the-default-branch-was-changed)** — The incorrect branch is used for re-run for a build finished in a default branch if the default branch was changed

**[TW-83313](https://youtrack.jetbrains.com/issue/TW-83313/Running-build-on-agent-may-hang-when-agents-buildAgent.properties-is-modified-during-the-previous-build)** — Running build on agent may hang when agent's buildAgent.properties is modified during the previous build

**[TW-80246](https://youtrack.jetbrains.com/issue/TW-80246/Build-results-page-URL-containing-the-id-of-a-substituted-build-gives-error-404)** — Build results page URL containing the id of a substituted build gives error 404

**[TW-83407](https://youtrack.jetbrains.com/issue/TW-83407/All-builds-stuck-with-build-settings-have-not-been-finalized-when-Kotlin-DSL-is-broken-in-a-large-project-with-many-queued)** — All builds stuck with build settings have not been finalized (when Kotlin DSL is broken in a large project with many queued builds)

**[TW-83497](https://youtrack.jetbrains.com/issue/TW-83497/Broken-DSL-data-file-on-disk-leads-to-non-working-Kotlin-DSL-versioned-settings-of-all-the-projects)** — Broken DSL data file on disk leads to non-working Kotlin DSL versioned settings of all the projects

**[TW-83429](https://youtrack.jetbrains.com/issue/TW-83429/When-restoring-permissions-in-docker-wrapper-TeamCity-does-not-add-a-build-problem-when-it-fails-to-fetch-busybox-image-to)** — When restoring permissions in docker wrapper, TeamCity does not add a build problem when it fails to fetch 'busybox' image to restore permissions

**[TW-83437](https://youtrack.jetbrains.com/issue/TW-83437/Broken-layout-on-the-build-configuration-page-with-a-running-build-classical-UI)** — Broken layout on the build configuration page with a running build (classical UI)

**[TW-79205](https://youtrack.jetbrains.com/issue/TW-79205/Test-metadata-videos-are-unavailable-due-to-This-image-failed-to-load)** — Test metadata videos are unavailable due to 'This image failed to load'

**[TW-83158](https://youtrack.jetbrains.com/issue/TW-83158/Commit-Status-Publisher-doesnt-remove-Queued-status-for-personal-builds-from-Bitbucket-Cloud)** — Commit Status Publisher doesn't remove Queued status for personal builds from Bitbucket Cloud

**[TW-83328](https://youtrack.jetbrains.com/issue/TW-83328/Inefficient-notifications-processing-for-user-groups)** — Inefficient notifications processing for user groups

**[TW-83354](https://youtrack.jetbrains.com/issue/TW-83354/TeamCity-2023.05.x-does-not-allow-to-start-a-build-with-a-parameter-with-null-value)** — TeamCity 2023.05.x does not allow to start a build with a parameter with null value

**[TW-83334](https://youtrack.jetbrains.com/issue/TW-83334/Do-not-fail-build-when-running-p4-trust-y-f)** — Do not fail build when running `p4 trust -y -f`



### Performance Problem

**[TW-83528](https://youtrack.jetbrains.com/issue/TW-83528/Avoid-running-vacuum-analyze-customdatabody-concurrently)** — Avoid running vacuum analyze custom_data_body concurrently

**[TW-82907](https://youtrack.jetbrains.com/issue/TW-82907/TeamCity-server-increase-in-memory-consumption-after-upgrade-when-lots-of-tests-are-loaded-in-memory)** — TeamCity server increase in memory consumption after upgrade, when lots of tests are loaded in memory



<!--project: TeamCity Fix versions: {2023.05.3 (129390)} #Fixed #{Security Problem}  -{Trunk issue}-->

### Security

2 security problems have been fixed.

> We do not share the details of security-related issues to avoid compromising clients that keep using previous bugfix and/or major versions of TeamCity. Check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2023.05.4) for the list of disclosed vulnerability fixes. Security bulletins for new versions are typically published within the next few days after the release date.
>
{style="note"}

