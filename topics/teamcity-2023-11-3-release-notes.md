[//]: # (title: TeamCity 2023.11.3 Release Notes)
[//]: # (auxiliary-id: TeamCity 2023.11.3 Release Notes)


**Build 147512, 30 January 2024**


<!--project: TeamCity Fix versions: {2023.11.3 (147512)}  #Fixed #Testing visible to: {All Users} -{Trunk issue}-->


### Bug

**[TW-85580](https://youtrack.jetbrains.com/issue/TW-85580/Artifacts-downloading-can-be-broken-if-the-same-host-as-the-Server-URL-and-Artifacts-URL-is-used)** — Artifacts downloading can be broken if the same host as the Server URL and Artifacts URL is used

**[TW-84713](https://youtrack.jetbrains.com/issue/TW-84713/Request-for-agent-files-tree-can-fail-with-ConversionException-exception)** — Request for agent files tree can fail with ConversionException exception

**[TW-85966](https://youtrack.jetbrains.com/issue/TW-85966/Matrix-build-overview-Pinned-and-Favorite-filters-are-always-disabled)** — Matrix build overview: "Pinned" and "Favorite" filters are always disabled

**[TW-86011](https://youtrack.jetbrains.com/issue/TW-86011/Versioned-settings-update-can-stop-working-in-some-projects-if-too-many-update-tasks-are-created)** — Versioned settings update can stop working in some projects if too many update tasks are created

**[TW-84305](https://youtrack.jetbrains.com/issue/TW-84305/extra-id-paramter-is-committed-to-DSL-Settings-for-awsConnection-if-Display-name-of-this-AWS-connection-was-changed-in-S3)** — extra id paramter is committed to DSL Settings for awsConnection if Display name of this AWS connection was changed in S3

**[TW-83683](https://youtrack.jetbrains.com/issue/TW-83683/Excessive-No-builds-found-in-whole-build-history-message-when-Search-Dependencies-in-the-List-finds-nothing)** — Excessive "No builds found in whole build history" message when Search Dependencies in the List finds nothing

**[TW-82731](https://youtrack.jetbrains.com/issue/TW-82731/custom-build-execution-timeout-in-Failure-Conditions-is-not-displayed-on-buildTypeSettings-page-of-a-Composite-build)** — custom build execution timeout in Failure Conditions is not displayed on buildTypeSettings page of a Composite build

**[TW-85999](https://youtrack.jetbrains.com/issue/TW-85999/Teamcity-no-longer-generates-a-test-coverage-report)** — Teamcity no longer generates a test coverage report

**[TW-85718](https://youtrack.jetbrains.com/issue/TW-85718/TeamCity-is-not-picking-up-changes-in-Perforce-VCS-root)** — TeamCity is not picking up changes in Perforce VCS root

**[TW-85849](https://youtrack.jetbrains.com/issue/TW-85849/On-copying-build-step-generate-step-ID-based-on-existing-one)** — On copying build step, generate step ID based on existing one

**[TW-85433](https://youtrack.jetbrains.com/issue/TW-85433/Typo-in-Add-Custom-Chart-statistics-screen)** — Typo in "Add Custom Chart" statistics screen

**[TW-84517](https://youtrack.jetbrains.com/issue/TW-84517/EC2-UI-do-not-clear-entered-server-URL-when-selecting-Amazon-EC2)** — EC2 UI: do not clear entered server URL when selecting "Amazon EC2"

**[TW-83765](https://youtrack.jetbrains.com/issue/TW-83765/EC2-Cloud-Profile-failed-connection-to-Amazon-after-choosing-type-of-a-cloud-profile)** — EC2 Cloud Profile: failed connection to Amazon after choosing type of a cloud profile



<!--project: TeamCity Fix versions: {2023.11.3 (147512)}  #Fixed #{Security Problem}  -{Trunk issue}-->

### Security

2 security problems have been fixed.

> We highly recommend installing this update as it includes a fix for a critical security vulnerability.
>
{style="warning"}

> We do not share the details of security-related issues to avoid compromising clients that keep using previous bugfix and/or major versions of TeamCity. Check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2023.11) for the list of disclosed vulnerability fixes. Security bulletins for new versions are typically published within the next few days after the release date.
>
{style="note"}

