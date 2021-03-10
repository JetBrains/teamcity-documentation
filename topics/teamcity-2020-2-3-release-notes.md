[//]: # (title: TeamCity 2020.2.2 Release Notes)
[//]: # (auxiliary-id: TeamCity 2020.2.2 Release Notes)

__Build: 86002__   
__10 March 2021__

### Feature

[**TW-66326**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66326) — update sbt in sbt plugin  
[**TW-67060**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67060) — Create project from URL: allow to specify the default branch  
[**TW-70106**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70106) — Allow to disable parameters update from agent completely (make it a plugin&#39;s responsibility)  
[**TW-63367**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-63367) — Agents screen loading state  
[**TW-69809**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69809) — Ability to compare builds dependencies  
[**TW-65898**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65898) — Agents overview: support tabs provided by plugins (Matrix, Diff and other)  
[**TW-64230**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64230) — New tab extending AgentDetailsTab does not show in experimental UI  
[**TW-69793**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69793) — Support running tests on assemblies in single test session via &quot;dotnet test a.dll b.dll c.dll&quot;  
[**TW-64198**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64198) — Remember the state of &quot;Hide archived projects associated with agent pools&quot; checkbox for the Agents Pool.  
[**TW-69145**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69145) — Add counters to Queue page sidebar

### Usability Problem

[**TW-69795**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69795) — Wait reason for queued builds is updated with a delay  
[**TW-67986**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67986) — Viewing full test stacktrace on build results requires additional clicks/scrolling  
[**TW-69510**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69510) — Hide/collapse basic auth login fields for customers using alternative auth mechanisms  
[**TW-68261**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68261) — Python runner. Provide some hints about how to specify test/inspect objects for pylint/pytest/flake8  
[**TW-69733**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69733) — Show build branch name in personal Slack notifications  
[**TW-63893**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-63893) — Sakura: don&#39;t hide tabs on overview pages  
[**TW-63928**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-63928) — Branches are not built by default when creating project from Git repository  
[**TW-70156**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70156) — Sort context parameters on versioned settings page (alphabetically)  
[**TW-66909**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66909) — Agent page: keep selected tab when switching between agents

### Bug

[**TW-68790**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68790) — New Queue page: estimated finish time and build duration are not displayed  
[**TW-70175**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70175) — Cannot globally re-enable Versioned Settings after upgrade  
[**TW-70471**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70471) — VCS trigger skips triggering on new pending changes after modification of the VCS root  
[**TW-70442**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70442) — Detached interrupted build can remain running on server  
[**TW-70116**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70116) — Could not find a test logger with AssemblyQualifiedName, URI or FriendlyName &#39;logger://teamcity/&#39;. on .NET 5.0.103  
[**TW-70046**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70046) — Cancelling build with service message doesn&#39;t work for builds with detached steps  
[**TW-69856**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69856) — .NET runner ignores Test case filter parameter  
[**TW-70415**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70415) — ERROR: out of shared memory  
[**TW-68822**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68822) — Problem with NuGet Dependency Trigger - Failed to check for package versions  
[**TW-70280**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70280) — Build log on overview displays verbose messages as normal ones  
[**TW-70419**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70419) — Incorrect pending changes in a build configuration with checkout rules after VCS root editing  
[**TW-70352**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70352) — Build can be marked as failed if it has non branched VCS root (Perforce, SVN) and implicit versioned settings VCS root and is triggered in a logical branch  
[**TW-67869**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67869) — External artifacts for composite builds aren&#39;t displayed on the secondary node  
[**TW-70193**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70193) — expired EC2 cloud instance can be terminated before the build is finished  
[**TW-69979**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69979) — Can&#39;t see Dependencies of a build in new UI while old shows them just fine  
[**TW-69965**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69965) — &quot;Build log messages&quot; block on Build Overview is not being refreshed  
[**TW-68734**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68734) — Wrong number of projects displayed in agent pool  
[**TW-70061**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70061) — Build configuration or template with ID &quot;\&lt;some id\&gt;&quot; has the same uuid error after template ID change with versioned settings enabled  
[**TW-70209**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70209) — Provide better DSL for TestNG reports in XML report processing build feature  
[**TW-70100**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70100) — Lots of projects were not loaded on server start which led to removal of several queued builds  
[**TW-67323**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67323) — Provide better DSL for XML report processing build feature  
[**TW-69605**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69605) — Empty tests list for personal builds in Experimental UI  
[**TW-18725**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-18725) — Agent on Mac exposes several JAVA\_MAIN\_CLASS\_XXX environment variables  
[**TW-70076**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70076) — Endless loop requests queued and non-queued builds  
[**TW-69962**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69962) — Just finished build has been removed by cleanup  
[**TW-69800**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69800) — &quot;Unauthorized&quot; section disappears from Agents sidebar when there authorized agents are disconnected.  
[**TW-68919**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68919) — NaN build is running, undefined agent is idle is shown in the header during agents update  
[**TW-70034**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70034) — New changes may not be attached to a build configuration if the configuration belongs to a project where versioned settings are enabled  
[**TW-69506**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69506) — Possible general changes collection slow-down because of retry for git roots  
[**TW-69893**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69893) — Builds do not start waiting to lock a single VCS Root instance for a long time, CPU 100%  
[**TW-69878**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69878) — Open Agent pool page by default when a new pool is created in Experimental UI.  
[**TW-69614**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69614) — Missed Start Page property in Kotlin DSL  
[**TW-67667**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67667) — Automatic Update hangs with &quot;Server was stopped to update, but didn&#39;t start in 120 seconds, see logs/teamcity-update.log for details.&quot;  
[**TW-69716**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69716) — Agent pools page in Classic UI should not redirect to the Experimental UI  
[**TW-69891**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69891) — Support EdgeHTML 18  
[**TW-68238**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68238) — &quot;Show root cause only \ own problems&quot; selector always shown on the build overview page  
[**TW-69906**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69906) — Copy build configuration with emoji - &quot;Failed to log action to audit. Incorrect string value&quot;  
[**TW-66626**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66626) — Incorrect build and files are displayed in Artifact Dependency Change section in new UI  
[**TW-69898**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69898) — TeamCity Updater: use the SSL certificates uploaded to root project  
[**TW-68583**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68583) — Python pylint inspection does not report &quot;Fatal errors&quot; as errors  
[**TW-68866**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68866) — New Agent Details page: Update Java dialog doesn&#39;t open automatically  
[**TW-69611**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69611) — Busy agent is shown as idle for user who doesn&#39;t have permissions to see the running build details  
[**TW-68880**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68880) — Build queue pager shows wrong number of builds and wrong link, if requested page does not exist (too few builds)  
[**TW-69745**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69745) — Cleanup: CleanupExtensionsExecutor#fixOldBuildsArtifactsDirectory calls can take a lot of time when there are a lot of builds that were created by old TeamCity

### Performance Problem

[**TW-70407**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70407) — Calculating short statistics does not cache results in runtime for new test failures for running builds  
[**TW-70458**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70458) — Slow processing of cleanupFinished event if a lot of changes are unloaded from cache at once and there are many thousands of build configurations  
[**TW-70302**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70302) — Calculating short statistics for a complex build with failed tests may hang UI for many users in a case of large build history  
[**TW-70311**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70311) — Additional copy files operation can slowdown freeze of queued builds settings  
[**TW-70235**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70235) — Slow working CommittedBuildsUpdater thread can accumulate a lot of running builds in its queue  
[**TW-70123**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70123) — Inefficient processing of builds deletion from builds metadata index  
[**TW-70121**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70121) — Endless loop in EMailNotificator.sendToAddress in case of enabled debug logging  
[**TW-70098**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70098) — Lots of build problems reported for some build on a secondary node can greatly reduce build queue processing time on the main server  
[**TW-70077**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70077) — Suboptimal import of tables during projects import  
[**TW-69992**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69992) — BuildQueueCountersByPoolNotificator constantly consumes CPU  
[**TW-68148**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68148) — NotificationProcessor threads can occupy significant amount of CPU while they compute build committers  
[**TW-69815**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69815) — BuildPromotionImpl.isIncomplete() invoked on a personal build which was interrupted because agent its disconnected can cause major slowdown

### Task

[**TW-69775**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69775) — Publish enum values for the REST API models  
[**TW-70336**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70336) — Update docker-compose and docker version in official agent Docker images  
[**TW-69980**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69980) — Change the link to Feedback page in TeamCity UI  
[**TW-70182**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70182) — Immediately show build triggered by me if there&#39;s only 1 (few) queued builds  
[**TW-69857**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69857) — Migrate to the new YouTrack REST endpoints  
[**TW-69340**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69340) — Support agent pool WS messages on agents pages

### Cosmetics

[**TW-68591**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68591) — Python script with non-ASCII symbols in name. Bad encoding in build log in case there are syntax errors.  
[**TW-69326**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69326) — Inconsistent capitalization in Python build step

### Security Problem

12 security problems have been fixed.