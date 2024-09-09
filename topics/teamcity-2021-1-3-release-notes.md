[//]: # (title: TeamCity 2021.1.3 Release Notes)
[//]: # (auxiliary-id: TeamCity 2021.1.3 Release Notes)

__Build: 92914__  
__08 September 2021__
{instance="tc"}

### Feature

[**TW-64230**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64230) — New tab extending AgentDetailsTab does not show in experimental UI

### Usability Problem

[**TW-72618**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72618) — Add build problems about compilation errors from .NET runner  
[**TW-72627**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72627) — Customized parameter dropdown should sort the list of parameters

### Bug

[**TW-71781**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71781) — Error loading VCS changes during server startup  
[**TW-72616**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72616) — Build info can be split between running and finished on build overview  
[**TW-72528**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72528) — Build may be cancelled due to S3 upload exception and restarted  
[**TW-72861**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72861) — Python runner not fails if exception message is empty  
[**TW-70836**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70836) — BuildStatsBar popup unstable refresh  
[**TW-72804**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72804) — Composite build dependencies are not reused in some cases (when composite build starts before the dependency which could&#39;ve been reused)  
[**TW-72791**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72791) — Obsolete revision is used by a build if branch tip revision is quite old and is already unloaded from the changes cache  
[**TW-72780**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72780) — GitHub password authentication health report is shown even if VCS root is already fixed (via commit in DSL)  
[**TW-72688**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72688) — Kotlin Script build step fails to find maven dependency jars due to caching of the results of compilation of .main.kts script  
[**TW-69951**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69951) — Artifacts produced by `Download All` are not extractable on Windows  
[**TW-72753**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72753) — Teamcity may produce incorrect UID for the running process  
[**TW-67257**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67257) — Tests muted by TeamCity&#39;s test retry feature should show reasonable mute info  
[**TW-67571**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67571) — Unarchived subprojects are missing on the project overview for archived project in the Experimental UI.  
[**TW-72292**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72292) — S3ArtifactsPublisher doesn&#39;t retry when S3 responds with an internal server error  
[**TW-69059**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69059) — Test History page. First and last bars are cut on Success Rate chart.  
[**TW-72373**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72373) — Inconsistent time presentation on the same page  
[**TW-72343**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72343) — Expand only one test in the test tree on the build overview  
[**TW-55349**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-55349) — Perform cleanup of agent\_pool\_project table when some time passed since project has been removed  
[**TW-72330**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72330) — Sometimes build log skips errors from .NET runner  
[**TW-71935**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71935) — &quot;All N tests&quot; on build overview is not a link  
[**TW-71848**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71848) — Old &quot;muted test&quot; icon on Build overview in the Experimental UI  
[**TW-72587**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72587) — Cosmetic issue on the All Builds page  
[**TW-72097**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72097) — Exception &quot;Server health reporter failed&quot; for Pull requests with &quot;First attached GIT VCS root&quot;  
[**TW-69090**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69090) — Errors on agent page if agent is gone  
[**TW-67769**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67769) — Can&#39;t get k8s pod informations from agent with k8s plugin  
[**TW-71728**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71728) — &quot;Connection to &quot;api.github.com&quot; is prohibited&quot; on secondary node when fetching available Kotlin compiler versions  
[**TW-72566**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72566) — Build page: other people&#39;s personal builds are excluded from dependencies count  
[**TW-72540**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72540) — Broken &quot;Resume build queue&quot; action in the health report  
[**TW-72553**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72553) — Endless triggering in a branch in case of per-checkin trigger with &quot;Trigger a build on changes in snapshot dependencies&quot; option disabled  
[**TW-72180**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72180) — Unified diff created by mercurial cannot be used as a personal build patch  
[**TW-72392**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72392) — Time in &quot;Last message from the agent was received on:&quot; on build overview is not refreshed  
[**TW-72932**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72932) — Broken expanded build presentation in the experimental UI

### Performance Problem

[**TW-72482**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72482) — FinishBuildTrigger collects changes way too long when starting a build  
[**TW-72423**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72423) — Too big count in REST API calls on the agent and builds history tabs

### Task

[**TW-72511**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72511) — Install venv to linux docker agent images  
[**TW-72596**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72596) — Detecting Visual Studio 2022 preview versions

### Security Problem

2 security problems have been fixed.
