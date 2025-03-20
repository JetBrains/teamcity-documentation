[//]: # (title: TeamCity 2025.03 Release Notes)
[//]: # (auxiliary-id: TeamCity 2025.03 Release Notes)


**Build 186049, 87 Movember 2077**

### Feature

* [**TW-79442**](https://youtrack.jetbrains.com/issue/TW-79442) — Perforce support in the Automatic Merge feature
* [**TW-90639**](https://youtrack.jetbrains.com/issue/TW-90639) — Improve navigation in TeamCity
* [**TW-89046**](https://youtrack.jetbrains.com/issue/TW-89046) — Edit mode for TeamCity project and build configuration administration area
* [**TW-89008**](https://youtrack.jetbrains.com/issue/TW-89008) — Redesign the page header panel for Build, Build Configuration and Project pages
* [**TW-91941**](https://youtrack.jetbrains.com/issue/TW-91941) — [Recipes] Using build steps from JetBrains Marketplace directly in the TeamCity interface
* [**TW-59840**](https://youtrack.jetbrains.com/issue/TW-59840) — Run build configuration in Docker container
* [**TW-91908**](https://youtrack.jetbrains.com/issue/TW-91908) — Add a health report for disconnected agents
* [**TW-87125**](https://youtrack.jetbrains.com/issue/TW-87125) — Add ability to control dep. parameters propagation along the build chain (Output parameters)
* [**TW-91697**](https://youtrack.jetbrains.com/issue/TW-91697) — Support Docker Wrapper for Kotlin Script runner

### Task

* [**TW-89640**](https://youtrack.jetbrains.com/issue/TW-89640) — Trigger only Pull Request builds by GitHub Checks Webhook trigger
* [**TW-91283**](https://youtrack.jetbrains.com/issue/TW-91283) — Update .NET in docker agent images
* [**TW-92074**](https://youtrack.jetbrains.com/issue/TW-92074) — Update bundled Kotlin tool to 2.1.10
* [**TW-64626**](https://youtrack.jetbrains.com/issue/TW-64626) — Stop supporting alternate credentials in Team Foundation Work Items integration
* [**TW-91541**](https://youtrack.jetbrains.com/issue/TW-91541) — Store VCS repository state in dedicated tables
* [**TW-91150**](https://youtrack.jetbrains.com/issue/TW-91150) — Gradle maxRetries from the Enterprise plugin is deprecated and will be removed in 9.0

### Bug

* [**TW-87691**](https://youtrack.jetbrains.com/issue/TW-87691) — EC2 Cloud Profile: TC only attempts to use one instance type from the list configured in the Spot-based Image settings 
* [**TW-92182**](https://youtrack.jetbrains.com/issue/TW-92182) — Artifact dependency change details cannot be displayed with eternal \"Loading...\"
* [**TW-68571**](https://youtrack.jetbrains.com/issue/TW-68571) — Add validation of Server URL field for GitLab CE/EE, GitHub Enterprise, Space connections
* [**TW-92499**](https://youtrack.jetbrains.com/issue/TW-92499) — Agentless Kubernetes Executor: Parameters available in executor not working for auto-generated build configs (f.e. Matrix Build Feature) 
* [**TW-86907**](https://youtrack.jetbrains.com/issue/TW-86907) — EC2 userdata script does not run for Windows AMI
* [**TW-92364**](https://youtrack.jetbrains.com/issue/TW-92364) — Confusing \"cannot find previous revision\" message after settings changes commit failure
* [**TW-91499**](https://youtrack.jetbrains.com/issue/TW-91499) — Git changes in 2.48.x may lead to changing existing TeamCity functionality 
* [**TW-87369**](https://youtrack.jetbrains.com/issue/TW-87369) — \"Promote the watched build\" in Schedule trigger can work incorrectly if artifact dependency has a different rule
* [**TW-90061**](https://youtrack.jetbrains.com/issue/TW-90061) — Unhandled error during Test Connect in Pull Request Build Feature when selected VCS hosting type does not match VCS root hosting type
* [**TW-92424**](https://youtrack.jetbrains.com/issue/TW-92424) — Messages related to artifacts publishing do not appear under the curresponding block
* [**TW-86451**](https://youtrack.jetbrains.com/issue/TW-86451) — Shared resource with distinct custom values sometimes doles out the same value to two same time running build configs
* [**TW-92466**](https://youtrack.jetbrains.com/issue/TW-92466) — Public Recipes: add support of the AWS_DEFAULT_REGION variable in aws-s3-copy recipe
* [**TW-91685**](https://youtrack.jetbrains.com/issue/TW-91685) — Token management: improve display of token scope in the VCS Auth Tokens table
* [**TW-76564**](https://youtrack.jetbrains.com/issue/TW-76564) — Unclear error if profile id exceeded maximum length
* [**TW-92498**](https://youtrack.jetbrains.com/issue/TW-92498) — BuildAgent may hang on a faulty artifact publishing command
* [**TW-92278**](https://youtrack.jetbrains.com/issue/TW-92278) — FormSelect element causes an empty checkbox to show up
* [**TW-79922**](https://youtrack.jetbrains.com/issue/TW-79922) — Kubernetes Executor: execution timeout applies to each build step separately
* [**TW-92400**](https://youtrack.jetbrains.com/issue/TW-92400) — Mistakes in HAProxy multinode configuration example
* [**TW-92199**](https://youtrack.jetbrains.com/issue/TW-92199) — Container Deployer runner is broken since 2024.12.1
* [**TW-92350**](https://youtrack.jetbrains.com/issue/TW-92350) — Tooltips position on buttons
* [**TW-91928**](https://youtrack.jetbrains.com/issue/TW-91928) — Build steps configured to execute always are terminated if build was stopped due to execution timeout
* [**TW-90921**](https://youtrack.jetbrains.com/issue/TW-90921) — dotCover doesn't work when building in Docker on Windows with different host and container drives
* [**TW-92226**](https://youtrack.jetbrains.com/issue/TW-92226) — New Edit Mode: lags after scrolling on Cloud Profiles page 
* [**TW-92052**](https://youtrack.jetbrains.com/issue/TW-92052) — Pause is visible only after page reload
* [**TW-91861**](https://youtrack.jetbrains.com/issue/TW-91861) — Inaccurate metric value - build_queue_incompatible
* [**TW-92303**](https://youtrack.jetbrains.com/issue/TW-92303) — Signal Not Propagated to Final Process on Build Cancellation
* [**TW-90937**](https://youtrack.jetbrains.com/issue/TW-90937) — When a build is removed from TeamCity, the audit details are also removed
* [**TW-91627**](https://youtrack.jetbrains.com/issue/TW-91627) — Unnecessary pending changes if \"show settings changes\" is enabled in versioned settings configuration
* [**TW-90738**](https://youtrack.jetbrains.com/issue/TW-90738) — Error in a single cloud AWS image blocks the whole cloud profile
* [**TW-91033**](https://youtrack.jetbrains.com/issue/TW-91033) — Confusing UX in build failure condition based on metric change
* [**TW-91148**](https://youtrack.jetbrains.com/issue/TW-91148) — Kubernetes Executor: K8s connection and pod template are lost after the project copying
* [**TW-65534**](https://youtrack.jetbrains.com/issue/TW-65534) — Agent fails with out of memory error on persisting many configuration parameters
* [**TW-92172**](https://youtrack.jetbrains.com/issue/TW-92172) — Composite build canceled on secondary node may start on main one (race condition)
* [**TW-92038**](https://youtrack.jetbrains.com/issue/TW-92038) — Action button icon changes when build has custom parameters
* [**TW-92035**](https://youtrack.jetbrains.com/issue/TW-92035) — Get token endpoint on HashiCorp Vault fails on secondary nodes
* [**TW-87135**](https://youtrack.jetbrains.com/issue/TW-87135) — Cloud Image changes trigger agents' termination
* [**TW-91598**](https://youtrack.jetbrains.com/issue/TW-91598) — Settings toggle jumps when user navigates within the settings options
* [**TW-91463**](https://youtrack.jetbrains.com/issue/TW-91463) — Token Management: Add a way to see the whole long project name in the \"Generate new token\" dialog
* [**TW-91524**](https://youtrack.jetbrains.com/issue/TW-91524) — No detection for .NET Framework ARM64 on Windows ARM64 computers
* [**TW-90029**](https://youtrack.jetbrains.com/issue/TW-90029) — After login using Azure Active Directory, UI falls back to classic UI
* [**TW-85282**](https://youtrack.jetbrains.com/issue/TW-85282) — Cloud profile errors are hardly visible
* [**TW-91547**](https://youtrack.jetbrains.com/issue/TW-91547) — Unfriendly error on attempt to create a repository from URL without providing credentials
* [**TW-64629**](https://youtrack.jetbrains.com/issue/TW-64629) — Stop supporting alternate credential in TFS plugin
* [**TW-91297**](https://youtrack.jetbrains.com/issue/TW-91297) — JSON credentials tokens may be moved into child projects on creating project copy
* [**TW-91519**](https://youtrack.jetbrains.com/issue/TW-91519) — [AWS Core] Permissions check returns error
* [**TW-91493**](https://youtrack.jetbrains.com/issue/TW-91493) — Experimental Overview usage statistics are reported incorrectly when Sakura is enabled by default globally
* [**TW-90934**](https://youtrack.jetbrains.com/issue/TW-90934) — Token Management: Add the audit record about the generation/deletion of the new VCS token
* [**TW-91286**](https://youtrack.jetbrains.com/issue/TW-91286) — Add tags to run types from .NET plugin to improve their discoverability
* [**TW-91280**](https://youtrack.jetbrains.com/issue/TW-91280) — Add tags to SimpleRunner to improve its discoverability
* [**TW-78963**](https://youtrack.jetbrains.com/issue/TW-78963) — Illegal argument exception in teamcity-server.log
* [**TW-88045**](https://youtrack.jetbrains.com/issue/TW-88045) — Close button not visible in build log dialog
* [**TW-90522**](https://youtrack.jetbrains.com/issue/TW-90522) — Change logging to INFO when no tests or test definitions are found in .trx files
* [**TW-69763**](https://youtrack.jetbrains.com/issue/TW-69763) — Pull requests build feature cannot determine which pull request a build is related too
* [**TW-90237**](https://youtrack.jetbrains.com/issue/TW-90237) — Review 2FA recovery codes dialog texts [ZD-6815480]
* [**TW-65491**](https://youtrack.jetbrains.com/issue/TW-65491) — The internal id is displayed as a build number in Jira Cloud
* [**TW-75030**](https://youtrack.jetbrains.com/issue/TW-75030) — Build triggers unexpectedly after unpausing job from Kotlin
* [**TW-90456**](https://youtrack.jetbrains.com/issue/TW-90456) — Maven archetype tc-sdk reload task does not work because of missing common-api.jar in new TeamCity versions
* [**TW-90739**](https://youtrack.jetbrains.com/issue/TW-90739) — NullPointerException in REST API when processing webhook in GitHub Webhooks plugin. 

### Performance Problem

* [**TW-90969**](https://youtrack.jetbrains.com/issue/TW-90969) — Builds triggered in a large project with versioned settings in \"use settings from VCS\" mode and Kotlin format can prevent start of unrelated builds
* [**TW-92370**](https://youtrack.jetbrains.com/issue/TW-92370) — Lots of concurrent getCurrentStateCalls to the same VCS repository
* [**TW-92147**](https://youtrack.jetbrains.com/issue/TW-92147) — Slow Artifacts tab of a composite build
* [**TW-92237**](https://youtrack.jetbrains.com/issue/TW-92237) — Inefficient code in VcsSettingsBean.getPopularVcsRoots
* [**TW-91377**](https://youtrack.jetbrains.com/issue/TW-91377) — Hanging REST API call (dependencies loop?)
* [**TW-91272**](https://youtrack.jetbrains.com/issue/TW-91272) — Lots of interlocking in BuildChainModifier during the versioned settings freeze if there are several similar build chains in the queue
* [**TW-89853**](https://youtrack.jetbrains.com/issue/TW-89853) — DirectoryScanner consumes a lot of memory on the agent side

### Security

11 security problems have been fixed. This number includes both native TeamCity issues and vulnerabilities found in 3rd-party libraries TeamCity depends on. Upstream library issues usually make up the majority of this total number, and are promptly resolved by updating these libraries to their newest versions.

To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2025.03). Security bulletins for new versions are typically published within the next few days after the release date.