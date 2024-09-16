[//]: # (title: TeamCity 2021.2.1 Release Notes)
[//]: # (auxiliary-id: TeamCity 2021.2.1 Release Notes)

__Build: 99602__  
__29 November 2021__
{instance="tc"}

### Feature

[**TW-73700**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73700) — Provide ability to use remote run in Eclipse plugin for user with enabled 2FA  
[**TW-73769**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73769) — Add copy branch name button

### Bug

[**TW-73776**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73776) — Impossible to retrieve nuget packages from TeamCity NuGet feed when 2FA is enabled.   
[**TW-73580**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73580) — Maximum call stack size exceeded after enabling &quot;Group by project&quot; option on the build chain view in the new UI   
[**TW-73982**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73982) — .NET runner and C# script runner do not propagate TEAMCITY\_PROCESS\_FLOW\_ID environment variables to spawned processes   
[**TW-59663**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-59663) — &#39;Latest check for changes: 19:15 (unknown or unset)&#39; when changes are collected on secondary node   
[**TW-73725**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73725) — Error on load project settings from VCS for custom chart with build step duration metric: &quot;Unresolved reference: buildstepname&quot;   
[**TW-73122**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73122) — Remote run impossible from IDE with two-factor authentication enabled.   
[**TW-73791**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73791) — Azure agents not cycling after upgrade to 2021.2   
[**TW-73453**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73453) — S3 Storage Artifacts cache hashsums are invalid when artifacts stored in cache during upload   
[**TW-74012**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74012) — Errors when building windows docker images for teamcity agents   
[**TW-74060**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74060) — Checkout rules mapping is not applied to paths in uploaded unified diff patch   
[**TW-74042**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74042) — C# script runner may fail in docker dotnet container with &quot;Failed to create CoreCLR, HRESULT: 0x80004005&quot;   
[**TW-73535**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73535) — Error &quot;Could not obtain the list of repositories&quot; during creation a project using JetBrains Space connection   
[**TW-73998**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73998) — SSL certificate cannot be uploaded under Root project due to the lack of permissions   
[**TW-73253**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73253) — Changes in Experimental UI show directories as files   
[**TW-73307**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73307) — Perforce Swarm. Display correct warning when Perforce Swarm URL is malformed in the Commit Status publisher.   
[**TW-73086**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73086) — Single change page in new UI. Status is missing.   
[**TW-73935**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73935) — 2FA. Take into consideration possible clock drifts between a client application and a TeamCity server.   
[**TW-73746**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73746) — .net builds could fail when DeterministicSourcePaths set to true   
[**TW-65392**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65392) — Displaying stacktraces for &quot;Snapshot dependency failure&quot; build problems on the Overview page   
[**TW-73223**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73223) — IntelliJ IDEA plugin is incompatible with the latest IDEA EAP   
[**TW-73119**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73119) — Single change page: block with &#39;Download patch/Download patch to IDE/Open in GitHub/Run build with this change&#39; links is absent   
[**TW-73088**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73088) — Single change page in new UI. Display &quot;No failed tests&quot; message.   
[**TW-73851**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73851) — P4 Error message seems contradictory: &quot;RpcTransport: partial message read SSL receive failed. read: socket: The operation completed successfully ...&quot;   
[**TW-73665**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73665) — Remote run from non-modal commit dialog doesn&#39;t work   
[**TW-73938**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73938) — Dependent build may not start if it uses a parameter from a composite build   
[**TW-73955**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73955) — Only VCS name shown for non-admin users   
[**TW-73563**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73563) — Switching between Build Configurations leads to the JS Error   
[**TW-73134**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73134) — Single change page may show wrong builds list (show one build several times)   
[**TW-73057**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73057) — Pending changes popup is not refreshed on new changes   
[**TW-73705**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73705) — Changes tab does not show avatar in users selector, if a user is already selected   
[**TW-72944**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72944) — Incorrect authentication information in the http request info added to the thread name   
[**TW-73559**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73559) — Bad formatting for compilation errors in the new UI   
[**TW-73634**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73634) — java.net.UnknownHostException when fetching Git LFS files   
[**TW-72227**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72227) — Build unexpectedly started on new agent between authorization and re-pooling   
[**TW-73864**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73864) — Test duration chart does not work in the classic UI   
[**TW-73873**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73873) — Git VCS root empty username prevents lfs authentication and causes errors during checkout on agent   
[**TW-73845**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73845) — Agent may restart after upgrade without plugins   
[**TW-72434**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72434) — Some unified diff patches with multiple chunks per single file cannot be applied during the personal build   
[**TW-73591**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73591) — Remove redundant libs from docker plugin   
[**TW-73802**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73802) — Secondary node should be able to load a build configuration, VCS root or project by external id alias   
[**TW-73814**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73814) — Adding a Ruby Env Configurator in DSL creates an arbitrary agent requirement for &quot;env.AAAA&quot;   
[**TW-73759**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73759) — Support for DSA/DSS ssh keys has disappeared in 2021.1.4   
[**TW-73789**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73789) — Wrong link to pull request in Pull Request Details block for Bitbucket repos without projects   
[**TW-72568**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72568) — VCS root usages are updated incorrectly in case when VCS root is removed from a build configuration while the same VCS root is inherited via a template   
[**TW-73779**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73779) — `Error while parsing communication protocols returned by the server` if the agent is running under java 17   
[**TW-73745**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73745) — Builds with .NET SDK version \&lt; 6.0 failed with &quot;error MSB1008: Only one project can be specified.&quot; (on agents installed on paths with spaces)   
[**TW-73751**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73751) — Dependencies tab stack overflow   
[**TW-73620**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73620) — The &quot;Highlight my changes and investigations&quot; setting does not work in terms of highlighting my changes   
[**TW-73689**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73689) — Guest user doesn&#39;t see build revision in the experimental UI   
[**TW-73690**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73690) — &quot;Show files&quot; checkbox doesn&#39;t work for guest users in the experimental UI   
[**TW-73742**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73742) — &quot;Could not read data from the Dsl data dir&quot; warning because writing to system/pluginData/.../dslData.zip is prohibited   
[**TW-73688**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73688) — &quot;It&#39;s me&quot; link is shown for guest user on the Change page   
[**TW-73647**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73647) — Weird search result on attempt to find a project/configuration by keyword &quot;xml&quot;   
[**TW-45709**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-45709) — UUID clash when something is edited in UI and on disk simultaneously   
[**TW-73290**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73290) — Single change page. Switch tab shows &quot;Open in build log&quot; window if it was previously closed

### Performance Problem

[**TW-74099**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74099) — Slow fetch in case of many branches   
[**TW-73931**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73931) — Download build log request hangs after the connection is closed by the client   
[**TW-73960**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73960) — High CPU usage and potentially slower than before checking for changes process because of VcsSettingsTrackerImpl.findMissingRegularUsages   
[**TW-73718**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73718) — Very slow versioned settings change log for a project stored in XML format with many thousands of build configurations   
[**TW-66379**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66379) — Multiple new UI performance failures on build server   
[**TW-73786**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73786) — Lots of unnecessary REST API requests for /app/rest/ui/projects?fields= endpoint in case of removal of a large project   
[**TW-73817**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73817) — Very slow processing of config persisting tasks if there are thousands of them (caused by inefficient fetching of tasks from DB)   
[**TW-73788**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73788) — CacheEstimateCalculator.shutdown slows down server shutdown

### Usability Problem

[**TW-73549**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73549) — Select JetBrains space as connection type in Add connection dialog when user selects Space icon with no connection configured.   
[**TW-74046**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74046) — C# script tool help is hardly readable in light console mode   
[**TW-72736**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72736) — Add &quot;Open in build log&quot; icon   
[**TW-73756**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73756) — Server health report about disabled native git operations and git version should be more detailed   
[**TW-73708**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73708) — Custom Statistics Chart with pattern shows build step ID (buildStageDuration:buildStepRunner\_N) instead of build step name   
[**TW-73739**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73739) — Switch of the verbosity level on the new build log collapses all the nodes

### Task

[**TW-74054**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-74054) — Upgrade Perforce helix client to a more recent version   
[**TW-72680**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72680) — Auto update: check if auto update is possible before starting it (i.e. ensure that the currently used Java is supported by the new TeamCity version)   
[**TW-72860**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72860) — Replace usages of scheduled-to-remove `StringBuilderSpinAllocator`

### Security Problem

7 security problems have been fixed.