[//]: # (title: TeamCity 2024.12 Release Notes)
[//]: # (auxiliary-id: TeamCity 2024.12 Release Notes)


**Build 174331, 5 December 2024**

### Feature

* [**TW-89624**](https://youtrack.jetbrains.com/issue/TW-89624) — Skip dependencies in the chain while a build chain is already running
* [**TW-88446**](https://youtrack.jetbrains.com/issue/TW-88446) — If TeamCity VCSRoot syncs a label - There's no parameter that states the Changelist synced
* [**TW-65341**](https://youtrack.jetbrains.com/issue/TW-65341) — Execute a dependency basing on condition
* [**TW-82501**](https://youtrack.jetbrains.com/issue/TW-82501) — Build flow in the executor mode
* [**TW-82378**](https://youtrack.jetbrains.com/issue/TW-82378) — Support for using AWS connection to configure AWS EC2 cloud agents
* [**TW-89122**](https://youtrack.jetbrains.com/issue/TW-89122) — Run meta-runner in container
* [**TW-87479**](https://youtrack.jetbrains.com/issue/TW-87479) — Simple Token Management in Admin UI
* [**TW-78593**](https://youtrack.jetbrains.com/issue/TW-78593) — Tokens issued via GitHub App Connection must be restricted to multiple relevant repositories
* [**TW-78586**](https://youtrack.jetbrains.com/issue/TW-78586) — Ability to approve a whole build chain
* [**TW-88124**](https://youtrack.jetbrains.com/issue/TW-88124) — Kotlin DSL snippets
* [**TW-48885**](https://youtrack.jetbrains.com/issue/TW-48885) — Separate server log capturing server restarts and the most important data
* [**TW-86136**](https://youtrack.jetbrains.com/issue/TW-86136) — Extend branch filter functionality in triggers to support filtering by pull request attributes
* [**TW-90227**](https://youtrack.jetbrains.com/issue/TW-90227) — Improve navigation in TeamCity
* [**TW-79941**](https://youtrack.jetbrains.com/issue/TW-79941) — Kubernetes Connection
* [**TW-86106**](https://youtrack.jetbrains.com/issue/TW-86106) — Service message to call \"Undo personal changes\" (Perforce)
* [**TW-81067**](https://youtrack.jetbrains.com/issue/TW-81067) — Support for migration of existing build artifacts from/to Azure Storage
* [**TW-23238**](https://youtrack.jetbrains.com/issue/TW-23238) — Allow to download all build artifacts right from artifacts popup

### Task

* [**TW-82362**](https://youtrack.jetbrains.com/issue/TW-82362) — Agent terminal should block cloud agent being revoked
* [**TW-84594**](https://youtrack.jetbrains.com/issue/TW-84594) — Allow to start TeamCity Server on java 21
* [**TW-86182**](https://youtrack.jetbrains.com/issue/TW-86182) — \"Choose agent provider\" page
* [**TW-86829**](https://youtrack.jetbrains.com/issue/TW-86829) — Update Linux Base Image: Ubuntu 20.04 -> 22.04 LTS
* [**TW-74035**](https://youtrack.jetbrains.com/issue/TW-74035) — Perforce personal build shelve changelist API should use common buildQueue REST endpoint
* [**TW-76932**](https://youtrack.jetbrains.com/issue/TW-76932) — AWS Core plugin: Refactor to use the new Sakura UI
* [**TW-84743**](https://youtrack.jetbrains.com/issue/TW-84743) — Allow to run agent on java 21
* [**TW-59455**](https://youtrack.jetbrains.com/issue/TW-59455) — Add VCS hosting to usage statistics
* [**TW-89342**](https://youtrack.jetbrains.com/issue/TW-89342) — Upgrade docker version to latest in all images
* [**TW-79896**](https://youtrack.jetbrains.com/issue/TW-79896) — Add new operation mode to execute all the finish build stages after the build itself
* [**TW-78134**](https://youtrack.jetbrains.com/issue/TW-78134) — Add new operation mode to execute all the build stages before the build itself
* [**TW-89039**](https://youtrack.jetbrains.com/issue/TW-89039) — Provide a metric for the number of unfinished settings persist tasks
* [**TW-90284**](https://youtrack.jetbrains.com/issue/TW-90284) — Add an internal property to control Kotlin DSL compilation language version
* [**TW-89966**](https://youtrack.jetbrains.com/issue/TW-89966) — Add webUrl field to CloudImage entity
* [**TW-22179**](https://youtrack.jetbrains.com/issue/TW-22179) — Show warning if artifacts dependency resolving will cause checkout directory cleanup

### Bug

* [**TW-89816**](https://youtrack.jetbrains.com/issue/TW-89816) — Fix build log message \"Container wrapper: prepare reusable container\"
* [**TW-85529**](https://youtrack.jetbrains.com/issue/TW-85529) — Builds replacement log is not persistent and is not shared among secondary nodes
* [**TW-89239**](https://youtrack.jetbrains.com/issue/TW-89239) — NullPointerException while editing build configuration settings
* [**TW-88160**](https://youtrack.jetbrains.com/issue/TW-88160) — Artifacts are not cleaned from S3 when a project is deleted in TeamCity
* [**TW-90258**](https://youtrack.jetbrains.com/issue/TW-90258) — Projects with UTF8 project names are not supported on misconfigured MySQL
* [**TW-85398**](https://youtrack.jetbrains.com/issue/TW-85398) — Predefined parameter teamcity.build.branch is not available in Parallel tests and Matrix buids
* [**TW-90221**](https://youtrack.jetbrains.com/issue/TW-90221) — Checkout rules for files are working despite the note in the checkout rules UI
* [**TW-75511**](https://youtrack.jetbrains.com/issue/TW-75511) — Propagate proxy settings to native git on server-side
* [**TW-83107**](https://youtrack.jetbrains.com/issue/TW-83107) — Build triggering on a wrong branch instead of requested one (Git, multi-node setup)
* [**TW-85769**](https://youtrack.jetbrains.com/issue/TW-85769) — SSH agent build feature fails with NPE with Windows native SSH
* [**TW-87526**](https://youtrack.jetbrains.com/issue/TW-87526) — VCS root with parametrized URL does not work with Refreshable access token
* [**TW-80467**](https://youtrack.jetbrains.com/issue/TW-80467) — Cannot find a node:100479888 may occur when collecting VCS changes on the secondary node
* [**TW-90751**](https://youtrack.jetbrains.com/issue/TW-90751) — Usage statistics collector is activated before usage stats providers were able to load their state from disk
* [**TW-90239**](https://youtrack.jetbrains.com/issue/TW-90239) — All build caches are cleaned when there is at least one invalidated
* [**TW-89641**](https://youtrack.jetbrains.com/issue/TW-89641) — Error while building a Gradle project with lots of test tasks
* [**TW-85058**](https://youtrack.jetbrains.com/issue/TW-85058) — Log \"Gradle failure report\" block as an error
* [**TW-90318**](https://youtrack.jetbrains.com/issue/TW-90318) — lib/jdbc directory needs to be writable
* [**TW-90289**](https://youtrack.jetbrains.com/issue/TW-90289) — VCS Auth Tokens page fails when token's OAuth provider is not available
* [**TW-89539**](https://youtrack.jetbrains.com/issue/TW-89539) — Agent terminal doesn't close automatically after 5 minutes of inactivity until a user clicks OK button in browser alert
* [**TW-80928**](https://youtrack.jetbrains.com/issue/TW-80928) — GitHub App: Acquire new token in Issue Tracker settings doesn't check accessibility of Repository URL
* [**TW-90026**](https://youtrack.jetbrains.com/issue/TW-90026) — Critical error about missing parent project may not be shown or can be hidden behind other errors
* [**TW-89716**](https://youtrack.jetbrains.com/issue/TW-89716) — HashiCorp Vault does not work with Executors
* [**TW-90022**](https://youtrack.jetbrains.com/issue/TW-90022) — Kubernetes Executor: build is failed to start after settings changing
* [**TW-88561**](https://youtrack.jetbrains.com/issue/TW-88561) — Test Connection fails for GitLab Commit Status Publisher for users with a transitive role in the project
* [**TW-88780**](https://youtrack.jetbrains.com/issue/TW-88780) — Provide better information about Matrix Builds in GitHub Checks
* [**TW-86628**](https://youtrack.jetbrains.com/issue/TW-86628) — Server log is spammed with \"Error while processing VCS Trigger\" if branch filter in the trigger settings is invalid
* [**TW-89621**](https://youtrack.jetbrains.com/issue/TW-89621) — Missing authorize button for unauthorized agent
* [**TW-90270**](https://youtrack.jetbrains.com/issue/TW-90270) — VMWare: properties processing error when trying to delete an image
* [**TW-88656**](https://youtrack.jetbrains.com/issue/TW-88656) — Pipeline Misalignment in TeamCity for Merge Requests with Same Commit in GitLab
* [**TW-85702**](https://youtrack.jetbrains.com/issue/TW-85702) — Cannot generate Kotlin DSL with Java 21 locally (using mvn plugin)
* [**TW-56107**](https://youtrack.jetbrains.com/issue/TW-56107) — AccessDeniedException while opening Agents Statistics page if per-project agents filtering is enabled
* [**TW-89121**](https://youtrack.jetbrains.com/issue/TW-89121) — Deprecate duplicating and undocumented teamcity.git. parameters
* [**TW-90424**](https://youtrack.jetbrains.com/issue/TW-90424) — Main navigation redesign: Help -> Share feedback, Feedback page should pe opened in the new browser window
* [**TW-88993**](https://youtrack.jetbrains.com/issue/TW-88993) — ClassNotFoundException: Class 'org.dom4j.DocumentException' was not found
* [**TW-83646**](https://youtrack.jetbrains.com/issue/TW-83646) — Add a property to deny Default Credential Provider Chain for AWS  EC2 but not for S3
* [**TW-90494**](https://youtrack.jetbrains.com/issue/TW-90494) — SynchronizeInstancesOperation stuck in AmazonEC2RequestEventLoop
* [**TW-90011**](https://youtrack.jetbrains.com/issue/TW-90011) — Make sure the thread name contains information about the currently executed request
* [**TW-90179**](https://youtrack.jetbrains.com/issue/TW-90179) — Bad error reporting when accessing non-existing agent
* [**TW-90343**](https://youtrack.jetbrains.com/issue/TW-90343) — BuildTypeNotFoundException is thrown from the DefaultMessageProcessor if build configuration does not exist anymore
* [**TW-90429**](https://youtrack.jetbrains.com/issue/TW-90429) — P shortcut not working in Sakura
* [**TW-90765**](https://youtrack.jetbrains.com/issue/TW-90765) — Long build configuration names are cut on project overview
* [**TW-90503**](https://youtrack.jetbrains.com/issue/TW-90503) — Add an internal property to disable converting artifact dependencies into optional for skipped snapshot dependencies
* [**TW-88367**](https://youtrack.jetbrains.com/issue/TW-88367) — Executor - UI Bug in the connection Display
* [**TW-90496**](https://youtrack.jetbrains.com/issue/TW-90496) — Builds are assigned to node despite read-only state
* [**TW-89653**](https://youtrack.jetbrains.com/issue/TW-89653) — Adapt S3 parameters in Artifact Migration Tool for other storages 
* [**TW-90311**](https://youtrack.jetbrains.com/issue/TW-90311) — [SNS Trigger] Build is not triggered
* [**TW-89852**](https://youtrack.jetbrains.com/issue/TW-89852) — CloudQuotaCheckerImpl throws FailedToStartInstanceException: No agent pool with id: -3
* [**TW-89885**](https://youtrack.jetbrains.com/issue/TW-89885) — BUILD_STARTED webhook event doesn't contain revisions
* [**TW-79913**](https://youtrack.jetbrains.com/issue/TW-79913) — Kubernetes Executor: Confusing build log message \"Inactive build step New build step (Command Line) is skipped\"
* [**TW-90136**](https://youtrack.jetbrains.com/issue/TW-90136) — .old cleaner removes content inside symlinked folders
* [**TW-90040**](https://youtrack.jetbrains.com/issue/TW-90040) — Provide ability to stop triggering in a particular build configuration
* [**TW-89969**](https://youtrack.jetbrains.com/issue/TW-89969) — If 'treat manually started builds as approve' checkbox is checked, then all builds in the build chain should be automatically approved
* [**TW-90037**](https://youtrack.jetbrains.com/issue/TW-90037) — Do not send notifications if only one approval is required from the group
* [**TW-89472**](https://youtrack.jetbrains.com/issue/TW-89472) — [Kubernetes Executor] Build limits feature doesn't take into account its own build
* [**TW-89360**](https://youtrack.jetbrains.com/issue/TW-89360) — Enabling the \"Apply changes in snapshot dependencies and version control settings\" option in Version Settings via the REST API doesn't work
* [**TW-89386**](https://youtrack.jetbrains.com/issue/TW-89386) — <Error class: unknown class> in the DSL documentation
* [**TW-87388**](https://youtrack.jetbrains.com/issue/TW-87388) — No warning during loading of the repos using GitHub App Connection if threshold.time is exceeded
* [**TW-89070**](https://youtrack.jetbrains.com/issue/TW-89070) — S3 Artifact Migration Tool cannot process project when run on Windows
* [**TW-89824**](https://youtrack.jetbrains.com/issue/TW-89824) — Project creation from the GitHub App fails with 404
* [**TW-88483**](https://youtrack.jetbrains.com/issue/TW-88483) — The \"Build Customization\" tab is absent for \"GitHub Checks Webhook Trigger\" 
* [**TW-89699**](https://youtrack.jetbrains.com/issue/TW-89699) — Error calling method BuildServerListener.buildChangedStatus for listener jetbrains.buildServer.pullRequests.impl.space.SpacePullRequestBuildReporter
* [**TW-89369**](https://youtrack.jetbrains.com/issue/TW-89369) — The target source in the migration plan is not updated without relaunching the migration tool
* [**TW-89470**](https://youtrack.jetbrains.com/issue/TW-89470) — Number of Agents instead of Max Number of Agents is shown for the True-Up license on the new page
* [**TW-86541**](https://youtrack.jetbrains.com/issue/TW-86541) — Build dependencies view are not available when there is one dependency with lack of access
* [**TW-88831**](https://youtrack.jetbrains.com/issue/TW-88831) — Some builds might be detected untrusted, because the pull request information is not present on a secondary node
* [**TW-89189**](https://youtrack.jetbrains.com/issue/TW-89189) — New token's project scope is different from the configured one
* [**TW-86376**](https://youtrack.jetbrains.com/issue/TW-86376) — Error when creating project or build configuration from repository URL with trailing slash
* [**TW-87597**](https://youtrack.jetbrains.com/issue/TW-87597) — Improve error messages from artifacts migration tool when Azure environment variables are not defined
* [**TW-89057**](https://youtrack.jetbrains.com/issue/TW-89057) — Lots of DEBUG log messages in teamcity-commit-status.log on a server without Space connection
* [**TW-88999**](https://youtrack.jetbrains.com/issue/TW-88999) — teamcity.https.nonProxyHosts internal property is ignored
* [**TW-88869**](https://youtrack.jetbrains.com/issue/TW-88869) — BB Cloud: 401 (Unauthorized) for CSP and PR build features when project was created with token only
* [**TW-66655**](https://youtrack.jetbrains.com/issue/TW-66655) — A lot of threads blocked in ExternalBuildArtifactsCacheImpl.getCachedStream
* [**TW-89149**](https://youtrack.jetbrains.com/issue/TW-89149) — Do not place parameters with default values to build's URL
* [**TW-87266**](https://youtrack.jetbrains.com/issue/TW-87266) — Bitbucket server network connection errors are displayed as \"unknown error\"
* [**TW-89024**](https://youtrack.jetbrains.com/issue/TW-89024) — OAuth Token of a deleted user is depicted as non-personal
* [**TW-88237**](https://youtrack.jetbrains.com/issue/TW-88237) — Generate Token: popup window stays open on validation error
* [**TW-88256**](https://youtrack.jetbrains.com/issue/TW-88256) — Performance problem on issuing GitHub App installation token

### Performance Problem

* [**TW-85169**](https://youtrack.jetbrains.com/issue/TW-85169) — Unregistered and unauthorized agents can occupy too much memory
* [**TW-89580**](https://youtrack.jetbrains.com/issue/TW-89580) — Many unauthorized agents can slow down fetching of data about default agent pool (and make Agents tab slower too) 
* [**TW-89581**](https://youtrack.jetbrains.com/issue/TW-89581) — High contention on a write lock when a new test name is saved in DB can slow down processing of messages from the builds
* [**TW-87587**](https://youtrack.jetbrains.com/issue/TW-87587) — build hang, error msg: Could not add 85 messages to the build messages queue because the queue is full\" while calling XML-RPC handler
* [**TW-88775**](https://youtrack.jetbrains.com/issue/TW-88775) — [TCI] Each instance of Cloud Profile requires its own Thread to operate
* [**TW-91003**](https://youtrack.jetbrains.com/issue/TW-91003) — Use more efficient API to remove several projects from the favorites
* [**TW-90529**](https://youtrack.jetbrains.com/issue/TW-90529) — PullRequestBranchSpecProvider slows down changes collecting in every VCS root even if pull requests feature is not enabled
* [**TW-90438**](https://youtrack.jetbrains.com/issue/TW-90438) — My investigations page can be slow on a server with many build configurations if there are build problems investigations
* [**TW-90615**](https://youtrack.jetbrains.com/issue/TW-90615) — /app/metrics endpoint is locked and slow when running builds are updated on the server node
* [**TW-90468**](https://youtrack.jetbrains.com/issue/TW-90468) — Builds leak in build promotion manager
* [**TW-90428**](https://youtrack.jetbrains.com/issue/TW-90428) — FailedTestAndBuildProblemsDispatcher from investigations auto assigner plugin can consume too much memory
* [**TW-78360**](https://youtrack.jetbrains.com/issue/TW-78360) — Slow /app/rest/builds query because of Build.lambda$getApprovalInfo
* [**TW-89976**](https://youtrack.jetbrains.com/issue/TW-89976) — Inefficient filtering of queued builds by compatible agent types
* [**TW-89961**](https://youtrack.jetbrains.com/issue/TW-89961) — VCS trigger can be slow in build configurations with many static branches and checkout rules

### Security

13 security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2024.12). Security bulletins for new versions are typically published within the next few days after the release date.