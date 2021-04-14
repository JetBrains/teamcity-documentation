[//]: # (title: TeamCity 2020.2.4 Release Notes)
[//]: # (auxiliary-id: TeamCity 2020.2.4 Release Notes)

__Build: 86060__   
__15 April 2021__

### Feature  

[**TW-64726**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64726) — Show statuses and counters of non favorite build types and projects in sidebar

### Usability Problem

[**TW-70933**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70933) — &quot;Managing Projects and Build Configurations&quot; page is not about managing projects  
[**TW-70635**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70635) — Create from URL fails for GitHub with Not Found (404) in case of password authentication  
[**TW-70601**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70601) — Don&#39;t show the artifacts isolation health report if artifacts URL isn&#39;t configured

### Bug

[**TW-71041**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71041) — Unable to re-run a build if there is no CUSTOMIZE\_BUILD\_REVISIONS permission for its dependency  
[**TW-70612**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70612) — Make the Parameters tab default again in comparison  
[**TW-70495**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70495) — Applying patch changes adds characters at the front of file  
[**TW-70557**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70557) — &quot;Too many open files&quot; error during agent startup on some systems  
[**TW-70207**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70207) — A lot of memory occupied by Lucene build indexer (600M ScoreDoc instances)  
[**TW-70475**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70475) — NuGet Trigger doesn&#39;t work with multi-node setup and enabled process trigger responsibility  
[**TW-70871**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70871) — AWS cloud agent machine is never removed  
[**TW-58305**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-58305) — GitHub Commit Hooks plugin does not always persist last commit hook information and may unreasonably clean up working commit hook settings  
[**TW-70899**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70899) — Python runer left script file in buildTemp directory  
[**TW-70890**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70890) — DSL patch is generated when adding a project parameter (if config version from settings.kts file does not equal to the current server config version)  
[**TW-58365**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-58365) — Amazon ECR Connection ignores IAM Role specified  
[**TW-70806**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70806) — Tests can point to the wrong position in the build log  
[**TW-67378**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67378) — VCS trigger may not trigger a build in case of a change in settings VCS root and branch move in a regular VCS root  
[**TW-66254**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66254) — Limit build artifacts highlight width in the experimental UI  
[**TW-70879**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70879) — Failed to fetch the issue, reason: Unexpected JSON type: class com.google.gson.JsonPrimitive  
[**TW-70631**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70631) — VCS trigger with quiet period does not trigger build on branch move  
[**TW-70457**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70457) — Schedule trigger triggers builds in obsolete branches if branches temporary disappear and then reappear again  
[**TW-49658**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-49658) — Schedule build trigger runs build without changes  
[**TW-70521**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70521) — Ignored maven.repo.local when not reading the pom  
[**TW-65641**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65641) — Disable jgit internal auto git gc  
[**TW-70793**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70793) — Copying of secure values on the Tokens tab can cause a commit of the project settings to VCS  
[**TW-70823**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70823) — SSH Exec: Username field is ignored for &#39;Default private key&#39; if there is an ssh config on the agent host  
[**TW-70796**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70796) — GitHub commits hook plugin is incompatible with TeamCity 2020.2  
[**TW-70804**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70804) — Audit: &#39;User was created&#39; records have incorrect value in User field  
[**TW-70834**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70834) — MultiNodesEvents usage prevents plugin from upload without server restart  
[**TW-70774**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70774) — TFS Workspace deletion  
[**TW-69873**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69873) — Don&#39;t show configure sidebar dialog for an empty server  
[**TW-70509**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70509) — Impossible to use repository with &quot;--config&quot;  
[**TW-70611**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70611) — Dependencies comparison doesn&#39;t work when there are no common dependent builds for chosen builds  
[**TW-67239**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67239) — Unexpected error &quot;bean newVersionBean not found within scope&quot; for non-administrators on the health item  
[**TW-70552**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70552) — Problem investigation with manual resolution removed automatically after successful build and clean-up

### Performance Problem

[**TW-70686**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70686) — Too much memory occupied by Lucene Document instance  
[**TW-70572**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70572) — High memory usage due to absence of limits in tracing code

### Task

[**TW-69316**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69316) — Github is deprecating support for password authentication for git operations

### Security Problem

5 security problems have been fixed.