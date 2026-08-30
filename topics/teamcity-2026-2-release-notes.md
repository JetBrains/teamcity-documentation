[//]: # (title: TeamCity 2026.2 Release Notes)
[//]: # (help-id: TeamCity 2026.2 Release Notes)

**Build 000000, 2 September 2026**

### Question

* [**TW-91554**](https://youtrack.jetbrains.com/issue/TW-91554) — Is this expected behavior when using Elasticsearch: "Error while deleting build ..., deleted 0/97 docs."

### Meta Issue

* [**TW-100716**](https://youtrack.jetbrains.com/issue/TW-100716) — [Bugfix] Pipelines GA in TeamCity On-Premises

### Feature

* [**TW-80371**](https://youtrack.jetbrains.com/issue/TW-80371) — Plugin for reporting issues from TeamCity directly
* [**TW-100703**](https://youtrack.jetbrains.com/issue/TW-100703) — "Run job even if upstream fails” option for a job
* [**TW-100323**](https://youtrack.jetbrains.com/issue/TW-100323) — Branch-based pipeline editing
* [**TW-96371**](https://youtrack.jetbrains.com/issue/TW-96371) — Add ability to retry a build in place if it is a part of a build chain
* [**TW-76755**](https://youtrack.jetbrains.com/issue/TW-76755) — Add support for zstd extension in artifact paths (.tar.zst)
* [**TW-93094**](https://youtrack.jetbrains.com/issue/TW-93094) — Offload Kotlin DSL execution to TeamCity agents
* [**TW-98182**](https://youtrack.jetbrains.com/issue/TW-98182) — AI Assistant: features for TeamCity Pipelines General Availability
* [**TW-100279**](https://youtrack.jetbrains.com/issue/TW-100279) — Support of Pipelines in TeamCity MCP
* [**TW-101074**](https://youtrack.jetbrains.com/issue/TW-101074) — Introduce BYOK option for AI Assistant
* [**TW-101737**](https://youtrack.jetbrains.com/issue/TW-101737) — Support Amazon ECR in Pipelines (YAML-only)
* [**TW-100910**](https://youtrack.jetbrains.com/issue/TW-100910) — Stop relying on the pull request specific branches in GitHub pull requests support
* [**TW-99588**](https://youtrack.jetbrains.com/issue/TW-99588) — Support OAuth flows for MCP server
* [**TW-89540**](https://youtrack.jetbrains.com/issue/TW-89540) — UX improvements to the Build overview filters
* [**TW-41074**](https://youtrack.jetbrains.com/issue/TW-41074) — Build time estimation should account for difference in parameters
* [**TW-68000**](https://youtrack.jetbrains.com/issue/TW-68000) — No way to choose multiple filters for the builds lists

### Task

* [**TW-102329**](https://youtrack.jetbrains.com/issue/TW-102329) — Provide Debug Pipeline functionality in TeamCity
* [**TW-96859**](https://youtrack.jetbrains.com/issue/TW-96859) — Provide a way to pass the context about the pipelines
* [**TW-100136**](https://youtrack.jetbrains.com/issue/TW-100136) — Re-enable parameter inheritance and update UI
* [**TW-98117**](https://youtrack.jetbrains.com/issue/TW-98117) — Add "Analyze it" button for the pipelines
* [**TW-103136**](https://youtrack.jetbrains.com/issue/TW-103136) — Stop restricting network operations in secondary node security manager
* [**TW-100065**](https://youtrack.jetbrains.com/issue/TW-100065) — Extended permission handling in the OAuth PKCE server flow
* [**TW-101919**](https://youtrack.jetbrains.com/issue/TW-101919) — [QA] Support Amazon ECR in Pipelines (YAML-only)
* [**TW-60136**](https://youtrack.jetbrains.com/issue/TW-60136) — Projects import: import access tokens when users are imported
* [**TW-97643**](https://youtrack.jetbrains.com/issue/TW-97643) — Add "Promote" action for Pipelines
* [**TW-100931**](https://youtrack.jetbrains.com/issue/TW-100931) — Projects import: preserve user ids on importing to empty server
* [**TW-55009**](https://youtrack.jetbrains.com/issue/TW-55009) — Projects import: preserve ids on importing to empty server, make a clear warn that ids are changed when they do
* [**TW-100299**](https://youtrack.jetbrains.com/issue/TW-100299) — Show the Performance Monitor tab for jobs
* [**TW-100160**](https://youtrack.jetbrains.com/issue/TW-100160) — Pipeline parameters: Show all inherited parameters by default
* [**TW-100964**](https://youtrack.jetbrains.com/issue/TW-100964) — Update Performance Monitor plugin UI to match Ring UI design system

### Bug

* [**TW-96749**](https://youtrack.jetbrains.com/issue/TW-96749) — AI Assistant doesn't answer correctly questions about pipelines setup
* [**TW-96901**](https://youtrack.jetbrains.com/issue/TW-96901) — PR and CPS features are assigned to the old VCS root in case if while VCS root editing a copy was created
* [**TW-102024**](https://youtrack.jetbrains.com/issue/TW-102024) — Pipelines: Checkout Rules: Checkout rules not respected for pipelines with only main repo
* [**TW-100645**](https://youtrack.jetbrains.com/issue/TW-100645) — It's not clear TFS is not supported for pipelines
* [**TW-100237**](https://youtrack.jetbrains.com/issue/TW-100237) — Successful job is shown for "Failed to start" pipelines with dependencies
* [**TW-101545**](https://youtrack.jetbrains.com/issue/TW-101545) — Pipelines without main vcs root do not have branch selector widget
* [**TW-76904**](https://youtrack.jetbrains.com/issue/TW-76904) — Limited access tokens: add "Change build source code with a custom patch" permission so that users can run personal builds
* [**TW-101394**](https://youtrack.jetbrains.com/issue/TW-101394) — Connection to uploads is prohibited by node restrictions from not-readonly secondary node
* [**TW-102580**](https://youtrack.jetbrains.com/issue/TW-102580) — Running icons are visible above the panel on the pipeline page
* [**TW-95025**](https://youtrack.jetbrains.com/issue/TW-95025) — Do not apply hard wrapping for long commands in YAML
* [**TW-100753**](https://youtrack.jetbrains.com/issue/TW-100753) — Heartbeat thread can't recover after DB communication failure
* [**TW-101336**](https://youtrack.jetbrains.com/issue/TW-101336) — If pop-up is closed during the "Collecting diagnostic data" stage, it doesn't appear again
* [**TW-103187**](https://youtrack.jetbrains.com/issue/TW-103187) — Composite finishes SUCCESS when a running dependency is canceled after its retries run out
* [**TW-96489**](https://youtrack.jetbrains.com/issue/TW-96489) — Failed to publish artifacts: java.lang.OutOfMemoryError: Java heap space
* [**TW-103318**](https://youtrack.jetbrains.com/issue/TW-103318) — DslGeneratorContextController should generate security options w/o enabled sandbox
* [**TW-101036**](https://youtrack.jetbrains.com/issue/TW-101036) — Builds on the old pull requests are re-run after changing "branches to detect" mode if their branches were previously covered by VCS Root branch spec.
* [**TW-103131**](https://youtrack.jetbrains.com/issue/TW-103131) — Fix the illustration for the YAML empty state
* [**TW-101678**](https://youtrack.jetbrains.com/issue/TW-101678) — Two "Search" icons are displayed in Dependencies tab, one of them disappear with flickering
* [**TW-101662**](https://youtrack.jetbrains.com/issue/TW-101662) — Configuration below gets into the loader area from the top one on "Expand all"
* [**TW-101578**](https://youtrack.jetbrains.com/issue/TW-101578) — Flickering of "Showing..." on Pipeline overview
* [**TW-103113**](https://youtrack.jetbrains.com/issue/TW-103113) — Pipeline editor initializes branch-scoped redux state before resolving settings storage
* [**TW-103348**](https://youtrack.jetbrains.com/issue/TW-103348) — Sandbox is not disabled properly in DSL on agent execution mode
* [**TW-100644**](https://youtrack.jetbrains.com/issue/TW-100644) — GitLab EE webhooks: Error java.lang.IllegalArgumentException: Argument for @NotNull parameter 's' in logs
* [**TW-102282**](https://youtrack.jetbrains.com/issue/TW-102282) — YAML in branches: false positive commit  in case if non-yaml changes
* [**TW-101049**](https://youtrack.jetbrains.com/issue/TW-101049) — Create Subproject Button in old UI is Broken.
* [**TW-101046**](https://youtrack.jetbrains.com/issue/TW-101046) — Links Not Rendered Properly When Searching For Job Steps in Pipelines
* [**TW-102527**](https://youtrack.jetbrains.com/issue/TW-102527) — Welcome and What's new popups are shown after upgrade from major release to its bugfix
* [**TW-102286**](https://youtrack.jetbrains.com/issue/TW-102286) — YAML in branches: false positive feedback while first attempt to commit to protected main
* [**TW-102291**](https://youtrack.jetbrains.com/issue/TW-102291) — YAML in branches: error "can't update settings" isn't branch-based
* [**TW-102294**](https://youtrack.jetbrains.com/issue/TW-102294) — Yaml in branches: no possibility to reset settings from  branch
* [**TW-99101**](https://youtrack.jetbrains.com/issue/TW-99101) — Error about the empty required fields are shown even if user doesn't try to save anything
* [**TW-98249**](https://youtrack.jetbrains.com/issue/TW-98249) — AWS cloud integration stops processing another instance types in AMI mode when the first one is unavailable in specific region.
* [**TW-100507**](https://youtrack.jetbrains.com/issue/TW-100507) — Snapshot dependency build is not reused when the chain head falls back to an older revision
* [**TW-101158**](https://youtrack.jetbrains.com/issue/TW-101158) — Caching proxies might cache stale buildAgent.zip after server upgrade
* [**TW-98011**](https://youtrack.jetbrains.com/issue/TW-98011) — UI bug that keeps expanding/collapsing tree views 
* [**TW-101234**](https://youtrack.jetbrains.com/issue/TW-101234) — Cannot select log range in new Performance Monitor UI
* [**TW-102598**](https://youtrack.jetbrains.com/issue/TW-102598) — Builds in old branches are triggered after changing the branch filter of the build configuration
* [**TW-92085**](https://youtrack.jetbrains.com/issue/TW-92085) — Overview header: "Open terminal" is suggested for local agents
* [**TW-62293**](https://youtrack.jetbrains.com/issue/TW-62293) — Users with view-settings permission are not able to see DSL
* [**TW-100955**](https://youtrack.jetbrains.com/issue/TW-100955) — Favorite Projects page: archived sub-projects of favorited parent appear in extra "N Archived Projects" section
* [**TW-102774**](https://youtrack.jetbrains.com/issue/TW-102774) — Build may not stop while preprocessing/packing artifacts
* [**TW-102867**](https://youtrack.jetbrains.com/issue/TW-102867) — Personal Access Tokens: handle state when no projects are yet available to the user
* [**TW-102349**](https://youtrack.jetbrains.com/issue/TW-102349) — Copy Build Step dialog lists only the first directly-assigned role project as a copy target (2026.1.1 regression)
* [**TW-102160**](https://youtrack.jetbrains.com/issue/TW-102160) — Use `x-token-auth` as default username for BitBucket Cloud OAuth tokens 
* [**TW-102213**](https://youtrack.jetbrains.com/issue/TW-102213) — Slack notifier breadcrumbs point to auto-generated virtual build configurations
* [**TW-98036**](https://youtrack.jetbrains.com/issue/TW-98036) — Commit status publisher reports failure for a composite build, when its dependency with "support test retry" is still running
* [**TW-20071**](https://youtrack.jetbrains.com/issue/TW-20071) — A user without "View user profile" permission can get user's roles
* [**TW-102358**](https://youtrack.jetbrains.com/issue/TW-102358) — Pipeline artifacts are not published after rerun
* [**TW-102011**](https://youtrack.jetbrains.com/issue/TW-102011) — YAML in branch: no dialogue is shown for the initial commit
* [**TW-102285**](https://youtrack.jetbrains.com/issue/TW-102285) — YAML in branches: wrong settings are shown for the main branch
* [**TW-102264**](https://youtrack.jetbrains.com/issue/TW-102264) — YAML in branches: dialogue is shown for storing settings on Server
* [**TW-102260**](https://youtrack.jetbrains.com/issue/TW-102260) — After committing to the new branch we should select that branch in the editor
* [**TW-102266**](https://youtrack.jetbrains.com/issue/TW-102266) — YAML in branches: UI tries to read settings from the initial commit
* [**TW-88948**](https://youtrack.jetbrains.com/issue/TW-88948) — Color Contrast for Warning and Error Rows in Build Logs
* [**TW-100173**](https://youtrack.jetbrains.com/issue/TW-100173) — Numeric parameters from pipeline DSL cannot be parsed in YAML, visual editor blocked  
* [**TW-95135**](https://youtrack.jetbrains.com/issue/TW-95135) — Delay "The first build error occurs" notification, if Test retry support is enabled
* [**TW-102008**](https://youtrack.jetbrains.com/issue/TW-102008) — YAML in branch: completely no feedback if there is no access to the repository in about 3 minutes
* [**TW-99767**](https://youtrack.jetbrains.com/issue/TW-99767) — The additional repo isn't deleted if the user switched to YAML during setting edition
* [**TW-99143**](https://youtrack.jetbrains.com/issue/TW-99143) — Jumping from Build Log to Problems after finishing a Job can be annoying for users
* [**TW-101157**](https://youtrack.jetbrains.com/issue/TW-101157) — Cloud: Add script for Self-hosted agent installation to the Agents page
* [**TW-99974**](https://youtrack.jetbrains.com/issue/TW-99974) — Mark test ignored if it was interrupted and parent block gets closed
* [**TW-100479**](https://youtrack.jetbrains.com/issue/TW-100479) — TeamCity builds sporadically fail to perform checkout with Git 2.54.0: unable to find all commit-graph files
* [**TW-102012**](https://youtrack.jetbrains.com/issue/TW-102012) — YAML in branch: overwhelming error message if not possible to commit
* [**TW-102292**](https://youtrack.jetbrains.com/issue/TW-102292) — YAML in branches: make an error message more readable
* [**TW-100785**](https://youtrack.jetbrains.com/issue/TW-100785) — Compiling Pipeline YAML schema on UI flattens properties
* [**TW-99933**](https://youtrack.jetbrains.com/issue/TW-99933) — Installation of bundled Maven in TeamCity server in docker fails with "java.nio.file.ReadOnlyFileSystemException"
* [**TW-97226**](https://youtrack.jetbrains.com/issue/TW-97226) — Builds skipped using partial chain execution are reported as failed to GitHub by commit status publisher build feature
* [**TW-101613**](https://youtrack.jetbrains.com/issue/TW-101613) — Oauth tokens are broken in imported VCS roots
* [**TW-101426**](https://youtrack.jetbrains.com/issue/TW-101426) — Notifications from Pipelines use a different prefix than regular notifications [TeamCity Pipelines, ...]
* [**TW-101425**](https://youtrack.jetbrains.com/issue/TW-101425) — Wrong Changes link in Pipeline Email notifications
* [**TW-101179**](https://youtrack.jetbrains.com/issue/TW-101179) — Branch name is cut to agressively in branch selector on Pipeline overview
* [**TW-101177**](https://youtrack.jetbrains.com/issue/TW-101177) — Pipelines: The additional repository checkout direectoty path is not autfilled anymore
* [**TW-102147**](https://youtrack.jetbrains.com/issue/TW-102147) — Pipelines: TFS repo is in the list for secondary repos in pipelines
* [**TW-101354**](https://youtrack.jetbrains.com/issue/TW-101354) — Build cache build feature doesn't work with Artifact lazy processing
* [**TW-99497**](https://youtrack.jetbrains.com/issue/TW-99497) — Committing settings into the global repository may get stuck
* [**TW-101516**](https://youtrack.jetbrains.com/issue/TW-101516) — Cannot login to GitHub from GitHub App connection after project import
* [**TW-101772**](https://youtrack.jetbrains.com/issue/TW-101772) — On opening, the Investigation dialog should show existing investigation info based on the current context project
* [**TW-101733**](https://youtrack.jetbrains.com/issue/TW-101733) — Complete YAML schema flattens all runners properties
* [**TW-100146**](https://youtrack.jetbrains.com/issue/TW-100146) — pipelinesCount FUS metric reports 3x more pipelines than are present on the server
* [**TW-101551**](https://youtrack.jetbrains.com/issue/TW-101551) — MessagesController (/app/messages): return 404 instead of 500 when build is not found
* [**TW-101585**](https://youtrack.jetbrains.com/issue/TW-101585) — Pipeline YAML editor: non-existing job dependency in object (files:) form is not flagged as invalid
* [**TW-92799**](https://youtrack.jetbrains.com/issue/TW-92799) — Limited access tokens: Not all permissions can be assigned
* [**TW-101460**](https://youtrack.jetbrains.com/issue/TW-101460) — build isn't retrying if it's assigned to the secondary node
* [**TW-100762**](https://youtrack.jetbrains.com/issue/TW-100762) — TeamCity Build Pipeline No longer working in 2026.1
* [**TW-98856**](https://youtrack.jetbrains.com/issue/TW-98856) — Pipeline defined in DSL without the main VCS root is not able to run
* [**TW-100714**](https://youtrack.jetbrains.com/issue/TW-100714) — Cloud: "No compatible agent" is shown while waiting for start of JetBrains-hosted agent
* [**TW-100525**](https://youtrack.jetbrains.com/issue/TW-100525) — Exception during the start of the TeamCity server secondary node
* [**TW-90348**](https://youtrack.jetbrains.com/issue/TW-90348) — Builds that require approval are hidden by default
* [**TW-101044**](https://youtrack.jetbrains.com/issue/TW-101044) — teamcity_rest_get: nested locator dimensions incorrectly detected as outer pagination
* [**TW-102009**](https://youtrack.jetbrains.com/issue/TW-102009) — YAML in branch: branch selection dialogue is shown when "store on server" is chosen
* [**TW-102013**](https://youtrack.jetbrains.com/issue/TW-102013) — YAML in branch: the flow is unclear in case of a commit to the default, which failed because of protection rules
* [**TW-102018**](https://youtrack.jetbrains.com/issue/TW-102018) — YAML in branch: a stale error is presented, blocking VCS settings reading
* [**TW-102017**](https://youtrack.jetbrains.com/issue/TW-102017) — YAML in branch: flow after the settings were committed is not clear 
* [**TW-102263**](https://youtrack.jetbrains.com/issue/TW-102263) — Show commit dialog when yaml file location changed to VCS repository.
* [**TW-100711**](https://youtrack.jetbrains.com/issue/TW-100711) — project import: don't import useless data
* [**TW-100545**](https://youtrack.jetbrains.com/issue/TW-100545) — Do not shutdown docker compose right on build interruption, as it prevents 'always running' steps from proper execution
* [**TW-99369**](https://youtrack.jetbrains.com/issue/TW-99369) — Pipeline YAML Editor: incorrect error count + misaligned error view
* [**TW-43738**](https://youtrack.jetbrains.com/issue/TW-43738) — Allow Run Custom Build dialog to be resized (to show long values in drop-downs)
* [**TW-101037**](https://youtrack.jetbrains.com/issue/TW-101037) — Pipeline Full Screen View: Unable to switch to another job in pipeline while in Full Screen mode
* [**TW-100427**](https://youtrack.jetbrains.com/issue/TW-100427) — Pipelines: Edit Repository: CSP and PR toggles are available for Any Git Repo with auth type - Anonymous
* [**TW-94605**](https://youtrack.jetbrains.com/issue/TW-94605) — "Unmet requirements: exists:" for incompatible agents instead of clear description
* [**TW-96778**](https://youtrack.jetbrains.com/issue/TW-96778) — No "Log in" next to the newly created connection in the dropdown
* [**TW-100373**](https://youtrack.jetbrains.com/issue/TW-100373) — Pipelines repository description: show "Branches to monitor: default" in case of empty branch specification
* [**TW-96418**](https://youtrack.jetbrains.com/issue/TW-96418) — Pipeline integrations: bring the "Test connection" back
* [**TW-101111**](https://youtrack.jetbrains.com/issue/TW-101111) — Switching between jobs in the full screen mode stops changing content after the first switch
* [**TW-99890**](https://youtrack.jetbrains.com/issue/TW-99890) — Horizontal sidebar behaviour is unclear and unstable
* [**TW-100881**](https://youtrack.jetbrains.com/issue/TW-100881) — "+:/refs/heads/*" is a default branch spec for Perforce roots
* [**TW-94724**](https://youtrack.jetbrains.com/issue/TW-94724) — Creation flow: repository description is always absent for the connections
* [**TW-98233**](https://youtrack.jetbrains.com/issue/TW-98233) — NPM integration has unnesasary text and jumps
* [**TW-99648**](https://youtrack.jetbrains.com/issue/TW-99648) — Using the Filter button on Change Log tab of Versioned Settings leads to 403 CRSF error
* [**TW-100405**](https://youtrack.jetbrains.com/issue/TW-100405) — Unable to attach an existing VCS Root to Pipeline
* [**TW-99093**](https://youtrack.jetbrains.com/issue/TW-99093) — SVG element code is visible after hovering on Copy button on Build/Pipeline Overview pages
* [**TW-100650**](https://youtrack.jetbrains.com/issue/TW-100650) — VCS trigger doesn't trigger build on empty branch
* [**TW-92837**](https://youtrack.jetbrains.com/issue/TW-92837) — Free Disk Space should be able to delete volumes of docker builder
* [**TW-95958**](https://youtrack.jetbrains.com/issue/TW-95958) — TCP Run in Docker: Docker Image autocomplete prioritizes loose substring matches over prefix
* [**TW-100917**](https://youtrack.jetbrains.com/issue/TW-100917) — Adding tags to a pipeline leads to "Error while loading data", the pipeline is inaccessible
* [**TW-100305**](https://youtrack.jetbrains.com/issue/TW-100305) — The Add Step dialog is not switched automatically to "All" when searching
* [**TW-100374**](https://youtrack.jetbrains.com/issue/TW-100374) — Pipelines: Edit Repository: Missing CSP status in the repository component description
* [**TW-100891**](https://youtrack.jetbrains.com/issue/TW-100891) — Dorm: Canceled build without agent cannot be displayed: "You do not have enough permissions to view requested agent pool"
* [**TW-99711**](https://youtrack.jetbrains.com/issue/TW-99711) — An empty Repository entity is created if the user switches from the pipeline setting to the Admin area
* [**TW-96420**](https://youtrack.jetbrains.com/issue/TW-96420) — New creation flow: "Create build configuration from this template" action leads to the old flow
* [**TW-93400**](https://youtrack.jetbrains.com/issue/TW-93400) — Pipelines: highlight repository names by exact match, not by symbols
* [**TW-100226**](https://youtrack.jetbrains.com/issue/TW-100226) — Do not show the text about DSL inconsistency from the DSL tab of pipelines stored in YAML
* [**TW-101033**](https://youtrack.jetbrains.com/issue/TW-101033) — Cleanup of build type groups can fail with an unknown error
* [**TW-99293**](https://youtrack.jetbrains.com/issue/TW-99293) — POM location is in the Advanced Maven settings for pipelines
* [**TW-96700**](https://youtrack.jetbrains.com/issue/TW-96700) — Cannot create Pipeline, if there is a Build configuration with the same name on the same project level
* [**TW-97236**](https://youtrack.jetbrains.com/issue/TW-97236) — Creation flow: handle Unauthorized exception in the UI
* [**TW-96422**](https://youtrack.jetbrains.com/issue/TW-96422) — Provide more information about the origin of the TeamCity server workspaces
* [**TW-99991**](https://youtrack.jetbrains.com/issue/TW-99991) — SAML plugin: 500 server error while login
* [**TW-99396**](https://youtrack.jetbrains.com/issue/TW-99396) — IndexOutOfBoundsException in org.jetbrains.teamcity.fus.client.events.TeamCityListener
* [**TW-101854**](https://youtrack.jetbrains.com/issue/TW-101854) — Make it possible to create a limited token for multiple projects at once
* [**TW-98815**](https://youtrack.jetbrains.com/issue/TW-98815) — Pipeline creation page: VCS editing tooltip alignment
* [**TW-94052**](https://youtrack.jetbrains.com/issue/TW-94052) — Branches are not loaded when configuring a second SSH repository in Pipeline Settings
* [**TW-97285**](https://youtrack.jetbrains.com/issue/TW-97285) — CSP feature isn't shown for a secondary pipeline repo
* [**TW-98845**](https://youtrack.jetbrains.com/issue/TW-98845) — Newlines are rendered differently in YAML and Visual pipeline editors
* [**TW-99085**](https://youtrack.jetbrains.com/issue/TW-99085) — Pipelines: All the artifacts from different jobs are published into the same folder on Pipeline level (can be overwritten)
* [**TW-95057**](https://youtrack.jetbrains.com/issue/TW-95057) — [pipeline ea] Agent requirements warning "None of the agents satisfy existing agent requirements" during usage of predefined parameters
* [**TW-97053**](https://youtrack.jetbrains.com/issue/TW-97053) — Inability to Run Pipelines With Two SSH Repositories
* [**TW-95625**](https://youtrack.jetbrains.com/issue/TW-95625) — Parameters in Pipelines: redefined build and VCS root parameters are not correctly applied in the pipelines runs
* [**TW-92635**](https://youtrack.jetbrains.com/issue/TW-92635) — The last configuration in the sidebar is hardly visible if the build log is open

### Performance Problem

* [**TW-99591**](https://youtrack.jetbrains.com/issue/TW-99591) — Open Pipeline settings in UI may take long time 
* [**TW-101463**](https://youtrack.jetbrains.com/issue/TW-101463) — Audit REST requests always scan the entire audit history potentially occupying too much memory if there is no selective filtering
* [**TW-77267**](https://youtrack.jetbrains.com/issue/TW-77267) — BuildPTRIndexer$BuildTask.beforeExecute consumes CPU in case of a large queue
* [**TW-102476**](https://youtrack.jetbrains.com/issue/TW-102476) — Memory Leak and Tab Crashes on TeamCity Agent Pages in Firefox and Chromium
* [**TW-103119**](https://youtrack.jetbrains.com/issue/TW-103119) — S3 artifact storage sends a Lens availability OPTIONS probe on every artifact upload, even when Lens events are disabled
* [**TW-76149**](https://youtrack.jetbrains.com/issue/TW-76149) — Maven dependencies resolution can hang on the server leading to hanging builds
* [**TW-101005**](https://youtrack.jetbrains.com/issue/TW-101005) — Avoid loading the entire VCS root DAG of commits in memory
* [**TW-100647**](https://youtrack.jetbrains.com/issue/TW-100647) — Server cleanup duration ~3x regression on SQL Server after 2025.11.3 upgrade
* [**TW-100746**](https://youtrack.jetbrains.com/issue/TW-100746) — "Artifacts storage" page loading is slow
* [**TW-99694**](https://youtrack.jetbrains.com/issue/TW-99694) — Avoid running Git prune before each fetch operation
* [**TW-100646**](https://youtrack.jetbrains.com/issue/TW-100646) — Project import loads the whole custom_data_body table
* [**TW-97758**](https://youtrack.jetbrains.com/issue/TW-97758) — Add a primary key to the ignored_tests table to help improve performance of DB replication
* [**TW-100723**](https://youtrack.jetbrains.com/issue/TW-100723) — Calculation of unprocessed node events takes a lot of time
* [**TW-100562**](https://youtrack.jetbrains.com/issue/TW-100562) — Processing of the events blocks on VCS commits cache initialization


### Security

126 security problems have been fixed.
To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2026.2).

Security bulletins are typically published few days after the release date.


