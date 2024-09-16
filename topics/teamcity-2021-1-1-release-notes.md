[//]: # (title: TeamCity 2021.1.1 Release Notes)
[//]: # (auxiliary-id: TeamCity 2021.1.1 Release Notes)

__Build: 92714__  
__18 June 2021__
{instance="tc"}

#### Feature

[**TW-71903**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71903) — Allow to specify timeout for `docker stop` command in build configuration parameters  
[**TW-71227**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71227) — Agent pool page: reimplement agent pool projects tab with GraphQL  
[**TW-71280**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71280) — Gradle runner should warn that enabling &quot;Debug&quot; can add sensitive information to Build Log  
[**TW-71691**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71691) — Respect &quot;branch&quot; config from .gitmodules during agent checkout with mirror  
[**TW-67347**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67347) — Two infinite running build steps will ignore build timeout policy  
[**TW-69505**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69505) — Allow to hide VCS roots used in archived projects only on Project -\&gt; VCS roots page in admin area  
[**TW-68673**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68673) — REST API: make `/app/rest/server/licensingData` available for anyone who can authorize agents

#### Usability Problem

[**TW-68081**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68081) — Do not auto-detect &quot;IntelliJ IDEA Project&quot; build steps if project does not contain JVM-based modules  
[**TW-69432**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69432) — Build log: new line characters between time and message when copying part of the log  
[**TW-71373**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71373) — Move &quot;Save anyway&quot; button from main settings form to &quot;Save&quot; dialog  
[**TW-71408**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71408) — Container Wrapper can hit the rate limit on dockerhub when pulling busybox in a large enough installation  
[**TW-63117**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-63117) — There is no information on the pool page that project also relates to other pools  
[**TW-71715**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71715) — Agent pool page: move &quot;archived projects&quot; counter to the checkbox  
[**TW-62151**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-62151) — Consider expanding Inspections/PR/... section by default on a build page in the experimental UI  
[**TW-64703**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-64703) — How to sort dependencies in teamcity  
[**TW-69452**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69452) — Configure sidebar: No way to reset an order of the Root subprojects  
[**TW-60093**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-60093) — Confusing presentation of the failed to start build  
[**TW-71451**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71451) — Remember the chosen grouped tests mode on the build overview page  
[**TW-69676**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69676) — Improve empty pool page in the experimental UI  
[**TW-71611**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71611) — Node.js runner: improve autodetection for eslint tool  
[**TW-68353**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68353) — Build page: &quot;Select all&quot; action in the tests list looks too similar to test lines

#### Bug

[**TW-71887**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71887) — The checkbox corresponding to a package in the list of tests is not settable  
[**TW-71457**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71457) — Custom build dialog treats two parameters as the same if they only differ in one character ( . (dot) vs \_ (underscore) )  
[**TW-70322**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70322) — s3 artifacts fail to upload, but upload step succeeds  
[**TW-71832**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71832) — git &quot;Passphrase&quot; setting not shown for existing VCS roots with encrypted key  
[**TW-65043**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65043) — Error while applying patch &quot;reference is not a tree&quot; build error when submodule revision is not reachable from refs/heads  
[**TW-71631**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71631) — After update to 2021.1 (build 92597), teamcity.dotnet.vstest.16.0 was undefined and builds would no longer execute VSTest step  
[**TW-71741**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71741) — Ant plugin bin Unix scripts have Windows EOL instead of Unix one  
[**TW-71827**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71827) — Perforce agent checkout may not work correctly (Null directory (//) not allowed in) error  
[**TW-71720**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71720) — Cannot tick checkboxes on agent&#39;s &quot;Compatible Configurations&quot; page  
[**TW-71537**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71537) — Illegal hex characters in escape (%) pattern - For input string: &quot;sy&quot; (IntelliJ IDEA Coverage)  
[**TW-71285**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71285) — Docker clean-up: TC tries to clean-up images after disabling &quot;On server clean-up, delete pushed Docker images from registry&quot; option  
[**TW-68855**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68855) — Build log search: &quot;Next result&quot; and &quot;Search&quot; buttons don&#39;t scroll page horizontally  
[**TW-71679**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71679) — VCS trigger does not trigger a build in a branch after the merge (build configuration with checkout rules)  
[**TW-70668**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70668) — Search mode changes from versioned settings are not shown within a session  
[**TW-70665**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70665) — Improve versioned settings for Elastic Search  
[**TW-71375**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71375) — Return &quot;Drop&quot; button to Disabled Local(Lucene) indexer  
[**TW-65197**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65197) — Verbose mode in build log resets after choosing build step using timeline  
[**TW-71826**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71826) — Sub projects are not stored in VCS repository when versioned settings in XML format are enabled in the project  
[**TW-65670**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65670) — Cancel button behaves incorrectly in the Changes content filter.  
[**TW-69884**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69884) — Active projects in archived hierarchy are not counted and not displayed in agent pools.  
[**TW-71213**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71213) — Node.js runner: Add a validation for a NPM Registry connection form  
[**TW-71667**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71667) — Issues list has broken encoding (for YouTrack issues)  
[**TW-71779**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71779) — The switcher between the old and new UI is broken on the queued build page in new UI  
[**TW-67210**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-67210) — Pop-up &quot;Responding with error, status code: 404&quot; appears when Flaky Test Detector is disabled  
[**TW-71154**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71154) — Node.js runner: don&#39;t allow deletion of npm connection if it&#39;s in use  
[**TW-71676**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71676) — DSL converters are not applied for DSL with config version 2019.2  
[**TW-71494**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71494) — Builds may not be triggered in pull request branches of an Azure DevOps repository if merge conflicts got resolved  
[**TW-71527**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71527) — Do not display twice redefined custom parameter in the Triggers parameter description.  
[**TW-63003**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-63003) — There is no suggestions for assigning investigations to the user for failed tests in new UI  
[**TW-65676**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65676) — Browser console and UI popup forbidden request errors on changes page if logged under non-admin role user  
[**TW-71722**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71722) — Inspections (ReSharper): spaces in &quot;Additional InspectCode parameters&quot; aren&#39;t properly handled  
[**TW-70025**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70025) — Run shallow fetch for submodules with shallow clone for git VCS  
[**TW-71719**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71719) — Cleanup: base rules processor continue to use configured artifact patterns after the &quot;clean history&quot; period if the build is a snapshot dependency  
[**TW-70164**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70164) — Could not get RequestDispatcher error in teamcity-server.log  
[**TW-71693**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71693) — testMetadata service message now requires testName  
[**TW-70498**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70498) — No information about artifacts location in Artifacts view in the new UI  
[**TW-71518**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71518) — Deleted npm connections and connections with changed scopes are not updated in .npmrc file on the agent checkout directory  
[**TW-71649**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71649) — Build triggers customization settings are not returned in Rest API response  
[**TW-65666**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65666) — Changes content filter does not resolve part of revision.  
[**TW-65668**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-65668) — Changes content filter should be case insensitive in Experimental UI.  
[**TW-71655**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71655) — Enabling of versioned settings in Kotlin format for a large project does not work because of exception from deadlock detector  
[**TW-68794**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-68794) — Build Queue page: No builds found in whole build history  
[**TW-63728**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-63728) — Experimental UI =\&gt; build log timezone is UTC, while should be local  
[**TW-71662**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71662) — Commit hook can be missed if VCS periodical executor queue is full  
[**TW-71635**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71635) — Perforce commit hook may work incorrectly if a VCS root user does not have access to some of the Perforce repository paths  
[**TW-71180**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71180) — Gitlab PR isn&#39;t detected after PR change with a target branch filter  
[**TW-71152**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71152) — Node.js runner: Build steps autodetector can suggest excess steps for running tests  
[**TW-71615**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71615) — Node.js runner: auto-detector suggests incorrect package to install for running Hermione tests  
[**TW-71629**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71629) — Dependency cycle detected among ordered items: dataDir, order is unpredictable  
[**TW-71320**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71320) — Warning &quot;Failed to log action to audit&quot; in teamcity-server.log after switching main node  
[**TW-71622**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71622) — Obsolete revision is taken in a build after checkout rules change and rebase  
[**TW-62992**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-62992) — No stacktrace information is shown in the new UI for system problems  
[**TW-71526**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71526) — %\dep.extId.param% parameters are not updated in trigger build customization when external id of dependency changes  
[**TW-71555**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71555) — Duplicate custom shared resource values will result in an unpopulated &#39;teamcity.locks.readLock.\*&#39; parameter  
[**TW-69874**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-69874) — Improve an empty Agent Overview page  
[**TW-71476**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71476) — Better handling of cases with long suites/packages/classes tests names  
[**TW-71607**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71607) — Branch selector field: no longer reacts to arrow up/down keys  
[**TW-71531**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71531) — Node tries to load disabled plugin on switch main node responsibility  
[**TW-71532**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71532) — A node is unable to load plugins that depend on another plugin  
[**TW-71495**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71495) — Invalid request when triggering a new build for a new build configuration  
[**TW-71489**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71489) — Display only one 404 pop-up on a page in the experimental UI  
[**TW-71561**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71561) — Warnings from triggers on the secondary node are shown in the logs on the main node  
[**TW-71562**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71562) — Passing test with the empty status being marked as ignored in JUnit report.  
[**TW-71282**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71282) — Value from MAVEN\_OPTS has higher priority over the JVM arguments provided by the Maven runner

#### Exception

[**TW-71785**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71785) — NullPointerException upon calling SubscriptionsWebSocketEndpoint.closeOnError

#### Cosmetics

[**TW-71623**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71623) — Tests scope does not show a colon in test suite  
[**TW-54352**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-54352) — Commit Status Publisher fails to publish statuses to GitHub if Emojicon Unicode characters occur in build configuration names

#### Performance Problem

[**TW-71456**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71456) — Large delay for showing disconnected/unauthorized agents in the classic UI  
[**TW-70800**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70800) — /app/timeline call for a build with huge build log leads to extensive memory usage  
[**TW-71471**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71471) — Sub optimal memory usage during the /app/rest/projects?fields= REST API call  
[**TW-71424**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71424) — InvestigationsCleanupExtension cleaner takes too much time to complete  
[**TW-71540**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71540) — Slow RelatedIssuesTab.isAvailable if invoked for a composite build

#### Task

[**TW-71817**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71817) — Automatically increase polling interval for broken VCS roots  
[**TW-71564**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71564) — Add the ability to control ignoreCase and matchType in the comment, file:path and version fields, at the endpoint of changes in REST  
[**TW-71429**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-71429) — Add the ability to get a list of committers by a changes locator
