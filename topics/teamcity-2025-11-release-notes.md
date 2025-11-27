[//]: # (title: TeamCity 2025.11 Release Notes)
[//]: # (help-id: TeamCity 2025.11 Release Notes)

**Build 207946, 27 November 2025**

### Feature

* [**TW-96355**](https://youtrack.jetbrains.com/issue/TW-96355) — AI Assistant 
* [**TW-96117**](https://youtrack.jetbrains.com/issue/TW-96117) — New Project Creation Page
* [**TW-96118**](https://youtrack.jetbrains.com/issue/TW-96118) — New Build Configuration Creation Page
* [**TW-91558**](https://youtrack.jetbrains.com/issue/TW-91558) — Ability to provide a custom encryption key via environment variable
* [**TW-96119**](https://youtrack.jetbrains.com/issue/TW-96119) — New VCS Connection Creation Page
* [**TW-96138**](https://youtrack.jetbrains.com/issue/TW-96138) — New Pipeline & Build Chain Viewer with Minimap

### Task

* [**TW-84045**](https://youtrack.jetbrains.com/issue/TW-84045) — Ability to filter builds by startDate, finishDate
* [**TW-92834**](https://youtrack.jetbrains.com/issue/TW-92834) — Add ability to capture agent process memory dump from the TeamCity server
* [**TW-94792**](https://youtrack.jetbrains.com/issue/TW-94792) — Don't check the project scope when deleting tokens via Token management
* [**TW-94806**](https://youtrack.jetbrains.com/issue/TW-94806) — Token Management: Show full token timestamp on hover
* [**TW-92918**](https://youtrack.jetbrains.com/issue/TW-92918) — Clean agent git mirror only if git-fsck verification fails
* [**TW-95185**](https://youtrack.jetbrains.com/issue/TW-95185) — Don't execute git fsck verification if the remote access error occurs during git command on the agent
* [**TW-95186**](https://youtrack.jetbrains.com/issue/TW-95186) — Don't execute git fsck verification if the repository size is bigger than the predefined threshold
* [**TW-94990**](https://youtrack.jetbrains.com/issue/TW-94990) — Add validation for the GitHub URL in the Commit Status Publisher settings 
* [**TW-95355**](https://youtrack.jetbrains.com/issue/TW-95355) — Update TC on-premises License Agreement
* [**TW-95890**](https://youtrack.jetbrains.com/issue/TW-95890) — Provide additional details about connections and projects in GitHub app threads processing GitHub web hooks
* [**TW-95962**](https://youtrack.jetbrains.com/issue/TW-95962) — Optimize iteration over projects during webhook processing
* [**TW-96788**](https://youtrack.jetbrains.com/issue/TW-96788) — Show warning on Administration -> Updates page on attempt to upgrade to 2026.1+ if current server agents are running under Java < 21
* [**TW-61470**](https://youtrack.jetbrains.com/issue/TW-61470) — Add "The spot instance is scheduled for termination" to build cancelation status comment.
* [**TW-96150**](https://youtrack.jetbrains.com/issue/TW-96150) — Improve Docker Image Selection UX
* [**TW-95090**](https://youtrack.jetbrains.com/issue/TW-95090) — Remove Maven 2
* [**TW-96707**](https://youtrack.jetbrains.com/issue/TW-96707) — Docker: update Git within TeamCity Docker images: 2.51.0 -> 2.51.1
* [**TW-94852**](https://youtrack.jetbrains.com/issue/TW-94852) — Deprecate the StarTeam plugin
* [**TW-93184**](https://youtrack.jetbrains.com/issue/TW-93184) — Update default JaCoCo tool version from 0.7.5 to 0.8.8
* [**TW-79379**](https://youtrack.jetbrains.com/issue/TW-79379) — Update bundled JaCoCo to latest version (0.8.8)
* [**TW-84334**](https://youtrack.jetbrains.com/issue/TW-84334) — Matrix builds: Provide a way to get artifacts from auto-generated dependencies by default
* [**TW-93587**](https://youtrack.jetbrains.com/issue/TW-93587) — Update Linux Base Image: Ubuntu 22.04 -> 24.04 LTS
* [**TW-95033**](https://youtrack.jetbrains.com/issue/TW-95033) — Allow filtering of test investigations and mutes by affected project
* [**TW-96713**](https://youtrack.jetbrains.com/issue/TW-96713) — Docker: bump-up dependencies for the bundled tools
* [**TW-21890**](https://youtrack.jetbrains.com/issue/TW-21890) — Allow build configurations to have a multiline 'Long Description' field
* [**TW-95678**](https://youtrack.jetbrains.com/issue/TW-95678) — Disable cloud profile reset upon DSL update
* [**TW-93863**](https://youtrack.jetbrains.com/issue/TW-93863) — Cleanup unreachable VCS modifications
* [**TW-95714**](https://youtrack.jetbrains.com/issue/TW-95714) — Hide versions of the dotCover tool in the tool selector starting with 2025.2.1
* [**TW-94808**](https://youtrack.jetbrains.com/issue/TW-94808) — Untrusted builds: show approval requirement reason to non-approving users
* [**TW-95040**](https://youtrack.jetbrains.com/issue/TW-95040) — Remove teamcity.internal.docker.containerReuse.enabled

### Bug

* [**TW-44279**](https://youtrack.jetbrains.com/issue/TW-44279) — Build on branch can fail with error for unrelated branch (broken Git submodules case)
* [**TW-85221**](https://youtrack.jetbrains.com/issue/TW-85221) — java.lang.ArrayIndexOutOfBoundsException from PerfMon
* [**TW-97192**](https://youtrack.jetbrains.com/issue/TW-97192) — Changes in Matrix build feature are not shown until page refresh
* [**TW-96375**](https://youtrack.jetbrains.com/issue/TW-96375) — Agent scripts on Linux prefers x86 Java
* [**TW-92313**](https://youtrack.jetbrains.com/issue/TW-92313) — AWS Ec2: Broken cloud image leads to broken profile
* [**TW-84331**](https://youtrack.jetbrains.com/issue/TW-84331) — No health report is shown for Pull Request and Commit Status Publisher features if refreshable token is unavailable (the related connection was deleted)
* [**TW-94917**](https://youtrack.jetbrains.com/issue/TW-94917) — HTTP 203 is not treated as an error in Azure DevOps publisher
* [**TW-96369**](https://youtrack.jetbrains.com/issue/TW-96369) — Build / Pipeline execution status doesn't update on configuration / job page
* [**TW-96289**](https://youtrack.jetbrains.com/issue/TW-96289) — CSAT form disappears after following links in its text
* [**TW-95700**](https://youtrack.jetbrains.com/issue/TW-95700) — "Unsafe TeamCity data directory permissions" health item when the TC server is running as a Windows service under a local account
* [**TW-96668**](https://youtrack.jetbrains.com/issue/TW-96668) — S3 URL verification false alarm if a build configuration is renamed during a build
* [**TW-96465**](https://youtrack.jetbrains.com/issue/TW-96465) — Java upgrade related health report does not show affected agents if shown on the Administration -> Server health page
* [**TW-97167**](https://youtrack.jetbrains.com/issue/TW-97167) — Matrix build feature ignores Custom path in Versioned settings
* [**TW-89809**](https://youtrack.jetbrains.com/issue/TW-89809) — The parameter spec for the inherited parameter can be overwritten if the parameter value was changed before
* [**TW-85751**](https://youtrack.jetbrains.com/issue/TW-85751) — Changes are not shown in build on main node, if collected on a secondary node and build is started before changes collection
* [**TW-40005**](https://youtrack.jetbrains.com/issue/TW-40005) — Confusing implicit requirement for teamcity.build.vcs.branch.XXX when VCS repository is inaccessible
* [**TW-96222**](https://youtrack.jetbrains.com/issue/TW-96222) — Build duration chart is not shown for the composite build
* [**TW-94936**](https://youtrack.jetbrains.com/issue/TW-94936) — Changes in settings are not detached from a build configuration without builds in default branch
* [**TW-93647**](https://youtrack.jetbrains.com/issue/TW-93647) — No button settings is a link
* [**TW-92415**](https://youtrack.jetbrains.com/issue/TW-92415) — UX optimization of the Projects sidebar
* [**TW-96918**](https://youtrack.jetbrains.com/issue/TW-96918) — Many of "AccessDeniedException" from FUS in teamcity-server.log  of a read-only secondary node
* [**TW-91938**](https://youtrack.jetbrains.com/issue/TW-91938) — Artifacts are not split per batch when running Parallel Tests which causes confusion
* [**TW-95413**](https://youtrack.jetbrains.com/issue/TW-95413) — Adjusting compatible configurations via REST API generates unnecessary audit logs
* [**TW-96564**](https://youtrack.jetbrains.com/issue/TW-96564) — Artifact dependencies can stop working if build configuration internal id changed
* [**TW-88819**](https://youtrack.jetbrains.com/issue/TW-88819) — Error moving or renaming a project via Kotlin DSL: jetbrains.buildServer.serverSide.ProjectRemoveFailedException
* [**TW-96163**](https://youtrack.jetbrains.com/issue/TW-96163) — "Show history" button does not work for configuration-level investigation
* [**TW-95126**](https://youtrack.jetbrains.com/issue/TW-95126) — TCP Merge: warning about build feature configuring for used VCS roots based on URL
* [**TW-87633**](https://youtrack.jetbrains.com/issue/TW-87633) — Commit Status Publisher reports failure while the build is still running (test retries)
* [**TW-94885**](https://youtrack.jetbrains.com/issue/TW-94885) — VS2022 configuration parameter shows incorrect version after Visual Studio upgrade
* [**TW-96374**](https://youtrack.jetbrains.com/issue/TW-96374) — Parametrized tokenId is displayed as invalid in the VCS Root settings UI
* [**TW-93246**](https://youtrack.jetbrains.com/issue/TW-93246) — Playwright broken relative links issue
* [**TW-86457**](https://youtrack.jetbrains.com/issue/TW-86457) — UnsupportedOperationException in FTP Uploader when using FTPS
* [**TW-95590**](https://youtrack.jetbrains.com/issue/TW-95590) — VCS trigger may start a build when there are no relevant changes
* [**TW-96762**](https://youtrack.jetbrains.com/issue/TW-96762) — Public recipes: PublicRecipesDownloadedPrecondition can get repeatedly called with an unhandled BuildTypeNotFoundException

* [**TW-94129**](https://youtrack.jetbrains.com/issue/TW-94129) — TCP Merge: remove piplines head and virtual build configuration from "Assigne Buid to agent popup"
* [**TW-96421**](https://youtrack.jetbrains.com/issue/TW-96421) — A step with BuildStep.ExecutionMode.ALWAYS may not run
* [**TW-96793**](https://youtrack.jetbrains.com/issue/TW-96793) — Build cannot be interrupted during PublishBuildPropertiesFStage
* [**TW-96325**](https://youtrack.jetbrains.com/issue/TW-96325) — GitHub App creation menu doesn't update the save button block when switching modes
* [**TW-96321**](https://youtrack.jetbrains.com/issue/TW-96321) — Cant delete VCS auth tokens
* [**TW-90003**](https://youtrack.jetbrains.com/issue/TW-90003) — Agent Terminal doesn't block the termination of the cloud agents which should be terminated after the first build if the terminal was opened after the build is started
* [**TW-94728**](https://youtrack.jetbrains.com/issue/TW-94728) — TCP Merge: connection name can be out of selector bounds
* [**TW-96339**](https://youtrack.jetbrains.com/issue/TW-96339) — [save-private-tags] IllegalStateException: Attempted to open a nested transaction. Analyse stack trace to find two transaction entrances
* [**TW-93213**](https://youtrack.jetbrains.com/issue/TW-93213) — Can't download JDBC driver as `datadir` not writable
* [**TW-96315**](https://youtrack.jetbrains.com/issue/TW-96315) — List of connections doesn't scroll down after returning from the "Add a new connection" page
* [**TW-96213**](https://youtrack.jetbrains.com/issue/TW-96213) — Private recipes: uploading a new recipe overwrites an existing recipe with a matching id
* [**TW-96456**](https://youtrack.jetbrains.com/issue/TW-96456) — Project isolation: Information about inaccessible dependency is not logged to teamcity-server.log
* [**TW-96202**](https://youtrack.jetbrains.com/issue/TW-96202) — Private recipes: do not reload the page after saving changes to a private recipe
* [**TW-95878**](https://youtrack.jetbrains.com/issue/TW-95878) — Test Connection for GitHub App webhooks  shows an error if the App was initially configured with the wrong webhook secret
* [**TW-95882**](https://youtrack.jetbrains.com/issue/TW-95882) — Project Copy copies the description from the parent project to all newly created subprojects overwriting their original descriptions
* [**TW-93043**](https://youtrack.jetbrains.com/issue/TW-93043) — Scheduled build gets optimized after 15 days
* [**TW-87661**](https://youtrack.jetbrains.com/issue/TW-87661) — VCS hosting icons don't fit a single line
* [**TW-94209**](https://youtrack.jetbrains.com/issue/TW-94209) — Test Retry feature causes incorrect test filter to be passed to next build
* [**TW-96054**](https://youtrack.jetbrains.com/issue/TW-96054) — Integrate the inherited parameters table in the UI
* [**TW-95156**](https://youtrack.jetbrains.com/issue/TW-95156) — Deadlock on attempt to save data to test_metadata table
* [**TW-94834**](https://youtrack.jetbrains.com/issue/TW-94834) — Warnings "Cannot collect state" in the teamcity-server.log after the update
* [**TW-94678**](https://youtrack.jetbrains.com/issue/TW-94678) — TCP Merge: do not trigger runs for each change if there are queued ones
* [**TW-96056**](https://youtrack.jetbrains.com/issue/TW-96056) — Unable to open Build Info Popup on Test History Chart in Chrome 
* [**TW-96042**](https://youtrack.jetbrains.com/issue/TW-96042) — Popup with test duration chart appears in the wrong location
* [**TW-86867**](https://youtrack.jetbrains.com/issue/TW-86867) — Custom S3 Storages are not supported by Artifacts Migration Tool
* [**TW-95116**](https://youtrack.jetbrains.com/issue/TW-95116) — Incorrect escaping in cloud image error 
* [**TW-94895**](https://youtrack.jetbrains.com/issue/TW-94895) — Azure OAuth App are no longer avialable
* [**TW-94174**](https://youtrack.jetbrains.com/issue/TW-94174) — Public Recipes: Add link to documentation and/or some hint/label to the Recipe tab for the Enabling/Disabling integration with Marketplace
* [**TW-67272**](https://youtrack.jetbrains.com/issue/TW-67272) — downloaded_artifacts table has no primary key so the replication lags
* [**TW-62258**](https://youtrack.jetbrains.com/issue/TW-62258) — Information about artifacts downloaded by build can be lost if proxy switches requests from the main server to the secondary because main server does not respond
* [**TW-91875**](https://youtrack.jetbrains.com/issue/TW-91875) — Kubernetes Executor: missing DSL version for image and policy
* [**TW-90749**](https://youtrack.jetbrains.com/issue/TW-90749) — UI Issues with Test Metadata Charts in TeamCity
* [**TW-95619**](https://youtrack.jetbrains.com/issue/TW-95619) — Metric tags are not visible on the metrics page
* [**TW-94733**](https://youtrack.jetbrains.com/issue/TW-94733) — Perforce changes collection fails for invalid child streams
* [**TW-93352**](https://youtrack.jetbrains.com/issue/TW-93352) — Sidebar state (pinned/unpinned) resets after relogin
* [**TW-94819**](https://youtrack.jetbrains.com/issue/TW-94819) — Lots of log messages 'The Artifact Dependency Id is too long (50 characters) and will be truncated to 40 characters.'
* [**TW-93651**](https://youtrack.jetbrains.com/issue/TW-93651) — Retry to retrieve token on agent even if 404 were returned
* [**TW-94066**](https://youtrack.jetbrains.com/issue/TW-94066) — Support optional artifact dependencies rules in DSL
* [**TW-93162**](https://youtrack.jetbrains.com/issue/TW-93162) — Keyboard navigation in sidebar for settings mode doesn't work

### Pipeline Enhancements

* [**TW-96143**](https://youtrack.jetbrains.com/issue/TW-96143) — Investigations, Mutes, Pins, Comments & Tags support for Pipelines
* [**TW-94828**](https://youtrack.jetbrains.com/issue/TW-94828) — Support project parameters inherited from parent project in pipelines
* [**TW-95443**](https://youtrack.jetbrains.com/issue/TW-95443) — Add "contains" agent requirement
* [**TW-95277**](https://youtrack.jetbrains.com/issue/TW-95277) — It's not obvious what pipeline is shown on the Pipeline Overview page
* [**TW-95798**](https://youtrack.jetbrains.com/issue/TW-95798) — Misleading Branch specification texts for repositories and triggers
* [**TW-93910**](https://youtrack.jetbrains.com/issue/TW-93910) — Missing wait reasons for queued pipeline without jobs
* [**TW-93732**](https://youtrack.jetbrains.com/issue/TW-93732) — Pipeline Head in audit actions
* [**TW-95412**](https://youtrack.jetbrains.com/issue/TW-95412) — Unable to create pipeline using a previous name: collision with the project name
* [**TW-94642**](https://youtrack.jetbrains.com/issue/TW-94642) — "Uncaught no such element exception" from pipeline page
* [**TW-95024**](https://youtrack.jetbrains.com/issue/TW-95024) — Artifacts name from pipeline/job got by "Download all" include Pipeline Head
* [**TW-94640**](https://youtrack.jetbrains.com/issue/TW-94640) — Pipelines heads and jobs are shown in Queue Priority classes
* [**TW-95733**](https://youtrack.jetbrains.com/issue/TW-95733) — Pipeline broken (repo settings are overwritten) after editing the pipeline's settings by user without access to repo
* [**TW-96667**](https://youtrack.jetbrains.com/issue/TW-96667) — Deleting one Docker/NPM integration disables the others in the same pipeline
* [**TW-96425**](https://youtrack.jetbrains.com/issue/TW-96425) — Rename "From used VCS root" option to "From an existing VCS root"
* [**TW-95381**](https://youtrack.jetbrains.com/issue/TW-95381) — Can't open the pipeline settings with invalid YML (that was successfully saved)
* [**TW-94336**](https://youtrack.jetbrains.com/issue/TW-94336) — Bulk edit IDs doesn't hide Pipeline Head
* [**TW-95840**](https://youtrack.jetbrains.com/issue/TW-95840) — Custom version specification in Maven runner in pipelines doesn't work
* [**TW-96936**](https://youtrack.jetbrains.com/issue/TW-96936) — Pass the full default branch specification on Pipeline creation
* [**TW-94624**](https://youtrack.jetbrains.com/issue/TW-94624) — Pipeline doesn't work with VCS roots with parametrized credentials or branches
* [**TW-97051**](https://youtrack.jetbrains.com/issue/TW-97051) — Pipeline trigger branch filter list updates with additional values after adding pull request syntax to specific branches


### Performance Problem

* [**TW-94856**](https://youtrack.jetbrains.com/issue/TW-94856) — Consider implementing a cache for the corrected region settings in S3
* [**TW-96540**](https://youtrack.jetbrains.com/issue/TW-96540) — Jetbrains.buildServer.metrics eat G1 Old Mem
* [**TW-95349**](https://youtrack.jetbrains.com/issue/TW-95349) — The project change log page consumes a lot of memory
* [**TW-69382**](https://youtrack.jetbrains.com/issue/TW-69382) — Create Access Token performance improvements:
* [**TW-92724**](https://youtrack.jetbrains.com/issue/TW-92724) — Search by build configurations in sidebar works very slow
* [**TW-94478**](https://youtrack.jetbrains.com/issue/TW-94478) — New UI test history page works much slower than the old UI
* [**TW-97042**](https://youtrack.jetbrains.com/issue/TW-97042) — Slow computing revisions for settings root
* [**TW-94832**](https://youtrack.jetbrains.com/issue/TW-94832) — "Create Pipeline/Build configuration" pages load a few minutes if one of the connection is not accessible
* [**TW-96221**](https://youtrack.jetbrains.com/issue/TW-96221) — Don't resolve VCS Roots when the webhook is processed 
* [**TW-95810**](https://youtrack.jetbrains.com/issue/TW-95810) — UI Performance degradation during tests muting
* [**TW-94735**](https://youtrack.jetbrains.com/issue/TW-94735) — Very slow initialization of InvestigationTestRunsHolderImpl (accompanied by large memory usage)
* [**TW-96255**](https://youtrack.jetbrains.com/issue/TW-96255) — Composite build with finished dependencies should finish right away
* [**TW-94857**](https://youtrack.jetbrains.com/issue/TW-94857) — Updates to private recipes become visible with large delay (multi node setup)
* [**TW-36807**](https://youtrack.jetbrains.com/issue/TW-36807) — Slow TestImpl.getAllResponsibilities API call (used in REST)
* [**TW-95166**](https://youtrack.jetbrains.com/issue/TW-95166) — AgentsJVMGeneralExtension health item loads too much data
* [**TW-72527**](https://youtrack.jetbrains.com/issue/TW-72527) — TestHistory API should be extended to support limited number of test runs and limited number of fetched builds
* [**TW-76621**](https://youtrack.jetbrains.com/issue/TW-76621) — Slow loading of the Test History page due to N+1 loading of data from agent_type_param
* [**TW-94401**](https://youtrack.jetbrains.com/issue/TW-94401) — Slow test metadata processing for large number of tests
* [**TW-94764**](https://youtrack.jetbrains.com/issue/TW-94764) — long scheduling of the settings freeze


### Security

Nine security problems have been fixed.
To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2025.11).

Security bulletins are typically published few days after the release date.


