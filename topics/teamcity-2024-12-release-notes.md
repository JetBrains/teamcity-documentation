[//]: # (title: TeamCity 2024.12 Release Notes)
[//]: # (auxiliary-id: TeamCity 2024.12 Release Notes)


**Build 174331, 5 December 2024**

### Feature

* [**Skip dependencies in the chain while a build chain is already running**](https://youtrack.jetbrains.com/issue/Skip dependencies in the chain while a build chain is already running) — TW-89624
* [**If TeamCity VCSRoot syncs a label - There's no parameter that states the Changelist synced**](https://youtrack.jetbrains.com/issue/If TeamCity VCSRoot syncs a label - There's no parameter that states the Changelist synced) — TW-88446
* [**Execute a dependency basing on condition**](https://youtrack.jetbrains.com/issue/Execute a dependency basing on condition) — TW-65341
* [**Build flow in the executor mode**](https://youtrack.jetbrains.com/issue/Build flow in the executor mode) — TW-82501
* [**Support for using AWS connection to configure AWS EC2 cloud agents**](https://youtrack.jetbrains.com/issue/Support for using AWS connection to configure AWS EC2 cloud agents) — TW-82378
* [**Run meta-runner in container**](https://youtrack.jetbrains.com/issue/Run meta-runner in container) — TW-89122
* [**Simple Token Management in Admin UI**](https://youtrack.jetbrains.com/issue/Simple Token Management in Admin UI) — TW-87479
* [**Tokens issued via GitHub App Connection must be restricted to multiple relevant repositories**](https://youtrack.jetbrains.com/issue/Tokens issued via GitHub App Connection must be restricted to multiple relevant repositories) — TW-78593
* [**Ability to approve a whole build chain**](https://youtrack.jetbrains.com/issue/Ability to approve a whole build chain) — TW-78586
* [**Kotlin DSL snippets**](https://youtrack.jetbrains.com/issue/Kotlin DSL snippets) — TW-88124
* [**Separate server log capturing server restarts and the most important data**](https://youtrack.jetbrains.com/issue/Separate server log capturing server restarts and the most important data) — TW-48885
* [**Extend branch filter functionality in triggers to support filtering by pull request attributes**](https://youtrack.jetbrains.com/issue/Extend branch filter functionality in triggers to support filtering by pull request attributes) — TW-86136
* [**Improve navigation in TeamCity**](https://youtrack.jetbrains.com/issue/Improve navigation in TeamCity) — TW-90227
* [**Kubernetes Connection**](https://youtrack.jetbrains.com/issue/Kubernetes Connection) — TW-79941
* [**Service message to call \"Undo personal changes\" (Perforce)**](https://youtrack.jetbrains.com/issue/Service message to call \"Undo personal changes\" (Perforce)) — TW-86106
* [**Support for migration of existing build artifacts from/to Azure Storage**](https://youtrack.jetbrains.com/issue/Support for migration of existing build artifacts from/to Azure Storage) — TW-81067
* [**Allow to download all build artifacts right from artifacts popup**](https://youtrack.jetbrains.com/issue/Allow to download all build artifacts right from artifacts popup) — TW-23238

### Task

* [**Agent terminal should block cloud agent being revoked**](https://youtrack.jetbrains.com/issue/Agent terminal should block cloud agent being revoked) — TW-82362
* [**Allow to start TeamCity Server on java 21**](https://youtrack.jetbrains.com/issue/Allow to start TeamCity Server on java 21) — TW-84594
* [**\"Choose agent provider\" page**](https://youtrack.jetbrains.com/issue/\"Choose agent provider\" page) — TW-86182
* [**Update Linux Base Image: Ubuntu 20.04 -> 22.04 LTS**](https://youtrack.jetbrains.com/issue/Update Linux Base Image: Ubuntu 20.04 -> 22.04 LTS) — TW-86829
* [**Perforce personal build shelve changelist API should use common buildQueue REST endpoint**](https://youtrack.jetbrains.com/issue/Perforce personal build shelve changelist API should use common buildQueue REST endpoint) — TW-74035
* [**AWS Core plugin: Refactor to use the new Sakura UI**](https://youtrack.jetbrains.com/issue/AWS Core plugin: Refactor to use the new Sakura UI) — TW-76932
* [**Allow to run agent on java 21**](https://youtrack.jetbrains.com/issue/Allow to run agent on java 21) — TW-84743
* [**Add VCS hosting to usage statistics**](https://youtrack.jetbrains.com/issue/Add VCS hosting to usage statistics) — TW-59455
* [**Upgrade docker version to latest in all images**](https://youtrack.jetbrains.com/issue/Upgrade docker version to latest in all images) — TW-89342
* [**Add new operation mode to execute all the finish build stages after the build itself**](https://youtrack.jetbrains.com/issue/Add new operation mode to execute all the finish build stages after the build itself) — TW-79896
* [**Add new operation mode to execute all the build stages before the build itself**](https://youtrack.jetbrains.com/issue/Add new operation mode to execute all the build stages before the build itself) — TW-78134
* [**Provide a metric for the number of unfinished settings persist tasks**](https://youtrack.jetbrains.com/issue/Provide a metric for the number of unfinished settings persist tasks) — TW-89039
* [**Add an internal property to control Kotlin DSL compilation language version**](https://youtrack.jetbrains.com/issue/Add an internal property to control Kotlin DSL compilation language version) — TW-90284
* [**Add webUrl field to CloudImage entity**](https://youtrack.jetbrains.com/issue/Add webUrl field to CloudImage entity) — TW-89966
* [**Show warning if artifacts dependency resolving will cause checkout directory cleanup**](https://youtrack.jetbrains.com/issue/Show warning if artifacts dependency resolving will cause checkout directory cleanup) — TW-22179

### Bug

* [**Fix build log message \"Container wrapper: prepare reusable container\"**](https://youtrack.jetbrains.com/issue/Fix build log message \"Container wrapper: prepare reusable container\") — TW-89816
* [**Builds replacement log is not persistent and is not shared among secondary nodes**](https://youtrack.jetbrains.com/issue/Builds replacement log is not persistent and is not shared among secondary nodes) — TW-85529
* [**NullPointerException while editing build configuration settings**](https://youtrack.jetbrains.com/issue/NullPointerException while editing build configuration settings) — TW-89239
* [**Artifacts are not cleaned from S3 when a project is deleted in TeamCity**](https://youtrack.jetbrains.com/issue/Artifacts are not cleaned from S3 when a project is deleted in TeamCity) — TW-88160
* [**Projects with UTF8 project names are not supported on misconfigured MySQL**](https://youtrack.jetbrains.com/issue/Projects with UTF8 project names are not supported on misconfigured MySQL) — TW-90258
* [**Predefined parameter teamcity.build.branch is not available in Parallel tests and Matrix buids**](https://youtrack.jetbrains.com/issue/Predefined parameter teamcity.build.branch is not available in Parallel tests and Matrix buids) — TW-85398
* [**Checkout rules for files are working despite the note in the checkout rules UI**](https://youtrack.jetbrains.com/issue/Checkout rules for files are working despite the note in the checkout rules UI) — TW-90221
* [**Propagate proxy settings to native git on server-side**](https://youtrack.jetbrains.com/issue/Propagate proxy settings to native git on server-side) — TW-75511
* [**Build triggering on a wrong branch instead of requested one (Git, multi-node setup)**](https://youtrack.jetbrains.com/issue/Build triggering on a wrong branch instead of requested one (Git, multi-node setup)) — TW-83107
* [**SSH agent build feature fails with NPE with Windows native SSH**](https://youtrack.jetbrains.com/issue/SSH agent build feature fails with NPE with Windows native SSH) — TW-85769
* [**VCS root with parametrized URL does not work with Refreshable access token**](https://youtrack.jetbrains.com/issue/VCS root with parametrized URL does not work with Refreshable access token) — TW-87526
* [**Cannot find a node:100479888 may occur when collecting VCS changes on the secondary node**](https://youtrack.jetbrains.com/issue/Cannot find a node:100479888 may occur when collecting VCS changes on the secondary node) — TW-80467
* [**Usage statistics collector is activated before usage stats providers were able to load their state from disk**](https://youtrack.jetbrains.com/issue/Usage statistics collector is activated before usage stats providers were able to load their state from disk) — TW-90751
* [**All build caches are cleaned when there is at least one invalidated**](https://youtrack.jetbrains.com/issue/All build caches are cleaned when there is at least one invalidated) — TW-90239
* [**Error while building a Gradle project with lots of test tasks**](https://youtrack.jetbrains.com/issue/Error while building a Gradle project with lots of test tasks) — TW-89641
* [**Log \"Gradle failure report\" block as an error**](https://youtrack.jetbrains.com/issue/Log \"Gradle failure report\" block as an error) — TW-85058
* [**lib/jdbc directory needs to be writable**](https://youtrack.jetbrains.com/issue/lib/jdbc directory needs to be writable) — TW-90318
* [**VCS Auth Tokens page fails when token's OAuth provider is not available**](https://youtrack.jetbrains.com/issue/VCS Auth Tokens page fails when token's OAuth provider is not available) — TW-90289
* [**Agent terminal doesn't close automatically after 5 minutes of inactivity until a user clicks OK button in browser alert**](https://youtrack.jetbrains.com/issue/Agent terminal doesn't close automatically after 5 minutes of inactivity until a user clicks OK button in browser alert) — TW-89539
* [**GitHub App: Acquire new token in Issue Tracker settings doesn't check accessibility of Repository URL**](https://youtrack.jetbrains.com/issue/GitHub App: Acquire new token in Issue Tracker settings doesn't check accessibility of Repository URL) — TW-80928
* [**Critical error about missing parent project may not be shown or can be hidden behind other errors**](https://youtrack.jetbrains.com/issue/Critical error about missing parent project may not be shown or can be hidden behind other errors) — TW-90026
* [**HashiCorp Vault does not work with Executors**](https://youtrack.jetbrains.com/issue/HashiCorp Vault does not work with Executors) — TW-89716
* [**Kubernetes Executor: build is failed to start after settings changing**](https://youtrack.jetbrains.com/issue/Kubernetes Executor: build is failed to start after settings changing) — TW-90022
* [**Test Connection fails for GitLab Commit Status Publisher for users with a transitive role in the project**](https://youtrack.jetbrains.com/issue/Test Connection fails for GitLab Commit Status Publisher for users with a transitive role in the project) — TW-88561
* [**Provide better information about Matrix Builds in GitHub Checks**](https://youtrack.jetbrains.com/issue/Provide better information about Matrix Builds in GitHub Checks) — TW-88780
* [**Server log is spammed with \"Error while processing VCS Trigger\" if branch filter in the trigger settings is invalid**](https://youtrack.jetbrains.com/issue/Server log is spammed with \"Error while processing VCS Trigger\" if branch filter in the trigger settings is invalid) — TW-86628
* [**Missing authorize button for unauthorized agent**](https://youtrack.jetbrains.com/issue/Missing authorize button for unauthorized agent) — TW-89621
* [**VMWare: properties processing error when trying to delete an image**](https://youtrack.jetbrains.com/issue/VMWare: properties processing error when trying to delete an image) — TW-90270
* [**Pipeline Misalignment in TeamCity for Merge Requests with Same Commit in GitLab**](https://youtrack.jetbrains.com/issue/Pipeline Misalignment in TeamCity for Merge Requests with Same Commit in GitLab) — TW-88656
* [**Cannot generate Kotlin DSL with Java 21 locally (using mvn plugin)**](https://youtrack.jetbrains.com/issue/Cannot generate Kotlin DSL with Java 21 locally (using mvn plugin)) — TW-85702
* [**AccessDeniedException while opening Agents Statistics page if per-project agents filtering is enabled**](https://youtrack.jetbrains.com/issue/AccessDeniedException while opening Agents Statistics page if per-project agents filtering is enabled) — TW-56107
* [**Deprecate duplicating and undocumented teamcity.git. parameters**](https://youtrack.jetbrains.com/issue/Deprecate duplicating and undocumented teamcity.git. parameters) — TW-89121
* [**Main navigation redesign: Help -> Share feedback, Feedback page should pe opened in the new browser window**](https://youtrack.jetbrains.com/issue/Main navigation redesign: Help -> Share feedback, Feedback page should pe opened in the new browser window) — TW-90424
* [**ClassNotFoundException: Class 'org.dom4j.DocumentException' was not found**](https://youtrack.jetbrains.com/issue/ClassNotFoundException: Class 'org.dom4j.DocumentException' was not found) — TW-88993
* [**Add a property to deny Default Credential Provider Chain for AWS  EC2 but not for S3**](https://youtrack.jetbrains.com/issue/Add a property to deny Default Credential Provider Chain for AWS  EC2 but not for S3) — TW-83646
* [**SynchronizeInstancesOperation stuck in AmazonEC2RequestEventLoop**](https://youtrack.jetbrains.com/issue/SynchronizeInstancesOperation stuck in AmazonEC2RequestEventLoop) — TW-90494
* [**Make sure the thread name contains information about the currently executed request**](https://youtrack.jetbrains.com/issue/Make sure the thread name contains information about the currently executed request) — TW-90011
* [**Bad error reporting when accessing non-existing agent**](https://youtrack.jetbrains.com/issue/Bad error reporting when accessing non-existing agent) — TW-90179
* [**BuildTypeNotFoundException is thrown from the DefaultMessageProcessor if build configuration does not exist anymore**](https://youtrack.jetbrains.com/issue/BuildTypeNotFoundException is thrown from the DefaultMessageProcessor if build configuration does not exist anymore) — TW-90343
* [**P shortcut not working in Sakura**](https://youtrack.jetbrains.com/issue/P shortcut not working in Sakura) — TW-90429
* [**Long build configuration names are cut on project overview**](https://youtrack.jetbrains.com/issue/Long build configuration names are cut on project overview) — TW-90765
* [**Add an internal property to disable converting artifact dependencies into optional for skipped snapshot dependencies**](https://youtrack.jetbrains.com/issue/Add an internal property to disable converting artifact dependencies into optional for skipped snapshot dependencies) — TW-90503
* [**Executor - UI Bug in the connection Display**](https://youtrack.jetbrains.com/issue/Executor - UI Bug in the connection Display) — TW-88367
* [**Builds are assigned to node despite read-only state**](https://youtrack.jetbrains.com/issue/Builds are assigned to node despite read-only state) — TW-90496
* [**Adapt S3 parameters in Artifact Migration Tool for other storages **](https://youtrack.jetbrains.com/issue/Adapt S3 parameters in Artifact Migration Tool for other storages ) — TW-89653
* [**[SNS Trigger] Build is not triggered**](https://youtrack.jetbrains.com/issue/[SNS Trigger] Build is not triggered) — TW-90311
* [**CloudQuotaCheckerImpl throws FailedToStartInstanceException: No agent pool with id: -3**](https://youtrack.jetbrains.com/issue/CloudQuotaCheckerImpl throws FailedToStartInstanceException: No agent pool with id: -3) — TW-89852
* [**BUILD_STARTED webhook event doesn't contain revisions**](https://youtrack.jetbrains.com/issue/BUILD_STARTED webhook event doesn't contain revisions) — TW-89885
* [**Kubernetes Executor: Confusing build log message \"Inactive build step New build step (Command Line) is skipped\"**](https://youtrack.jetbrains.com/issue/Kubernetes Executor: Confusing build log message \"Inactive build step New build step (Command Line) is skipped\") — TW-79913
* [**.old cleaner removes content inside symlinked folders**](https://youtrack.jetbrains.com/issue/.old cleaner removes content inside symlinked folders) — TW-90136
* [**Provide ability to stop triggering in a particular build configuration**](https://youtrack.jetbrains.com/issue/Provide ability to stop triggering in a particular build configuration) — TW-90040
* [**If 'treat manually started builds as approve' checkbox is checked, then all builds in the build chain should be automatically approved**](https://youtrack.jetbrains.com/issue/If 'treat manually started builds as approve' checkbox is checked, then all builds in the build chain should be automatically approved) — TW-89969
* [**Do not send notifications if only one approval is required from the group**](https://youtrack.jetbrains.com/issue/Do not send notifications if only one approval is required from the group) — TW-90037
* [**[Kubernetes Executor] Build limits feature doesn't take into account its own build**](https://youtrack.jetbrains.com/issue/[Kubernetes Executor] Build limits feature doesn't take into account its own build) — TW-89472
* [**Enabling the \"Apply changes in snapshot dependencies and version control settings\" option in Version Settings via the REST API doesn't work**](https://youtrack.jetbrains.com/issue/Enabling the \"Apply changes in snapshot dependencies and version control settings\" option in Version Settings via the REST API doesn't work) — TW-89360
* [**<Error class: unknown class> in the DSL documentation**](https://youtrack.jetbrains.com/issue/<Error class: unknown class> in the DSL documentation) — TW-89386
* [**No warning during loading of the repos using GitHub App Connection if threshold.time is exceeded**](https://youtrack.jetbrains.com/issue/No warning during loading of the repos using GitHub App Connection if threshold.time is exceeded) — TW-87388
* [**S3 Artifact Migration Tool cannot process project when run on Windows**](https://youtrack.jetbrains.com/issue/S3 Artifact Migration Tool cannot process project when run on Windows) — TW-89070
* [**Project creation from the GitHub App fails with 404**](https://youtrack.jetbrains.com/issue/Project creation from the GitHub App fails with 404) — TW-89824
* [**The \"Build Customization\" tab is absent for \"GitHub Checks Webhook Trigger\" **](https://youtrack.jetbrains.com/issue/The \"Build Customization\" tab is absent for \"GitHub Checks Webhook Trigger\" ) — TW-88483
* [**Error calling method BuildServerListener.buildChangedStatus for listener jetbrains.buildServer.pullRequests.impl.space.SpacePullRequestBuildReporter**](https://youtrack.jetbrains.com/issue/Error calling method BuildServerListener.buildChangedStatus for listener jetbrains.buildServer.pullRequests.impl.space.SpacePullRequestBuildReporter) — TW-89699
* [**The target source in the migration plan is not updated without relaunching the migration tool**](https://youtrack.jetbrains.com/issue/The target source in the migration plan is not updated without relaunching the migration tool) — TW-89369
* [**Number of Agents instead of Max Number of Agents is shown for the True-Up license on the new page**](https://youtrack.jetbrains.com/issue/Number of Agents instead of Max Number of Agents is shown for the True-Up license on the new page) — TW-89470
* [**Build dependencies view are not available when there is one dependency with lack of access**](https://youtrack.jetbrains.com/issue/Build dependencies view are not available when there is one dependency with lack of access) — TW-86541
* [**Some builds might be detected untrusted, because the pull request information is not present on a secondary node**](https://youtrack.jetbrains.com/issue/Some builds might be detected untrusted, because the pull request information is not present on a secondary node) — TW-88831
* [**New token's project scope is different from the configured one**](https://youtrack.jetbrains.com/issue/New token's project scope is different from the configured one) — TW-89189
* [**Error when creating project or build configuration from repository URL with trailing slash**](https://youtrack.jetbrains.com/issue/Error when creating project or build configuration from repository URL with trailing slash) — TW-86376
* [**Improve error messages from artifacts migration tool when Azure environment variables are not defined**](https://youtrack.jetbrains.com/issue/Improve error messages from artifacts migration tool when Azure environment variables are not defined) — TW-87597
* [**Lots of DEBUG log messages in teamcity-commit-status.log on a server without Space connection**](https://youtrack.jetbrains.com/issue/Lots of DEBUG log messages in teamcity-commit-status.log on a server without Space connection) — TW-89057
* [**teamcity.https.nonProxyHosts internal property is ignored**](https://youtrack.jetbrains.com/issue/teamcity.https.nonProxyHosts internal property is ignored) — TW-88999
* [**BB Cloud: 401 (Unauthorized) for CSP and PR build features when project was created with token only**](https://youtrack.jetbrains.com/issue/BB Cloud: 401 (Unauthorized) for CSP and PR build features when project was created with token only) — TW-88869
* [**A lot of threads blocked in ExternalBuildArtifactsCacheImpl.getCachedStream**](https://youtrack.jetbrains.com/issue/A lot of threads blocked in ExternalBuildArtifactsCacheImpl.getCachedStream) — TW-66655
* [**Do not place parameters with default values to build's URL**](https://youtrack.jetbrains.com/issue/Do not place parameters with default values to build's URL) — TW-89149
* [**Bitbucket server network connection errors are displayed as \"unknown error\"**](https://youtrack.jetbrains.com/issue/Bitbucket server network connection errors are displayed as \"unknown error\") — TW-87266
* [**OAuth Token of a deleted user is depicted as non-personal**](https://youtrack.jetbrains.com/issue/OAuth Token of a deleted user is depicted as non-personal) — TW-89024
* [**Generate Token: popup window stays open on validation error**](https://youtrack.jetbrains.com/issue/Generate Token: popup window stays open on validation error) — TW-88237
* [**Performance problem on issuing GitHub App installation token**](https://youtrack.jetbrains.com/issue/Performance problem on issuing GitHub App installation token) — TW-88256

### Performance Problem

* [**Unregistered and unauthorized agents can occupy too much memory**](https://youtrack.jetbrains.com/issue/Unregistered and unauthorized agents can occupy too much memory) — TW-85169
* [**Many unauthorized agents can slow down fetching of data about default agent pool (and make Agents tab slower too) **](https://youtrack.jetbrains.com/issue/Many unauthorized agents can slow down fetching of data about default agent pool (and make Agents tab slower too) ) — TW-89580
* [**High contention on a write lock when a new test name is saved in DB can slow down processing of messages from the builds**](https://youtrack.jetbrains.com/issue/High contention on a write lock when a new test name is saved in DB can slow down processing of messages from the builds) — TW-89581
* [**build hang, error msg: Could not add 85 messages to the build messages queue because the queue is full\" while calling XML-RPC handler**](https://youtrack.jetbrains.com/issue/build hang, error msg: Could not add 85 messages to the build messages queue because the queue is full\" while calling XML-RPC handler) — TW-87587
* [**[TCI] Each instance of Cloud Profile requires its own Thread to operate**](https://youtrack.jetbrains.com/issue/[TCI] Each instance of Cloud Profile requires its own Thread to operate) — TW-88775
* [**Use more efficient API to remove several projects from the favorites**](https://youtrack.jetbrains.com/issue/Use more efficient API to remove several projects from the favorites) — TW-91003
* [**PullRequestBranchSpecProvider slows down changes collecting in every VCS root even if pull requests feature is not enabled**](https://youtrack.jetbrains.com/issue/PullRequestBranchSpecProvider slows down changes collecting in every VCS root even if pull requests feature is not enabled) — TW-90529
* [**My investigations page can be slow on a server with many build configurations if there are build problems investigations**](https://youtrack.jetbrains.com/issue/My investigations page can be slow on a server with many build configurations if there are build problems investigations) — TW-90438
* [**/app/metrics endpoint is locked and slow when running builds are updated on the server node**](https://youtrack.jetbrains.com/issue//app/metrics endpoint is locked and slow when running builds are updated on the server node) — TW-90615
* [**Builds leak in build promotion manager**](https://youtrack.jetbrains.com/issue/Builds leak in build promotion manager) — TW-90468
* [**FailedTestAndBuildProblemsDispatcher from investigations auto assigner plugin can consume too much memory**](https://youtrack.jetbrains.com/issue/FailedTestAndBuildProblemsDispatcher from investigations auto assigner plugin can consume too much memory) — TW-90428
* [**Slow /app/rest/builds query because of Build.lambda$getApprovalInfo**](https://youtrack.jetbrains.com/issue/Slow /app/rest/builds query because of Build.lambda$getApprovalInfo) — TW-78360
* [**Inefficient filtering of queued builds by compatible agent types**](https://youtrack.jetbrains.com/issue/Inefficient filtering of queued builds by compatible agent types) — TW-89976
* [**VCS trigger can be slow in build configurations with many static branches and checkout rules**](https://youtrack.jetbrains.com/issue/VCS trigger can be slow in build configurations with many static branches and checkout rules) — TW-89961

### Security

13 security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2024.12). Security bulletins for new versions are typically published within the next few days after the release date.