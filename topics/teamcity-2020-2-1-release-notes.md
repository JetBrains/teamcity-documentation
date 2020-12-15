[//]: # (title: TeamCity 2020.2.1 Release Notes)
[//]: # (auxiliary-id: TeamCity 2020.2.1 Release Notes)

__Build: 85633__  
__16 December 2020__

### Feature

[**TW-66202**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66202) — Allow S3 upload chunk size and minimum multipart threshold to be configurable   
[**TW-68940**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68940) — Maven detects not default tool if a pom file contains rule maven-enforcer-plugin/requireMavenVersion that satisfies the default tool condition.   
[**TW-65810**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65810) — Allow external connections from javaagent on the secondary node   
[**TW-68587**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68587) — Health report for a build configuration which does docker pull from Docker Hub without prior authentication   
[**TW-67068**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67068) — Builds duration chart for Test History Page   
[**TW-60700**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-60700) — Report progress of publishing to S3   
[**TW-60401**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-60401) — TeamCity S3 Artifact Storage - S3 timeout - Enhancement Request   
[**TW-68890**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68890) — Chain view: group composite builds by default   
[**TW-65251**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65251) — &#39;Promote&#39; action in dependencies list   
[**TW-68508**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68508) — .NET 5 in docker images   
[**TW-68158**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68158) — Agent compatible configurations tab: design how should agent incompatibilities look like

### Usability Problem

[**TW-67649**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67649) — Build page, dependencies tab: let the user to switch status filter regardless the page loading progress   
[**TW-69127**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69127) — Some data is presented in different order on Usage Statistic page on seconadary node.   
[**TW-68490**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68490) — Not able to scroll manually to above build log in Sakura UI   
[**TW-67621**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67621) — Change the list of links that drop out of the username   
[**TW-68422**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68422) — Build log search shows &quot;No results in the first 100,000 lines&quot; even if search is currently performed after 100,000 lines   
[**TW-68369**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68369) — Build log search trims leading and trailing whitespaces in a search string   
[**TW-68334**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68334) — Python runner. Command = &quot;File&quot;. Add hint about supported paths for &quot;File&quot; field   
[**TW-69005**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69005) — Build failure condition (change in metric) - add a log entry if build was found on default branch instead of feature one   
[**TW-67549**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67549) — Build tests tab does not allow to see test duration chart   
[**TW-66480**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66480) — .NET: unclear way to call a dotnet tool using a new &quot;custom&quot; command   
[**TW-68894**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68894) — Do not display version as \&lt;unknown\&gt; for disconnected build agents in the Experimental UI.   
[**TW-68971**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68971) — Show a warning on the Docker Info tab when the build pulls images without prior authentication   
[**TW-68705**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68705) — No way to configure oauth authentication in the simple mode   
[**TW-68828**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68828) — Authentication admin tab: allow to add authentication modules in the simple mode

### Bug   
[**TW-69274**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69274) — Return docker to teamcity-agent images (all tags)   
[**TW-69040**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69040) — .net runner and --launch-profile parameter   
[**TW-34480**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-34480) — TeamCity can poll Amazon too frequently   
[**TW-69263**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69263) — Build is not triggered in a branch despite pending changes presence (branch move &amp; change from fallback branch)   
[**TW-69255**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69255) — Cannot sign up/in using GitHub.com   
[**TW-68532**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68532) — Bitbucket cloud pull requests: pull request block disappeared after TeamCity update   
[**TW-69046**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69046) — Auto-detected pytest/flake8/pylint projects with depth=2+ do not work without editing   
[**TW-60676**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-60676) — TeamCity docker images should not ignore failures in custom user scripts   
[**TW-69006**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69006) — Agent with id &quot;xxx&quot; does not exist page is displayed after user switches from unexisting agent to existing one in Agents sidebar.   
[**TW-69080**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69080) — Secondary node may not start with error &#39;This node is not allowed to execute SQL query: SQL DML: insert into domain\_sequence&#39;   
[**TW-69056**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69056) — Artifacts of a new build can be removed by cleanup   
[**TW-42878**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-42878) — Starting builds can be lost on server restart   
[**TW-68629**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68629) — New Agent Detail page: no OS icon   
[**TW-69095**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69095) — Broken navigation in recently published Java Docs   
[**TW-68986**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68986) — Disable Agent dialog: align &#39;Disable&#39; button   
[**TW-68869**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68869) — Highlight an opened build in the Dependencies Chain   
[**TW-68881**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68881) — Build queue &quot;Show only my personal builds&quot; is reset after page refresh   
[**TW-69109**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69109) — Errors on editing Hub Settings on the secondary node.   
[**TW-69047**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69047) — Python build steps auto-detection sometimes does not find ini-files (pytest.ini) on depth=2   
[**TW-69178**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69178) — Branch is not propagated in promote build dialog in some cases   
[**TW-64037**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64037) — Don&#39;t fail the whole build because of failed tests if gradle step was not failed   
[**TW-69053**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69053) — Lots of &quot;Optimized task for build ... modification: &quot; messages in teamcity-server.log   
[**TW-68882**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68882) — Pull request plugin tends to synchronously (and unnecessarily so) request VCS hostings when build details page is displayed for a relevant build   
[**TW-65988**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65988) — Don&#39;t use &#39;Build dependencies have not been built yet&#39; wait reason for composite builds   
[**TW-68847**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68847) — Python runner can&#39;t have virtualenv when requirements.txt is missing   
[**TW-68935**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68935) — Insecure Tomcat connector attributes: missing secure attributes   
[**TW-69225**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69225) — TeamCity may stop executing config persisting tasks if server was killed   
[**TW-68843**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68843) — Secondary Node: Cannot create connections   
[**TW-66791**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66791) — java.lang.OutOfMemoryError: Java heap space during git clean on Windows agent: long file names case   
[**TW-69135**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69135) — Page autoscrolls to the bottom border of the header on a Build Page   
[**TW-69175**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69175) — Decrease default timeout for pending threads in NuGet feed   
[**TW-69208**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69208) — [REST API] testOccurrences request may fail if build authentication is used   
[**TW-58332**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-58332) — Obsolete revision can be taken by a build in case when build configuration was created then removed and added again with the same uuid   
[**TW-68746**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68746) — Build estimate can show incorrect time for agentless build steps   
[**TW-67378**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67378) — VCS trigger may not trigger a build in case of a change in settings VCS root and branch move in a regular VCS root   
[**TW-67070**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67070) — Checking for changes action does nothing if current configuration does not have own VCS roots but has snapshot dependencies   
[**TW-68722**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68722) — Multiple selection on the Pools sidebar in Queued builds page.   
[**TW-68692**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68692) — Python: show all internal logging only in Verbosity output level   
[**TW-68435**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68435) — Agents Statistics and Matrix tabs should respect agentless builds   
[**TW-68801**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68801) — Esc key doesn&#39;t close new dialogs in sakura UI   
[**TW-68638**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68638) — Broken projects pop-up in breadcrumbs   
[**TW-67003**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67003) — BuildLog: Page continuously scrolls down to the last place in buildLog   
[**TW-68424**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68424) — Build log search: &quot;Next result&quot; and &quot;Search&quot; buttons don&#39;t scroll page to the found line   
[**TW-68371**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68371) — Branch filter isn&#39;t always shown on the test history page   
[**TW-68860**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68860) — Commit Status Publisher. Test Connection to Bitbucket Cloud and AzureDevops publishers fails on the secondary node.   
[**TW-69108**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69108) — Agent incorrectly reports JDK locations for macOS 11.0/10.16   
[**TW-68451**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68451) — No build icon for running builds on Agent statistics   
[**TW-67278**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67278) — Notify user with Commit Status Publisher feature if Space Connection was removed   
[**TW-69092**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69092) — No way to choose Root project in notifications rules   
[**TW-69009**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69009) — Missing arrowhead expand button near Projects in the classic UI in the new header.   
[**TW-69097**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69097) — Build log search. No jump to the next result in there are leading/trailing whitespaces in a search string   
[**TW-68698**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68698) — Add logging to a build for case when snapshot dependencies settings were taken from a branch   
[**TW-68836**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68836) — Python: move &quot;Environment name&quot; field out of advanced   
[**TW-69079**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69079) — Flaky test always shows &quot;0 failures&quot; if muted with &quot;support test retry&quot; option   
[**TW-68059**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68059) — Inapplicable snapshot dependency changes still reported when such changes are in fact taken into account   
[**TW-67284**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67284) — Client ID for Space connection in the list of connections is not cut off as in other connections   
[**TW-69074**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69074) — Python plugin. No drive letter in build problem message on Windows agent   
[**TW-51454**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-51454) — &#39;ssh upload&#39; plugin adds &#39;execute&#39; bit to files   
[**TW-69018**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69018) — Import settings from VCS does not work on the secondary node   
[**TW-54059**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-54059) — Improve message and logging on error parsing msbuild file   
[**TW-68310**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68310) — Dragging of the &quot;window&quot; on the top graph stops when my mouse gets out of the graph   
[**TW-68852**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68852) — Impossible to upload/delete Maven settings on the secondary node.   
[**TW-69107**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69107) — .NET runner does not allow to pass dotCover command line arguments containing semicolon   
[**TW-69015**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69015) — Load project settings from VCS button does not work on the secondary node   
[**TW-69038**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69038) — Current Status isn&#39;t displayed in Version Settings tab on the secondary node   
[**TW-69048**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69048) — No way to view or add context parameters on secondary node   
[**TW-69039**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69039) — Don&#39;t fail build if automatic login fails, use warning instead   
[**TW-69078**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69078) — Java Exception on Agent Statistics tab   
[**TW-69084**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69084) — java.lang.IllegalArgumentException: Comparison method violates its general contract (Build estimates calculator)   
[**TW-69085**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69085) — VCS trigger with quiet period can trigger a build in a new branch on an older commit than the one which created this branch (only if checking for changes is done by the secondary node)   
[**TW-68034**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68034) — Layout in &quot;Run Custom Build&quot; dialog differs in Sakura and Main UI   
[**TW-68939**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68939) — &#39;ssh upload&#39; plugin put files always onto c:/   
[**TW-64191**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64191) — Editing tags of a dependency opens wrong tags   
[**TW-67081**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67081) — Incorrect popup is displayed for the dependency builds in the Experimental UI.   
[**TW-68806**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68806) — Two scrollbars are displayed in the Configure Sidebar dialog   
[**TW-65772**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65772) — Agents -\&gt; Agent Push tab. &quot;Loading. Please wait&quot; progress message is displayed with unfriendly header &quot;object MouseEvent&quot;   
[**TW-64331**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64331) — Dockerfile source &quot;File content&quot; overwrites &quot;File&quot; after source change   
[**TW-68401**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68401) — Long stacktrace is scrolled automatically to the top every ~5 seconds in a running build   
[**TW-68392**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68392) — Test History Page. Graph is shown incorrectly when all test run times are 0ms   
[**TW-68269**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68269) — Test History page. Correct vertical scale values displaying for tests with long durations.   
[**TW-68374**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68374) — Build log search: search phrase is displayed two times in build log and overlaps main text if found text has custom font (e.g. bold)   
[**TW-69001**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69001) — Build steps auto-detect can fail to find build steps with long polling interval   
[**TW-68872**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68872) — Add validation for Bitbucket Cloud PR username/password   
[**TW-68521**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68521) — Pull request plugin for Bitbucket Cloud: Username and password are displayed in the DSL view after changing of the authentication type   
[**TW-69042**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69042) — CleanupIdsGroupsTableConverter hangs during upgrade in some cases   
[**TW-68938**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68938) — Bug in OptimizeAndCleanupIdsGroupsTableConverter converter causes upgrade failure on PostgreSQL and Oracle databases   
[**TW-68824**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68824) — Python plugin does not autodetect pytest project   
[**TW-68689**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68689) — Python steps autodetection should suggest all available options   
[**TW-68997**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68997) — Do not auto-detect python step if there is only requirements.txt (virtualenv) and nothing else to do   
[**TW-69034**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69034) — Changes screen: action bar alignment is broken in case of a wide project select   
[**TW-68905**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68905) — Projects remove and rename actions are not processed correctly on the secondary node when the main node is stopped.   
[**TW-67559**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67559) — Do not remember recently opened tab for Pool\Overview on the agents page   
[**TW-63513**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-63513) — Switching from classic UI to the new one leads to the wrong destination on some agents pages   
[**TW-68593**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68593) — Removal agent on secondary node causes next agent(s) to fail to operate   
[**TW-68786**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68786) — &quot;c.onUnexpectedError is not a function&quot; error when the same id is changed simultaneously on two nodes.   
[**TW-68998**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68998) — Enable &quot;Test reporting via teamcity-messages&quot; for auto-detected pytest step   
[**TW-68904**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68904) — Impossible to copy/move/rename a project containing enforced settings template on the secondary node.   
[**TW-68883**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68883) — Problem setting enforced template configuration for a project from secondary node   
[**TW-67279**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67279) — Incorrect page is loaded when user switch to Experimental UI from the Pools tab.   
[**TW-68648**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68648) — Agent&#39;s page and sidebar are not refreshed when the cloud agent is stopped.   
[**TW-67287**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67287) — Agent page: sync agent title color in the sidebar and on the page   
[**TW-67151**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67151) — Wrong project agent pool name and link can be displayed on the build Overview page   
[**TW-68963**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68963) — Add &quot;debug-search&quot; to &quot;debug-all&quot; logging preset   
[**TW-68726**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68726) — Error &quot;Cannot set property &#39;innerHTML&#39; of null&quot; if token name contains not allowed symbols   
[**TW-68921**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68921) — Sakura UI: Indent between parts of the label in Dependencies | Chain   
[**TW-65460**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65460) — YouTrack issue link is broken in changes list view in new UI   
[**TW-68548**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68548) — GitHub Enterprise and GitLab CE/EE authentication: unclear error is displayed during login when https certificates weren&#39;t uploaded to TeamCity   
[**TW-68727**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68727) — Error &quot;Cannot read property &#39;resetPermissionsSelector&#39; of undefined&quot; after creation of token for other user   
[**TW-68732**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68732) — Previous token settings are displayed in the form of creation a new token   
[**TW-68724**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68724) — Add a note what user can&#39;t login using scope token   
[**TW-68096**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68096) — TeamCity statistics: sendBeacon doesn&#39;t work with CSRF protection

### Performance Problem

[**TW-69279**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69279) — Computing of build chain changes can be inefficient if called from VCS trigger (FindPromotionStrategyFactory.load &amp; ChangesRange.processChanges)   
[**TW-68183**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68183) — isAvailable is called for every SAKURA\* prefixed plugin   
[**TW-69100**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69100) — Slow processing of build messages because of TestName2IndexImpl.unloadUnusedTestNames()   
[**TW-69051**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69051) — RunningBuildsManager.findRunningBuildById can be expensive for just finished builds causing slower processing of buildFinished event   
[**TW-68967**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68967) — Slow admin UI on the secondary node because of read only projects filtering in AdminPermissionsUtil.getAllEditableProjects   
[**TW-68831**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68831) — Improve threading in Background Build Indexer

### Cosmetics

[**TW-66856**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66856) — Improve wording for the Incorrect Proxy Configuration health report   
[**TW-68118**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68118) — Python runner. Improve build log for build steps with Virtualenv

### Security Problem

11 security problems have been fixed.  