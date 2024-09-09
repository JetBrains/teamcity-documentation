[//]: # (title: TeamCity 2023.11.4 Release Notes)
[//]: # (auxiliary-id: TeamCity 2023.11.4 Release Notes)


**Build 147586, 4 March 2024**


<!--project: TeamCity Fix versions: 2023.11.4  #Fixed #Testing visible to: {All Users} -{Trunk issue}-->


### Bug

**[TW-86288](https://youtrack.jetbrains.com/issue/TW-86288/If-TeamCity-is-unable-to-reach-jetbrains.com-it-can-slow-down-the-start-of-the-builds-as-well-as-some-pages)** — If TeamCity is unable to reach jetbrains.com, it can slow down the start of the builds as well as some pages

**[TW-86574](https://youtrack.jetbrains.com/issue/TW-86574/jacocoReport.xml-is-created-empty-due-to-ClassNotFoundError-if-non-bundled-Jacoco-is-used)** — jacocoReport.xml is created empty due to ClassNotFoundError (if non bundled Jacoco is used)

**[TW-86186](https://youtrack.jetbrains.com/issue/TW-86186/AWS-instance-role-profile-permissions-no-longer-used-by-EC2-Cloud-Profile-in-2023.11.3)** — AWS instance role/profile permissions no longer used by EC2 Cloud Profile in 2023.11.3

**[TW-86234](https://youtrack.jetbrains.com/issue/TW-86234/Builds-take-incorrect-revision-if-using-settings-from-VCS-and-only-negative-checkout-rules-are-defined)** — Builds take incorrect revision if using settings from VCS and only negative checkout rules are defined

**[TW-86511](https://youtrack.jetbrains.com/issue/TW-86511/EC2-cloud-profile-configuration-missing-agent-push-preset)** — EC2 cloud profile configuration missing agent push preset

**[TW-80467](https://youtrack.jetbrains.com/issue/TW-80467/Cannot-find-a-node100479888-may-occur-when-collecting-VCS-changes-on-the-secondary-node)** — Cannot find a node:100479888 may occur when collecting VCS changes on the secondary node

**[TW-75123](https://youtrack.jetbrains.com/issue/TW-75123/Log-statuses-from-Commit-status-publisher-to-the-teamcity-commit-status.log)** — Log statuses from Commit Status Publisher to the teamcity-commit-status.log

**[TW-86261](https://youtrack.jetbrains.com/issue/TW-86261/Regression-java.lang.InstantiationException-bean-bean-not-found-within-scope-while-processing-request-GET-admin)** — Regression: java.lang.InstantiationException: bean not found within scope while processing request

**[TW-84895](https://youtrack.jetbrains.com/issue/TW-84895/Agent-JDKs-Clean-Up-JDK-distribution-if-it-was-replaced)** — Agent JDKs: Clean Up JDK distribution if it was replaced

**[TW-84805](https://youtrack.jetbrains.com/issue/TW-84805/Publishing-build-cache-build-log-is-never-closed-after-error-duration-keeps-increasing)** — "Publishing build cache" build log is never closed after error, duration keeps increasing

**[TW-85436](https://youtrack.jetbrains.com/issue/TW-85436/Build-Cache-Support-parameter-references-in-cache-names)** — Build Cache: Support parameter references in cache names

**[TW-86166](https://youtrack.jetbrains.com/issue/TW-86166/Update-teamcity-maven-plugin-to-be-compatible-with-2023.11)** — Update teamcity maven plugin to be compatible with 2023.11

**[TW-85243](https://youtrack.jetbrains.com/issue/TW-85243/S3-Storage-new-UI-unnecessary-patch-is-generated-after-saving-S3-settings-using-UI-after-TeamCity-update)** — S3 Storage new UI: unnecessary patch is generated after saving S3 settings using UI after TeamCity update

**[TW-86255](https://youtrack.jetbrains.com/issue/TW-86255/GitHub-App-installation-token-can-not-be-acquired-in-Pull-requests-Build-feature-and-Commit-Status-publisher-if-VCS-Root-URL-has)** — GitHub App installation token can not be acquired in Pull requests Build feature and Commit Status Publisher if VCS Root URL has alternative scp-like syntax user@host.xz:owner/repo.git

**[TW-84599](https://youtrack.jetbrains.com/issue/TW-84599/Log-info-about-agent-selected-based-on-CPU-build-duration-and-agent-priority-to-teamcity-server.log-debug)** — Log info about agent, selected based on CPU, build duration and agent priority, to teamcity-server.log (debug)

**[TW-82610](https://youtrack.jetbrains.com/issue/TW-82610/TeamCity-server-cant-automatically-restart)** — TeamCity server can't automatically restart

**[TW-86244](https://youtrack.jetbrains.com/issue/TW-86244/Cross-node-event-processing-hangs-due-to-a-concurrency-problem)** — Cross-node event processing hangs due to a concurrency problem

**[TW-85794](https://youtrack.jetbrains.com/issue/TW-85794/Failed-muted-tests-may-produce-failed-build-if-server-was-restarted-changes-running-node)** — Failed muted tests may produce failed build if server was restarted/changes running node

**[TW-80270](https://youtrack.jetbrains.com/issue/TW-80270/Flying-loader-on-show-all-N-configurations)** — Flying loader on "show all N configurations"



<!--project: TeamCity Fix versions: 2023.11.4  #Fixed #Testing  #{Security Problem}  -{Trunk issue}-->

### Security

4 security problems have been fixed.

> We highly recommend installing this update as it includes a fix for critical security vulnerabilities.
>
{style="warning"}

> We do not share the details of security-related issues to avoid compromising clients that keep using previous bugfix and/or major versions of TeamCity. Check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2023.11) for the list of disclosed vulnerability fixes. Security bulletins for new versions are typically published within the next few days after the release date.
>
{style="note"}

