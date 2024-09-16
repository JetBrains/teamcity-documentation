[//]: # (title: TeamCity 2021.2.2 Release Notes)
[//]: # (auxiliary-id: TeamCity 2021.2.2 Release Notes)   

__Build: 99660__  
__11 January 2022__
{instance="tc"}   

### Usability Problem

[**TW-69110**](https://youtrack.jetbrains.com/issue/TW-69110) — Consider adding &quot;Create project&quot; button to the sidebar  
[**TW-72145**](https://youtrack.jetbrains.com/issue/TW-72145) — New black header: Issue with accessibility   
[**TW-74046**](https://youtrack.jetbrains.com/issue/TW-74046) — C# script tool help is hardly readable in light console mode   

### Bug

[**TW-74255**](https://youtrack.jetbrains.com/issue/TW-74255) — VCS trigger runs builds on old changes in obsolete branches after the VCS root settings changes and builds cleanup   
[**TW-74526**](https://youtrack.jetbrains.com/issue/TW-74526) — Build can start without dependencies sometimes   
[**TW-74287**](https://youtrack.jetbrains.com/issue/TW-74287) — Perforce Shelve Trigger&#39;s vcsRoot.\&lt;ID\&gt;.shelvedChangelist parameter is not passed to snapshot dependencies   
[**TW-74176**](https://youtrack.jetbrains.com/issue/TW-74176) — TeamCity 2021.2.1 counts parametrized .NET tests as one test   
[**TW-72694**](https://youtrack.jetbrains.com/issue/TW-72694) — It is not obvious that the build is running if its end time is not known   
[**TW-74142**](https://youtrack.jetbrains.com/issue/TW-74142) — Update .NET SDK 3.1 -\&gt; 6 in docker images   
[**TW-73852**](https://youtrack.jetbrains.com/issue/TW-73852) — Wrong code coverage for kotlin files   
[**TW-73485**](https://youtrack.jetbrains.com/issue/TW-73485) — unknown test name is displayed for the passed tests after the first loading of the page   
[**TW-72225**](https://youtrack.jetbrains.com/issue/TW-72225) — Cleanup rules list shows Keep rule branch filter always in lower case   
[**TW-73959**](https://youtrack.jetbrains.com/issue/TW-73959) — GraphQL API: make it possible to `unassignProjectFromAgentPool` recursively   
[**TW-74190**](https://youtrack.jetbrains.com/issue/TW-74190) — Versioned settings update process should not rely on low priority executor   
[**TW-74163**](https://youtrack.jetbrains.com/issue/TW-74163) — S3 storage Allowed unicode characters break multipart presigned upload   
[**TW-74228**](https://youtrack.jetbrains.com/issue/TW-74228) — Builds fail if Perforce server has high latency   
[**TW-74112**](https://youtrack.jetbrains.com/issue/TW-74112) — Gradle runner error: Could not find matching constructor for: java.io.File(String, File)   
[**TW-74094**](https://youtrack.jetbrains.com/issue/TW-74094) — Unexpected error occurred on server when assigning a role to a group   
[**TW-74338**](https://youtrack.jetbrains.com/issue/TW-74338) — SQL exception on attempt to execute &quot;optimize table build\_type\_vcs\_change&quot; statement   
[**TW-74386**](https://youtrack.jetbrains.com/issue/TW-74386) — CloudImage.getAgentPoolId() returns wrong result   

### Performance Problem

[**TW-74032**](https://youtrack.jetbrains.com/issue/TW-74032) — Sidebar loads excess fields that overload the server   
[**TW-72395**](https://youtrack.jetbrains.com/issue/TW-72395) — Low priority executor pool overflown with BuildTypeBranchesCacheUpdater tasks   
[**TW-74140**](https://youtrack.jetbrains.com/issue/TW-74140) — Slow cleanup because of inefficient SQL query in VcsChangeTableCleaner   
[**TW-74302**](https://youtrack.jetbrains.com/issue/TW-74302) — Inefficient cleanup of data under the pluginData/kotlinDslData slows down application of the versioned settings   
[**TW-74377**](https://youtrack.jetbrains.com/issue/TW-74377) — Not needed locks when loading Test history data   
[**TW-74376**](https://youtrack.jetbrains.com/issue/TW-74376) — TeamCity may inter-lock requests for some non-related builds   
[**TW-74281**](https://youtrack.jetbrains.com/issue/TW-74281) — Server may occupy all http-nio threads by blocked requests from agents   
[**TW-74050**](https://youtrack.jetbrains.com/issue/TW-74050) — Interlocking between UI threads and a thread which is applying changes from the versioned settings   

### Task

[**TW-72679**](https://youtrack.jetbrains.com/issue/TW-72679) — Health report about deprecating Java 1.8   
[**TW-73685**](https://youtrack.jetbrains.com/issue/TW-73685) — Update IntelliJ IDEA versions list available for download in tools   
[**TW-74357**](https://youtrack.jetbrains.com/issue/TW-74357) — Remove log4j classes from incremental-compiler.jar      
[**TW-73659**](https://youtrack.jetbrains.com/issue/TW-73659) — VCS updater: handle the case when update settings from VCS takes too much time

### Security Problem

1 security problem has been fixed.