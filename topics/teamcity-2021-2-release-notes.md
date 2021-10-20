[//]: # (title: TeamCity 2021.2 Release Notes)
[//]: # (auxiliary-id: TeamCity 2021.2 Release Notes)

__Build: 99529__  
__20 October 2021__

### Feature

[**TW-12514**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-12514) — Add support for avatar  
[**TW-28423**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-28423) — Support reporting build status to Perforce Swarm  
[**TW-73115**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73115) — Support access token authorization option for Bitbucket Server in the Pull Requests plugin DSL extension  
[**TW-46634**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-46634) — Implement Multi Factor Authentication  
[**TW-41783**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-41783) — Ability to match dependencies by prefix or suffix in their id in reverse.dep. parameters  
[**TW-68899**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68899) — Impossible to pause or resume build queue on the secondary node.  
[**TW-73278**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73278) — Allow to create projects, build configuration and VCS root using Space connection  
[**TW-64956**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64956) — Allow authenticating through Space  
[**TW-68494**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68494) — Authentication via Azure DevOps  
[**TW-72197**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72197) — C# script runner  
[**TW-72735**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72735) — Extend &quot;Run custom build&quot; dialog to allow triggering of personal builds based on Perforce shelved changelists  
[**TW-11406**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-11406) — Support builds triggering for Perforce shelved changelists  
[**TW-39618**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-39618) — Allow to obtain P4 variables for different VCS Roots after checkout on agent  
[**TW-36751**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-36751) — Use perforce automatic labels  
[**TW-69739**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69739) — Single change page in new UI  
[**TW-64968**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64968) — Add possibility to show failed tests from the dependencies on the build overview page  
[**TW-73079**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73079) — TeamCity-Api-JS: add ServiceMessage typings  
[**TW-73238**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73238) — Update git, NET, JDK, mercurial in docker images  
[**TW-73067**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73067) — Add a boolean field &#39;Run tests in a single session&#39; for dotnet test and vstest commands  
[**TW-73374**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73374) — Use .NET 6 with C# script runner  
[**TW-73305**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73305) — Allow to authenticate with Access Tokens in IDEA plugin  
[**TW-73221**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73221) — Simplify uploading custom jacoco versions  
[**TW-72620**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72620) — Support GIT\_TRACE=1 when running git commands  
[**TW-73110**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73110) — Provide grace period for registered users without 2FA configured when two-factor authentication mode is set to mandatory.  
[**TW-73078**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73078) — Expose AlertService in ReactUI  
[**TW-73133**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73133) — Edit and remove agent pool UI  
[**TW-73284**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73284) — Add hashes to avatar URLs returned by REST  
[**TW-73283**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73283) — Allow admins to set or delete user&#39;s avatar  
[**TW-71044**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71044) — Provide dedicated Kotlin DSL for ReSharper inspections/duplicates  
[**TW-53318**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-53318) — Kotlin DSL for charts is missing  
[**TW-66277**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66277) — BuildFeature Kotlin DSL for JetBrains.SharedResources feature  
[**TW-72774**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72774) — Perforce Shelve Trigger. Add support for additional custom parameters for builds started via trigger  
[**TW-72770**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72770) — Support nuget version ranges  
[**TW-53284**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-53284) — Access denied when accessing an URL from Kotlin DSL project  
[**TW-72846**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72846) — Make containerd.io version configurable  
[**TW-60774**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-60774) — Add &#39;change from snapshot dependencies&#39; icon to the change line  
[**TW-72303**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72303) — C# script: auto-detect .csx files in a repository  
[**TW-63700**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-63700) — New UI: use new changes list on build configuration Pending Changes tab  
[**TW-72612**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72612) — API for searching messages in reverse order in the build log doesn&#39;t work as expected  
[**TW-64679**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64679) — Grouping and sorting agents on the All agents tab  
[**TW-70339**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70339) — Free disk space requirement should use &quot;git clean&quot; before removing the entire checkout directory

### Usability Problem

[**TW-73251**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73251) — Append custom CSP to the default one instead of overriding it  
[**TW-72801**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72801) — PowerShell on ARM64  
[**TW-73258**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73258) — Consider changing title in the browser tab on Single change page  
[**TW-71548**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71548) — Modify server health report that is displayed when artifacts cannot be cleaned up by plugin.  
[**TW-73250**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73250) — Add copy revision icon to the Single change page  
[**TW-72781**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72781) — Inverse default order by duration on the new build Tests tab  
[**TW-73176**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73176) — Single change page. &quot;Author&quot; does not show full Teamcity/VCS username information  
[**TW-73106**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73106) — Single change page: not obvious how to find &quot;it&#39;s me!&quot; link for unknown VCS user  
[**TW-73214**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73214) — Single change page: always two-lines build presentation  
[**TW-65578**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65578) — Make &quot;show/hide sidebar&quot; button more visible  
[**TW-57643**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-57643) — Shortcut to easily &quot;Copy&quot; the name of a failed test to Clipboard  
[**TW-69661**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69661) — Build Status text colors are hard to distinguish for people with impaired color perception  
[**TW-71443**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71443) — Add a way to see more than 5 child elements for grouped tests on build overview page  
[**TW-73146**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73146) — Sakura: I can&#39;t select a test name  
[**TW-64101**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64101) — Allow to show more failed tests on the build summary page without resorting to Show all  
[**TW-72881**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72881) — Confusing Build Queue Message: Build with Shared Resource read lock waiting on build with read lock.  
[**TW-72669**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72669) — Move the External Changes button for the new UI  
[**TW-71654**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71654) — Expanded build: Stacktrace No stacktrace confusing message  
[**TW-72828**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72828) — Python runner DSL unclear error if command is not specified  
[**TW-68383**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68383) — Build log search: add Previous result button for jumping to the previous occurrence in log  
[**TW-64360**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64360) — it&#39;s not obvious that I need to configure favorites in the sidebar if I want to add some projects to the \&lt;Projects\&gt; dashboard  
[**TW-72561**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72561) — Open &quot;How to connect to the Jetbrains Space?&quot; in a new tab  
[**TW-72541**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72541) — Add a Copy URL button for the JetBrains Space Connection hint  
[**TW-66770**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66770) — Investigation information missing on Sakura project home  
[**TW-72459**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72459) — Display build data as a table  
[**TW-69879**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69879) — Use different presentations for hierarchy and archived projects for the agent pool.  
[**TW-72096**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72096) — Some popups are rendered one over another and overlap each other  
[**TW-62981**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-62981) — No way to edit tags quickly in the new build page  
[**TW-72255**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72255) — Show errors in the experimental UI in a more user-friendly form

### Bug

[**TW-60864**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-60864) — Support personal access tokens for Bitbucket Server in Pull Requests plugin  
[**TW-73100**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73100) — Obsolete revision can be taken by a build if it&#39;s configuration was not available while changes affecting it were detected  
[**TW-60938**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-60938) — Incorrect revision can be taken by a build in a branch if the branch was moved to a commit which was detected in the same VCS root before the builds&#39; build configuration was created  
[**TW-73071**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73071) — Showing composite build parameters from build.finish.properties in the UI  
[**TW-73452**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73452) — Changes list is not filtered by user name after clicking on user avatar  
[**TW-67322**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67322) — Provide better DSL for Ruby environment configurator build feature  
[**TW-72368**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72368) — Provide ability to open changes list in dependent build from changes tab of current build.  
[**TW-72447**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72447) — &#39;Change from snapshot dependencies&#39; icon has no information about build configuration and build number  
[**TW-73476**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73476) — No user name is shown on hover user&#39;s avatar, if there are changes from one user  
[**TW-73530**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73530) — Endless loading on the build configuration page in the classic UI  
[**TW-73109**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73109) — Incorrect response when empty password is entered in Two-Factor Authentication form.  
[**TW-72991**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72991) — Confusing warning on clean-up page when there were failures during cleaning-up artifacts from s3  
[**TW-71394**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71394) — Do not display hints for hidden settings.  
[**TW-70081**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70081) — SQL deadlock when cleanup is starting at the same time that a large amount of builds are being triggered by schedule  
[**TW-73393**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73393) — Warning in teamcity-server.log from Single change page: Unknown route parameter :changeId. Can&#39;t check access.  
[**TW-73503**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73503) — Incorrect handling of duplicates in test\_metadata\_dict leads to a missing metadata in the dictionary  
[**TW-73443**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73443) — Add REST API handler for permission check for commit message editing  
[**TW-73240**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73240) — Single change page may not show long files list (overlapping file names)  
[**TW-73285**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73285) — Agent pools. Path to subprojects is displayed incorrectly when some parent project is archived.  
[**TW-73332**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73332) — Missing DSL for Rake build step  
[**TW-72968**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72968) — Email verification warning is not shown in the Sakura UI  
[**TW-73257**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73257) — Provide better DSL for issue trackers  
[**TW-73320**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73320) — Create project from Space: wrong counter of found repositories  
[**TW-69315**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69315) — S3 Artifacts storage doesn&#39;t use Artifacts Cache  
[**TW-73318**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73318) — Trigger&#39;s custom option &quot;Delete all files in checkout directory before each snapshot dependency build&quot; does not work for composite builds  
[**TW-72560**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72560) — Rename Space auth module type  
[**TW-73220**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73220) — Missing DSL for Xcode Project runner  
[**TW-73216**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73216) — Support better DSL for FxCop runner  
[**TW-73219**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73219) — Missing DSL for Simple Build Tool (Scala) runner  
[**TW-73111**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73111) — Single change page shows wrong change for patches/remote run  
[**TW-73401**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73401) — Long change column text overlaps with hovered avatars  
[**TW-54307**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-54307) — Builds are not reused from default branch if branch is created from a revision which is excluded by checkout rules  
[**TW-69828**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69828) — There is no warning about a lack of permissions on the Queue page  
[**TW-71356**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71356) — GraphQL API: support agent pool projects connection mutations  
[**TW-73302**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73302) — Incorrect behavior of Show All N Items action  
[**TW-68459**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68459) — JS error on the project page in the classic UI  
[**TW-72933**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72933) — Clean-up base rules form is always editable  
[**TW-71124**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71124) — Can&#39;t disable clean-up keep rule modification with role permission  
[**TW-67671**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67671) — jQuery with version 1.12.1 that used at login page has four known medium vulnerabilities  
[**TW-69540**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69540) — Promote uses upper limit revision if promoted build was built on a revision which is not reachable from the current VCS root instance  
[**TW-73127**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73127) — Broken zip artifact can prevent download of other artifacts of a composite build  
[**TW-61244**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-61244) — Changes to the Rake runner options enabled by default are not reflected in the Kotlin DSL  
[**TW-73321**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73321) — /Volumes/teamcity/agent/temp/globalTmp/depXXXarch\_temp files can consume/leak agent disk space  
[**TW-69877**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69877) — Adapt trends presentation for en empty build history cases  
[**TW-70550**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70550) — Number of tests on build overview is not synchronized with list of tests  
[**TW-54638**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-54638) — S3 Storage: log a user-friendly message if there were connection problems during artifacts cleanup  
[**TW-73200**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73200) — unset: bad argument count  
[**TW-72824**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72824) — Hanging SlackWebApiImpl.request leads to exhausted HTTP thread pool  
[**TW-71360**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71360) — Missing DSL for IDEA tool version (inspections/duplicates)  
[**TW-71680**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71680) — &quot;Discard literals&quot; option in absent in ReSharper dupFinder DSL  
[**TW-73046**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73046) — Missing DSL for SSH Exec  
[**TW-73178**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73178) — Run/Edit/Actions buttons for build configurations are absent  
[**TW-67317**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67317) — There is no DSL for NuGet Dependency Trigger  
[**TW-73169**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73169) — GitLab.com connection suggests incorrect URL for registering the application  
[**TW-73105**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73105) — FTP upload. Incorrect DSL is generated when AuthMethod is changed.  
[**TW-73099**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73099) — Error on commit Deployer SSH/SMB/Container steps to Versioned settings after TeamCity upgrade  
[**TW-73085**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73085) — Perforce Shelve trigger does not work on secondary node  
[**TW-71621**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71621) — Error while generating build config from Kotlin: jetbrains.buildServer.configs.kotlin.v2019\_2.BuildFeature  
[**TW-73047**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73047) — Missing DSL for Container Deployer  
[**TW-73044**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73044) — Missing DSL for SSH Upload  
[**TW-73045**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73045) — Missing DSL for SMB Upload  
[**TW-69465**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69465) — java.lang.IllegalStateException: Bad request: no action is found for &quot;userGroups&quot;  
[**TW-73033**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73033) — Do not allow to remove SSH keys used for CloudFront in S3 artifacts storage.  
[**TW-72307**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72307) — It&#39;s possible to copy only visible part of a build log in the experimental UI  
[**TW-72763**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72763) — Azure DevOps OAuth Connection: Handle errors that happened during project creation  
[**TW-71092**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71092) — NuGet feed: An invalid cache entry was found for URL &#39;...index.json&#39; and will be replaced  
[**TW-72872**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72872) — Build log preview and timeline are displayed with a long delay  
[**TW-67316**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67316) — Provide better DSL for Branch Remote Run Trigger  
[**TW-72825**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72825) — Kotlin DSL uses build feature defaults for project feature with same type  
[**TW-72639**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72639) — Request for buildTypes with a given change does not return all of them  
[**TW-72642**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72642) — Request for builds with a given change does not return all of them  
[**TW-72760**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72760) — Password/access token is not extracted in case when separate VCS root is created using the Azure DevOps OAuth connection  
[**TW-68633**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68633) — git command std error is reported to WARN category in teamcity-vcs.log on agent  
[**TW-66896**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66896) — &#39;No build configurations&#39; on Project overview takes too much space  
[**TW-72792**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72792) — No way to publish an artifact with Cyrillic name using S3 publisher  
[**TW-59344**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-59344) — Cleanup does not clean an artifact dependency build despite &quot;do not prevent cleanup&quot; option in the using build configuration, if the using one has any snapshot dependency  
[**TW-72915**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72915) — Msbuild fails with system.OutDir parameter with space and trailing backslash in path  
[**TW-72864**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72864) — Better DSL for Azure DevOps OAuth Connection  
[**TW-72857**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72857) — Azure DevOps OAuth connection: no validation message &quot;Application ID must not be empty&quot;  
[**TW-72580**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72580) — System properties serialized incorrectly to the response file  
[**TW-70904**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70904) — Build Overview doesn&#39;t preserve service message output formatting  
[**TW-67638**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67638) — Build log: Selected line resets on new messages  
[**TW-72865**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72865) — The wrong number of storage usages is shown storage details popup  
[**TW-71556**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71556) — VCS roots from dependencies are duplicated on the Changes page in the experimental UI  
[**TW-72693**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72693) — Project developer can see enable/disable self-hosted agent button but has no grants to do it  
[**TW-72759**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72759) — Click on the &quot;Reported statistic values&quot; sub tab can lead to a double header  
[**TW-72664**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72664) — &quot;Show all&quot; from pending changes popup opens changes with &quot;All branches&quot; filter  
[**TW-71638**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71638) — Two line change pop-up in case of many changed files (experimental UI)  
[**TW-72652**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72652) — Link N pending change(s) may lead not to Pending changes tab if changes were in branches with special symbols in the name (#)  
[**TW-71936**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71936) — Download running build log link isn&#39;t aligned property (long step name)  
[**TW-72707**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72707) — Perforce Shelved Trigger. The personal build should be triggered when keyword is added to description of changelist with shelved files.  
[**TW-63082**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-63082) — There are no information about changes, artifacts, and actions button in deployment section in the build page  
[**TW-72628**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72628) — Secondary node doesn&#39;t pause build queue on Insufficient disk space (main node is not up)  
[**TW-72645**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72645) — A &quot;tool&quot; field should be visible when a default tool version is not specified  
[**TW-72328**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72328) — Incorrect counter is displayed on Pending changes tab when different branches contain changes.  
[**TW-72600**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72600) — C# script: make &quot;TeamCity C# script tool&quot; field mandatory  
[**TW-72374**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72374) — Display &quot;Change in Settings&quot; icon on Pending Changes tab in Experimental UI.  
[**TW-72599**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72599) — Support VS 2022  
[**TW-72441**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72441) — Authentication to TeamCity using Space account doesn&#39;t work without specifying connectionId in the list with redirect uris  
[**TW-72448**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72448) — No &#39;Change from snapshot dependencies&#39; icon for pending changes  
[**TW-72222**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72222) — Pending changes popup shows changes in all branches instead of default one  
[**TW-72466**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72466) — Agent is empty on running build overview  
[**TW-62183**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-62183) — Rework or clarify &quot;white&quot; area in the timeline presentation  
[**TW-71723**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71723) — No DSL for workingDir in Node.JS runner  
[**TW-71814**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71814) — No DSL for working directory in Python runner  
[**TW-72595**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72595) — Provide better DSL for SSH agent build feature  
[**TW-72601**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72601) — C# custom script steps hang on non-Windows agents  
[**TW-65501**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65501) — Reordering in the sidebar doesn&#39;t work after filtering  
[**TW-72559**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72559) — Sort available authentication modules alphabetically  
[**TW-72458**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72458) — C# script: Warnings &quot;Can&#39;t set executable bit: file does not exist or is not a valid file&quot; on linux agent  
[**TW-67321**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67321) — Provide better DSL for AssemblyInfo patcher build feature  
[**TW-72563**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72563) — Unable to edit Space Connection if it&#39;s settings were corrupted in the xml file  
[**TW-72504**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72504) — Failed to start builds with SSH Upload or SSH exec runners with SSH-Agent authentication method  
[**TW-72320**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72320) — C# script: missing DSL for docker wrapper fields  
[**TW-72535**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72535) — Error calling method BuildServerListener.beforeBuildFinish for listener org.jetbrains.teamcity.testDuration.FinishBuildListener: jetbrains.buildServer.serverSide.auth.AccessDeniedException: Cannot locate this build project  
[**TW-72542**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72542) — Don&#39;t show hint about creation a Space application for the existing connections  
[**TW-72454**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72454) — Fix C# position in the server tools list  
[**TW-69827**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69827) — Build Queue: there is no &quot;time to start&quot; pop-up in experimenal UI  
[**TW-64009**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64009) — Incomplete estimates details are shown on the new UI queued build page  
[**TW-72440**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72440) — Add information about application type and minimal rights for Commit Status Publisher to JetBrains Space connection  
[**TW-72239**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72239) — Status from agent-less build step may be not reported in multi-node setup (build hangs)  
[**TW-71773**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71773) — REST: add information about planned agent and &quot;delayed by&quot; for queued build  
[**TW-72148**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72148) — Teamcity does not tag network interfaces on AWS EC2 instance creation  
[**TW-69338**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69338) — Test history is incomplete when using Gradle test distribution  
[**TW-72260**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72260) — Change format of a nuget reference  
[**TW-66699**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66699) — There&#39;s missing &quot;View in SonarQube&quot; link on build overview page in new UI

### Exception

[**TW-73123**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73123) — InstantiationException in teamcity-server.log

### Performance Problem

[**TW-72660**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72660) — Many agents requests occupy http threads in case when build messages queue becomes full  
[**TW-45825**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-45825) — Many builds in long &quot;checking for changes&quot; (hours), each checking the same VCS repository when VCS repository gets a bit slower  
[**TW-73087**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73087) — Build tabs loading duration is too long  
[**TW-73196**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73196) — Speedup TestsTab.isAvailable method  
[**TW-71912**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71912) — Slow opening Diagnostics/Cache tab (~4 minutes)  
[**TW-67968**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67968) — Favorite Projects page with a lot of projects can freeze during expanding all projects  
[**TW-67910**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67910) — Slowdown in TestName2IndexImpl.getTestNames() caused hanging UI and finishing of builds  
[**TW-64098**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64098) — Pages with many loaders take too much CPU resource  
[**TW-67720**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67720) — It takes seconds to render builds of the selected project even though all REST API calls are fast  
[**TW-72367**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72367) — Make sure composite build publishes build.finish.properties upon finishing  
[**TW-55940**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-55940) — PoolableConnectionFactory#passivateObject(Object) in commons-dbcp executes unnecessary ROLLBACKs  
[**TW-69759**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69759) — Slow agent compatible configurations tab can affect builds startup  
[**TW-72391**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72391) — A lot of memory used by some of the agent related pages (calculation of compatibility with many build configurations)  
[**TW-69802**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69802) — Using quick search by projects in sidebar makes the page unusable due to performance issues  
[**TW-72194**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72194) — Move to top operation can lock build queue processing  
[**TW-72301**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72301) — TeamCitySummaryFactory is slow because of fetching of muted tests information

### Cosmetics

[**TW-73116**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73116) — Remove unnecessary input in two-factor authentication form.  
[**TW-73120**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73120) — Single change page shows commit author starting with a capital  
[**TW-68009**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68009) — Build page, failed tests section: consider renaming &quot;show all&quot; to &quot;show all failed&quot;  
[**TW-58361**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-58361) — Improvements for displaying build configurations without builds  
[**TW-66728**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66728) — Bad wording for triggered by of restarted build on build details overview page

### Task

[**TW-69990**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69990) — Unbundle Jabber plugin  
[**TW-69408**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69408) — Unbundle RSS feed plugin  
[**TW-73234**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73234) — Update bundled kotlin compiler version to the latest one (1.5.31)  
[**TW-73227**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73227) — Update bundled Ant to version 1.10.11  
[**TW-73230**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73230) — Update bundled JaCoCo version  
[**TW-73231**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73231) — Update bundled Java to 11.0.12.7.1  
[**TW-72766**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72766) — API to retrieve builds and build configurations for Deployments tab on a Change page  
[**TW-69554**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69554) — Agent should open XML-RPC port (9090) only on local interface (drop bidirectional agent-server communication protocol)  
[**TW-71352**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71352) — GraphQL API: support agents tree manipulation mutations  
[**TW-73185**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73185) — Remove retina.js library  
[**TW-72988**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72988) — Do not show &quot;Choose node type&quot; screen if teamcity.startup.maintenance=false  
[**TW-73279**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73279) — Cleanup vcs\_change table for old/expired commits  
[**TW-72728**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72728) — Add to the REST information from which configuration / build the change came  
[**TW-73073**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73073) — Remove aopalliance-1.0.jar from server distribution  
[**TW-73075**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73075) — Update documentation page regarding supported Perforce versions  
[**TW-72996**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72996) — Web-sockets: send the &quot;agent pool touched&quot; notification when the assigned project is removed  
[**TW-72636**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72636) — ReactUI: Support tabs for Change page  
[**TW-72832**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72832) — Ability to get failed tests tree related to a specific change  
[**TW-72640**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72640) — API for Problems tree in a same fashion as for tests  
[**TW-67314**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67314) — Better DSL for artifact storage settings  
[**TW-72722**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72722) — REST API: get TestOccurrences with investigations  
[**TW-71136**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71136) — Upgrade Kotlin used to run DSL to version 1.5  
[**TW-68492**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68492) — Update Corretto in Docker images to 11  
[**TW-72532**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72532) — teamcity-auth.log is full of &quot;Successful login for user ... with auth module &quot;HTTP-Token-Based&quot; for session&quot;  
[**TW-71466**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71466) — Delete notification about TeamCity Cloud  
[**TW-70531**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70531) — Get rid of Bintray and JCenter usages in Kotlin DSL  
[**TW-72425**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72425) — teamcity-configs-maven-plugin pom.xml has a dependency on maven-core:3.0.5 that is deprecated  
[**TW-66056**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-66056) — Remove old cleanup rules controller and related UI classes

#### Security Problem

4 security problems have been fixed.