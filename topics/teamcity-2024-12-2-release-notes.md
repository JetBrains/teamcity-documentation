[//]: # (title: TeamCity 2024.12.2 Release Notes)
[//]: # (auxiliary-id: TeamCity 2024.12.2 Release Notes)


**Build 174504, 25 March 2025**

### Task

* [**TW-89512**](https://youtrack.jetbrains.com/issue/TW-89512) — [S3] Parallelise download

### Bug

* [**TW-91564**](https://youtrack.jetbrains.com/issue/TW-91564) — Hashicorp Vault: Not all exceptions are retried on the agent side
* [**TW-91816**](https://youtrack.jetbrains.com/issue/TW-91816) — Kubernetes Executor: default value of image pull policy is pushed as a param to DSL
* [**TW-91766**](https://youtrack.jetbrains.com/issue/TW-91766) — Warnings \"Recieved XML RPC request without 'TeamCity-Rpc-Method' header\" pollute teamcity-server.log
* [**TW-90008**](https://youtrack.jetbrains.com/issue/TW-90008) — Agents are not terminated after changes in Images Settings with the enabled \"Terminate active instances\" option
* [**TW-91776**](https://youtrack.jetbrains.com/issue/TW-91776) —  Malformed input or input contains unmappable characters - VCS git
* [**TW-91621**](https://youtrack.jetbrains.com/issue/TW-91621) — Git via SSH on TC server in 2024.12 assumes that 'nc' has the '-X' option
* [**TW-91831**](https://youtrack.jetbrains.com/issue/TW-91831) — Color of the health item changes depending of the theme
* [**TW-91743**](https://youtrack.jetbrains.com/issue/TW-91743) — Querying builds running on a specific agent returns agent-less builds
* [**TW-91589**](https://youtrack.jetbrains.com/issue/TW-91589) — teamcity.build.chain.onlyTags and teamcity.build.chain.skipTags don't work with %% parameter references
* [**TW-91832**](https://youtrack.jetbrains.com/issue/TW-91832) — Python runner doesn't log into teamcity-build.log
* [**TW-91454**](https://youtrack.jetbrains.com/issue/TW-91454) — Branch name lost when navigating away from build configuration
* [**TW-89989**](https://youtrack.jetbrains.com/issue/TW-89989) — teamcity-server.log may be spammed with \"Error while checking data directory permissions\" warnings
* [**TW-91109**](https://youtrack.jetbrains.com/issue/TW-91109) — PerfMon shows wrong data for broken build
* [**TW-83762**](https://youtrack.jetbrains.com/issue/TW-83762) — No way to delete a project if its parent uses versioned settings and is read-only
* [**TW-90695**](https://youtrack.jetbrains.com/issue/TW-90695) — Trigger branch filter: remove build error for Finish Trigger
* [**TW-90703**](https://youtrack.jetbrains.com/issue/TW-90703) — Kubernetes Executor: pod cannot be started when - is the first symbol in its name
* [**TW-91714**](https://youtrack.jetbrains.com/issue/TW-91714) — Alias to get Perforce VCS root's sync changelist prevents to run a build configuration
* [**TW-89297**](https://youtrack.jetbrains.com/issue/TW-89297) — Slack Notifier Incorrectly Flags domains
* [**TW-91688**](https://youtrack.jetbrains.com/issue/TW-91688) — Token management: token's scope can be silently expanded during the copying of the build configuration
* [**TW-89524**](https://youtrack.jetbrains.com/issue/TW-89524) — Do not keep the scroll position when user is clicking to different projecs
* [**TW-91684**](https://youtrack.jetbrains.com/issue/TW-91684) — Token management: a message about token scope expansion can be wrongly shown if the project with the token was copied outside the connection scope
* [**TW-91764**](https://youtrack.jetbrains.com/issue/TW-91764) — UI error: Cannot find module './investigation-paused.svg'
* [**TW-91063**](https://youtrack.jetbrains.com/issue/TW-91063) — Kubernetes Connection: Consider adding the information about some parameters to the Connections table
* [**TW-91643**](https://youtrack.jetbrains.com/issue/TW-91643) — Kubernetes executor: build log mentions Swabra, even though build configuration does not have this build feature
* [**TW-89601**](https://youtrack.jetbrains.com/issue/TW-89601) — Intermittent job cancellation due to XmlRpcClientException
* [**TW-91702**](https://youtrack.jetbrains.com/issue/TW-91702) — Wrong mapping refs/remotes/origin/HEAD to remote ref
* [**TW-90598**](https://youtrack.jetbrains.com/issue/TW-90598) — Build hangs and cannot deliver messages to agent
* [**TW-45624**](https://youtrack.jetbrains.com/issue/TW-45624) — Error processing XML RPC requests on the server should be reported back to agent
* [**TW-91626**](https://youtrack.jetbrains.com/issue/TW-91626) — TeamCity keeps old p4_commit temporary directories after commit of VCS settings
* [**TW-90922**](https://youtrack.jetbrains.com/issue/TW-90922) — Kubernetes Connection: filled fields for certificate, key and CA are collapsed on edition
* [**TW-91189**](https://youtrack.jetbrains.com/issue/TW-91189) — Token management: Permissions and Accessible Entities fields are quickly shown in the \"Generate new token\" dialog even if they shouldn't

### Security

-1 security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2024.12.2). Security bulletins for new versions are typically published within the next few days after the release date.