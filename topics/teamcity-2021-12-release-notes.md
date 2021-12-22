[//]: # (title: TeamCity 2021.12 Release Notes)
[//]: # (auxiliary-id: TeamCity 2021.12 Release Notes)

__Build: 107109__  
__22 December 2021__

### Feature

[**TW-73700**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73700) — Provide ability to use remote run in Eclipse plugin for user with enabled 2FA.  
[**TW-73769**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73769) — Add copy branch name button  
[**TW-48449**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-48449) — Support Cloud profiles in Kotlin DSL project  
[**TW-73796**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73796) — Allow to create a build problem for every match found with &quot;Fail Build on Specific Text&quot; failure condition  
[**TW-73101**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73101) — Publish a build status to the Merge request in JetBrains Space  
[**TW-70296**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70296) — Add support for Space merge requests to Pull requests build feature  
[**TW-73161**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73161) — UI to manage agent pool projects  
[**TW-66588**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66588) — Provide a way to automatically choose some parent project as scope when assigning an investigation or muting tests  
[**TW-73967**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73967) — Report the build status in the head commit of the Space Merge request  
[**TW-73167**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73167) — Single change page. No &quot;Show projects hierarchy&quot; option on Builds tab  
[**TW-73691**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73691) — Disallow adding more builds in the build queue using REST requests if the build queue is too large

### Usability Problem

[**TW-69110**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69110) — Consider adding &quot;Create project&quot; button to the sidebar  
[**TW-72145**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72145) — New black header: Issue with accessibility  
[**TW-73739**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73739) — Switch of the verbosity level on the new build log collapses all of the nodes  
[**TW-73756**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73756) — Server health report about disabled native git operations and git version should be more detailed  
[**TW-72736**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72736) — Add &quot;Open in build log&quot; icon  
[**TW-73549**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73549) — Select JetBrains space as connection type in Add connection dialog when user selects Space icon with no connection configured.  
[**TW-69011**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69011) — UI to edit agent pools  
[**TW-29414**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-29414) — Include name of the deleted object into &quot;Are you sure you want to delete this project&quot; confirmation  
[**TW-61639**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-61639) — Improve Build overview tabs placement on expanded build area  
[**TW-74090**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74090) — Need to make more visible that there are several builds behind this configuration  
[**TW-74046**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74046) — C# script tool help is hardly readable in light console mode  
[**TW-74056**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74056) — Impossible to navigate back from Server Health page  
[**TW-72734**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72734) — Build log search: Don&#39;t enable Previous result button if only one result was found  
[**TW-59968**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-59968) — No need to change projects list in sidebar if the search field is selected and empty. Only after typing the letters  
[**TW-68390**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68390) — Build log search: make Next button search for new results starting from the place user has clicked  
[**TW-69949**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69949) — Show duration for currently running stages on Build log  
[**TW-68366**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68366) — Build log search: Next result button is disabled when the last result is reached  
[**TW-73875**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73875) — Assign agents/projects dialog is reloaded every time when any pool is changed.  
[**TW-73708**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73708) — Custom Statistics Chart with pattern shows build step ID (buildStageDuration:buildStepRunner\_N) instead of build step name

### Bug

[**TW-72225**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72225) — Cleanup rules list shows Keep rule branch filter always in lower case  
[**TW-73959**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73959) — GraphQL API: make it possible to `unassignProjectFromAgentPool` recursively  
[**TW-74190**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74190) — Versioned settings update process should not rely on low priority executor  
[**TW-73485**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73485) — \&lt;unknown test name\&gt; is displayed for the passed tests after the first loading of the page  
[**TW-74163**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74163) — S3 storage Allowed unicode characters break multipart presigned upload  
[**TW-74228**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74228) — Builds fail if Perforce server has high latency  
[**TW-74112**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74112) — Gradle runner error: Could not find matching constructor for: java.io.File(String, File)  
[**TW-74094**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74094) — Unexpected error occurred on server when assigning a role to a group  
[**TW-74338**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74338) — SQL exception on attempt to execute &quot;optimize table build\_type\_vcs\_change&quot; statement  
[**TW-74386**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74386) — CloudImage.getAgentPoolId() returns wrong result  
[**TW-74255**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74255) — VCS trigger runs builds on old changes in obsolete branches after the VCS root settings changes and builds cleanup  
[**TW-74142**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74142) — Update .NET SDK 3.1 -\&gt; 6 in docker images  
[**TW-74173**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74173) — IntelliJ 2021.3 cannot be uploaded as a tool in TeamCity: &quot;Package is invalid&quot;  
[**TW-73290**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73290) — Single change page. Switch tab shows &quot;Open in build log&quot; window if it was previously closed  
[**TW-45709**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-45709) — UUID clash when something is edited in UI and on disk simultaneously  
[**TW-73647**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73647) — Weird search result on attempt to find a project/configuration by keyword &quot;xml&quot;  
[**TW-73688**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73688) — &quot;It&#39;s me&quot; link is shown for guest user on the Change page  
[**TW-73742**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73742) — &quot;Could not read data from the Dsl data dir&quot; warning because writing to system/pluginData/.../dslData.zip is prohibited  
[**TW-73690**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73690) — &quot;Show files&quot; checkbox doesn&#39;t work for guest users in the experimental UI  
[**TW-73689**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73689) — Guest user doesn&#39;t see build revision in the experimental UI  
[**TW-73620**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73620) — The &quot;Highlight my changes and investigations&quot; setting does not work in terms of highlighting my changes  
[**TW-73751**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73751) — Dependencies tab stack overflow  
[**TW-73779**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73779) — `Error while parsing communication protocols returned by the server` if the agent is running under java 17  
[**TW-72568**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72568) — VCS root usages are updated incorrectly in case when VCS root is removed from a build configuration while the same VCS root is inherited via a template  
[**TW-73759**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73759) — Support for DSA/DSS ssh keys has disappeared in 2021.1.4  
[**TW-73591**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73591) — Remove redundant libs from docker plugin  
[**TW-73845**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73845) — Agent may restart after upgrade without plugins  
[**TW-73873**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73873) — Git VCS root empty username prevents lfs authentication and causes errors during checkout on agent  
[**TW-73864**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73864) — Test duration chart does not work in the classic UI  
[**TW-72227**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72227) — Build unexpectedly started on new agent between authorization and re-pooling  
[**TW-73559**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73559) — Bad formatting for compilation errors in the new UI  
[**TW-73705**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73705) — Changes tab does not show avatar in users selector, if a user is already selected  
[**TW-73057**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73057) — Pending changes popup is not refreshed on new changes  
[**TW-73134**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73134) — Single change page may show wrong builds list (show one build several times)  
[**TW-73563**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73563) — Switching between Build Configurations leads to the JS Error  
[**TW-73938**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73938) — Dependent build may not start if it uses a parameter from a composite build  
[**TW-73665**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73665) — Remote run from non-modal commit dialog doesn&#39;t work  
[**TW-73851**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73851) — P4 Error message seems contradictory: &quot;RpcTransport: partial message read SSL receive failed. read: socket: The operation completed successfully ...&quot;  
[**TW-73746**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73746) — .net builds could fail when DeterministicSourcePaths set to true  
[**TW-73119**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73119) — Single change page: block with &#39;Download patch/Download patch to IDE/Open in GitHub/Run build with this change&#39; links is absent  
[**TW-73086**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73086) — Single change page in new UI. Status is missing.  
[**TW-73307**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73307) — Perforce Swarm. Display correct warning when Perforce Swarm URL is malformed in the Commit Status publisher.  
[**TW-73253**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73253) — Changes in Experimental UI show directories as files  
[**TW-73791**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73791) — Azure agents not cycling after upgrade to 2021.2  
[**TW-73122**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73122) — Remote run impossible from IDE with two-factor authentication enabled.  
[**TW-73580**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73580) — Maximum call stack size exceeded after enabling &quot;Group by project&quot; option on the build chain view in the new UI  
[**TW-74060**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74060) — Checkout rules mapping is not applied to paths in uploaded unified diff patch  
[**TW-72434**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72434) — Some unified diff patches with multiple chunks per single file cannot be applied during the personal build  
[**TW-73535**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73535) — Error &quot;Could not obtain the list of repositories&quot; during creation a project using JetBrains Space connection  
[**TW-72604**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72604) — Build Progress messages are displayed inconsistently in Build Status  
[**TW-72962**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72962) — Deadlock found when trying to get lock (FailedTestsStorageImpl.markTestFailed)  
[**TW-74366**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74366) — Class is not shown for xunit tests with DisplayName  
[**TW-74253**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74253) — Agent pool agents tab: remove redundant headers  
[**TW-73852**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73852) — Wrong code coverage for kotlin files  
[**TW-73989**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73989) — Update sbt launcher to 1.5.5  
[**TW-74161**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74161) — Allow to disable/enable integration with Space merge requests without restart of server  
[**TW-63121**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-63121) — Max Agents parameter is not displayed in the agents pool page  
[**TW-74120**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74120) — Java code duplicates runner fails on some versions of JDK  
[**TW-74287**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74287) — Perforce Shelve Trigger&#39;s vcsRoot.\&lt;ID\&gt;.shelvedChangelist parameter is not passed to snapshot dependencies  
[**TW-73874**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73874) — GraphQL API: make ids globally unique  
[**TW-71862**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71862) — Display the number of projects that cannot be seen by user due to lack of permissions on GraphQL agent pool projects tab.  
[**TW-74284**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74284) — Endless agent upgrades loop  
[**TW-73772**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73772) — Favorite builds are sometimes not returned even though they are inside lookupLimit  
[**TW-73982**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73982) — .NET runner and C# script runner do not propagate TEAMCITY\_PROCESS\_FLOW\_ID environment variables to spawned processes  
[**TW-73984**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73984) — Agent pool projects page: it should not be possible to remove a project, assigned only with the default pool, and the project pool, from the default pool  
[**TW-71874**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71874) — Renaming/moving/archiving of projects should be reflected in GraphQL agent pool projects tab  
[**TW-73766**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73766) — Provide the limit for number of symbols in Agent Pool name in Experimental UI.  
[**TW-73453**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73453) — [S3 Storage] Artifacts cache hashsums are invalid when artifacts stored in cache during upload  
[**TW-73776**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73776) — Impossible to retrieve nuget packages from TeamCity NuGet feed when 2FA is enabled.  
[**TW-74088**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74088) — Build may be cancelled when the server is temporary unavailable  
[**TW-73725**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73725) — Error on load project settings from VCS for custom chart with build step duration metric: &quot;Unresolved reference: buildstepname&quot;  
[**TW-73973**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73973) — GraphQL API: add the `excludedCount` prop to the `AgentPoolProjectsConnection` type  
[**TW-73911**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73911) — Deadlock on attempt to store a new metadata storage key id  
[**TW-74012**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74012) — Errors when building windows docker images for teamcity agents  
[**TW-74042**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74042) — C# script runner may fail in docker dotnet container with &quot;Failed to create CoreCLR, HRESULT: 0x80004005&quot;  
[**TW-73138**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73138) — Agents parameters subtabs links leads to old UI  
[**TW-73998**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73998) — SSL certificate cannot be uploaded under Root project due to the lack of permissions  
[**TW-74040**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74040) — Investigations auto assigner should not automatically assign investigations on setUp/tearDown methods  
[**TW-73217**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73217) — Missing DSL for coverage parameters in IntelliJ IDEA Project runner  
[**TW-73939**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73939) — Agent pools. Images without running instances should be displayed on Agent Pool page in Experimental UI.  
[**TW-73935**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73935) — 2FA. Take into consideration possible clock drifts between a client application and a TeamCity server.  
[**TW-65392**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65392) — Displaying stacktraces for &quot;Snapshot dependency failure&quot; build problems on the Overview page  
[**TW-73223**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73223) — IntelliJ IDEA plugin is incompatible with the latest IDEA EAP  
[**TW-73088**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73088) — Single change page in new UI. Display &quot;No failed tests&quot; message.  
[**TW-73965**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73965) — TeamCity unexpectedly closes the test with suite, so test names loose suite names  
[**TW-66332**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66332) — Rendering SGR parameters in ANSI sequences in build log in new UI  
[**TW-73912**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73912) — Absence of an index on test\_metadata(key\_id) slows down startup and can cause failure to start if connection timeout is configured  
[**TW-73917**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73917) — IDEA&#39;s remote run fails to determine related run configurations  
[**TW-73955**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73955) — Only VCS name shown for non-admin users  
[**TW-73211**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73211) — Missing DSL for some parameters in .Net runner  
[**TW-73634**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73634) — java.net.UnknownHostException when fetching Git LFS files  
[**TW-73773**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73773) — Linux TeamCity agent running under WSL on windows detects .NET SDKs and runtimes installed on the windows host  
[**TW-73218**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73218) — Missing DSL for Script Argument parameter in PowerShell runner  
[**TW-73213**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73213) — Missing DSL for some parameters in Docker runner  
[**TW-73215**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73215) — Missing DSL for Distinguish fields parameter in Duplicates finder (Java) runner  
[**TW-73814**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73814) — Adding a Ruby Env Configurator in DSL creates an arbitrary agent requirement for &quot;env.AAAA&quot;  
[**TW-73789**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73789) — Wrong link to pull request in Pull Request Details block for Bitbucket repos without projects  
[**TW-73745**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73745) — Builds with .NET SDK version \&lt; 6.0 failed with &quot;error MSB1008: Only one project can be specified.&quot; (on agents installed on paths with spaces)  
[**TW-72760**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72760) — Password/access token is not extracted in case when separate VCS root is created using the Azure DevOps OAuth connection  
[**TW-72763**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72763) — Azure DevOps OAuth Connection: Handle errors that happened during project creation  
[**TW-72876**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72876) — Do not display Perforce personal build section in Custom Run dialog for user without &quot;Change build source code with a custom patch&quot; permission.

### Cosmetics

[**TW-71876**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71876) — Display zero counter if Agents pool contains no agents in Experimental UI.  
[**TW-72526**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72526) — Agents pools. Provide correct tooltip on assigned projects tab.  
[**TW-73006**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73006) — Scrollbars always show on agents page in new UI

### Performance Problem

[**TW-74032**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74032) — Sidebar loads excess fields that overload the server  
[**TW-72395**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72395) — Low priority executor pool overflown with BuildTypeBranchesCacheUpdater tasks  
[**TW-74140**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74140) — Slow cleanup because of inefficient SQL query in VcsChangeTableCleaner  
[**TW-74302**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74302) — Inefficient cleanup of data under the pluginData/kotlinDslData slows down application of the versioned settings  
[**TW-73817**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73817) — Very slow processing of config persisting tasks if there are thousands of them (caused by inefficient fetching of tasks from DB)  
[**TW-73786**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73786) — Lots of unnecessary REST API requests for /app/rest/ui/projects?fields= endpoint in case of removal of a large project  
[**TW-66379**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66379) — Multiple new UI performance failures on build server  
[**TW-73718**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73718) — Very slow versioned settings change log for a project stored in XML format with many thousands of build configurations  
[**TW-73960**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73960) — High CPU usage and potentially slower than before checking for changes process because of VcsSettingsTrackerImpl.findMissingRegularUsages  
[**TW-74099**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74099) — Slow fetch in case of many branches  
[**TW-73929**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73929) — Slow versioned settings change log for a commit with thousands of files on GitHub  
[**TW-74229**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74229) — Drop Edge 18 support  
[**TW-74050**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74050) — Interlocking between UI threads and a thread which is applying changes from the versioned settings  
[**TW-73931**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73931) — Download build log request hangs after the connection is closed by the client  
[**TW-56119**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-56119) — Improve Git getCurrentState operation in case when several threads attempt to perform ls-remote against the same repository  
[**TW-73732**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73732) — Build agents messages processing delay because of global synchronization in TestMetadataStorage  
[**TW-73788**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73788) — CacheEstimateCalculator.shutdown slows down server shutdown

### Task

[**TW-73360**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73360) — GraphQL: Implement bulk versions of mutations assignAgentWithAgentPool and assignCloudImageWithAgentPool  
[**TW-73749**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73749) — GraphQL: Implement authorizeAgent &amp; bulkAuthorizeAgents  
[**TW-71351**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71351) — GraphQL API: implement agents tree query missing properties  
[**TW-72860**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72860) — Please replace usages of scheduled-to-remove `StringBuilderSpinAllocator` class  
[**TW-72680**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72680) — Auto update: check if auto update is possible before starting it (i.e. ensure that the currently used Java is supported by the new TeamCity version)  
[**TW-74054**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74054) — Upgrade Perforce helix client to a more recent version  
[**TW-74357**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74357) — Remove log4j classes from webapps/ROOT/WEB-INF/plugins/jps-tool/agent/jps.zip!lib/scala-plugin/incremental-compiler.jar  
[**TW-73228**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73228) — Bundle IntelliJ IDEA 2021.2.3 with TeamCity 2021.2  
[**TW-74333**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74333) — Update Kotlin DSL to version 2021.12  
[**TW-74153**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74153) — Remove version from TeamCity Cloud servers footer  
[**TW-74160**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74160) — Enable integration with Space merge requests by default  
[**TW-73947**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73947) — GraphQL API: `connected` and `enabled` agent properties  
[**TW-73659**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73659) — VCS updater: handle the case when update settings from VCS takes too much time  
[**TW-73980**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73980) — Build log controller: allow searching from the end

### Security Problem

7 security problems have been fixed.