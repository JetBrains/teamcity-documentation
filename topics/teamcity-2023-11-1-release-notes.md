[//]: # (title: TeamCity 2023.11.1 Release Notes)
[//]: # (auxiliary-id: TeamCity 2023.11.1 Release Notes)


**Build 147412, 15 December 2023**


<!--project: TeamCity Fix versions: 2023.11.1 #Fixed #Testing visible to: {All Users} -{Trunk issue}-->

### Bug

**[TW-85393](https://youtrack.jetbrains.com/issue/TW-85393)** — Bitbucket commit status rejected due to too long key

**[TW-85409](https://youtrack.jetbrains.com/issue/TW-85409/Multiple-Problems-after-Upgrade-to-2023.11)** — Multiple Problems after Upgrade to 2023.11

**[TW-85386](https://youtrack.jetbrains.com/issue/TW-85386/Test-parsing-doesnt-work)** — Test parsing doesn't work

**[TW-85310](https://youtrack.jetbrains.com/issue/TW-85310/Access-to-Subversion-SVN-broken-NoSuchMethodError-boolean-org.apache.sshd.client.future.ConnectFuture.await)** — Access to Subversion (SVN) broken "NoSuchMethodError: 'boolean org.apache.sshd.client.future.ConnectFuture.await()'"

**[TW-84965](https://youtrack.jetbrains.com/issue/TW-84965/Matrix-build-does-not-checkout-from-VCS-root-defined-in-DSL-feature-branch)** — Matrix build does not checkout from VCS root defined in DSL feature branch

**[TW-85398](https://youtrack.jetbrains.com/issue/TW-85398/Predefined-parameter-teamcity.build.branch-is-not-available-in-Parallel-tests-and-Matrix-buids)** — Predefined parameter teamcity.build.branch is not available in Parallel tests and Matrix buids

**[TW-83797](https://youtrack.jetbrains.com/issue/TW-83797/Personal-access-token-with-outdated-or-corrupted-refresh-token-can-not-be-refreshed)** — Personal access token with outdated or corrupted refresh token can not be refreshed

**[TW-79981](https://youtrack.jetbrains.com/issue/TW-79981/Commit-Status-Publisher-overrides-statuses-of-newer-finished-builds-by-publishing-statuses-of-older-finished-builds-for-the-same)** — Commit Status Publisher overrides statuses of newer finished builds by publishing statuses of older finished builds for the same revision

**[TW-85533](https://youtrack.jetbrains.com/issue/TW-85533/Matrix-build-configuration-Set-focus-on-Name-Value-field-after-adding-new-parameter-name-value)** — Matrix build configuration: Set focus on Name/Value field after adding new parameter name/value

**[TW-85444](https://youtrack.jetbrains.com/issue/TW-85444/LDAP-sync-retrieves-only-1000-users)** — LDAP sync retrieves only 1000 users

**[TW-85240](https://youtrack.jetbrains.com/issue/TW-85240/Matrix-build-Show-one-parameter-value-by-default)** — Matrix build: Show one parameter value by default

**[TW-85507](https://youtrack.jetbrains.com/issue/TW-85507/Re-run-matrix-build-with-OS-parameter-cannot-rebuild-virtual-dependency-Cyclic-snapshot-dependency-detected)** — Re-run matrix build with OS parameter cannot rebuild virtual dependency: Cyclic snapshot dependency detected

**[TW-85194](https://youtrack.jetbrains.com/issue/TW-85194/Allow-to-manually-specify-revision-for-Versioned-settings-VCS-root-if-there-are-no-VCS-roots-attached-to-the-build-configuration)** — Allow to manually specify revision for Versioned settings VCS root, if there are no VCS roots attached to the build configuration

**[TW-85328](https://youtrack.jetbrains.com/issue/TW-85328/Perforce-build-continues-executing-even-if-there-are-problems-during-checkout)** — Perforce: build continues executing even if there are problems during checkout

**[TW-84990](https://youtrack.jetbrains.com/issue/TW-84990/NUnit-parallel-tests-with-Parametrized-TestFixture-run-in-one-batch-case-with-brackets-in-classnames)** — NUnit parallel tests with Parametrized TestFixture run in one batch (case with brackets in classnames)

**[TW-85382](https://youtrack.jetbrains.com/issue/TW-85382/Dependencies-drop-down-no-longer-displays-branch-name)** — Dependencies drop-down no longer displays branch name

**[TW-85203](https://youtrack.jetbrains.com/issue/TW-85203/The-favorite-projects-page-after-the-first-server-start-Minified-exception-occurred)** — The favorite projects page after the first server start: Minified exception occurred

**[TW-85181](https://youtrack.jetbrains.com/issue/TW-85181/Dotnet-test-with-DotCover-gets-stuck-with-.NET-8)** — Dotnet test with DotCover gets stuck with .NET 8

**[TW-85055](https://youtrack.jetbrains.com/issue/TW-85055/Matrix-build-configuration-Sort-the-list-of-predefined-variables)** — Matrix build configuration: Sort the list of predefined variables

**[TW-80160](https://youtrack.jetbrains.com/issue/TW-80160/Improve-the-warning-in-Compatible-Agents-tab-if-the-project-assosiated-with-pool-with-no-agents)** — Improve the warning in Compatible Agents tab if the project assosiated with pool with no agents

**[TW-63914](https://youtrack.jetbrains.com/issue/TW-63914/Bad-diagnostics-when-registering-VCS-root-in-build-type-which-was-not-previously-registered-in-the-project)** — Bad diagnostics when registering VCS root in build type which was not previously registered in the project

**[TW-85476](https://youtrack.jetbrains.com/issue/TW-85476/Top-block-is-not-closed-in-build-log-of-personal-build)** — Top block is not closed in build-log of personal build

**[TW-85283](https://youtrack.jetbrains.com/issue/TW-85283/Incorrect-and-confusing-message-on-the-Compatible-agents-page-if-default-pool-does-not-have-agents)** — Incorrect and confusing message on the Compatible agents page if default pool does not have agents

**[TW-85357](https://youtrack.jetbrains.com/issue/TW-85357/Exception-in-IntelliJ-IDEA-plugin)** — Exception in IntelliJ IDEA plugin

**[TW-74905](https://youtrack.jetbrains.com/issue/TW-74905/Select-agent-in-Custom-run-dialog-for-Parallel-Tests-build-has-no-effect)** — Select agent in Custom run dialog for Parallel Tests build has no effect

**[TW-83449](https://youtrack.jetbrains.com/issue/TW-83449/Environment-Variables-specified-in-buildAgent.properties-not-being-passed-when-a-Docker-Wrapper-is-configured-as-an-empty-image)** — Environment Variables specified in buildAgent.properties not being passed when a Docker Wrapper is configured as an empty image

**[TW-72730](https://youtrack.jetbrains.com/issue/TW-72730/Unable-to-delete-unused-maven-settings-file)** — Unable to delete unused maven settings file

**[TW-85404](https://youtrack.jetbrains.com/issue/TW-85404/Virtual-projects-are-not-being-removed-if-their-parent-project-is-removed-from-DSL)** — Virtual projects are not being removed if their parent project is removed from DSL

**[TW-61884](https://youtrack.jetbrains.com/issue/TW-61884/It-is-possible-to-remove-a-parent-project-of-some-sub-project-via-versioned-settings)** — It is possible to remove a parent project of some sub project via versioned settings

**[TW-85413](https://youtrack.jetbrains.com/issue/TW-85413/vcsRoot.id.-parameters-are-not-available-in-matrix-parallel-tests-builds)** — vcsRoot.&lt;id>.* parameters are not available in matrix & parallel tests builds

**[TW-79938](https://youtrack.jetbrains.com/issue/TW-79938/Agent-instances-are-not-terminated-if-Cloud-image-was-deleted-changed-from-a-secondary-node)** — Agent instances are not terminated, if Cloud image was deleted/changed from a secondary node

**[TW-82946](https://youtrack.jetbrains.com/issue/TW-82946/Misleading-warning-during-checkout-process-involving-multiple-VCS-Roots)** — Misleading warning during checkout process involving multiple VCS Roots

**[TW-84639](https://youtrack.jetbrains.com/issue/TW-84639/Missing-dependencies-tests-in-Matrix-build-status-in-a-two-node-setup)** — Missing dependencies/tests in Matrix build status in a two-node setup

**[TW-82709](https://youtrack.jetbrains.com/issue/TW-82709/Re-run-build-with-customized-password-parameters-uses-initial-parameter-value)** — Re-run build with customized password parameters uses initial parameter value

**[TW-85155](https://youtrack.jetbrains.com/issue/TW-85155/Cryptic-error-instead-of-clear-Authentication-failed-message-on-attempt-to-create-a-VCS-root-from-URL)** — Cryptic error instead of clear "Authentication failed" message on attempt to create a VCS root from URL

**[TW-84066](https://youtrack.jetbrains.com/issue/TW-84066/Test-Connection-for-GitHub-App-doesnt-check-the-Webhook-secret)** — "Test Connection" for GitHub App doesn't check the Webhook secret

**[TW-85329](https://youtrack.jetbrains.com/issue/TW-85329/AWS-Tel-Aviv-region-is-missing-in-region-selector-in-AWS-related-features)** — AWS Tel Aviv region is missing in region selector in AWS-related features

**[TW-82970](https://youtrack.jetbrains.com/issue/TW-82970/Unable-to-set-any-value-for-configuration-parameters-with-prefix-dep-using-a-service-message)** — Unable to set any value for configuration parameters with prefix "dep" using a service message

**[TW-83796](https://youtrack.jetbrains.com/issue/TW-83796/Exception-on-token-scope-parsing)** — Exception on token scope parsing

**[TW-84413](https://youtrack.jetbrains.com/issue/TW-84413/TeamCity-spams-a-lot-of-messages-to-teamcity-server.log-and-automatically-saved-thread-dumps-if-GitHub-Server-is-unavailable)** — TeamCity spams a lot of messages to teamcity-server.log and automatically saved thread dumps if GitHub Server is unavailable

**[TW-84469](https://youtrack.jetbrains.com/issue/TW-84469/Update-aws-java-sdk-s3-version)** — Update aws-java-sdk-s3 version

**[TW-83740](https://youtrack.jetbrains.com/issue/TW-83740/Classic-UI-changes-dropdown-is-not-visible)** — Classic UI: changes dropdown is not visible

**[TW-83817](https://youtrack.jetbrains.com/issue/TW-83817/Agen-JDKs-cant-download-one-particular-JDK)** — Agen JDKs: can't download one particular JDK

**[TW-2068](https://youtrack.jetbrains.com/issue/TW-2068/Provide-information-on-reason-builds-are-preserved-during-cleanup)** — Provide information on reason builds are preserved during cleanup

**[TW-82710](https://youtrack.jetbrains.com/issue/TW-82710/Retry-trigger-starts-a-build-with-value-for-customized-password-parameter)** — Retry trigger starts a build with ***** value for customized password parameter

**[TW-85004](https://youtrack.jetbrains.com/issue/TW-85004/Clean-up-is-hanging-due-to-inefficient-query-in-VcsChangeTableCleaner)** — Clean-up is hanging due to inefficient query in VcsChangeTableCleaner

**[TW-84912](https://youtrack.jetbrains.com/issue/TW-84912/java.lang.UnsupportedOperationException-Build-has-not-been-populated)** — java.lang.UnsupportedOperationException: Build has not been populated

**[TW-84957](https://youtrack.jetbrains.com/issue/TW-84957/VCS-Trigger-doesnt-start-a-build-if-per-checkin-triggering-and-quiet-period-are-enabled)** — VCS Trigger doesn't start a build if per-checkin triggering and quiet period are enabled

**[TW-84925](https://youtrack.jetbrains.com/issue/TW-84925/Default-Tools-integration-role-does-not-have-enough-permissions-for-Youtrack)** — Default "Tools integration" role does not have enough permissions (for Youtrack)

**[TW-58596](https://youtrack.jetbrains.com/issue/TW-58596/All-numbers-values-of-test-metadata-are-showed-as-miliseconds)** — All numbers values of test metadata are showed as miliseconds

**[TW-84908](https://youtrack.jetbrains.com/issue/TW-84908/NPE-in-jetbrains.buildServer.serverSide.buildDistribution.WaitReason.getDescription)** — NPE in jetbrains.buildServer.serverSide.buildDistribution.WaitReason.getDescription()

**[TW-84713](https://youtrack.jetbrains.com/issue/TW-84713/Request-for-agent-files-tree-can-fail-with-ConversionException-exception)** — Request for agent files tree can fail with ConversionException exception

**[TW-83918](https://youtrack.jetbrains.com/issue/TW-83918/Inconsistent-disk-usage-information)** — Inconsistent disk usage information


### Performance Problem

**[TW-85482](https://youtrack.jetbrains.com/issue/TW-85482/Too-slow-revision-computation-lots-of-time-spent-in-CheckoutRulesRevWalk.collectUninterestingCommits)** — Too slow revision computation (lots of time spent in CheckoutRulesRevWalk.collectUninterestingCommits)

**[TW-85326](https://youtrack.jetbrains.com/issue/TW-85326/Slow-preloading-of-test-names-when-last-used-names-cache-is-absent)** — Slow preloading of test names when last used names cache is absent

**[TW-85164](https://youtrack.jetbrains.com/issue/TW-85164/Slow-loading-of-My-Investigations-page)** — Slow loading of My Investigations page

**[TW-85342](https://youtrack.jetbrains.com/issue/TW-85342/A-lot-of-memory-may-be-occupied-by-GitClonesUpdater)** — A lot of memory may be occupied by GitClonesUpdater

**[TW-84258](https://youtrack.jetbrains.com/issue/TW-84258/Perforce-Shelve-Triggers-may-consume-a-lot-of-CPU)** — Perforce Shelve Triggers may consume a lot of CPU




<!--project: TeamCity Fix versions: {2023.11 (147318)} #Fixed #{Security Problem}  -{Trunk issue}-->

### Security

6 security problems have been fixed.

> We do not share the details of security-related issues to avoid compromising clients that keep using previous bugfix and/or major versions of TeamCity. Check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2024.03) for the list of disclosed vulnerability fixes. Security bulletins for new versions are typically published within the next few days after the release date.
>
{style="note"}

