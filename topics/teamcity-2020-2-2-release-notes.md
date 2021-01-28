[//]: # (title: TeamCity 2020.2.2 Release Notes)
[//]: # (auxiliary-id: TeamCity 2020.2.2 Release Notes)

__Build: 85882__   
__28 January 2021__

### Feature

[**TW-62901**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-62901) — Create agent pool in the new UI   
[**TW-64315**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64315) — New Queue page   
[**TW-69146**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69146) — API for counters in queue page sidebar   
[**TW-68467**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68467) — Allow support for multline values in Secret Tokens   
[**TW-69226**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69226) — Add an ability to cancel long-executing tasks on the &#39;Settings Persist Status&#39; tab of the Diagnostics page   
[**TW-67635**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67635) — Support connections in the usage statistics reports   
[**TW-55062**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-55062) — Show tests in the failed order in build page   
[**TW-66708**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66708) — Delete secure token values   
[**TW-33525**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-33525) — Add the commit date as an attribute to VcsModification   
[**TW-65765**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65765) — Edit sidebar: add &#39;Reset order&#39; button   
[**TW-68964**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68964) — Split flake8 inspection messages to several categories   
[**TW-68713**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68713) — Split pylint inspection messages to several categories (i.e. by checker)   
[**TW-18233**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-18233) — Artifact tabs containing HTML should be able to handle links to a directory   
[**TW-61485**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-61485) — dotnet cli should skip dotnet-core-runtime installations and find dotnet-core-sdk only while searching for dotnet executable

### Usability Problem

[**TW-69767**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69767) — Error message when Perforce cannot commit settings to VCS is not clear   
[**TW-68783**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68783) — Agent pages: limit the width   
[**TW-66364**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66364) — The popup shoul be displayed beneath the chevron in the new header   
[**TW-69730**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69730) — Conflicting advice when setting up PR decoration (Azure)   
[**TW-69003**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69003) — Add a note near branch specification fields explaining what is a logical branch name and how it can be controlled   
[**TW-69651**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69651) — Add a link to documentation to the build steps conditions   
[**TW-69649**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69649) — Add description to &#39;Docker image platform&#39; field in Docker wrapper and Docker build runners   
[**TW-68078**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68078) — Edit Configuration should be View Configuration when UI editing is disabled.   
[**TW-67010**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67010) — Build overview page: &#39;Edit Configuration&#39; button jumps left while the page is loaded   
[**TW-68893**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68893) — Archived build configurations are shown in Dependencies Chain by default   
[**TW-68767**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68767) — Scroll doesn&#39;t move to the top of page after switching by pools or pages   
[**TW-67427**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67427) — Maximum build artifact file size (global build setting) is exceeded. Errors in build logs always show file size in bytes.   
[**TW-67217**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67217) — Versioned Settings UI: confusing warning icon when some secure tokens are missing   
[**TW-69185**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69185) — Add Description About How to Show and Hide Sidebar   
[**TW-69052**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69052) — Open the Overview page after the first login   
[**TW-69317**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69317) — Agent &quot;Build Runners&quot; tab should alpha-sort runners   
[**TW-69394**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69394) — Improve TeamCity-readme.txt

### Bug

[**TW-62246**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-62246) — Build log is empty for passed tests in experimental UI   
[**TW-69777**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69777) — S3 Artifact storage upload intermittent 403 failure   
[**TW-69705**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69705) — Re-run build dialogue: failed dependencies isn&#39;t re-used by default   
[**TW-67440**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67440) — re-run build not restoring Dependencies used   
[**TW-69050**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69050) — Warning &quot;This node cannot modify configuration files on disk&quot; is displayed on the secondary node after copying a value for a token from another project   
[**TW-69019**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69019) — Agent page: support &quot;Show date/time in my timezone&quot; setting   
[**TW-68809**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68809) — Add processing of test absence on the test history page (page 404)   
[**TW-69765**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69765) — TeamCity `docker events` processes may be not terminated   
[**TW-50931**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-50931) — Don&#39;t mark tests as passed when build is cancelled   
[**TW-68432**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68432) — Add checksum verification to Windows-based docker images   
[**TW-69492**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69492) — Python is not detected on Linux agent. IllegalStateException: System.getenv(&quot;path&quot;) must not be null   
[**TW-68919**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68919) — NaN build is running, undefined agent is idle is shown in the header during agents update   
[**TW-68213**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68213) — $commented magic word search doesn&#39;t work for non-pinned builds   
[**TW-68721**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68721) — Queued build page. Reasons are endlessly loading in Build Details.   
[**TW-69277**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69277) — Unauthorized agents page: redirect to the overview when the last agent was authorized   
[**TW-67073**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67073) — New UI: Agent summary screen gets 404 errors when agents deregister   
[**TW-69567**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69567) — The header is not shown in Classic UI   
[**TW-67056**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67056) — Build configuration overview page is not properly refreshed when the build is removed.   
[**TW-69618**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69618) — Commit Status Publisher may cause overflow of the thread pool it uses and of the multi-node task queue as a result   
[**TW-69744**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69744) — Cleanup: PriorityClassManagerImpl#savePriorityClasses calls can take too much time when there are a lot of removed build configurations   
[**TW-69719**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69719) — catalina.out: many warnings like &#39;org.apache.catalina.webresources.Cache.getResource Unable to add the resource&#39;   
[**TW-69690**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69690) — GitHub authentication: &#39;You don&#39;t belong to the allowed GitHub organization&#39; when the TeamCity OAuth is not approved in organization settings   
[**TW-69572**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69572) — 2020.2.1 agent Docker image doesn&#39;t upgrade properly against 2020.2.1 server with changes   
[**TW-69697**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69697) — Perform schedule trigger invocation after server restart   
[**TW-69462**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69462) — Issues tab is absent on build   
[**TW-68880**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68880) — Build queue pager shows wrong number of builds and wrong link, if requested page does not exist (too few builds)   
[**TW-69570**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69570) — Build can consistently take obsolete revision if branch pointer was moved and VCS settings changed   
[**TW-69089**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69089) — Reset all button in the Dependencies tab of the custom build dialog doesn&#39;t work   
[**TW-69506**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69506) — Possible general changes collection slow-down because of retry for git roots   
[**TW-69602**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69602) — Error calling method BuildServerListener.buildFinished for listener org.jetbrains.teamcity.docker.serverHealth.ImagePullingWithoutAuthHealthReport$1   
[**TW-67738**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67738) — Some versioned settings not applied unless new commit   
[**TW-64907**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64907) — Overview / Project: buttons &quot;Add new..&quot; appear, even if UI editing disabled prior to DSL   
[**TW-67149**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67149) — Build History page for agents is jumping during scrolling   
[**TW-69423**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69423) — Can&#39;t check for EC2 options if there&#39;s an unknown region   
[**TW-67301**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67301) — Agent page. Experimental UI shows enable/disable agent comment with guillemet quotation marks and redundant line break   
[**TW-66496**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66496) — Message about lack of permissions divided into several lines on Agents -\&gt; Overview (on pools) page   
[**TW-67349**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67349) — Information about test results isn&#39;t published in Bitbucket 7.5.0   
[**TW-69569**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69569) — &quot;Copy to clipboard&quot; action on the Docker tag opens a new tab with docker table   
[**TW-67345**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67345) — Excess and wrong statuses can be published on Builds tab in Bitbucket 7.4.1+   
[**TW-40782**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-40782) — TeamCity doesn&#39;t handle non-URL encoded +&#39;s in links inside reports   
[**TW-69400**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69400) — dotnet vstest command does not work in docker with error: The Test Logger URI &#39;console:verbosity=normal&#39; is not valid   
[**TW-69484**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69484) — &#39;Could not deserialize versioned settings status from JSON&#39; error   
[**TW-68703**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68703) — A new build configuration created in a non-default branch of versioned settings can break its build chain without reporting any problems   
[**TW-68769**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68769) — Queued builds list can be cached and displayed in wrong pool   
[**TW-69468**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69468) — Agent fails to checkout when using git 2.30   
[**TW-64656**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64656) — The same changes appear in two consecutive builds if a force push occurred   
[**TW-68728**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68728) — Queue page: actions button moved down if tags were added for a build   
[**TW-69445**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69445) — Notifications build feature: Suggestion of available parameters doesn&#39;t work for Send to EMail field   
[**TW-69459**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69459) — Build can be mistakenly marked as incomplete in the database if it finished on the secondary node but main node still thinks it is running   
[**TW-69303**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69303) — Unable to restart server run from jetbrains/teamcity-server (Windows based)   
[**TW-69442**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69442) — A change from dependency is not shown as pending in a branch but is shown in a build in this branch once it starts   
[**TW-69396**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69396) — Changes in dependent builds are affected by a build in default branch of some dependency   
[**TW-68666**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68666) — Parametrization is limited for new Build Feature - Notifications   
[**TW-65759**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65759) — Docker build additional arguments revert to --pull if set blank   
[**TW-57028**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-57028) — &quot;Project scope&quot; drop down in Investigate/Mute dialog can turn into text without ability to turn it select another project   
[**TW-62658**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-62658) — Build canceled message is written inside the step section on the third level   
[**TW-67529**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67529) — Improve warning message in the server log when Perforce server is down   
[**TW-69184**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69184) — VCS root from GitHub OAuth connection sometimes auto-populates credentials password, sometimes doesn&#39;t   
[**TW-68367**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68367) — Whitespace triggers Kotlin DSL patch generation   
[**TW-69337**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69337) — Custom Chart documentation is faulty   
[**TW-69356**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69356) — Calling the backup rest api results with an error: Responding with error, status code: 415 (Unsupported Media Type).   
[**TW-69360**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69360) — VCS trigger triggers builds in non default branches on old commits   
[**TW-69140**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69140) — User without global permission to edit group cannot edit group if it has no roles assigned   
[**TW-69372**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69372) — OOME: git integration may consume a lot of memory when collecting changes for submodules   
[**TW-69285**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69285) — gp3 EBS volumes cannot be used in AWS Cloud provider plugin spot fleet requests   
[**TW-69244**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69244) — Handle situation with non-working links in the custom report tab   
[**TW-68889**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68889) — Secondary node: Build Queue priorities are not replicated to the main node.

### Performance Problem

[**TW-69312**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69312) — Lots of threads hanging computing tests statistics for a composite running build   
[**TW-69685**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69685) — ObsoleteFilesCleaner thread produces a lot of garbage while it processes directories   
[**TW-69604**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69604) — Starting with 2019.2 VCS trigger works much slower if build configuration monitors tenths of thousands of branches   
[**TW-67892**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67892) — RecentlyFailedTestsCalculator may prevent build from starting   
[**TW-69359**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69359) — Inefficient generation of pre-signed URLs in S3 storage plugin   
[**TW-66715**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66715) — [S3 Storage] Excessive sockets usage for presigned upload   
[**TW-68835**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68835) — New Queue page sends a lot of HTTP requests   
[**TW-69520**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69520) — Possible high CPU usage in server side patch transferring code   
[**TW-68754**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68754) — The queued builds page is loading slowly when a lot of builds are added to the queue.   
[**TW-69291**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69291) — Build configuration history page is slow on first opening or reload (new UI)

### Task

[**TW-68436**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68436) — Update Kotlin version used in TeamCity DSL projects to 1.4.21   
[**TW-69749**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69749) — TeamСity Idea plugin requires IntelliJ 2019.3   
[**TW-69296**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69296) — Web-socket support of the create, update and delete agent pool events   
[**TW-69610**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69610) — Add more debug logging to Commit Status Publisher   
[**TW-69366**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69366) — Update copyright (2020 -\> 2021)   
[**TW-69227**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69227) — Add some logging about too long-executing persist tasks   
[**TW-67231**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67231) — Scalability: review all server configs persisting, add ability to change them from different nodes

### Security Problem

10 security problems have been fixed.