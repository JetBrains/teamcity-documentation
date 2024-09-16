[//]: # (title: TeamCity 2021.2.3 Release Notes)
[//]: # (auxiliary-id: TeamCity 2021.2.3 Release Notes)     

__Build: 99711__  
__16 February 2022__
{instance="tc"}

### Feature  

[**TW-73850**](https://youtrack.jetbrains.com/issue/TW-73850) — Add compatibility with BitBucket&#39;s checks for merging pull requests.  

### Usability Problem  

[**TW-73596**](https://youtrack.jetbrains.com/issue/TW-73596) — Changes from unknown VCS users are not shown among avatars anyhow  

### Bug  

[**TW-68935**](https://youtrack.jetbrains.com/issue/TW-68935) — Insecure Tomcat connector attributes: missing secure attributes  
[**TW-74505**](https://youtrack.jetbrains.com/issue/TW-74505) — Jacoco report service message with reportDir parameter does not generate report  
[**TW-74948**](https://youtrack.jetbrains.com/issue/TW-74948) — Suite name does not appear in the tests report sometimes  
[**TW-74916**](https://youtrack.jetbrains.com/issue/TW-74916) — Build with two VCS roots fails with &quot;Builds in default branch are disabled in build configuration&quot; even though one VCS root has a desired branch  
[**TW-74763**](https://youtrack.jetbrains.com/issue/TW-74763) — Pull Requests build feature uses wrong state parameter when querying for open merge requests  
[**TW-74992**](https://youtrack.jetbrains.com/issue/TW-74992) — Python (pytest) Runner incorrectly escapes coverage arguments preventing multiple arguments  
[**TW-73608**](https://youtrack.jetbrains.com/issue/TW-73608) — Empty changes list on click to avatar for users with &quot;+&quot; character  
[**TW-74173**](https://youtrack.jetbrains.com/issue/TW-74173) — IntelliJ 2021.3 cannot be uploaded as a tool in TeamCity: &quot;Package is invalid&quot;  
[**TW-74885**](https://youtrack.jetbrains.com/issue/TW-74885) — Secondary node cannot update of the projects model in case when some project was removed by the main node  
[**TW-62154**](https://youtrack.jetbrains.com/issue/TW-62154) — jetbrains.buildServer.configs.kotlin.v2018\_2.BuildSteps#dockerBuild should be marked as deprecated  
[**TW-73509**](https://youtrack.jetbrains.com/issue/TW-73509) — No error on attempt to save avatar on a secondary node without &quot;Processing user requests to modify data&quot; permission  
[**TW-74895**](https://youtrack.jetbrains.com/issue/TW-74895) — Missing build step metric names in Build -\&gt; Parameters -\&gt; Reported statistic values  
[**TW-74547**](https://youtrack.jetbrains.com/issue/TW-74547) — S3 artifact storage is no longer working correctly after upgrade from TeamCity 2021.1.4 to 2021.2.1  
[**TW-69199**](https://youtrack.jetbrains.com/issue/TW-69199) — &quot;Detached from agent&quot; label is cropped in the classic UI  
[**TW-74881**](https://youtrack.jetbrains.com/issue/TW-74881) — Secondary node does not start if there are unfinished configuration persisting tasks created by it  
[**TW-74772**](https://youtrack.jetbrains.com/issue/TW-74772) — Should not add comments for Perforce Helix Swarm about personal builds not related to shelved changelists  
[**TW-72259**](https://youtrack.jetbrains.com/issue/TW-72259) — Info about snapshot dependencies in classic UI is broken  
[**TW-74824**](https://youtrack.jetbrains.com/issue/TW-74824) — Listener error when using TestNg 7.5  
[**TW-73670**](https://youtrack.jetbrains.com/issue/TW-73670) — Buttons &quot;Upload new image&quot; and &quot;Delete&quot; are available in other accounts for administrators without &quot;Modify user profile and roles&quot; permission  
[**TW-74592**](https://youtrack.jetbrains.com/issue/TW-74592) — Agent checkout may fail after force push with git 2.34+  
[**TW-74656**](https://youtrack.jetbrains.com/issue/TW-74656) — Wrong PR number in teamcity.pullRequest.number parameter  
[**TW-74661**](https://youtrack.jetbrains.com/issue/TW-74661) — Account lockout when using ticket value in VCS Root with Perforce and active directory domain integrated authentication  
[**TW-74746**](https://youtrack.jetbrains.com/issue/TW-74746) — Docker run does not work in teamcity-agent docker images  
[**TW-74709**](https://youtrack.jetbrains.com/issue/TW-74709) — Xcode Project Runner does not work with Xcode 13.2.x  
[**TW-74516**](https://youtrack.jetbrains.com/issue/TW-74516) — &quot;Error while applying patch&quot; after changing Checkout policy in a Git VCS root  
[**TW-69931**](https://youtrack.jetbrains.com/issue/TW-69931) — Changing build problems count can be confusing (switch between &quot;root cause problems&quot; / &quot;own problems&quot;)  
[**TW-74292**](https://youtrack.jetbrains.com/issue/TW-74292) — When creating or editing a VCS root using a connection button, a user must press it twice  
[**TW-68252**](https://youtrack.jetbrains.com/issue/TW-68252) — Python runner. Absolute path to Python script does not work  
[**TW-74453**](https://youtrack.jetbrains.com/issue/TW-74453) — Dependency parameters are displayed as missing yet they have defined values  
[**TW-74571**](https://youtrack.jetbrains.com/issue/TW-74571) — Agent cannot checkout source code from GitHub (You&#39;re using an RSA key with SHA-1, which is no longer allowed)  
[**TW-70053**](https://youtrack.jetbrains.com/issue/TW-70053) — Validate login/password for the max length  

### Exception  

[**TW-73441**](https://youtrack.jetbrains.com/issue/TW-73441) — SecurityException on a secondary (read-only) node (update failed\_tests table)  

### Performance Problem  

[**TW-74787**](https://youtrack.jetbrains.com/issue/TW-74787) — Slow VCS root instances calculation because of inefficient code in DefaultToolVersionsImpl  
[**TW-75004**](https://youtrack.jetbrains.com/issue/TW-75004) — Slow processing VCS trigger with enabled quiet period  
[**TW-74876**](https://youtrack.jetbrains.com/issue/TW-74876) — Every request from a logged in user checks existence of an avatar on disk  
[**TW-74833**](https://youtrack.jetbrains.com/issue/TW-74833) — Slow loading of a large project configuration files probably because of ProjectsLoader.preprocessIds performing unnecessary file operations (network file system)  

### Task  

[**TW-74861**](https://youtrack.jetbrains.com/issue/TW-74861) — Drop unused packages from the log4j1.12 library  
[**TW-74570**](https://youtrack.jetbrains.com/issue/TW-74570) — Update README.md in Commit Status Publisher to reflect that it is built by Gradle  
[**TW-73625**](https://youtrack.jetbrains.com/issue/TW-73625) — Return all possible commit authors if two users have the same vcsUsername  

### Security Problem  

4 security problems have been fixed.