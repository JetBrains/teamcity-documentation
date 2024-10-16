[//]: # (title: TeamCity 2023.11.2 Release Notes)
[//]: # (auxiliary-id: TeamCity 2023.11.2 Release Notes)


**Build 147486, 18 January 2024**


<!--project: TeamCity Fix versions: 2023.11.2 #Fixed #Testing visible to: {All Users} -{Trunk issue}-->

### Bug

**[TW-85832](https://youtrack.jetbrains.com/issue/TW-85832/Docker-exec-overrides-user-defined-working-directory)** — Docker exec overrides user-defined working directory

**[TW-85778](https://youtrack.jetbrains.com/issue/TW-85778/logout-function-does-not-redirect-to-login-page)** — logout function does not redirect to login page

**[TW-77502](https://youtrack.jetbrains.com/issue/TW-77502/Secure-parameters-are-not-encrypted-with-the-current-encryption-key-after-updating-a-project-parameter)** — Secure parameters are not encrypted with the current encryption key after updating a project parameter

**[TW-85834](https://youtrack.jetbrains.com/issue/TW-85834/Cannot-override-build-configuration-environment-variables-in-docker-wrapper)** — Cannot override build configuration environment variables in docker wrapper

**[TW-84110](https://youtrack.jetbrains.com/issue/TW-84110/Matrix-builds-overview-Dynamic-status-filters-make-parameters-dropdown-jumping-for-a-running-build)** — Matrix builds overview: Dynamic status filters make parameters dropdown jumping for a running build

**[TW-85853](https://youtrack.jetbrains.com/issue/TW-85853/Failed-to-perform-runner-finish-stage-jetbrains.buildServer.agent.impl.buildStages.runnerStages.finish.PublishStepStatusFStage)** — Failed to perform runner finish stage jetbrains.buildServer.agent.impl.buildStages.runnerStages.finish.PublishStepStatusFStage: java.lang.NullPointerException

**[TW-85845](https://youtrack.jetbrains.com/issue/TW-85845/DOC-Bug-or-doc-issue-with-default-system-properties-in-Gradle-builds)** — [DOC] Bug or doc issue with default system properties in Gradle builds

**[TW-74850](https://youtrack.jetbrains.com/issue/TW-74850/Copying-a-project-adds-VCS-root-to-read-only-Root-project)** — Copying a project adds VCS root to read-only _Root project

**[TW-85584](https://youtrack.jetbrains.com/issue/TW-85584/agentPushPreset-gets-removed-every-time-cloud-profile-is-updated)** — agentPushPreset gets removed every time cloud profile is updated

**[TW-85618](https://youtrack.jetbrains.com/issue/TW-85618/Cannot-install-plugins-due-to-NoSuchMethodError-during-Ajax-request-processing.)** — Cannot install plugins due to NoSuchMethodError during Ajax request processing.

**[TW-85563](https://youtrack.jetbrains.com/issue/TW-85563/Project-status-is-affected-by-last-builds-in-virtual-build-configurations)** — Project status is affected by last builds in virtual build configurations

**[TW-85787](https://youtrack.jetbrains.com/issue/TW-85787/Build-is-stuck-in-Call-Finish-Stage-when-the-build-has-already-finished)** — Build is stuck in Call Finish Stage when the build has already finished

**[TW-85417](https://youtrack.jetbrains.com/issue/TW-85417/Switching-Main-node-responsibility-when-a-server-is-down-blocks-port-443-on-that-server-when-it-is-brought-back-up)** — Switching Main node responsibility when a server is down blocks port 443 on that server when it is brought back up

**[TW-85027](https://youtrack.jetbrains.com/issue/TW-85027/Incorrect-calculation-of-unrelated-builds-in-the-new-UI-build-chains-view)** — Incorrect calculation of unrelated builds in the new UI build chains view

**[TW-85775](https://youtrack.jetbrains.com/issue/TW-85775/The-link-content-regarding-the-bug-in-the-network-settings-is-currently-invalid)** — The link content regarding the "bug in the network settings" is currently invalid

**[TW-85897](https://youtrack.jetbrains.com/issue/TW-85897/Exception-on-attempt-to-reload-removed-parent-project-from-disk-if-it-has-virtual-sub-projects)** — Exception on attempt to reload removed parent project from disk if it has virtual sub projects

**[TW-85902](https://youtrack.jetbrains.com/issue/TW-85902/Virtual-build-configuration-file-prevents-reload-of-the-project-on-the-secondary-node)** — Virtual build configuration file prevents reload of the project on the secondary node

**[TW-85755](https://youtrack.jetbrains.com/issue/TW-85755/Matrix-build-overview-Column-names-may-not-match-table-contents)** — Matrix build overview: Column names may not match table contents

**[TW-85466](https://youtrack.jetbrains.com/issue/TW-85466/Fix-message-texts-for-new-authentication-functionality)** — Fix message texts for new authentication functionality

**[TW-85753](https://youtrack.jetbrains.com/issue/TW-85753/Impossible-to-enable-DSL-Versioned-settings-if-there-are-virtual-projects-without-a-prefix-corresponding-to-parent-project-id)** — Impossible to enable DSL Versioned settings, if there are virtual projects without a prefix corresponding to parent project id

**[TW-85711](https://youtrack.jetbrains.com/issue/TW-85711/Impossible-to-list-Azure-DevOps-repositories-with-OAuth2.0-connection-if-user-has-access-to-huge-repositories)** — Impossible to list Azure DevOps repositories with OAuth2.0 connection, if user has access to huge repositories

**[TW-85422](https://youtrack.jetbrains.com/issue/TW-85422/Publishing-of-queued-build-status-over-final-status-for-the-same-revision)** — Publishing of queued build status over final status for the same revision

**[TW-85287](https://youtrack.jetbrains.com/issue/TW-85287/An-agent-waits-too-long-to-update-after-a-tool-plugin-is-installed-in-case-where-there-are-agents-with-a-bundled-JDK)** — An agent waits too long to update after a tool/plugin is installed (in case where there are agents with a bundled JDK)

**[TW-85495](https://youtrack.jetbrains.com/issue/TW-85495/TeamCity-unable-to-start-after-upgrade)** — TeamCity unable to start after upgrade

**[TW-85333](https://youtrack.jetbrains.com/issue/TW-85333/Agent-JDKs-remove-mention-of-tools-from-the-distribution-table)** — Agent JDKs: remove mention of "tools" from the distribution table

**[TW-84896](https://youtrack.jetbrains.com/issue/TW-84896/Agent-JDKs-Multinode-distribution-isnt-build-in-case-of-JDK-URL-replacement)** — Agent JDKs: Multinode, distribution isn't build in case of JDK URL replacement

**[TW-85771](https://youtrack.jetbrains.com/issue/TW-85771/GitHub-App-webhooks-may-fail-if-more-than-one-GitHub-App-is-configured-for-a-TeamCity-instance)** — GitHub App webhooks may fail if more than one GitHub App is configured for a TeamCity instance

**[TW-77823](https://youtrack.jetbrains.com/issue/TW-77823/Experimental-UI-allows-activation-unpausing-of-kotlin-based-jobs-that-are-paused-via-code)** — Experimental UI allows activation (unpausing) of kotlin-based jobs that are paused via code

**[TW-85690](https://youtrack.jetbrains.com/issue/TW-85690/NPE-and-HTTP-500-when-triggering-a-build-via-REST-API-if-the-payload-contains-the-empty-properties-element)** — NPE and HTTP 500 when triggering a build via REST API if the payload contains the empty properties element

**[TW-85236](https://youtrack.jetbrains.com/issue/TW-85236/Pressing-of-the-escape-key-while-editing-Matrix-feature-closes-the-entire-dialog)** — Pressing of the escape key while editing Matrix feature closes the entire dialog

**[TW-85625](https://youtrack.jetbrains.com/issue/TW-85625/Perforce-possible-race-condition-when-collecting-changes)** — Perforce: possible race condition when collecting changes

**[TW-83921](https://youtrack.jetbrains.com/issue/TW-83921/Server-tries-to-run-two-builds-on-the-same-agent)** — Server tries to run two builds on the same agent

**[TW-85595](https://youtrack.jetbrains.com/issue/TW-85595/Warning-Creating-another-checking-for-changes-task-for-the-build-promotion-in-teamcity-server.log-on-starting-build-in-DSL)** — Warning "Creating another checking for changes task for the build promotion" in teamcity-server.log on starting build in DSL feature branch with modified checkout rules

**[TW-83558](https://youtrack.jetbrains.com/issue/TW-83558/Test-history-page-is-not-responsive)** — Test history page is not responsive

**[TW-85156](https://youtrack.jetbrains.com/issue/TW-85156/Queue-status-can-be-missed-for-BitBucket-Cloud-publisher)** — Queue status can be missed for BitBucket Cloud publisher

**[TW-85560](https://youtrack.jetbrains.com/issue/TW-85560/DSL-Security-Agent-doesnt-allow-reflection-for-classes-loaded-by-same-classloader)** — DSL Security Agent doesn't allow reflection for classes loaded by same classloader

**[TW-83064](https://youtrack.jetbrains.com/issue/TW-83064/Build-status-cannot-be-updated-to-success-with-service-message-if-failureConditions.errorMessage-is-set)** — Build status cannot be updated to success with service message if failureConditions.errorMessage is set

**[TW-85418](https://youtrack.jetbrains.com/issue/TW-85418/remove-build-dialogue-wording)** — "remove build" dialogue wording

**[TW-85472](https://youtrack.jetbrains.com/issue/TW-85472/Artifact-dependency-by-tag-may-not-work-if-the-dependent-build-is-in-the-excluded-branch)** — Artifact dependency by tag may not work if the dependent build is in the excluded branch


### Performance Problem

**[TW-85640](https://youtrack.jetbrains.com/issue/TW-85640/Exclude-testOccurrencecount-from-request-on-a-build-overview-page)** — Exclude testOccurrence(count) from request on a build overview page

**[TW-85638](https://youtrack.jetbrains.com/issue/TW-85638/Slow-notification-generation-if-build-has-a-commit-with-huge-number-of-changed-files)** — Slow notification generation if build has a commit with huge number of changed files




<!--project: TeamCity Fix versions: 2023.11.2 #Fixed #{Security Problem}  -{Trunk issue}-->

### Security

6 security problems have been fixed.

> We do not share the details of security-related issues to avoid compromising clients that keep using previous bugfix and/or major versions of TeamCity. Check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2023.11) for the list of disclosed vulnerability fixes. Security bulletins for new versions are typically published within the next few days after the release date.
>
{style="note"}

