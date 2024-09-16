[//]: # (title: TeamCity 2021.1.2 Release Notes)
[//]: # (auxiliary-id: TeamCity 2021.1.2 Release Notes)

__Build: 92869__  
__02 August 2021__
{instance="tc"}

### Feature

[**TW-32486**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-32486) — Running a build as personal via custom run dialog should run all dependent builds also as personal  
[**TW-68391**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68391) — Provide some shortcut for Next button in build log search  
[**TW-72300**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72300) — Python detection logic: add a way to use a custom path / override auto-detected instance  
[**TW-64663**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64663) — Add message &#39;This build is hanging&#39; on the build overview page  
[**TW-68857**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68857) — Dependencies Chain: No information about reused builds from previous chain  
[**TW-48481**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-48481) — Performance Monitor should show the build log section for selected time period  
[**TW-70758**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70758) — Include a README with the build agent download  
[**TW-68134**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68134) — PerfMon: show build log with &#39;before&#39; messages  
[**TW-67098**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67098) — Report PerfMon in build runtime  
[**TW-49243**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-49243) — Performance Monitor should publish data for canceled build  
[**TW-68797**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68797) — Allow to filter build parameter tab to show only customized parameters  
[**TW-71924**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71924) — Experimental ability to fetch all branches available in remote on each fetch instead of fetching current state  
[**TW-71018**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71018) — Consider automatic auto-detection of python tests as &quot;pytest&quot; command, not &quot;file&quot;

### Usability Problem

[**TW-71737**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71737) — Apply filtering to unified diff patch according to build configuration checkout rules  
[**TW-71477**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71477) — Confusing health report about multi-node setup without HTTP proxy  
[**TW-66130**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66130) — Build log tab in tail mode by default is inconvenient for running builds  
[**TW-69344**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69344) — Experimental UI: Clicking on a line in the build log doesn&#39;t update focusLine in the url  
[**TW-71452**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71452) — &quot;No name&quot; can be shown in grouped tests view, if there are no suite/package, but there are a few child elements  
[**TW-70678**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70678) — UI suggestion, deduplicate two `Search` buttons  
[**TW-67583**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67583) — Deployment section: run deployment button shifts down after running the build  
[**TW-68006**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68006) — Information in inactive (opened in new tab) tab isn&#39;t loaded in experimental UI  
[**TW-65692**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65692) — Scrolling in a long build log in the new new UI generates dozens of history entries in the browser  
[**TW-71359**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71359) — Rename &quot;Move to top&quot; button on the Build log tab  
[**TW-72118**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72118) — Provide drag &amp; drop support for uploading a personal patch  
[**TW-72120**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72120) — Generate more specific personal patch change description for the uploaded unified diff patch  
[**TW-68907**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68907) — Dependencies Chain: No way to view details for a few builds in experimental UI  
[**TW-72041**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72041) — Inspections (ReSharper): Add info that Additional parameters should be newline-delimited to the UI

### Bug

[**TW-72484**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72484) — NoClassDefFound exception related to email notification  
[**TW-72095**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72095) — NuGet feed in 2021.1 returns 406 when it should return 404  
[**TW-71933**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71933) — Git submodule update fails on agent in 2021.1.1  
[**TW-72216**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72216) — TC Plugin: Unable to open tabs  
[**TW-67092**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67092) — Wrong number of pending changes are displayed in the tab counter after changing the branch  
[**TW-71731**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71731) — TeamCity agent cannot meet compatibility requirement  
[**TW-67689**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67689) — Overlapping text in a build log  
[**TW-69717**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69717) — &quot;Agent logs&quot; tab is not refreshed on agent connect/disconnect until page reload  
[**TW-72451**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72451) — Perforce checkout could fail with client mapping in some cases (with &quot;must create client &#39;tw-28076&#39; to access local files&quot; error)  
[**TW-72450**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72450) — &quot;Cannot build patch: java.lang.NullPointerException&quot; could occur with some client mappings  
[**TW-72430**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72430) — Enable/disable agent dialog: the select options popup renders behind  
[**TW-68799**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68799) — Default associated pool is not displayed in Authorized agent dialogue  
[**TW-68365**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68365) — Build log search: Next result button doesn&#39;t jump to each occurrence in one line  
[**TW-72314**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72314) — 404 alert on artifacts tab using local storage  
[**TW-72119**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72119) — Resetting buildsMetadata cache can cause failure to find NuGet packages in the built-in feed  
[**TW-72213**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72213) — .NET runner breaking change - vstest no longer supports varying target frameworks  
[**TW-66726**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66726) — No information about investigations assignee for build problem in experimental UI  
[**TW-72221**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72221) — Display information about unknown user in the Change details popup  
[**TW-72058**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72058) — Run and Run custom build buttons don&#39;t work after closing the pop-up error  
[**TW-72361**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72361) — Change red frame color for log preview on build overview page  
[**TW-69736**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69736) — Build log search: jumps after a few seconds  
[**TW-72291**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72291) — Arithmetic overflow error on attempt to compute total duration of tests  
[**TW-70731**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70731) — Artifact dependency changes shows &quot;Loading&quot; instead of number of files  
[**TW-64900**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64900) — Different usernames are displayed in the Changes tab and in Pending Changes tab  
[**TW-72321**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72321) — Parametrized finish build trigger may not fire, if its parameters is a subset of parameters of other trigger  
[**TW-65392**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65392) — Displaying stacktraces for &quot;Snapshot dependency failure&quot; build problems on the Overview page  
[**TW-70019**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70019) — Incorrect investigation options for flaky tests  
[**TW-72217**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72217) — Only one of the two build triggers with custom parameters gets executed  
[**TW-71736**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71736) — Twitching build columns when expanding build presentation on the build configuration page (experimental UI)  
[**TW-64795**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64795) — Build status in build configuration displays &quot;No changes&quot; if build was automatically triggered  
[**TW-53207**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-53207) — Dependency build is not marked as personal when the top build is triggered via Branch Remote Run Trigger  
[**TW-72298**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72298) — Excessive logging on agent start related to .NET detection  
[**TW-70580**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70580) — Configs DSL plugin issue: Failed to initialize Spring Context  
[**TW-72112**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72112) — Address the difference between how we calculate/show tests runs/successful rate in both UIs (ignored runs shouldn&#39;t be counted)  
[**TW-66869**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66869) — No information about time when build configuration sources will be cleaned after &quot;Enforce clean checkout&quot; action in experimental UI  
[**TW-72156**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72156) — Error collecting changes for VCS repository: Invalid Path from archived projects  
[**TW-72192**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72192) — &quot;Edit this VCS root&quot; link in the popup about a VCS problem does not include WebApp context  
[**TW-71974**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71974) — Missing pull requests branches for Azure DevOps  
[**TW-69374**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69374) — Python Custom Script reports tests from Service Messages only when(if) build step is finished  
[**TW-63968**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-63968) — Agents pages: add a link to the agents dashboard to all agents 404 pages  
[**TW-66606**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66606) — Agents sidebar: selecting an Agents Overview page doesn&#39;t scroll the sidebar to the top  
[**TW-72210**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72210) — &quot;Last successful builds&quot; range cannot be saved in Keep rule  
[**TW-62916**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-62916) — Sakura UI: &#39;Parameters&#39; - &#39;Reported statistic values&#39; tab is opened in classic UI  
[**TW-65287**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65287) — Personal builds change revision field is poorly displayed in new UI  
[**TW-72198**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72198) — unable to update project pointing to submodule with branch=. (special value indicating current branch from parent repo)  
[**TW-64899**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64899) — Displaying unknown TC users in changes popup and tab  
[**TW-65690**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65690) — Incorrect agent information if the Cloud Agent profile is selected in the Run Custom dialog  
[**TW-72154**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72154) — Context properties of a project with read only versioned settings may not be updated via REST API  
[**TW-72200**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72200) — Backup can fail on SQL server if test\_info table has more than 2 billions of rows  
[**TW-71810**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71810) — Scroll bar grows forever on Duplicates tab in Experimental UI  
[**TW-72181**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72181) — BuildServiceMessagesTranslator keeps all translators in an internal map and does not refresh them  
[**TW-67989**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67989) — Build log: Home shortcut not working  
[**TW-71991**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71991) — NuGet feed no longer contains published packages  
[**TW-72164**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72164) — Finish build trigger constantly adds new builds to the queue for the same finished build  
[**TW-72022**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72022) — Cloud Agent might be removed immediately after disconnect  
[**TW-71990**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71990) — Cloud instance start by a limited user might lead to odd behaviour  
[**TW-66836**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66836) — REST returns 403 on artifact lookup of none artifacts available  
[**TW-71954**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71954) — Binary Git patches are not supported in personal builds with uploaded unified diff  
[**TW-71921**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71921) — &quot;View script content&quot; link doesn&#39;t work for me  
[**TW-72018**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72018) — Build without VCS roots fails with &quot;Error while applying patch&quot; if run as a personal build with some unified diff patch even though the patch could be applied to its dependencies  
[**TW-72039**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72039) — Inspections (ReSharper): I&#39;m not able to add more than one &#39;Additional InspectCode parameters&#39;  
[**TW-71735**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71735) — Cleanup: if some keep rule has the &quot;Keep artifact dependencies&quot; option then the whole build configuration is considered as having this option  
[**TW-72003**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72003) — Inspections (ReSharper) no longer loads dotSettings file after update to Team City 2021.1.1.1  
[**TW-62140**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-62140) — Git plugin ignores checkout rules when computes applicable build configurations for a remote run  
[**TW-71687**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71687) — Cannot find build promotion with id: 121802658. Error occurred while processing this request. Request: GET &#39;/app/rest/ui/builds?...&#39;  
[**TW-71980**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71980) — Inspections (ReSharper): Output file is not specified error (with --output parameter)  
[**TW-70011**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70011) — Changing `VCS root ID` breaks Pull Request feature  
[**TW-52948**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-52948) — Personal patch is still applied/undone when VCS root is set to &quot;do not checkout&quot; mode  
[**TW-72030**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72030) — Custom build dialog silently closes on attempt to run a personal build with a dependency where personal builds are disabled  
[**TW-71858**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71858) — Move the line status column of the files to the left  
[**TW-71738**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71738) — TestOccurrences request does not do proper checks for requested fields in fastpath  
[**TW-69762**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69762) — /usr/bin/python3.x is not detected if there is no /usr/bin/python3  
[**TW-71803**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71803) — Pull requests plugin does not handle exceptions raised in EHCache

### Performance Problem

[**TW-72519**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72519) — Do not recalculate status text of finished composite builds in case of out of order builds finishing  
[**TW-72419**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72419) — High contention on attempt to load the same build into the cache from the several threads  
[**TW-71792**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71792) — Build configuration page: consecutive expanding/collapsing of the same build feels laggy  
[**TW-71948**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71948) — Excessive memory usage in StringPoolInstance in some cases (eg concurrent execution of long tests)  
[**TW-71979**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71979) — Slow processing of VCS triggers with triggering rules even if triggering rule matches all of the files of a commit

### Task

[**TW-71934**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71934) — Add the changesCollectingInProgress field to the build entity  
[**TW-71293**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71293) — Ask in which mode the server is starting if teamcity.server.nodeId is provided but there is no data directory yet

### Cosmetics

[**TW-72456**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72456) — The &quot;--&quot; delimiter in kotlin script runner seems to be superfluous

### Security Problem

6 security problems have been fixed.
