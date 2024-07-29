[//]: # (title: TeamCity 2024.03.1 Release Notes)
[//]: # (auxiliary-id: TeamCity 2024.03.1 Release Notes)


**Build 156270, 3 May 2024**


<!--project: TeamCity Fix versions: {2024.03 (156166)} , {2024.01 Cloud} , -{2023.11 (147331)} , -{2023.11.1 (147412)} , -{2023.11.2 (147486)} , -{2023.11.3 (147512)} , -{2023.11.4 (147586)} #Fixed #Testing visible to: {All Users} -{Trunk issue}-->

<!--project: TeamCity Fix versions: {2024.03.1 (156270)} , -{2024.03 (156166)} #Fixed #Testing visible to: {All Users} -{Trunk issue}-->

### Bug

**[TW-87363](https://youtrack.jetbrains.com/issue/TW-87363/Nuget-feed-auth-fails-with-error-401-if-NTLM-auth-module-is-enabled-on-the-server)** — Nuget feed auth fails with error 401, if NTLM auth module is enabled on the server

**[TW-86555](https://youtrack.jetbrains.com/issue/TW-86555/Confusing-message-Gradles-configuration-cache-is-enabled-in-the-build-log-if-the-old-Gradle-version-and-incorrect-configuration)** — Confusing message "Gradle's configuration-cache is enabled" in the build log if the old Gradle version and incorrect configuration-cache parameter are used in the project

**[TW-87322](https://youtrack.jetbrains.com/issue/TW-87322/Upload-custom-tool-fails-with-error-Failed-to-move-the-unpacked-tool-from-the-temporary-directory)** — Upload custom tool fails with error "Failed to move the unpacked tool from the temporary directory"

**[TW-75160](https://youtrack.jetbrains.com/issue/TW-75160/Builds-in-default-branch-do-not-start-if-fallback-to-default-branch-is-disabled)** — Builds in default branch do not start, if fallback to default branch is disabled

**[TW-87156](https://youtrack.jetbrains.com/issue/TW-87156/Agent-Disconnected-Unregistered-because-of-inactivity)** — Agent - Disconnected, Unregistered because of inactivity

**[TW-87460](https://youtrack.jetbrains.com/issue/TW-87460/Build-Status-icons-are-shifted-1px-left)** — Build Status icons are shifted 1px left

**[TW-82824](https://youtrack.jetbrains.com/issue/TW-82824/Agent-checkout-of-TFS-repo-failed-with-Problem-while-checkout-on-agent-java.lang.NoClassDefFoundError-javax-activation)** — Agent checkout of TFS repo failed with "Problem while checkout on agent: java.lang.NoClassDefFoundError: javax/activation/DataSource"

**[TW-82293](https://youtrack.jetbrains.com/issue/TW-82293/bean-currentUser-not-found-within-scope-error-in-the-logs-when-an-unauthorized-user-opens-a-non-existing-page)** — "bean currentUser not found within scope" error in the logs when an unauthorized user opens a non existing page

**[TW-85720](https://youtrack.jetbrains.com/issue/TW-85720/A-lot-of-NPEs-may-be-logged-if-caching-estimator-was-unable-to-initialize)** — A lot of NPEs may be logged if caching estimator was unable to initialize

**[TW-87404](https://youtrack.jetbrains.com/issue/TW-87404/Classic-UI-build-status-icons-are-always-grey)** — Classic UI build status icons are always grey

**[TW-87162](https://youtrack.jetbrains.com/issue/TW-87162/ClassicUI-Stop-the-run-button-disappeared-on-project-overview-pages)** — ClassicUI: Stop the run button disappeared on project/overview pages

**[TW-87243](https://youtrack.jetbrains.com/issue/TW-87243/Request-authenticated-via-Build-credentials-fails-with-You-do-not-have-enough-permissions)** — Request authenticated via Build credentials fails with "You do not have enough permissions"

**[TW-86963](https://youtrack.jetbrains.com/issue/TW-86963/New-EC2-UI-User-Data-field-pressing-p-key-opens-project-popup)** — New EC2 UI User Data field: pressing "p" key opens project popup

**[TW-87025](https://youtrack.jetbrains.com/issue/TW-87025/Empty-response-body-for-401-unauthorized-error-in-2024.03)** — Empty response body for 401 unauthorized error in 2024.03+

**[TW-87336](https://youtrack.jetbrains.com/issue/TW-87336/Misleading-documentation-for-scheduled-trigger-with-build-change)** — Misleading documentation for scheduled trigger with build change

**[TW-87242](https://youtrack.jetbrains.com/issue/TW-87242/400-Bad-Request-page-at-attempt-to-open-overview-of-Canceled-build-that-required-approval-from-Untrusted-build-feature)** — 400 (Bad Request) page at attempt to open overview of Canceled build that required approval from Untrusted build feature

**[TW-87170](https://youtrack.jetbrains.com/issue/TW-87170/teamcity-process-environment-fetcher-plugin-corrupted-jar-reported-by-TeamCity.Node-plugin)** — teamcity process environment fetcher plugin corrupted jar reported by TeamCity.Node plugin

**[TW-87274](https://youtrack.jetbrains.com/issue/TW-87274/Bitbucket-server-OAuth-sign-in-can-fail-to-fetch-current-user)** — Bitbucket server: OAuth sign-in can fail to fetch current user

**[TW-87293](https://youtrack.jetbrains.com/issue/TW-87293/Internal-error-occurred-in-Docker-Compose-builds-running-on-podman-agents)** — "Internal error occurred" in Docker Compose builds running on podman agents

**[TW-86616](https://youtrack.jetbrains.com/issue/TW-86616/Cant-connect-GitHub-Enterprise-and-Space-account-with-TeamCity-user-account-in-users-profile)** — Can't connect GitHub Enterprise and Space account with TeamCity user account in user's profile

**[TW-87198](https://youtrack.jetbrains.com/issue/TW-87198/Add-new-parameter-dialog-set-cursor-after-parameter-kind-when-system-or-env-parameter-is-selected)** — Add new parameter dialog: set cursor after parameter kind when system or env parameter is selected

**[TW-49917](https://youtrack.jetbrains.com/issue/TW-49917/Allow-to-modify-read-only-parameter-value-when-parameter-spec-is-editable)** — Allow to modify read-only parameter value when parameter spec is editable

**[TW-80888](https://youtrack.jetbrains.com/issue/TW-80888/Images-pushed-by-podman-are-not-cleaned-up-by-Docker-support-build-feature)** — Images pushed by podman are not cleaned up by Docker support build feature

**[TW-49608](https://youtrack.jetbrains.com/issue/TW-49608/GitHub-publisher-should-parse-errors-in-HTTP-response-and-report-it-to-the-user)** — GitHub publisher should parse errors in HTTP response and report it to the user

**[TW-86315](https://youtrack.jetbrains.com/issue/TW-86315/Failed-to-perform-checkout-on-agent-Problem-while-checkout-on-agent-java.lang.IllegalStateException-NotNull-method-jetbrains)** — Failed to perform checkout on agent: Problem while checkout on agent: java.lang.IllegalStateException: @NotNull method jetbrains/buildServer/vcs/perforce/ClientNameBuilder.getWorkspaceName must not return null

**[TW-87019](https://youtrack.jetbrains.com/issue/TW-87019/refreshable-token-cannot-be-used-in-commit-status-publisher-if-vcs-root-is-defined-higher-in-the-hierarchy-than-GitHubApp)** — refreshable token cannot be used in Commit Status Publisher if VCS root is defined higher in the hierarchy than GitHubApp connection

**[TW-87024](https://youtrack.jetbrains.com/issue/TW-87024/A-build-does-not-start-for-hours-until-server-restart)** — A build does not start for hours, until server restart

**[TW-87179](https://youtrack.jetbrains.com/issue/TW-87179/possibility-to-obtain-non-GitHubApp-token-with-GitHubApp-token-authentication-type)** — possibility to obtain non-GitHubApp token with "GitHubApp token" authentication type

**[TW-86594](https://youtrack.jetbrains.com/issue/TW-86594/Docker-compose-runner-doesnt-work-with-a-podman-compose)** — Docker-compose runner doesn't work with a podman-compose

**[TW-87194](https://youtrack.jetbrains.com/issue/TW-87194/Plugins-which-depend-on-rest-api-plugin-are-not-reloadable)** — Plugins which depend on rest-api plugin are not reloadable

**[TW-87195](https://youtrack.jetbrains.com/issue/TW-87195/Implementations-of-JerseyInjectableBeanProvider-in-plugins-are-not-registered-in-the-container)** — Implementations of JerseyInjectableBeanProvider in plugins are not registered in the container

**[TW-87097](https://youtrack.jetbrains.com/issue/TW-87097/untrusted-builds-excessive-logging-for-canceled-builds)** — untrusted builds: excessive logging for canceled builds

**[TW-86577](https://youtrack.jetbrains.com/issue/TW-86577/Retry-trigger-does-not-retry-a-build-on-the-same-revisions-while-there-is-a-running-build-already)** — Retry trigger does not retry a build on the same revisions while there is a running build already

**[TW-86799](https://youtrack.jetbrains.com/issue/TW-86799/Agent-link-for-dead-cloud-instance-should-lead-to-a-cloud-image-instead)** — Agent link for dead cloud instance should lead to a cloud image instead

**[TW-86773](https://youtrack.jetbrains.com/issue/TW-86773/untrusted-builds-auto-approval-doesnt-approve-generated-subbuilds)** — untrusted builds: auto-approval doesn't approve generated subbuilds

**[TW-87173](https://youtrack.jetbrains.com/issue/TW-87173/Wrong-time-unit-milliseconds-instead-of-seconds-in-Time-saved-by-reusing-dependency-builds-statistic-value)** — Wrong time unit (milliseconds instead of seconds) in "Time saved by reusing dependency builds" statistic value

**[TW-87174](https://youtrack.jetbrains.com/issue/TW-87174/Time-saved-by-reusing-dependency-builds-0-is-reported-to-statistics-if-reused-dependency-was-already-running-when-composite)** — "Time saved by reusing dependency builds = 0" is reported to statistics, if reused dependency was already running when composite build was started (optimized chain)

**[TW-87054](https://youtrack.jetbrains.com/issue/TW-87054/Confusing-logging-of-user-creation)** — Confusing logging of user creation

**[TW-87034](https://youtrack.jetbrains.com/issue/TW-87034/Build-agents-lib-dir-isnt-accessible-while-building-in-a-Docker-container-using-a-TC-agent-from-a-Docker-image)** — Build agent's lib dir isn't accessible while building in a Docker container using a TC agent from a Docker image

**[TW-87132](https://youtrack.jetbrains.com/issue/TW-87132/The-Json-body-should-contain-a-list-of-builds-in-the-Reusing-Builds-with-Indirect-Dependencies-section)** — The Json body should contain a list of builds in the "Reusing Builds with Indirect Dependencies" section

**[TW-87093](https://youtrack.jetbrains.com/issue/TW-87093/Add-a-note-that-no-security-patches-are-currently-available-for-semi-automatical-installation)** — Add a note that no security patches are currently available for semi-automatical installation

**[TW-86852](https://youtrack.jetbrains.com/issue/TW-86852/Visible-HTML-tags-in-displayed-health-items.)** — Visible HTML tags in displayed health items.

**[TW-86701](https://youtrack.jetbrains.com/issue/TW-86701/Build-Artifacts-files-considered-folders-in-some-cases)** — Build Artifacts: files considered folders in some cases


### Performance Problem

**[TW-87183](https://youtrack.jetbrains.com/issue/TW-87183/Commit-Status-Publisher-submits-multinode-tasks-for-all-build-configurations)** — Commit Status Publisher submits multinode tasks for all build configurations

**[TW-78253](https://youtrack.jetbrains.com/issue/TW-78253/Slow-triggers-processing-possibly-because-some-of-the-VCS-commits-are-unloaded-from-the-cache-because-they-are-too-old)** — Slow triggers processing possibly because some of the VCS commits are unloaded from the cache because they are too old

**[TW-87192](https://youtrack.jetbrains.com/issue/TW-87192/Improve-performance-of-multi-node-tasks-processing)** — Improve performance of multi node tasks processing

**[TW-87133](https://youtrack.jetbrains.com/issue/TW-87133/Improve-performance-of-builds-filtering-with-help-of-sinceBuild-untilBuild-in-REST-API-locator)** — Improve performance of builds filtering with help of sinceBuild/untilBuild in REST API locator

**[TW-86794](https://youtrack.jetbrains.com/issue/TW-86794/Project-loading-may-consume-all-memory-in-case-of-a-cycle)** — Project loading may consume all memory in case of a cycle

**[TW-86911](https://youtrack.jetbrains.com/issue/TW-86911/Inefficient-code-in-Change.isVersionedSettings-possibly-leading-to-higher-CPU-usage)** — Inefficient code in Change.isVersionedSettings possibly leading to higher CPU usage


<!--project: TeamCity Fix versions: {2024.03 (156166)} , {2024.01 Cloud} , -{2023.11 (147331)} , -{2023.11.1 (147412)} , -{2023.11.2 (147486)} , -{2023.11.3 (147512)} , -{2023.11.4 (147586)} #Fixed #Testing #{Security Problem} -{Trunk issue}-->

<!--project: TeamCity Fix versions: {2024.03.1 (156270)}, -{2024.03 (156166)} #Fixed #Testing #{Security Problem} -{Trunk issue}-->

### Security

2 security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2024.03). Security bulletins for new versions are typically published within the next few days after the release date.