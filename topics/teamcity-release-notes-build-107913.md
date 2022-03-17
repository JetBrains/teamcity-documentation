[//]: # (title: TeamCity Release Notes: Build 107913)
[//]: # (auxiliary-id: TeamCity Release Notes: Build 107913;TeamCity 2022.02 Release Notes)

__Build: 107913__  
__15 March 2022__

### Feature

[**TW-63772**](https://youtrack.jetbrains.com/issue/TW-63772) — FlowID is ignored in the new build log  
[**TW-73266**](https://youtrack.jetbrains.com/issue/TW-73266) — Support reporting of the build status to the review directly via build status API of Helix Swarm  
[**TW-68011**](https://youtrack.jetbrains.com/issue/TW-68011) — Run custom build dialog: allow specifying an arbitrary branch name  
[**TW-53412**](https://youtrack.jetbrains.com/issue/TW-53412) — Batch operations with builds  
[**TW-74421**](https://youtrack.jetbrains.com/issue/TW-74421) — Add support (connection type) for public ECR  
[**TW-64974**](https://youtrack.jetbrains.com/issue/TW-64974) — Snapshot dependencies block improvements on the build overview page  
[**TW-73147**](https://youtrack.jetbrains.com/issue/TW-73147) — Import avatars from GitHub, BitBucket and other external auth modules on user creation  
[**TW-73850**](https://youtrack.jetbrains.com/issue/TW-73850) — Add compatibility with BitBucket&#39;s checks for merging pull requests.  
[**TW-72261**](https://youtrack.jetbrains.com/issue/TW-72261) — Update language highlighting adding C#  
[**TW-52235**](https://youtrack.jetbrains.com/issue/TW-52235) — Upgrade to a newer version of FreeMarker  
[**TW-38093**](https://youtrack.jetbrains.com/issue/TW-38093) — REST API. Allow to change the status for the finished build.  
[**TW-74213**](https://youtrack.jetbrains.com/issue/TW-74213) — Expose counters required for popup on a graph on Changes page  
[**TW-74214**](https://youtrack.jetbrains.com/issue/TW-74214) — Add ability to filter builds related to change  
[**TW-74810**](https://youtrack.jetbrains.com/issue/TW-74810) — Ability to deduplicate changes  
[**TW-74613**](https://youtrack.jetbrains.com/issue/TW-74613) — UI: Add the ability to get the reasons and time to wait for the queued build.  
[**TW-61625**](https://youtrack.jetbrains.com/issue/TW-61625) — Allow to turn off agent downloaded artifacts cache on per-build basis  
[**TW-74619**](https://youtrack.jetbrains.com/issue/TW-74619) — REST: Add commiters field to a Change model  
[**TW-74329**](https://youtrack.jetbrains.com/issue/TW-74329) — REST: allow getting full list of ancestors for a project or build type  
[**TW-68720**](https://youtrack.jetbrains.com/issue/TW-68720) — REST: Add the ability to get the reasons and time to wait for the queued build.  
[**TW-52941**](https://youtrack.jetbrains.com/issue/TW-52941) — To add ability to view public part of the uploaded SSH key  
[**TW-69648**](https://youtrack.jetbrains.com/issue/TW-69648) — Add project popup to Entity Name in Sakura UI  
[**TW-29273**](https://youtrack.jetbrains.com/issue/TW-29273) — Display templates in breadcrumbs in administration pages for fast navigation

### Usability Problem

[**TW-68191**](https://youtrack.jetbrains.com/issue/TW-68191) — Kotlin DSL: several versions of the same class are available for import, one for each version of DSL API  
[**TW-74688**](https://youtrack.jetbrains.com/issue/TW-74688) — Rework `There are no fully-running cloud agents` wait reason wording  
[**TW-67258**](https://youtrack.jetbrains.com/issue/TW-67258) — &quot;No differences between builds were found&quot; phrase is formally incorrect  
[**TW-64933**](https://youtrack.jetbrains.com/issue/TW-64933) — Remember filter value in sidebar  
[**TW-30985**](https://youtrack.jetbrains.com/issue/TW-30985) — Improve browser page title to differentiate between build configurations better  
[**TW-74321**](https://youtrack.jetbrains.com/issue/TW-74321) — Reboot Agent action should not reload the page  
[**TW-74646**](https://youtrack.jetbrains.com/issue/TW-74646) — Get rid of meaningless debug logs for shutting down S3 clients  
[**TW-68007**](https://youtrack.jetbrains.com/issue/TW-68007) — Build Configuration Home/ Project Home links in build configuration/project settings are unobvious  
[**TW-59414**](https://youtrack.jetbrains.com/issue/TW-59414) — A health report on branch specs in VCS root overlapping with the ones provided by the Pull Requests build feature is required  
[**TW-74149**](https://youtrack.jetbrains.com/issue/TW-74149) — Artifacts publishing times are wrong in the build log  
[**TW-73849**](https://youtrack.jetbrains.com/issue/TW-73849) — Not related build log is shown for expanded problem Build failure on specific text in build log  
[**TW-74576**](https://youtrack.jetbrains.com/issue/TW-74576) — Make the colors of the modified files the same as in IDEA  
[**TW-72783**](https://youtrack.jetbrains.com/issue/TW-72783) — Not clear meaning of the Muted word on the Tests tab  
[**TW-73548**](https://youtrack.jetbrains.com/issue/TW-73548) — Information about pending changes is shown in a blind spot  
[**TW-62550**](https://youtrack.jetbrains.com/issue/TW-62550) — Small circle on the build log timeline is not self-descriptive

### Bug

[**TW-74062**](https://youtrack.jetbrains.com/issue/TW-74062) — Remote debug is listening wrong host and doesn&#39;t start build on TeamCity server  
[**TW-62349**](https://youtrack.jetbrains.com/issue/TW-62349) — Some TeamCity service messages are interrupted in stdOut by newline symbol in the case of .net core tests  
[**TW-71927**](https://youtrack.jetbrains.com/issue/TW-71927) — fix &quot; Please do not use ActionPlaces.UNKNOWN or the empty place&quot;  
[**TW-74430**](https://youtrack.jetbrains.com/issue/TW-74430) — Build duration (secs) appears is wrong while build is checking out sources  
[**TW-73738**](https://youtrack.jetbrains.com/issue/TW-73738) — Single change page: &quot;No failed tests&quot; message is not displayed if build reported problems  
[**TW-74458**](https://youtrack.jetbrains.com/issue/TW-74458) — &quot;preferredInvestigationProject&quot; parameter isn&#39;t taken into account by &quot;Assign investigation to ...&quot; button  
[**TW-74512**](https://youtrack.jetbrains.com/issue/TW-74512) — &quot;preferredInvestigationProject&quot; parameter isn&#39;t taken into account by the investigation auto-assignee build feature  
[**TW-74359**](https://youtrack.jetbrains.com/issue/TW-74359) — S3 Storage Artifact publishing becomes interrupted when build has stopped regardless of the option to always publish artifact  
[**TW-70151**](https://youtrack.jetbrains.com/issue/TW-70151) — The option &quot;Show all personal builds&quot; does not affect the result of the test runs list request  
[**TW-64269**](https://youtrack.jetbrains.com/issue/TW-64269) — Improve buttons presentations on hover  
[**TW-72775**](https://youtrack.jetbrains.com/issue/TW-72775) — Remove unnecessary scroll in Test Mute popup.  
[**TW-74916**](https://youtrack.jetbrains.com/issue/TW-74916) — Build with two VCS roots fails with &quot;Builds in default branch are disabled in build configuration&quot; even though one VCS root has a desired branch  
[**TW-74797**](https://youtrack.jetbrains.com/issue/TW-74797) — Build may fail to upload artifacts in multi-node setup  
[**TW-74645**](https://youtrack.jetbrains.com/issue/TW-74645) — Build details dropdown always has horizontal scrollbar  
[**TW-74542**](https://youtrack.jetbrains.com/issue/TW-74542) — Bad browser tab title on Single Change page, if change does not exist  
[**TW-74745**](https://youtrack.jetbrains.com/issue/TW-74745) — Build artifact directory can be removed by clean-up process if the build configuration name or the project external id was upper(lower)cased  
[**TW-74985**](https://youtrack.jetbrains.com/issue/TW-74985) — it&#39;s not possible to configure using path-style URL access to s3 bucket  
[**TW-57482**](https://youtrack.jetbrains.com/issue/TW-57482) — TeamCity Service Message - Build Problem &amp; Block Messages Issue  
[**TW-74173**](https://youtrack.jetbrains.com/issue/TW-74173) — IntelliJ 2021.3 cannot be uploaded as a tool in TeamCity: &quot;Package is invalid&quot;  
[**TW-74505**](https://youtrack.jetbrains.com/issue/TW-74505) — Jacoco report service message with reportDir parameter does not generate report  
[**TW-74948**](https://youtrack.jetbrains.com/issue/TW-74948) — Suite name does not appear in the tests report sometimes  
[**TW-72705**](https://youtrack.jetbrains.com/issue/TW-72705) — Default branch build with checkout rules specified isn&#39;t reused in non-default branch build chain  
[**TW-74763**](https://youtrack.jetbrains.com/issue/TW-74763) — Pull Requests build feature uses wrong state parameter when querying for open merge requests  
[**TW-74992**](https://youtrack.jetbrains.com/issue/TW-74992) — Python (pytest) Runner incorrectly escapes coverage arguments preventing multiple arguments  
[**TW-62154**](https://youtrack.jetbrains.com/issue/TW-62154) — jetbrains.buildServer.configs.kotlin.v2018\_2.BuildSteps#dockerBuild should be marked as deprecated  
[**TW-74895**](https://youtrack.jetbrains.com/issue/TW-74895) — Missing build step metric names in Build -\&gt; Parameters -\&gt; Reported statistic values  
[**TW-74931**](https://youtrack.jetbrains.com/issue/TW-74931) — Install of current &quot;IntelliJ Inspections and Duplicates Engine&quot; fails  
[**TW-58149**](https://youtrack.jetbrains.com/issue/TW-58149) — Java API: Composite queued build returns agents for getCanRunOnAgents call  
[**TW-74750**](https://youtrack.jetbrains.com/issue/TW-74750) — PerfMon data for running builds is not available on nodes which are not responsible for the current build  
[**TW-74575**](https://youtrack.jetbrains.com/issue/TW-74575) — K8s plugin: PVC created together with Pod (Custom Pod Template) might be orphaned if Pod failed to create  
[**TW-74595**](https://youtrack.jetbrains.com/issue/TW-74595) — Apply Ring UI-like styles to Classic UI controls  
[**TW-72990**](https://youtrack.jetbrains.com/issue/TW-72990) — Build Page: Titles Should be Bigger  
[**TW-74794**](https://youtrack.jetbrains.com/issue/TW-74794) — Schedule trigger may trigger a build twice  
[**TW-74488**](https://youtrack.jetbrains.com/issue/TW-74488) — Misaligned quick links on build expanded view  
[**TW-74734**](https://youtrack.jetbrains.com/issue/TW-74734) — Builds list is not updated when a build is taken from the queue  
[**TW-31663**](https://youtrack.jetbrains.com/issue/TW-31663) — Confusing project without parent on Overview and Configure visible projects dialog  
[**TW-62530**](https://youtrack.jetbrains.com/issue/TW-62530) — Hide sidebar automatically after search was complete with the selection of the item  
[**TW-74824**](https://youtrack.jetbrains.com/issue/TW-74824) — Listener error when using TestNg 7.5  
[**TW-52638**](https://youtrack.jetbrains.com/issue/TW-52638) — Manual build Promote action promotes the build to all artifact dependencies in dependent build  
[**TW-74629**](https://youtrack.jetbrains.com/issue/TW-74629) — Not all of the parameters of a composite build are shown on the build parameters tab  
[**TW-74814**](https://youtrack.jetbrains.com/issue/TW-74814) — Build steps names collision causes incomplete list of build steps to be displayed  
[**TW-74656**](https://youtrack.jetbrains.com/issue/TW-74656) — Wrong PR number in teamcity.pullRequest.number parameter  
[**TW-74467**](https://youtrack.jetbrains.com/issue/TW-74467) — Health items are not updated when a user navigates through Sakura UI  
[**TW-74661**](https://youtrack.jetbrains.com/issue/TW-74661) — Account lockout when using ticket value in VCS Root with Perforce and active directory domain integrated authentication  
[**TW-73619**](https://youtrack.jetbrains.com/issue/TW-73619) — The &quot;Highlight my changes and investigations&quot; setting does not work in terms of highlighting my investigations  
[**TW-73667**](https://youtrack.jetbrains.com/issue/TW-73667) — Incorrect rendering of a combination of emoji and space in autogenerated avatars  
[**TW-74509**](https://youtrack.jetbrains.com/issue/TW-74509) — Don&#39;t collapse build problem section on cmd + click  
[**TW-73473**](https://youtrack.jetbrains.com/issue/TW-73473) — The pager on the Build Changes tab disappears after switching between pages in reverse order  
[**TW-74694**](https://youtrack.jetbrains.com/issue/TW-74694) — ConcurrentModificationException in UserInvestigationsCounterProvider on server start  
[**TW-74251**](https://youtrack.jetbrains.com/issue/TW-74251) — More visible icon for assigned investigation (build page)  
[**TW-73390**](https://youtrack.jetbrains.com/issue/TW-73390) — Align and rename &quot;Remote run&quot; label for a single personal change  
[**TW-74292**](https://youtrack.jetbrains.com/issue/TW-74292) — When creating or editing a VCS root using a connection button, a user must press it twice  
[**TW-74666**](https://youtrack.jetbrains.com/issue/TW-74666) — Build cannot stop probably because it is routed to incorrect node with id MAIN\_SERVER instead of actual id of the main node  
[**TW-74111**](https://youtrack.jetbrains.com/issue/TW-74111) — Fix nesting of publishing artifacts messages (verbose logging level)  
[**TW-74503**](https://youtrack.jetbrains.com/issue/TW-74503) — failed to get project vcs-roots via API when another project with no access has vcs root with same name  
[**TW-74614**](https://youtrack.jetbrains.com/issue/TW-74614) — AssertionError in HierarchyMessagesProcessor.process(HierarchyMessagesProcessor.java:70)  
[**TW-74601**](https://youtrack.jetbrains.com/issue/TW-74601) — Parameter vcsRoot.id.p4client has a different case compared with other parameters  
[**TW-74427**](https://youtrack.jetbrains.com/issue/TW-74427) — Secure FTP uploads fail when using the FTP Upload build runner  
[**TW-74522**](https://youtrack.jetbrains.com/issue/TW-74522) — Pull request build feature may fail to provide relevant branches in a build configuration if it is misconfigured in another build configuration that uses the same VCS root  
[**TW-74453**](https://youtrack.jetbrains.com/issue/TW-74453) — Dependency parameters are displayed as missing yet they have defined values  
[**TW-68545**](https://youtrack.jetbrains.com/issue/TW-68545) — git ls-remote process hanging on Windows agent  
[**TW-74454**](https://youtrack.jetbrains.com/issue/TW-74454) — Artifacts upload: retry OPTIONS requests  
[**TW-73288**](https://youtrack.jetbrains.com/issue/TW-73288) — UnPin clears existing tags on first attempt (Classic UI)  
[**TW-74138**](https://youtrack.jetbrains.com/issue/TW-74138) — Warning about unset parameter &quot;teamcity.agent.hostname&quot;  
[**TW-73534**](https://youtrack.jetbrains.com/issue/TW-73534) — Hide &quot;New configuration... / New subprojects...&quot; under &quot;Edit project...&quot; dropdown  
[**TW-74391**](https://youtrack.jetbrains.com/issue/TW-74391) — Increase Block Headers on the Build Page  
[**TW-74513**](https://youtrack.jetbrains.com/issue/TW-74513) — An avatar doesn&#39;t fit in expanded build line  
[**TW-72942**](https://youtrack.jetbrains.com/issue/TW-72942) — Add &quot;Build log&quot; Icon to &quot;Show full log&quot; button  
[**TW-73610**](https://youtrack.jetbrains.com/issue/TW-73610) — Consider placing &quot;Copy to clipboard&quot; link at the bottom of the stacktrace  
[**TW-74501**](https://youtrack.jetbrains.com/issue/TW-74501) — Background is not uniform for expanded build in queue  
[**TW-68851**](https://youtrack.jetbrains.com/issue/TW-68851) — &quot;Failed to process &#39;buckets&#39; request: Connection to &quot;s3.amazonaws.com&quot; is prohibited by TeamCity node restrictions&quot; on the secondary node  
[**TW-72581**](https://youtrack.jetbrains.com/issue/TW-72581) — Docker Wrapper doesn&#39;t escape ampersand when passing environment variables into containers  
[**TW-72565**](https://youtrack.jetbrains.com/issue/TW-72565) — Add a note about the usage of permissions in information about connections  
[**TW-74289**](https://youtrack.jetbrains.com/issue/TW-74289) — Canceled build status is not published to Space Merge Request  
[**TW-72927**](https://youtrack.jetbrains.com/issue/TW-72927) — Files with special Perforce characters in names are displayed and applied incorrectly in perforce patch  
[**TW-74181**](https://youtrack.jetbrains.com/issue/TW-74181) — Concurrent publishing of artifacts via service messages sometimes causes build to fail with FileNotFoundException when there is a large number of artifacts  
[**TW-70200**](https://youtrack.jetbrains.com/issue/TW-70200) — Per-usage license: secondary node reports &quot;Failed to send server usage data&quot;  
[**TW-74372**](https://youtrack.jetbrains.com/issue/TW-74372) — Links to the Tabs for the Expanded Build UI

### Performance Problem

[**TW-74787**](https://youtrack.jetbrains.com/issue/TW-74787) — Slow VCS root instances calculation because of inefficient code in DefaultToolVersionsImpl  
[**TW-74803**](https://youtrack.jetbrains.com/issue/TW-74803) — Should not cleanup all caches in AgentTypeStorage when agent pool is deleted or projects are removed from it.  
[**TW-75085**](https://youtrack.jetbrains.com/issue/TW-75085) — Slow order and visibility computation for overview page  
[**TW-72284**](https://youtrack.jetbrains.com/issue/TW-72284) — Finish composite builds immediately when the last dependency is finished  
[**TW-75006**](https://youtrack.jetbrains.com/issue/TW-75006) — SequenceLoader recalculates readiness of all items in it  
[**TW-74833**](https://youtrack.jetbrains.com/issue/TW-74833) — Slow loading of a large project configuration files probably because of ProjectsLoader.preprocessIds performing unnecessary file operations (network file system)  
[**TW-73704**](https://youtrack.jetbrains.com/issue/TW-73704) — Test details expanding can take a lot of time  
[**TW-73572**](https://youtrack.jetbrains.com/issue/TW-73572) — Build agents cannot send messages to server because server does not accept the messages, XmlRpcPoolQueueOverflownException (caused by PerfMon plugin)  
[**TW-74722**](https://youtrack.jetbrains.com/issue/TW-74722) — REST API calls fetching all branches of a particular project are slow and CPU consuming  
[**TW-74355**](https://youtrack.jetbrains.com/issue/TW-74355) — JDK chooser control can slow down loading of the build step editing page  
[**TW-74349**](https://youtrack.jetbrains.com/issue/TW-74349) — Builds List in Safari has low FPS when a user scrolls/moves pointer over it.  
[**TW-73923**](https://youtrack.jetbrains.com/issue/TW-73923) — Avoid re-calculating tests statistics for composite builds without currently running dependencies  
[**TW-74377**](https://youtrack.jetbrains.com/issue/TW-74377) — Not needed locks when loading Test history data  
[**TW-74376**](https://youtrack.jetbrains.com/issue/TW-74376) — TeamCity may inter-lock requests for some non-related builds  
[**TW-74281**](https://youtrack.jetbrains.com/issue/TW-74281) — Server may occupy all http-nio threads by blocked requests from agents

### Task

[**TW-75087**](https://youtrack.jetbrains.com/issue/TW-75087) — Add intermediate messages about current progress  
[**TW-74870**](https://youtrack.jetbrains.com/issue/TW-74870) — Unbundle REST API plugins for 2017.1 and 2017.2 protocols  
[**TW-74864**](https://youtrack.jetbrains.com/issue/TW-74864) — Unbundle CVS plugin  
[**TW-74834**](https://youtrack.jetbrains.com/issue/TW-74834) — Add agent and build count to thread dumps  
[**TW-74837**](https://youtrack.jetbrains.com/issue/TW-74837) — Add a possibility to get user avatar URLs in JSP  
[**TW-74570**](https://youtrack.jetbrains.com/issue/TW-74570) — Update README.md in Commit Status Publisher to reflect that it is built by Gradle  
[**TW-74687**](https://youtrack.jetbrains.com/issue/TW-74687) — Restrict exception and stack trace visibility to admin users  
[**TW-74429**](https://youtrack.jetbrains.com/issue/TW-74429) — Update copyright (2021 -\&gt; 2022)  
[**TW-74649**](https://youtrack.jetbrains.com/issue/TW-74649) — S3 storage UI redesign  
[**TW-74612**](https://youtrack.jetbrains.com/issue/TW-74612) — Allow to hide servlet container details on error page  
[**TW-72679**](https://youtrack.jetbrains.com/issue/TW-72679) — Health report about deprecating Java 1.8  
[**TW-72394**](https://youtrack.jetbrains.com/issue/TW-72394) — Investigate necessity of a new index in build\_state table (build\_type\_id, build\_id)

### Security Problem

8 security problems have been fixed.