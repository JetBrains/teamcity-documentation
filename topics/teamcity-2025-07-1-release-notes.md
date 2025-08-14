[//]: # (title: TeamCity 2025.07.1 Release Notes)
[//]: # (help-id: TeamCity 2025.07.1 Release Notes)

**Build 197325, 14 August 2025**

### Task

* [**TW-78565**](https://youtrack.jetbrains.com/issue/TW-78565) — SetParameter service message called via Rest API log method does not work

### Bug

* [**TW-94822**](https://youtrack.jetbrains.com/issue/TW-94822) — Can't disable public recipes if they are a part of a private one
* [**TW-94481**](https://youtrack.jetbrains.com/issue/TW-94481) — "Could not start new instances. Quota exceeded" warning message on cloud image - AWS
* [**TW-94893**](https://youtrack.jetbrains.com/issue/TW-94893) — Commit status publisher with parameter references fails after upgrade to 2025.07
* [**TW-95131**](https://youtrack.jetbrains.com/issue/TW-95131) — Wrong test results are reported and test count varies from build to build without any changes
* [**TW-94999**](https://youtrack.jetbrains.com/issue/TW-94999) — Problem with VCS Trigger -jetbrains.buildServer.buildTriggers.BuildTriggerException (this.myVisitedAndNewerThanLowerBoundsCommits is null)
* [**TW-94163**](https://youtrack.jetbrains.com/issue/TW-94163) — Azure Board Work Items Documentation Mentions Deprecated Password Auth
* [**TW-94493**](https://youtrack.jetbrains.com/issue/TW-94493) — [Project isolation] Show in the UI, that list of trusted projects is ignored with the "All project" setting
* [**TW-94655**](https://youtrack.jetbrains.com/issue/TW-94655) — [Project isolation] Add progress for "Add currently dependent projects"
* [**TW-67478**](https://youtrack.jetbrains.com/issue/TW-67478) — Perforce stream builds may take revision from wrong stream (case of REST triggering with specified revision)
* [**TW-94472**](https://youtrack.jetbrains.com/issue/TW-94472) — Kubernetes Executor: Pod remains running for Failed to start build
* [**TW-95147**](https://youtrack.jetbrains.com/issue/TW-95147) — Internal properties with .days suffix can't be parsed
* [**TW-94891**](https://youtrack.jetbrains.com/issue/TW-94891) — 404 error on build page (too long request via IIS proxy)
* [**TW-94740**](https://youtrack.jetbrains.com/issue/TW-94740) — TCP Merge: Edit used VCS root: Changes are applied to parent project with VCS root, and not applied to Pipeline itself 
* [**TW-93795**](https://youtrack.jetbrains.com/issue/TW-93795) — TeamCity screens are recursively opened on the Agents page in case of insufficient permissions
* [**TW-94410**](https://youtrack.jetbrains.com/issue/TW-94410) — TCP Merge: CSP and PR features are not deleted, if were added for secondary repo and the main repo is from external VCS root
* [**TW-94823**](https://youtrack.jetbrains.com/issue/TW-94823) — Disabling AWS connection use in sub-projects prevents using S3 artifact storage configured in the same parent project
* [**TW-93347**](https://youtrack.jetbrains.com/issue/TW-93347) — TCP Merge: warning about configuring status publishing
* [**TW-95088**](https://youtrack.jetbrains.com/issue/TW-95088) — Builds are not shown on project overview (404 error behind IIS proxy)
* [**TW-71944**](https://youtrack.jetbrains.com/issue/TW-71944) — Error 404 on UI action RECEIVE_TEST_OCCURRENCES_INVOCATIONS
* [**TW-95021**](https://youtrack.jetbrains.com/issue/TW-95021) — 400 Bad request error for "last successful" and "last finished build" links
* [**TW-94807**](https://youtrack.jetbrains.com/issue/TW-94807) — TCP Merge: filter out pipelines for IntelliJ Idea plugin
* [**TW-94710**](https://youtrack.jetbrains.com/issue/TW-94710) —  TEAMCITY_PATH_PREFIX does not work with command Line runner "Executable with parameters" mode
* [**TW-93732**](https://youtrack.jetbrains.com/issue/TW-93732) — TCP Merge: Pipeline Head in audit actions
* [**TW-93984**](https://youtrack.jetbrains.com/issue/TW-93984) — [Conditional dependencies] Irrelevant Commit status text for builds canceled by skipQueuedBuilds service message
* [**TW-94396**](https://youtrack.jetbrains.com/issue/TW-94396) — Output parameters Usages report shows Unexpected error, if configuration doesn't exist anymore
* [**TW-93528**](https://youtrack.jetbrains.com/issue/TW-93528) — TCP Merge: Head build configuration is listed and accessible from the Builds Schedule page
* [**TW-94579**](https://youtrack.jetbrains.com/issue/TW-94579) — [Project isolation] Group Usages report table by "Depends on"
* [**TW-94256**](https://youtrack.jetbrains.com/issue/TW-94256) — TCP Merge: Only VCS Root could be disabled when checkout path is specified
* [**TW-95041**](https://youtrack.jetbrains.com/issue/TW-95041) — Agent is unregistered even though it communicated with the server recently
* [**TW-94693**](https://youtrack.jetbrains.com/issue/TW-94693) — [Project Isolation] Reduce logging for "Add currently dependent projects to the list"
* [**TW-94611**](https://youtrack.jetbrains.com/issue/TW-94611) — Failure of matrix and parallel test builds to publish artifacts to S3 if AWS connection use in sub-projects is not allowed
* [**TW-94407**](https://youtrack.jetbrains.com/issue/TW-94407) — [Project isolation] No messages in the UI about successful/unsuccessful Pre-fill
* [**TW-94647**](https://youtrack.jetbrains.com/issue/TW-94647) — "Could not update build problem in database: jetbrains.buildServer.serverSide.db.MySQL.MySqlIncorrectStringValueException: Incorrect string value" in teamcity-server.log
* [**TW-94384**](https://youtrack.jetbrains.com/issue/TW-94384) — [Project isolation] List of trusted projects is not sorted
* [**TW-94586**](https://youtrack.jetbrains.com/issue/TW-94586) — TCP Merge: Cannot publish artifacts to S3 storage that uses AWS connection with "Sub-projects can utilize the connection = false"
* [**TW-92865**](https://youtrack.jetbrains.com/issue/TW-92865) — BuildType.copy() doesn't copy conditions assigned to step
* [**TW-94626**](https://youtrack.jetbrains.com/issue/TW-94626) — TCP Merge: VCS root with SSH auth is shown as "none" in job settings
* [**TW-94650**](https://youtrack.jetbrains.com/issue/TW-94650) — TCP Merge: Schedule trigger next start time doesn't consider time zone
* [**TW-82882**](https://youtrack.jetbrains.com/issue/TW-82882) — Subgroup removal can cause a deadlock
* [**TW-94904**](https://youtrack.jetbrains.com/issue/TW-94904) — Deadlock on attempt to add a user to a group which is being deleted in a separate thread
* [**TW-94816**](https://youtrack.jetbrains.com/issue/TW-94816) — Incorrect OAuth account is connected to TeamCity account in the profile, if there are a few authentication modules
* [**TW-94817**](https://youtrack.jetbrains.com/issue/TW-94817) — Connection of GitHub and TeamCity accounts from user's profile doesn't work for GitHub App connection
* [**TW-94220**](https://youtrack.jetbrains.com/issue/TW-94220) — TCP Merge: unnecessary “Initial commit will be made shortly.” during the import from VCS and storing the pipeline in VCS
* [**TW-94156**](https://youtrack.jetbrains.com/issue/TW-94156) — TCP Merge: Notifications icons could not be loaded
* [**TW-94905**](https://youtrack.jetbrains.com/issue/TW-94905) — Secure build feature parameters can become unavailable after agent publishes parameters
* [**TW-94837**](https://youtrack.jetbrains.com/issue/TW-94837) — Private recipes: recipes from plugins are not handled correctly
* [**TW-94910**](https://youtrack.jetbrains.com/issue/TW-94910) — AccessDeniedException on attempt to get parameters inside RestRunningBuildsNotificator.fetchBuildJson call
* [**TW-94310**](https://youtrack.jetbrains.com/issue/TW-94310) — TCP Merge: Close "Maximum number of pipelines reached" error message tooltip after some time
* [**TW-94193**](https://youtrack.jetbrains.com/issue/TW-94193) — There is no way to collapse a project that only has pipelines in it
* [**TW-94320**](https://youtrack.jetbrains.com/issue/TW-94320) — TCP Merge: copy button is duplicated for a project  in case if both, build and pipelines limit is reached
* [**TW-94639**](https://youtrack.jetbrains.com/issue/TW-94639) — Private recipes: it's possible to add two recipes with the same name to a project if they have different formats
* [**TW-94672**](https://youtrack.jetbrains.com/issue/TW-94672) — Untrusted builds. When only logging is enabled and single build is started it will stay in queue forever with the reason "Failed to determine if build is untrusted"
* [**TW-94669**](https://youtrack.jetbrains.com/issue/TW-94669) — TCP Merge: "Delete pipeline" button is available in read-only mode 

### Performance Problem

* [**TW-94401**](https://youtrack.jetbrains.com/issue/TW-94401) — Slow test metadata processing for large number of tests
* [**TW-95070**](https://youtrack.jetbrains.com/issue/TW-95070) — Slow artifacts download because of constant build update
* [**TW-94992**](https://youtrack.jetbrains.com/issue/TW-94992) — TeamCity running slow due to "Heavy GC Overhead" critical warning
* [**TW-93915**](https://youtrack.jetbrains.com/issue/TW-93915) — Sharing of SSH keys takes a lot of time on big installations with several nodes


### Security

Three security problems have been fixed.
To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2025.07.1).

Security bulletins are typically published few days after the release date.


