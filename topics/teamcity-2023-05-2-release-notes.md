[//]: # (title: TeamCity 2023.05.2 Release Notes)
[//]: # (auxiliary-id: TeamCity 2023.05.2 Release Notes)

__Build: 129341__

__25 July 2023__


<!--project: TeamCity Fix versions: {2023.05.2 (129341)} #Fixed visible to: {All Users} -{Trunk issue}-->


### Bug

**[TW-82717](https://youtrack.jetbrains.com/issue/TW-82717/agent.bat-started-without-arguments-shows-error-instead-of-help)** — agent.bat started without arguments shows error instead of help

**[TW-82114](https://youtrack.jetbrains.com/issue/TW-82114/The-number-locator-now-fails-if-the-value-contains-special-symbols.)** — The "number" locator now fails if the value contains special symbols.

**[TW-82170](https://youtrack.jetbrains.com/issue/TW-82170/Windows-based-Docker-agent-images-are-not-compatible-with-containerd)** — Windows-based Docker agent images are not compatible with containerd

**[TW-80931](https://youtrack.jetbrains.com/issue/TW-80931/Push-to-internal-NuGet-feed-doesnt-work-on-agent-with-.NET-8)** — Push to internal NuGet feed doesn't work on agent with .NET 8

**[TW-82179](https://youtrack.jetbrains.com/issue/TW-82179/GitHub-App-connection-public-repos-of-one-user-can-invisible-to-another-user-if-the-second-user-is-not-a-collaborator-in-these)** — GitHub App connection: public repos of one user can invisible to another user (if the second user is not a collaborator in these repos)

**[TW-74760](https://youtrack.jetbrains.com/issue/TW-74760/Error-while-applying-patch-when-building-new-branch-git-shallow-clone)** — "Error while applying patch" when building new branch (git shallow clone)

**[TW-80435](https://youtrack.jetbrains.com/issue/TW-80435/Native-Git-VCS-labeling-fails-with-bad-object-type-for-new-VCS-root-or-after-changing-branch-specification)** — Native Git: VCS labeling fails with "bad object type" for new VCS root or after changing branch specification

**[TW-77955](https://youtrack.jetbrains.com/issue/TW-77955/Dispay-additional-information-in-UI-when-a-problem-occured-during-issuing-a-Lets-encrypt-certificate)** — Dispay additional information in UI when a problem occured during issuing a Let's encrypt certificate

**[TW-77940](https://youtrack.jetbrains.com/issue/TW-77940/Improve-audit-logging-for-HTTPS-certificate-removal)** — Improve audit logging for HTTPS certificate removal

**[TW-79902](https://youtrack.jetbrains.com/issue/TW-79902/javax.management.InstanceNotFoundException-when-uploading-new-HTTPS-certificate)** — javax.management.InstanceNotFoundException when uploading new HTTPS certificate

**[TW-81897](https://youtrack.jetbrains.com/issue/TW-81897/Unable-to-open-Dependencies-tab-Chain-mode-after-upgrading-to-2023.05)** — Unable to open Dependencies tab Chain mode after upgrading to 2023.05

**[TW-82127](https://youtrack.jetbrains.com/issue/TW-82127/Commit-Status-Publisher-fails-for-Azure-DevOps-with-shorter-versions-of-fetch-URL-to-default-repository-of-a-project)** — Commit Status Publisher fails for Azure DevOps with shorter versions of fetch URL to default repository of a project

**[TW-81115](https://youtrack.jetbrains.com/issue/TW-81115/Updating-of-Github-App-plugin-without-server-restart-failed)** — Updating of Github App plugin without server restart failed

**[TW-82210](https://youtrack.jetbrains.com/issue/TW-82210/Build-chain-loses-a-dependency-so-it-cannot-be-started-due-to-parameter-dependency-to-a-missing-dependent-build)** — Build chain loses a dependency so it cannot be started due to parameter dependency to a missing dependent build

**[TW-82030](https://youtrack.jetbrains.com/issue/TW-82030/.NET-7.0.202-restore-fails-with-error-NU1301-Unable-to-load-the-service-index-for-source-feedURL)** — .NET 7.0.202 restore fails with error NU1301: Unable to load the service index for source &lt;feedURL>

**[TW-82434](https://youtrack.jetbrains.com/issue/TW-82434/Fix-token-permissions-required-to-approve-build)** — Fix token permissions required to approve build

**[TW-81492](https://youtrack.jetbrains.com/issue/TW-81492/REST-method-versionedSettings-contextParameters-doesnt-return-parameters-without-values)** — REST: method versionedSettings/contextParameters doesn't return parameters without values

**[TW-82165](https://youtrack.jetbrains.com/issue/TW-82165/RejectedExecutionException-in-FailedTestAndBuildProblemsDispatcher-while-server-shutdown)** — RejectedExecutionException in FailedTestAndBuildProblemsDispatcher while server shutdown

**[TW-80608](https://youtrack.jetbrains.com/issue/TW-80608/Empty-file-is-cached-by-external-artifacts-storage-cache)** — Empty file is cached by external artifacts storage cache

**[TW-81960](https://youtrack.jetbrains.com/issue/TW-81960/Exception-when-redirecting-a-build-from-one-node-to-another)** — Exception when redirecting a build from one node to another

**[TW-81743](https://youtrack.jetbrains.com/issue/TW-81743/Pull-Request-Build-Feature-with-Use-VCS-root-credentials-does-not-work-if-username-is-not-specified)** — Pull Request Build Feature with "Use VCS root credentials" does not work, if username is not specified

**[TW-74774](https://youtrack.jetbrains.com/issue/TW-74774/Two-instances-of-WebPublisher-bean-in-agent-memory)** — Two instances of WebPublisher bean in agent memory

**[TW-80387](https://youtrack.jetbrains.com/issue/TW-80387/.NET-NUnit-Parallel-Tests-Incorrect-format-for-TestCaseFilter-Missing-Operator-or-.)** — .NET NUnit Parallel Tests: Incorrect format for TestCaseFilter Missing Operator '|' or '&'.


### Performance Problem

**[TW-82606](https://youtrack.jetbrains.com/issue/TW-82606/AnchorsFileWriter-slows-down-processing-of-the-build-messages-from-the-agent)** — AnchorsFileWriter slows down processing of the build messages from the agent

**[TW-82404](https://youtrack.jetbrains.com/issue/TW-82404/Slow-execution-of-project-settings-persisting-tasks-150K-build-configurations)** — Slow execution of project settings persisting tasks (~150K build configurations)

**[TW-82383](https://youtrack.jetbrains.com/issue/TW-82383/Slow-server-startup-in-case-of-many-critical-errors-and-many-thousands-of-build-configurations)** — Slow server startup in case of many critical errors and many thousands of build configurations


<!--project: TeamCity Fix versions: {2023.05.2 (129341)} #Fixed #{Security Problem}  -{Trunk issue}-->

### Security

3 security problems have been fixed.

> We do not share the details of security-related issues to avoid compromising clients that keep using previous bugfix and/or major versions of TeamCity. Check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2023.05.2) for the list of disclosed vulnerability fixes.
>
{style="note"}

