[//]: # (title: Upgrade Notes)
[//]: # (auxiliary-id: Upgrade Notes)

## Changes from 2019.1.1 to 2019.1.2

No noteworthy changes.

## Changes from 2019.1 to 2019.1.1

### Bundled Tools Updates

* The bundled IntelliJ IDEA has been updated to 2019.1.3.
* The bundled ReSharper tools (Inspections and Duplicates Finder) have been upgraded to 2019.1.0-eap08d version.

## Changes from 2018.2.x to 2019.1

* Tags are now __mandatory__ for all Amazon instances run by TeamCity, which helps identify them. If you use integration with Amazon EC2, make sure to add the [`ec2:CreateTags`](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-supported-iam-actions-resources.html#supported-iam-actions-tagging) resource-level permission in Amazon.
* Starting with 2019.1, the behavior of `reverse.dep` parameters has been changed, and this change can affect your existing builds. In versions prior to 2019.1, when a build chain is triggered, TeamCity only took into account the `reverse.dep` parameters specified in the top-most build of the chain, i.e. in the build which depends on all other builds. If some intermediate builds of the chain had `reverse.dep` parameters, they were ignored.   
  After [this fix](https://youtrack.jetbrains.com/issue/TW-41341) this is no longer the case. Now, when a build chain is triggered, all `reverse.dep` parameters specified in all nodes of the build chain will be processed.
* On upgrading to 2019.1, the Token-Based Authentication module will be enabled by default, so you can generate [access tokens](managing-your-user-account.md#Managing+Access+Tokens) and start using them right away.
* Now TeamCity Web UI uses more restrictive value for the [`Content-Security-Policy`](https://content-security-policy.com/) HTTP header. This provides extra security at the expense of prohibiting usage of the web resources not hosted on the TeamCity server.   
If you rely on external resources (for example, in the build report tabs content or by using not yet updated plugins), you can specify new header value in the `teamcity.web.header.Content-Security-Policy.protectedValue=<full header value>` [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties) (and `teamcity.web.header.Content-Security-Policy.adminUI.protectedValue` property for the web pages in Administration area). Plugins can use [`ContentSecurityPolicyConfig`](http://javadoc.jetbrains.net/teamcity/openapi/current/jetbrains/buildServer/web/ContentSecurityPolicyConfig.html) open API interface to add to the value configured.
* The requirements for the .NET Framework version used by ReSharper tools have changed. Now, if you use ReSharper tools (dotCover and ReSharper Inspections) of version 2018.2 or newer (including the version bundled with TeamCity 2019.1) in your build configuration, the requirements to build agents will change to .NET Framework 4.6.1 or newer. Make sure to update .NET Framework on agents.
* The `dotCover.dcvr` hidden artifact is no longer published by default. It is now created in the build temporary folder and removed when the build finishes.   
 If you use dotCover and rely on this artifact, specify the path to the `%system.teamcity.build.tempDir%\..\agentTmp\dotNetCoverageResults\dotCover.dcvr` file explicitly in the [Artifact paths](configuring-general-settings.md#Artifact+Paths).
* If you have been using beta versions of PowerShell, make sure to remove all beta versions earlier than v6.0.0-beta.9 before installing any new released PowerShell version. Due to updates in the PowerShell detector, if the old beta version is installed, TeamCity will use it instead of the new released one.
* Agents now download tools from the server only when starting the first build that requires these tools. Here is how it might affect your existing setup:
   * Agents upgrade significantly faster than before, but it takes extra time for an agent to download a tool for the first build that requires this tool. Such builds will be processed a little longer than usual.
   * Agents detect and download most of the required tools automatically, but you can request additional tools by referencing them in the [parameters](configuring-build-parameters.md) of the build (or project) configuration. TeamCity now only supports paths defined using a parameter reference (for example, `%teamcity.tool.jdk12%`). If the parameters you use comprise relative or absolute paths to tools, redefine their values via parameter references.   
   * Note that if you are about to specify a path to a tool in the project settings, TeamCity will not propose the path value if the tool has not been yet downloaded by any agent in the project. We are working on enabling the autocomplete feature on the project level, but for now please make sure to enter the proper path manually.
* If you use Docker images and Windows Server 2019 with process isolation, build agents may fail to start. To work around the issue, use the `hyper-v` isolation for Docker containers:

    ```Shell 
    
    docker run --isolation=hyperv …
    ```

### Bundled Tools Updates

* The latest JaCoCo version (0.8.4) has been added to TeamCity.
* The bundled .NET tools (dotCover and ReSharper CLT) have been upgraded to the latest released version, 2019.1.1.
* Automatically provided JDBC drivers for external databases have been updated to the following versions:
     * MySQL 8.0.16
     * MSSQL 7.2.2
     * PostgreSQL 42.2.5

## Changes from 2018.2.3 to 2018.2.4
There is a regression in TeamCity 2018.2.4 related to the data displayed on the unauthorized agents' page: the data on agent authorization, enabling/disabling comments, and the latest activity time has been removed from the page. This data is displayed on the agent details page.

When the performance is improved, we will return the data to the unauthorized agents' page.

## Changes from 2018.2.2 to 2018.2.3
TeamCity now ships Windows Docker images for 1803/1809 platforms.

### Known issues

Running builds are not shown on the build configuration page if there are no finished builds. To workaround the issue, stop the TeamCity server, replace the `TEAMCITY_DIRECTORY/webapps/ROOT/js/ring/bundle.js` with the `bundle.js` file  attached to [this issue](https://youtrack.jetbrains.com/issue/TW-59529#focus=streamItem-27-3327362.0-0) and start the server. 

## Changes from 2018.2.1 to 2018.2.2
* The bundled Tomcat has been updated to version 8.5.3
* The TeamCity agent Docker image supports .NET Core 2.2

## Changes from 2018.2 to 2018.2.1

No noteworthy changes

## Changes from 2018.1.x to 2018.2

### Known issues
* If upgrade fails with an error in MoveCustomDataStorageToDatabaseConverter or MoveRepositoryStateToCustomDataStorageConverter, apply workaround from [the issue](https://youtrack.jetbrains.com/issue/TW-58289#focus=streamItem-27-3207962-0-0).
* If you're using Subversion externals from the same repository, you may face an issue with incorrect revision detection. A workaround for the problem is described in [this issue](https://youtrack.jetbrains.com/issue/TW-58336).
* If you see OutOfMemoryError during TeamCity startup with `org.jetbrains.dokka` in stack trace, set the internal property `teamcity.kotlinConfigsDsl.docsGenerationXmx=768m` (as described in [this issue](https://youtrack.jetbrains.com/issue/TW-56408))

### Bundled Tools Updates
* The bundled .NET Tools (dotCover and ReSharper CLT) have been upgraded to the latest released version, 2018.1.4
* TeamCity 2018.2 comes bundled with IntelliJ IDEA 2018.3.1. The IntelliJ IDEA Project Runner uses JPS 2018.3.1 
* Since TeamCity 2018.2 OpenJDK is included in the Windows .exe TeamCity distribution (before 2018.2 Oracle Java was bundled with TeamCity Windows distribution). 

### NuGet feed

The parameters that were previously used to reference the global NuGet feed are removed. After upgrading, all of them will be converted to [the parameters referencing the default NuGet feed in Root project](using-teamcity-as-nuget-feed.md).

If you're still using deprecated NuGet feed references in project parameters please update them as following:

<table><tr>

<td>

Global feed name

</td>

<td>

Project feed name

</td></tr><tr>

<td>

teamcity.nuget.feed.server

</td>

<td>

teamcity.nuget.feed.guestAuth.\_Root.default.v2

</td></tr><tr>

<td>

teamcity.nuget.feed.auth.server

</td>

<td>

teamcity.nuget.feed.httpAuth.\_Root.default.v2

</td></tr><tr>

<td>

system.teamcity.nuget.feed.auth.serverRootUrlBased.server

</td>

<td>

teamcity.nuget.feed.httpAuth.\_Root.default.v2

</td></tr></table>

## Changes from 2018.1.4 to 2018.1.5

No noteworthy changes

## Changes from 2018.1.3 to 2018.1.4

### Known issues
* Builds which are running while the server upgrades to 2018.1.4 can get their build log and status truncated, not duly reporting build failure. The build log gets warning with "\_\_tc\_longResponseMarker" text ([details](https://youtrack.jetbrains.com/issue/TW-58266)). It is recommended to wait till there are not running builds when upgrading to this version.
### Misc

Support for Microsoft Internet Explorer 10 is discontinued in TeamCity 2018.1.4.

## Changes from 2018.1.2 to 2018.1.3

### Bundled Tools Updates

The latest JaCoCo version (0.8.2) has been added to TeamCity.

### Known issues

If you use JaCoCo coverage and you decide to downgrade from TeamCity 2018.1.3\+ to TeamCity versions 2018.1 \- 2018.1.2,  you may experience issues requiring manual fixes to make the affected configurations work again.

## Changes from 2018.1.1 to 2018.1.2

### Known issues

If you are using [tcWebHooks](https://plugins.jetbrains.com/plugin/8948-web-hooks-tcwebhooks-) third\-party TeamCity plugin, update it to the latest version before upgrading ([details](https://youtrack.jetbrains.com/issue/TW-56626)). 

We increased accuracy for recognizing similar TeamCity exit code build problems. The existing investigations and mutes for these build problems will be reset.

### Bundled Tools Updates

The bundled Tomcat has been updated to version 8.5.32.

## Changes from 2018.1 to 2018.1.1

### Known issues

If you're upgrading from 2018.1 to 2018.1.1 and you want to see the NuGet packages missing due to issues [TW-55703](https://youtrack.jetbrains.com/issue/TW-55703) and [TW-55833](https://youtrack.jetbrains.com/issue/TW-55833), do the following:
* Cleanup `.teamcity/nuget/packages.json` files in the build artifacts. Consider using this PowerShell script:


```Shell

> cd "%TeamCityDataDir%\system\artifacts\"
> Get-ChildItem -Recurse | Where-Object { $_.FullName -match '\.teamcity\\nuget\\packages\.json' } | Remove-Item
```

* On the __Administration | Diagnostics | Caches__ page, reset the "buildsMetadata" cache and wait while re\-indexing is finished. To temporary increase indexing speed, see the [following tip](https://confluence.jetbrains.com/display/TCD18/Known+Issues#KnownIssues-PackagesindexingisslowinTeamCityNuGetfeed).

### Docker Images

Since 2018.1.1 TeamCity has multi\-platform docker images marked by the "latest" and version number tags published in Docker Hub, e.g. "jetbrains/teamcity\-server", "jetbrains/teamcity\-server:2018.1.1". This allows using the same docker image reference for Linux and Windows docker containers, see [TW-55061](https://youtrack.jetbrains.com/issue/TW-56088) for details.

## Changes from 2017.2.x to 2018.1

### Known issues

While publishing NuGet packages into the TeamCity NuGet feed in multiple build steps, only the packages published by the first build step will be visible. See [TW-55703](https://youtrack.jetbrains.com/issue/TW-55703) for details. If you experience problems with download of NuGet packages published within archives see [TW-55833](https://youtrack.jetbrains.com/issue/TW-55833).

### Stricter rules for parameter names used in parameter references

Names of the Build Configuration parameters are now validated in more strict manner. While already existing parameters should continue to work, it is highly recommended to review the names and use Latin letters and no special symbols. [Details](configuring-build-parameters.md)

### User self-registration

If you have Built\-in authentication enabled with the "Allow user registration from the login page" setting on, the setting will be disabled on upgrade. If you need the registration, make sure the server is not open to unauthorized users access (e.g. not accessible from Internet) and enable the setting via the health item displayed at the top of the administration pages or in the "Administration | Authentication" under the "Built\-in" module settings.

### Bundled Tools Update
* The IntelliJ IDEA Project Runner uses JPS 2017.3.4 requiring Java 1.8 as the minimal version.
* The bundled ReSharper CLT and dotCover have been updated to version 2018.1.2

### NuGet feed
* Configuration of the NuGet feed was moved from the server level to the project level: now each project can have its own feed. The "NuGet packages indexer" build feature can be added to build configurations whose artifacts should be indexed.
* The following NuGet feed\-related build parameters are deprecated:
  * `teamcity.nuget.feed.auth.server`
  * `teamcity.nuget.feed.server`
  * `system.teamcity.nuget.feed.auth.serverRootUrlBased.server`
  
  You now need to explicitly specify the URL from the [NuGet Feed](using-teamcity-as-nuget-feed.md) page in the project settings.
  
* The enabled default NuGet feed with all published packages accessed by URL `/app/nuget/v1/FeedService.svc/` is now moved to the Root project feed `/app/nuget/feed/_Root/default/v2/`. It is recommended to switch to new URL in your projects.
* .nupkg files are now indexed on the agent side instead of the server which could slightly increase the time of builds for projects with the NuGet Feed feature and the automatic package indexing enabled or for builds with NuGet Packages Indexer build feature.
### REST API

REST API uses version 2018.1. The previous versions of the API are still available under /app/rest/2017.2, /app/rest/2017.1 (/app/rest/10.0), app/rest/9.1, /app/rest/9.0, /app/rest/8.1, /app/rest/7.0, /app/rest/6.0 URLs. It is recommended to stop using previous APIs URLs as we are going to remove them in the following releases.

#### Filtering builds by agent names

When agent name contains the parentheses symbols, instead of using agentName:&lt;name&gt;, use "agentName:(value:&lt;value&gt;)

#### Locators with "value:&lt;text&gt;"

Requests which used "value:&lt;text&gt;" locators (e.g. for matching properties) and no "matchType" dimension specification will start to use "equals" matching by default. Add "matchType:contains" to preserve the old behavior. [Details](https://youtrack.jetbrains.com/issue/TW-55350)

### VSS plugin is unbundled

The Visual SourceSafe plugin is no longer bundled with TeamCity but is available as a [separate download](https://plugins.jetbrains.com/plugin/10902-vcs-support-vss). Please contact our [support](https://confluence.jetbrains.com/display/TW/Feedback), if you still use this VSS for your builds.

### Other
* Commit Status Publisher supports Gerrit 2.6\+ versions. For support for older Gerrit versions, please turn to our [support](https://confluence.jetbrains.com/display/TW/Feedback).
* When upgrading from TeamCity versions before 9.1, if TeamCity 2018.1 starts and agents are upgraded, but then you decide to roll back the server to the previous TeamCity version, the agents will not be able to connect back to the old server and will need to be reinstalled manually.
* Make sure that no HTTP requests from the agents to the server are blocked (e.g. requests to .../app/agents/... URLs)
* Since 2018.1, TeamCity uses the full project name with "/" instead of "::" as the separator for Project \- Subproject wherever the full project name is present.

## Changes from 2017.2.3 to 2017.2.4

The Inspections (.NET) and Duplicates Finder (.NET) build steps were renamed to Inspections (ReSharper) and Duplicates finder (ReSharper)

## Changes from 2017.2.2 to 2017.2.3

### Build revisions

In all the build configurations, the builds run before the upgrade will not be reused in the chains and will run anew (only the last build on the default branch is affected if branches are used). This may also result in a clean checkout in the first run build for VCSs like Perforce. The behavior is similar tothat after VCS root editing.____

__Security__

When upgrading to 2017.2.x versions (please ignore when upgrading to 2018.1 and further versions): It is recommended to add "`teamcity.artifacts.restrictRequestsWithArtifactReferer=true`" [internal property](configuring-teamcity-server-startup-properties.md) to enhance security of the server.

##   Changes from 2017.2.1 to 2017.2.2

<anchor name="knownIssues_2017_2_2"/>

### Known issues

(Fixed 2017.2.3) If you use the Artifactory plugin and get the "Invalid RSA public key" browser message on opening build step settings, please apply the [workaround](https://youtrack.jetbrains.com/issue/TW-53562#comment=27-2698895).

Under Windows, when TeamCity server is started as a service, "logs\teamcity\-winservice.log" file is not created and server startup errors are nowhere to be seen. [Details](https://youtrack.jetbrains.com/issue/TW-54483)

### IDE Plugins

It is highly recommended to update the IDE plugins for all users to the latest version and then add the `teamcity.uploadPersonalPatch.requireAuthorization=true` [internal property](configuring-teamcity-server-startup-properties.md) to enhance security of the server.

### Perforce VCS Root executable paths

__Since TeamCity 2017.2.2__, the field which specifies the path to p4 works only on the agent side, for agent\-side checkout.

For the server, the p4 binary should be present in the PATH of the TeamCity server (or can be specified via the `teamcity.perforce.customP4Path ` [internal property](configuring-teamcity-server-startup-properties.md)). The `teamcity.perforce.p4PathOnServerWhitelist` [internal property](configuring-teamcity-server-startup-properties.md) can be used to specify a  semi\-colon\-separated list of allowed p4 paths. The paths from this list can be set in VCS Root p4 path parameter for the server side (to restore old behavior).

### Mercurial VCS Root properties

__Since TeamCity 2017.2.2__, a number of Mercurial VCS root properties change their behavior for security reasons.
* the "HG command path" is used on the TeamCity server only if included into the [whitelist](https://confluence.jetbrains.com/display/TCD10/Mercurial#Mercurial-Pathtohgexecutabledetection)
* the "Clone repository to" property is hidden if the VCS root doesn't have it already and is ignored by default. To make TeamCity display the property in all VCS roots, add the `teamcity.hg.showCustomClonePath=true` [internal property](configuring-teamcity-server-startup-properties.md). The value of the VCS root property is respected only if it is included into the whitelist specified by the `teamcity.hg.customClonePathWhitelist` [internal property](configuring-teamcity-server-startup-properties.md), which is a semi\-colon\-separated list of directories where a clone is allowed. Use `/path/to/dir/*` to allow clones to the child directories of the `/path/to/dir`.
* the "Mercurial config" is ignored on the server. If you need to enable some Mercurial plugins, please do that in the global .`hgrc` on the TeamCity server machine.

## Changes from 2017.2 to 2017.2.1

### Kotlin DSL changes

The versions in pom.xml were updated: `kotlin.version` is updated to `1.2.0`, `teamcity.dsl.version` is updated to `2017.2.1`. The dependency on `kotlin-stdlib` is replaced with the dependency on `kotlin-stdlib-jdk8` in order to provide access to additional functionality available in jdk8 (e.g. named groups in regexps). The dependency on the deprecated `kotlin-runtime` and the redundant dependency on `kotlin-compiler-embeddable` were dropped.

Now TeamCity provides a parent maven project for Kotlin DSL which defines the `teamcity.dsl.version` and `kotlin.version` properties. With such a parent project, you will not have to update your pom.xml after eachTeamCity upgrade.

The easiest way to apply these changes is to run the 'Download settings in Kotlin format' action in project admin area and use the pom.xml from the zip produced by  theTeamCity server.

### Bundled Java used in Docker images

The bundled Java used in Docker images has been updated to 8u151.

## Changes from 2017.1.x to 2017.2

<anchor name="knownIssues_2017_2"/>

### Known issues

(Fixed 2017.2.1) TFS in Java working mode (when Team Explorer is not installed on the machine) report "TFS subsystem was destroyed" errors. See [TW-52685](https://youtrack.jetbrains.com/issue/TW-52685) for details.

Upgrading using Windows installer can take significant time if your TeamCity installation directory contains lots of nested directories (e.g. TeamCity Data Directory is under it). The long stage can occur after "Extract: Uninstall.exe..." progress message. In you encounter this long step, please wait for the completion of the operation (the installer runs icacls.exe utility as a nested process). To prevent the issue it is recommendd to [move the data directory](https://confluence.jetbrains.com/display/TCD10/TeamCity+Data+Directory) out of TeamCity server installation home.

### Perforce branch specification change

There is a breaking change which requires your action if you use Perforce streams with enabled feature branches and you're using a non\-default branch filter.

Starting from TeamCity 2017.2, Perforce VCS Roots use the same format for Perforce streams and TeamCity feature branches specification.

In the VCS Root branch specification for Perforce, `+:stream_name` must now be replaced with `+://stream_depot/stream_name`. Also, for better presentation of stream names in the UI, you may want to replace the default branch specifications like `+:*` with `+://your_stream_depot/*`.

This change was made in the scope of fixing [TW-48038 ](https://youtrack.jetbrains.com/issue/TW-48038).

### Server process restart

Now if the server process is stopped unexpectedly or killed, the process will automatically restart. The server should be stopped using the `teamcity-server.bat/sh stop` command which performs a graceful stop.

### Bundled plugins

#### Docker integration plugin

The Docker integration plugin is bundled since TeamCity 2017.2.x. If you installed the plugin for the previous version manually, please [remove it](https://confluence.jetbrains.com/display/TCD10/Installing+Additional+Plugins).

#### .NET CLI plugin

The .NET CLI (.NET Core) plugin is bundled since TeamCity 2017.2.x. If you installed the plugin for the previous version manually, please [remove it](https://confluence.jetbrains.com/display/TCD10/Installing+Additional+Plugins).

During upgrade all existing .NET Core build steps will be converted into .NET CLI steps and existing .NET Core plugin will be disabled.

__Note__: The `DotNetCore` and `DotNetCore_Path` agent configuration parameters will be changed to  `DotNetCLI` and `DotNetCLI_Path`; please consider updating your agent requirements which depend on these parameters.

### REST API

REST API uses version 2017.2. The previous versions of the API are still available under /app/rest/2017.1 (/app/rest/10.0), app/rest/9.1, /app/rest/9.0, /app/rest/8.1, /app/rest/7.0, /app/rest/6.0 URLs.

__buildType entity__ 

has "templates" sub\-element now instead of "template" to support multiple templates.

__build entity__ No longer expose boolean "running" attribute, textual "state" attribute with value "running" is used instead.

### Windows Versions Support

Windows XP and Vista are no longer the supported versions of Windows for the TeamCity Server and Agent. While the server and agent will still most probably work on these old versions, we do not target the versions during our development. [Let us know](https://confluence.jetbrains.com/display/TW/Feedback) if the support for the versions is important for your TeamCity usage or you find any issues with the systems support.

### J2EE Servlet 2.5 container is no longer supported

J2EE Servlet container version 2.5 is not supported since TeamCity 2017.2. TeamCity does not guarantee support for Tomcat 6.x and Jetty 7.x implementing Servlet 2.5. For .war distribution (not recommended, .tar.gz distribution is recommended), TeamCity supports Apache Tomcat 7\+, J2EE Servlet 3.0\+ and JSP 2.2\+.

### Other
* The bundled Tomcat 8.5. restricted usage of special characters in the URL including curly bracket symbols (\{ \}). [Details](https://youtrack.jetbrains.com/issue/TW-51052).
* TeamCity integration with Intellij\-based IDEs no longer supports StarTeam and Visual Source Safe version controls.
## Changes from 2017.1.4 to 2017.1.5

The bundled JetBrains dotCover has been updated to version 2017.2

The SSH Agent build feature started to report a build problem if it fails to start an SSH agent with the specified SSH key (in the scope of [TW-42707](https://youtrack.jetbrains.com/issue/TW-42707)). Previously errors were only logged, but not reported as build problems. As a result, builds with invalid SSH Agent settings would start to fail after the upgrade.

## Changes from 2017.1.3 to 2017.1.4

### Known issues

TFS Personal support lists all build configurations for TFVC VCS root. See [TW-51497](https://youtrack.jetbrains.com/issue/TW-51497) for details.

## Changes from 2017.1.2 to 2017.1.3

[TW-50148](https://youtrack.jetbrains.com/issue/TW-50148) was fixed and the DSL API documentation was improved. If you need these changes for local development, please update the [maven dependency version](upgrading-dsl.md) to 2017.1.3.

Now TeamCity server runs 'git gc' automatically to improve performance of git operations. This requires a git client to be installed on the server and be  the server via the PATH environment variable. If a native git client cannot be found, then the corresponding health report is shown. For TeamCity to find the git client, the client needs to be installed on the server machine and added to `$PATH` (the server restart is required afterwards). Instead of modifying PATH, the path to the git client can be specified via the `teamcity.server.git.executable.path` [internal property](configuring-teamcity-server-startup-properties.md).

## Changes from 2017.1.1 to 2017.1.2

No noteworthy changes.

## Changes from 2017.1 to 2017.1.1

The bundled IntelliJ IDEA has been updated to 2017.1.2

## Changes from 10.0.x to 2017.1

### Known issues

Editing cloud profile cancels all builds on profile agents. See [TW-49616](https://youtrack.jetbrains.com/issue/TW-49616) for details.

### TeamCity Versioning Changes

Since 2017, TeamCity adopts the common JetBrains versioning scheme that identifies versions by year following the pattern: &lt;year&gt;.&lt;number of the feature release within the year&gt;.&lt;bugfix update number&gt;. The current version is TeamCity 2017.1 formerly known as TeamCity 10.1.

### Update settings in Kotlin DSL

In this version TeamCity settings format has been changed and if your settings are stored in Kotlin DSL, you might need to [update Kotlin DSL scripts](upgrading-dsl.md) before continuing to use them. Check any related server health reports after the server upgrade.

<anchor name="2017.1-CSRF"/>

### CSRF Protection: Modifying GET requests and Proper Proxy configurations

TeamCity now implements [CSRF protection](csrf-protection.md) to improve web UI security and this introduces several changes in behavior which might affect your installation. In particular:
* If you use a __reverse proxy__ before TeamCity, the proxy should not change the original "Host" request header (this typically requires configuring the proxy to set the Host header to the original request value). Also, it should not modify the "Origin" and "Referer" headers present in the original request. While this has been the recommended setup for a long time, now it becomes critical for the TeamCity web UI functioning;
* if you use non\-bundled clients which perform HTTP __GET requests__ to TeamCity, some of the GET requests (those which change the state of the server, like http://server/action.html?add2Queue=XXX) stop working in 2017.1, please change the requests to use POST instead of GET;
* the non\-browser clients which reuse authentication by supplying the __TCSESSIONID__ cookie with the request, need to be updated to supply the "Origin" HTTP header with the value the same as the host the request is being sent to.
If the check fails, you get the HTTP __403 response__ with the details of the failed check. The details are also logged into teamcity\-auth.log.

### Old IPR Runner

The old (TeamCity 6.0) IPR runner has been removed from the TeamCity. It was deprecated and has not been offered as an option since TeamCity 6.0, and now it is gone completely (the corresponding build configurations will not run anymore).

### Log4j configuration

It is recommended to overwrite the server's `conf\teamcity-server-log4j.xml` file with the content of the `conf\teamcity-server-log4j.xml.dist` file which represents the default logging configuration which has changed in this release. If you do need a custom logging configuration, consider using [logging presets](teamcity-server-logs.md) instead of modifying `conf\teamcity-server-log4j.xml`.

The same overwriting from the sibling .dist file is recommended for the TeamCity agents.The `conf\teamcity-*-log4j.xml.dist` file is created after the first start of the upgraded TeamCity version.

### Builds metadata storage (NuGet feed)

Builds metadata storage will be re\-created and builds will be reindexed right after the upgrade. As a result, immediately after the upgrade, the TeamCity internal NuGet feed will not contain all of the packages.During the builds reindexing, the following messages may appear in the `teamcity-server.log`:


```Shell

INFO - .index.BuildIndexer (metadata) - Enqueued next 100 builds for indexing, builds left: 2000, last build id: 3813
INFO - .index.BuildIndexer (metadata) - Enqueued next 100 builds for indexing, builds left: 1900, last build id: 3713
```



"builds left:" indicates the number of builds left to process. Note that TeamCity starts reindexing from the most recent builds, so all fresh builds should appear in the TeamCity NuGet feed in a relatively short time.

To increase the metadata indexing speed you could use the [following tip](known-issues.md).

### REST API

REST API has only minor changes, so the same API is exposed under the `app/rest/10.0` and `/app/rest/2017.1` URLs. API version has been updated to 2017.1 though to reflect the changes.The build's node "triggeredBy" now has more correct values of "type" attribute for the builds started after 2017.1 upgrade. In particular, the "buildType" value is not used anymore, the"finishBuild", "snapshot", etc. values are used instead.

### Visual Studio Add-in fails to install from TeamCity UI

As a workaround could be used [ReSharper web installer](https://www.jetbrains.com/resharper/download/#section=web-installer). See [TW-51680](https://youtrack.jetbrains.com/issue/TW-51680) for details.

## Changes from 10.0.4 to 10.0.5

If you are using TFS with agent\-side checkout, note that due to the fix of  [TW-48555](https://youtrack.jetbrains.com/issue/TW-48555)  TeamCity will have to re\-create TFS workspaces, which may result in a clean checkout on the agents after the upgrade.

## Changes from 10.0.3 to 10.0.4

### Precedence of %dep.ID.NAME% parameter references

When `%dep.ID.NAME%` parameter reference is used and there are several dependency paths to the same build configuration with id "ID" so that different builds are accessible via (direct or indirect) artifact dependencies, the result of the reference resolution could have used any of the builds without any guaranteed precedence.

Since 10.0.4 `dep.` parameter resolution works as follows:
1. if there is a snapshot dependency, the build from the same chain wins.
2. if there is no snapshot dependency and several builds are accessible via an artifact dependency, the build with a greater `buildId` wins. If there are several artifact dependencies from a single build configuration, only the first one is considered.
### Updates

AWS SDK has been updated to 1.11.66 to support new instance types (r4.4xlarge, f1.16xlarge, t2.2xlarge, t2.xlarge, r4.2xlarge, r4.xlarge, r4.large, r4.16xlarge, r4.8xlarge, f1.2xlarge).

## Changes from 10.0.2 to 10.0.3

### Amazon EBS–Optimized Instances

The behavior of [EBS optimization](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSOptimized.html), enabled by default since TeamCity 10.0, is changed similarly to what EC2 console offers:

1. EBS optimization is turned on by default for `c4.*`, `m4.*`, and `d2.*` (non-configurable).
2. EBS optimization is turned off by default for any other instance types.
3. EBS optimization can be turned on for instances that support it (such as `c3.xlarge`) by checking the appropriate box when configuring the image of the Amazon cloud profile.

### Bundled tools updates

The bundled dotCover has been updated to version 2016.2.2

## Changes from 10.0.1 to 10.0.2
* The known issue mentioned for 10.0.1 is fixed.
* The bundled JetBrains dotCover has been updated to version 2016.2.
* Jabber integration is now [more restrictive](https://youtrack.jetbrains.com/issue/TW-47046) with regard to SSL connection checking. If you are using jabber.org for sending notifications, and face a problem regarding SSL certificate, you should either enable "Use legacy SSL" option or (better) change JVM version for running TeamCity to at least 1.8.0\_101.
* Precedence of %dep.ID.NAME% parameters resolution was changed unintentionally in case of several different builds used as dependency. See [TW-47518](https://youtrack.jetbrains.com/issue/TW-47518) for details.
## Changes from 10.0 to 10.0.1

All known issues mentioned for 10.0 are fixed.

### Known Issues

(fixed in 10.0.2) TeamCity server temp folder can fill up if an [agent tool](installing-agent-tools.md) is installed as a directory in \<[TeamCity Data Directory](teamcity-data-directory.md)\>\/plugins\/.tools. [Details and workaround](http://youtrack.jetbrains.net/issue/TW-46648).

## Changes from 9.1.x to 10.0

### Known Issues

(These known issues are fixed in 10.0.1)

#### Failed to collect TFS changes - From version &lt;x&gt; is greater then current version &lt;y&gt;

If you use TFS version control and get "Error collecting changes for VCS repository ... Failed to collect TFS changes \- From version x is greater then current version y" error, either commit a new change so that it appears in the affected build configuration, ot install an [updated plugin](https://youtrack.jetbrains.com/issue/TW-46336#comment=27-1534797) (related [issue](https://youtrack.jetbrains.com/issue/TW-46336)).

#### Project administrator may not be able to re-define parameters inherited from the parent project

See request [TW-46372](https://youtrack.jetbrains.com/issue/TW-46372) for details and possible workaround.

#### Upgrade form TeamCity version before 8.1 fails with "Can't take exclusive lock when db lock is not held"

See request [TW-46385](https://youtrack.jetbrains.com/issue/TW-46385) for details and possible workaround.

#### Subversion VCS roots with svn+ssh:// protocol can report "Host key (xxx) can not be verified."

 See request    [ ](https://youtrack.jetbrains.com/issue/TW-46489)  for details and for the plugin with the fix  


[//]: # (Internal note. Do not delete. "Upgrade Notesd333e995.txt")    




### Changes in agent properties reporting .NET 4.x runtime

TeamCity agents before version 10 used to report DotNetFramework4.0\_\* properties whenever any of 4.x versions of .NET framework were installed on the agent. Since TeamCity 10, DotNetFramework4.0\_\* properties are reported only when 4.0 runtime (without updates) is installed. For 4.5.\*, 4.6.\* updates corresponding DotNetFramework4.N\_\* properties are reported. This change in behavior allows for more precise requirements definition.

If after the upgrade you get incompatible agents with __Unmet requirement: Exists=&gt;DotNetFramework4.0\_x86(/x64) exists__ message, review the explicit requirements in your build configuration. If your build is compatible with any .NET 4.x runtime (it is a most common case) please use __Exists=&gt;DotNetFramework4.\*\_x86(/x64)__ requirement. If you would like to run on .NET 4.5\+ agents \- use  __Exists=&gt;DotNetFramework4.(5|6).\*__ requirement. 

Some third\-party plugins are known to be affected. On the upgrade, make sure to upgrade __xUnit__ plugin to version [1.1.2+](https://github.com/carlpett/xUnit-TeamCity/releases/tag/1.1.2)

### Ignored tests table optimization

During upgrade TeamCity will optimize data in the ignored\_tests table (we do that in order to speedup the TeamCity built\-in backup / restore process). In some rare cases, when this table contains millions of rows, the process of table optimization may take significant time \- possibly a few hours. Among other data, the table contains the reasons why a particular test in a build was marked as ignored. If this information for old builds is not very important for you, you can start TeamCity server with the additional [JVM option](configuring-teamcity-server-startup-properties.md#JVM+Options): `-Dteamcity.truncateIgnoreReasonConverter.copyReasons=false`

In this case TeamCity will not copy the ignore reasons into the new, optimized table, and this particular step of the upgrade process will run much faster.

### Java 8

Starting from TeamCity 10, the TeamCity server __[requires Java 8 ](supported-platforms-and-environments.md)__ JRE/JDK (included in the Windows `.exe` distribution).

__TeamCity agents__ currently require Java 1.6\+, but starting from the next TeamCity version, the minimum requirement for [the Java on the agent](supported-platforms-and-environments.md) will be Java 8 (included in agent's Windows `.exe` distribution). It is recommended that you now consider upgrading the agents Java.

__Java memory options change__

It is recommended to remove the " `-XX:MaxPermSize=..."`  [JVM option](configuring-teamcity-server-startup-properties.md) from `TEAMCITY_SERVER_MEM_OPTS` environment variable, if previously configured. (This is due to the fact that Java 8 [does not use](http://javaeesupportpatterns.blogspot.ru/2013/02/java-8-from-permgen-to-metaspace.html) permanent generation (PermGen) anymore)

### Agent requirements and artifact dependencies disabling

Agent requirements and artifact dependencies can be disabled now. TeamCity plugins and REST API\- based code using a version of API prior to TeamCity 10 is likely to ignore the disabled status of these settings.

### TFS

 TeamCity comes with cross\-platform TFS integration: to work with TFS, you no longer need to install a TeamCity server on a Windows machine. 

### Visual Studio Online Work Items plugin 

Visual Studio Online Work Items plugin is __obsolete since TeamCity 10.0__ and can be safely removed. TeamCity 10.0 has a built\-in integration with [Team Foundation Work Items](https://confluence.jetbrains.com/display/TCD10/Team+Foundation+Work+Items) which supports TFS 2010\+ and Visual Studio Team Services. After upgrade, TeamCity will detect the existing issue tracker connections of this plugin and convert them into TFS Work Items.

### Default Checkout mode for newly created build configurations

The default setting for the VCS checkout mode on creating new build configurations has changed: now TeamCity will check out the sources on the agent before the build. If the agent\-side checkout is not possible, TeamCity  will use the server\-side checkout. Explicit server\-side or agent\-side checkout is still in place. The new default applies only to newly created build configurations; all the existing ones will work as configured before.

### Project-based Agent Management Permissions

New TeamCity installations now have different agent management permissions assignments: Project Administrator role does not include (global) Agent Manager role. Instead, Project administrator role has [agent-project permissions](role-and-permission.md) which allow managing agents from the agent pools with only projects where user has Project Administrator role.

Existing installations are not affected by this change in order not to change the user permissions. However, it is recommended to review the Project Administrator role and consider excluding "Agent Manager" role and adding the following permissions:
* Enable / disable agents associated with project
* Start / Stop cloud agent for project
* Change agent run configuration policy for project
* Administer project agent machines (e.g. reboot, view agent logs, etc.)
* Remove project agent
* Authorize project agent
### UI Changes

#### Server Administration UI

The new __Administration | Tools__ page allows setting up tools to be used by appropriate plugins. Tools are automatically distributed to all build agents and can be used in related runners.

#### New Create project / Create build configuration buttons

The new __Create subproject__ and __Create build configuration__ buttons have a drop\-down now allowing you to select whether you want to create a project from scratch (manually), from URL, or using the popular version control systems GitHub.com and Bitbucket. 

#### NuGet-related UI 

NuGet settings page is removed. NuGet.exe can be installed using the new __Tools__ page;  to set up TeamCity as a NuGet Server, go to the  __Administration__  |  __NuGet Feed__  page. 

#### Tests-related UI 

The Problematic Tests tab is no longer available and the View all tests failed within the last 120 hours link is removed from the the  Current Problems tab.

TeamCity now detects [Flaky tests](viewing-tests-and-configuration-problems.md) displayed on the dedicated tab for a given project.

### Visual Studio Add-in

The legacy version of the TeamCity Visual Studio Add\-in is no longer supported. Visual Studio 2005 and 2008 are not supported. 

TeamCity Visual Studio Add\-in is shipped [as a part of ReSharper Ultimate](visual-studio-addin.md). After installation, the TeamCity Add\-in will be available under the RESHARPER menu in Visual studio. 

Note that the installer will remove the pre\-bundle products versions: TeamCity and ReSharper versions prior to 9.0, dotCover prior to 3.0, dotTrace prior to 6.0.ReSharper Ultimate does not support the Visual Studio versions 2005 and 2008.

### IntelliJ IDEA Compatibility

IntelliJ IDEA 12.1 and older as long as other IntelliJ\-based products released prior to year 2013 no longer supported by    [ ](https://confluence.jetbrains.com/) . 

### Snapshot dependencies builds rebuilding

After the server upgrade, the builds used as snapshot dependencies may be rebuilt once even if the snapshot dependency has the "Do not run new build if there is a suitable one" option set to ON. This is done to fix the [issue](https://youtrack.jetbrains.com/issue/TW-43226).

### Perforce

Clean checkout will be enforced in builds with Stream\-based and Client\-based Perforce VCS Roots.

### Subversion

Starting from TeamCity 10, TeamCity does not accept by default connections to SVN servers accessed by https:// protocol with non\-trusted server SSL certificate. To enable access with such certificates, you should either import the certificate to server JVM keychain, or enable VCS Root option "__Accept non\-trusted SSL certificate__" (Enable non\-trusted SSL certificate in 10.0)  ([issue](https://youtrack.jetbrains.com/issue/TW-45592)).

### Bundled tools updates

Ant runner: the bundled Ant distribution has been upgraded from 1.9.6 to 1.9.7

.NET dotCover coverage: the bundled dotCover is updated to 2016.1

ReSharper command line tools: the bundled R# CLT is updated to 2016.1

Java inspections and duplicates: the bundled IntelliJ IDEA is updated to 2016.2

### GitHub Issue Tracker

If you were using the TeamCity\-GitHub  [third-party plugin](https://github.com/milgner/TeamCityGithub)  prior to TeamCity 10.0, you can safely  [remove](installing-additional-plugins.md)  it: the built\-in TeamCity integration will detect the existing connection to GitHub issue tracker and pick up your settings automatically. 

### NuGet Support

Configuration parameters `teamcity.tool.NuGet.CommandLine.%NUGET_VERSION%.nupkg` are not reported anymore. The `teamcity.tool.NuGet.CommandLine.%NUGET_VERSION%` parameters should be referenced instead.

For example, instead of using `%teamcity.tool.NuGet.CommandLine.DEFAULT.nupkg%` parameter reference `%teamcity.tool.NuGet.CommandLine.DEFAULT%` should be used.

### Build Statistics

Several statistic values (metrics) has been reworked and renamed:
* `BuildCheckoutTime` into `buildStageDuration:sourcesUpdate`
* `BuildArtifactsPublishingTime` into `buildStageDuration:artifactsPublishing`
* `ArtifactsResolvingTime` into `buildStageDuration:dependenciesResolving`

Old keys are still supported in charts definitions.

### REST API

REST API uses version 10.0.  The previous versions of the API are still available under `/app/rest/9.1`,` /app/rest/9.0`,` /app/rest/8.1`, `/app/rest/7.0`,` /app/rest/6.0` URLs.

Requests for a set of items with the locator addressing a single item which resulted in 404 responses previously will now return an empty set as a more consistent approach. For example, `.../app/rest/builds?locator=id:<non-existent build id>`. REST debug logging might have diagnostics message with more details as to the case.

Requests for set of items __may return not all/incomplete results__ (with zero or more items included) and provide `nextHref` sub\-element with the link to retrieve the next "page" of items. The search result is complete when no `nextHref` sub-element is provided.

Some requests for set of items (e.g. ...`/app/rest/vcs-roots` and .../`app/rest/vcs-root-instances`) will use paged results by default when queried without a locator (they used to list all the items). Add the "`count:NNN"` locator dimension to set page size.

 __Finding builds__ (`.../app/rest/builds/...` URL)   
 When performing builds scan to find those matched by the locator specified, by default for performance reasons TeamCity will return partial result limited by scanning only 5000 most recent builds. To process a larger portion of the history, check the `nextHref` attribute returned or set the `lookupLimit` locator dimension to a larger value.   
 Previously, until specifically requested the builds from non\-default branch as well as canceled, personal and failed to start builds were not returned. Now these filtered out builds are returned by default for running and queued builds queries as well as when filtering by agent or user. Use `defaultFIlter:true/false` locator dimension to manage the default filtering explicitly.   
 Also, `number:NNN` locator now adheres to the same default logic: only "usual" finished builds from default branch are searched by number and several builds can be returned if found.

__Finding VCS roots__ (`.../app/rest/vcsRoots/... URL`)(minor)   
A VCS root locator with `project` and `buildType` specified used `project` as the context for finding `buildType`. This is no longer the case,the  `buildType` locator should be full one to find the build configuration.

__Build Configuration's Artifact Dependencies__ (entities returned by` .../app/rest/buildTypes/...` URL)   
The `artifact-dependencies` sub\-element of the `buildType` element now uses textual generated ids instead of numeric ones which depended on the order previously. This also affects requests for artifact dependencies modification.   
The `agent-requirements` sub\-element of the `buildType` element now uses generated ids instead of the parameter name as id. This also affects requests for agent requirements modification.

__Editing agent requirements__ (`.../app/rest/buildTypes/.../artifact-requirements/...` URL)   
Previously, on adding  a new agent requirement for the same parameter, the existing one was overridden by the new one; now a new one is added.Previously, on adding  a new agent requirement, the parameter name was derived from the `id` attribute of the `agent-requirement` node. Since TeamCity 10, the parameter name is derived from the "`property-name`" property.

__Test and problem occurrences__ (`.../app/rest/testOccurrences`,` .../app/rest/problemOccurrences` URL)   
The sorting of the returned results is changed for some of the queries compared to the previous versions. For example, the`"../app/rest/testOccurrences?locator=build:(xxx)`" request now returns the tests in the order they were run in the build.   
Previously, test occurrences were sorted by the new status and then by name. Problem occurrences were sorted by problem id.   
Also, the `build` dimension in the test/problem\-related locators now supports multiple builds so for the requests which matched several builds via the "`build`" dimension, all the builds will be processed; previously only the first matching build was processed.

__Entities__   
The `property` entity used to have its "own" Boolean attribute indicating whether the parameter is redefined in the build configuration as opposed to inherited from a template/project. Now the attribute is renamed to '`inherited`' and its value is inverted.   
The `vcs-root` and `vcs-root-instance` used to have `status` and `lastChecked` attributes. Now VCS root instance has `status` element (with `current` node which has `status` and and `timestamp` attributes) and VCS root does not have the data as it produced undefined results in case of several VCS root instances.   
The `test` entity `id` became Sring instead of Long (due to [inability](https://youtrack.jetbrains.com/issue/TW-42028) to represent some Java Long value in JavaScript)

__Build type and template__   
The `settings` node used to contain all the supported settings. Now only those defined in the build configuration or template are present. The same is true for `.../app/rest/buldTypes/XXX/settings/*` requests: only the values changed from defaults are present.

__Compatible agents__   
When querying for compatible agents, only the agents which can actually run the builds are now returned. By default, unauthorized, disconnected and disabled agents are not listed. This behavior differs from that in previous versions which had a number of discrepancies. Affected requests and entities: `.../app/rest/agents?locator=compatible:(...)`; `../app/rest/agents/.../compatibleBuildTypes` and `incompatibleBuildTypes`; nested nodes `Agent.compatibleBuildTypes`, `QueuedBuild.compatibleAgents`, `BuildType.compatibleAgents`.

## Changes from 9.1.6 to 9.1.7

No noteworthy changes.

## Changes from 9.1.5 to 9.1.6

#### Known Issues

There is a [known issue](https://youtrack.jetbrains.com/issue/TW-43731) in the bundled dotCover run on Windows XP and Vista. You can use the [hotfix](https://youtrack.jetbrains.com/issue/TW-43731#comment=27-1286417) or the [workaround ](https://youtrack.jetbrains.com/issue/TW-43731#comment=27-1286522)provided. The issue will be fixed in the next dotCover release.

There is a [known issue](https://youtrack.jetbrains.com/issue/TW-44479) with [NuGet Credentials](https://confluence.jetbrains.com/display/TCD9/NuGet+Feed+Credentials) not working for the Authenticated Feed URL of the internal TeamCity NuCet server on the local agents. As a workaround, instead of using `%teamcity.nuget.feed.auth.server%`, please specify the external server URL in the build step and the build feature.

#### NuGet

NuGet versions prior to 2.8.6 require .Net Framework 4.0\+ installed on the build agent, NuGet 2.8.6 and later requires .NET 4.5.

#### NUnit

Since version 9.1.6, TeamCity does not support NUnit 3 beta versions (released before NUnit 3.0.0).  

The "Run a process per assembly" option of the [NUnit runner](nunit.md) has been removed from NUnit 3 settings. Configure the desired behavior using the required [command line options](https://github.com/nunit/nunit/wiki/Console-Command-Line) in the [corresponding field](nunit.md).

#### Bundled tools updates

Bundled IntelliJ IDEA updated to version # 143.1945 (roughly equivalent to 15.0.3 with a few additional fixes).

 Bundled version of Maven 3.2.x updated to 3.2.5. 

#### Performance Monitor  

Note on permissions: to [monitor performance](performance-monitor.md)   of a build agent run as a  as a Windows [service](https://confluence.jetbrains.com/display/TCD9/Setting+up+and+Running+Additional+Build+Agents#SettingupandRunningAdditionalBuildAgents-BuildAgentasaWindowsService), make sure the user starting the agent is member of the Performance Monitor Users group.

  

## Changes from 9.1.4 to 9.1.5

#### Known Issues

There is a [known issue](https://youtrack.jetbrains.com/issue/TW-43731) in the bundled dotCover run on Windows XP and Vista. You can use the [hotfix](https://youtrack.jetbrains.com/issue/TW-43731#comment=27-1286417) or the [workaround ](https://youtrack.jetbrains.com/issue/TW-43731#comment=27-1286522)provided. The issue will be fixed in the next dotCover release.

#### Product icons

JetBrains product icons are updated in accordance with the [new JetBrains branding](http://blog.jetbrains.com/blog/2015/12/10/the-drive-to-develop/).

#### Git

Since TeamCity 9.1.5, `git sparse-checkout`  [is disabled](https://youtrack.jetbrains.com/issue/TW-43330#comment=27-1229510) by default. To enable it in a TeamCity project, add the `teamcity.git.useSparseCheckout=true` [parameter ](https://confluence.jetbrains.com/display/TCD9/Project+and+Agent+Level+Build+Parameters#ProjectandAgentLevelBuildParameters-ProjectLevelBuildParameters)to this project.

#### Gradle: Breaking change compared to 9.1.2

Gradle runner `system.*` [properties](gradle.md), introduced in __TeamCity 9.1.2__, defined for the build as JVM's system properties of the Gradle process, will not work __since 9.1.5__. Now the TeamCity system properties can be accessed in Gradle scripts as Gradle properties (similarly to the ones defined in the `gradle.properties `file) and are to be referenced as follows:

a) name allowed as a Groovy identifier (the property name does not contain dots): `customUserProperty`   
b) name not allowed as a Groovy identifier (the property name contains dots, e.g. `build.vcs.number.1`): `project.ext["build.vcs.number.1"]`

#### Bundled tools updates

The bundled JetBrains IntelliJ IDEA (IDEA inspections and duplicates) has been updated to version 15.0.2

#### .Net tools updates

JetBrains ReSharper command line tools (.NET inspection and duplicates) have been updated to match ReSharper 10.0.2 releaseTeamCity Visual Studio Addin Web installer updated to ReSharper 10.0.2 releaseBundled JetBrains dotCover updated to version 10.0.2

## Changes from 9.1.3 to 9.1.4

#### Known Issues

Certain roles/permissions configurations can result in error loading roles and no ability for regular users to view projects. In such cases the  "__Circular reference is detected between roles__" critical server error is displayed on the server administration pages for those logged in as server administrator. Please check the [workaround](https://youtrack.jetbrains.com/issue/TW-43186#comment=27-1211556) for the issue.

__Git agent\-side checkout__ may malfunction (details at [TW-43202](https://youtrack.jetbrains.com/issue/TW-43202) ) when the `teamcity.git.use.native.ssh=true` parameter is specified in a build configuration or in the agent config. To fix that, install the [#snapshot-34](https://teamcity.jetbrains.com/viewType.html?buildTypeId=TeamCityPluginsByJetBrains_Git_JetBrainsGitPluginTeamCity91x) build of the Git\-plugin.

__Git agent\-side checkout__ works incorrectly with git client versions 1.7.0\-1.7.4: the checkout directory contains files only, all directories are missing (details at [TW-43330](https://youtrack.jetbrains.com/issue/TW-43330)). To workaround the problem, add the `teamcity.git.useSparseCheckout=false` parameter in the Root TeamCity project.

#### TeamCity Windows binaries signatures

Since 9.1.4  the TeamCity Windows binaries are signed with SHA\-2 code\-signing certificate following the [Microsoft SHA-2 policies](http://social.technet.microsoft.com/wiki/contents/articles/32288.windows-enforcement-of-authenticode-code-signing-and-timestamping.aspx). This means that on systems prior to Windows XP SP3, the executables will not pass code signing verification; newer Windows systems require the corresponding security update from Microsoft.  

#### Bundled tools updates

Bundled Oracle JRE (in both Server and Agent.exe installers) has been updated to version  1.8.0\_66 (32\-bit)

#### .Net tools updates

JetBrains ReSharper command line tools (.NET inspection and duplicates) have been updated to match ReSharper 10.0 release

TeamCity Visual Studio Addin Web installer updated to ReSharper 10.0 release

Bundled JetBrains dotCover updated to version 10.0

## Changes from 9.1.2 to 9.1.3

#### Known Issues

There is a [known issue](https://youtrack.jetbrains.com/issue/DCVR-7708) in the bundled dotCover 3.2 which could cause a build's failure with the following exception: "System.Security.VerificationException: System.Security.VerificationException: Operation could destabilize the runtime". The issue is fixed in dotCover 10.0 bundled with TeamCity 9.1.4 release.

Bundled JVM (Server Windows installer and Agent Windows installer) is updated which results in disabled SSL RC4 chiper suite for outgoing HTTPS connections. For example this makes connections to CloudForge SVN servers non\-functional with "SSLHandshakeException: Received fatal alert: handshake\_failure" error. ([details](https://youtrack.jetbrains.com/issue/TW-42577))

Changes from 9.1.1 to 9.1.2

#### Known issues

The Command line runner can fail to execute a custom script if it has a non\-default hashbang specified at the beginning of the script: [TW-42498](https://youtrack.jetbrains.com/issue/TW-42498)

When using Amazon EC2 cloud integration, an AMI\-image, containing build agent versions 9.1.2\-9.1.5 will appear twice with name EC2\-i\-abcdefgh and EC2\-i\-abcdefgh\-1 consuming 2 licenses. To fix the issue please use an AMI\-image with agent 9.1.6\+. Just updating server to 9.1.6 won't help, if AMI remains the same. ([details](https://youtrack.jetbrains.com/issue/TW-43992))

#### Build status icons

Build status icons updated to a more "standard" look and are of a bit larger now.

#### Bundled tools updates

JetBrains ReSharper command line tools (.NET inspection and duplicates) have been updated to match ReSharper 9.2 release

TeamCity Visual Studio Addin Web installer updated to ReSharper 9.2 release

Bundled JetBrains dotCover updated to version 3.2

Bundled Oracle JRE (in both Server and Agent .exe installers) updated to version 1.8.0\_60 (32\-bit)

## Changes from 9.1 to 9.1.1

Bundled Jacoco coverage library updated to version 0.7.5

Perforce VCS Roots with disabled ticket authentication won't run 'p4 login' operation anymore if password authentication is disabled on the Perforce server.   
I.e. if password authentication is disabled, "Use ticked\-based authentication" option must be enabled on the VCS Root. [TW-42818](https://youtrack.jetbrains.com/issue/TW-42818)

 

## Changes from 9.0.x to 9.1

#### Bundled Ant

Bundled Ant distribution has been upgraded form 1.8.4 to 1.9.6. Note that Ant build steps using bundled Ant will use another version of Ant after the server upgrade.  Ant 1.9.6 requires Java 1.5 at least, so builds using Ant and running under Java 1.4 will stop working.

#### MSTest runner converted into Visual Studio Tests runner

MSTest runner is merged with [VSTest console runner](https://confluence.jetbrains.com/display/TW/VSTest.Console+Runner) (previously provided as a separate plugin) into the [Visual Studio Tests](visual-studio-tests.md) runner. Note that after upgrade to TeamCity 9.1, MSTest build steps are automatically converted to the Visual Studio Tests runner steps, while VSTest steps remain unchanged.

<note>

If you have used [VSTest.Console runner plugin](https://confluence.jetbrains.com/display/TW/VSTest.Console+Runner), make sure that you have latest version (build __32407__) installed. The plugin version can be viewed on __Administration | Plugins List__ page. Earlier versions of this plugin are __not compatible__ with TeamCity 9.1 and may cause malfunction of .NET related build runners which can manifest with `java.lang.NoSuchMethodError: jetbrains.buildServer.runner.NUnit.NUnitVersion.parse(Ljava/lang/String;)` build errors. The plugin can be downloaded from [its page](https://confluence.jetbrains.com/display/TW/VSTest.Console+Runner).
Consider migrating your vstest.console execution steps to the bundled Visual Studio Tests runner.
</note>

#### MSTest installation agent properties

TeamCity agent automatically detects the installed MSTest and used to expose the locations in `system.MSTest.N.N` system properties.

__Since TeamCity 9.1__, the locations are exposed via `teamcity.dotnet.mstest.N.N` configuration parameters. Check [TW-41845](https://youtrack.jetbrains.com/issue/TW-41845) for a workaround if you cannot easily change the properties usage.

#### Nested test reporting

Previously TeamCity supported a case when one test could have been reported from within another test using [service messages](build-script-interaction-with-teamcity.md). Now, after the fix of [TW-40319](https://youtrack.jetbrains.com/issue/TW-40319), starting another test finishes the currently started test in the same "flow". To still report tests from within other tests, you will need to specify another [flowId](build-script-interaction-with-teamcity.md) in the nested test service messages.

#### REST API

REST API uses version 9.1. Previous versions of API are still available under `/app/rest/9.0`, `/app/rest/8.1`, `/app/rest/7.0`, `/app/rest/6.0` URLs.

__Finding builds__   
Summary (tl;dr): Some build filtering rules has subtle changes. Most importantly, a queued build can now be returned instead of 404 when searching by build id and meaning of the "project" locator dimension has changed to be not recursive. Also, failed to start builds are now not included until "failedToStart:any" locator dimension is specified.


[//]: # (Internal note. Do not delete. "Upgrade Notesd333e2056.txt")    


Details:   
Affected requests: /app/builds/&lt;locator&gt;..., /app/builds?locator=&lt;locator&gt;, /app/buildTypes/&lt;btLocator&gt;/builds and others with build locator

locator: id:&lt;number&gt; or taskId:&lt;number&gt;
* previously, if the matching build was a queued one, 404 (Not Found) was returned
* now the queued build is returned

locator: project:&lt;id&gt;...
* previously, all the builds belonging to build configurations of the project and all it's subprojects (recursively) were found
* now only the builds belonging to build configurations of the project specified are found. For finding the builds recursively, use "affectedProject:&lt;id&gt;." dimension. This makes the usage consistent with build type locators.

locator: tag:&lt;text&gt;
* previously, when "&lt;text&gt;" used ":" character, that used to treat the entire "&lt;text&gt;" as tag name
* now the "&lt;text&gt;" is parsed as a nested locator. For searhing tags with ":" character, locator "tag:(name:(&lt;tag&gt;))" should be used

locator: &lt;text&gt;
* previously, if &lt;text&gt; is not a number, response was 400 (Bad Request) with "LocatorProcessException: Invalid single value: '&lt;text&gt;'. Should be a number." message.
* now search by build number across all builds on the server is performed (this is not recommended to be used on production servers). For not found builds 404 (Not found) response is returned

locator: id:&lt;number&gt;,xxx:yyyy
* previously, build was found by id "&lt;number&gt;, other dimensions were ignored
* now if the build found by id does not match other dimensions, response is 404 (Not Found)

locator: agent:&lt;agentLocator&gt;
* previously &lt;agentLocator&gt; was used as is, without applying any defaults (unauthorized agents were included until specifically excluded)
* now &lt;agentLocator&gt; has the same behavior as in /app/rest/agents request: unauthorized agents are excluded by default

__Finding projects__   
Searching project by name used to return 404 error is several projects were matched. Now will return the first project found.

__Build's artifacts__   
There are several bugs fixed in listing build artifacts via /app/rest/builds/&lt;locator&gt;/artifacts/\* requests which can cause subtle changes in the results for the request. Check the new behavior if you relied on the response.The most important changes are:
* the initial path specified via URL part is searched for without current locator value, it will not generate 404 responses until there is no such artifact on disk.
* archives are not treated as directories (do not have children elements) by default. Specify "browseArchives:true" to treat archives as directories (in "recursive:true" mode only one level of archives is treated as directories)

__Agents__   
Agents which are not known to the system (which were deleted) used to have id "\-1". Now "id", "pool" and some other entries are no longer included for such agents.

#### Xcode 7 Support

[Experimental support](xcode-project.md) for Xcode 7 has been added.

#### Issue trackers integration

Due to API changes, third party issue trackers integration plugins might not be compatible with TeamCity version 9.1. Old plugins will not work and will report "java.lang.NoSuchMethodError: jetbrains.buildServer.issueTracker.AbstractIssueProviderFactory.&lt;init&gt;(Ljetbrains/buildServer/issueTracker/IssueFetcher;Ljava/lang/String;)V" error in `teamcity-server.log` log (more details in the [issue](https://youtrack.jetbrains.com/issue/TW-41508)). If you observe such errors, please contact the [plugin authors](https://plugins.jetbrains.com/teamcity). If you are the author of affected plugin, please refer to the related notes in [Open API Changes](https://confluence.jetbrains.com/display/TCD18/Open+API+Changes).

## Changes from 9.0.4 to 9.0.5

No noteworthy changes.

## Changes from 9.0.3 to 9.0.4

No noteworthy changes.

## Changes from 9.0.2 to 9.0.3

No noteworthy changes.

## Changes from 9.0.1 to 9.0.2

No noteworthy changes.

## Changes from 9.0 to 9.0.1

#### Known Issues

If you have enabled versioned settings for projects which use meta\-runners in TeamCity 9.0, on upgrade and following commit into the settings VCS root, the meta runners will be deleted from the server. Workaround is to commit the meta\-runners definitions into the settings repository manually. Related issue: [TW-39519](https://youtrack.jetbrains.com/issue/TW-39519).

#### Oracle 10.x JDBC driver is not supported anymore

Due to missing support for national character sets (nvarchar) in Oracle 10.x JDBC drivers, TeamCity 9.0.1 will ask to upgrade Oracle JDBC drivers to the latest version. The minimal supported version of Oracle JDBC driver is 11.1.

## Changes from 8.1.x to 9.0

#### Known Issues
* If you have custom artifact cleanup rules configured which mention ".teamcity" directory, build logs can be deleted by the cleanup procedure. Make sure you have build logs backup before upgrade and remove all the custom artifacts cleanup rules with ".teamcity". Related issue: [TW-40042](https://youtrack.jetbrains.com/issue/TW-40042). This issue is fixed in 9.0.3 release.
* If you use Microsoft SQL Server database with TeamCity, after the scheduled cleanup background run, TeamCity UI pages can lock until the server restart. See [TW-39549](https://youtrack.jetbrains.com/issue/TW-39549) for details. This issue is fixed in 9.0.2 release.
* If you use LDAP authentication on the server and there are lots of login attempts on the server (e.g. there is an active REST\-using script), OutOfMemory errors can occur and require server restart. Consider installing an LDAP plugin with a fix from the [issue](https://youtrack.jetbrains.com/issue/TW-39316). This issue is fixed in 9.0.1 release.
* If you have large Maven projects, you can see builds failing with OutOfMemoryError. This is caused by update of back\-end embedded Maven to 3.2.3 which has bigger memory footprint. Consider increasing Build Agent [memory limits](https://confluence.jetbrains.com/display/TCD9/Configuring+Build+Agent+Startup+Properties) Related issue: [TW-41052](https://youtrack.jetbrains.com/issue/TW-41052)

#### UUID in XML settings files

__Since TeamCity 9.0__, disk\-stored XML settings definitions of projects, build configurations and VCS roots have unique non\-human\-readable id (uuid) stored. These ids are automatically generated and are assumed to globally unique. On settings files copying, you need to change/make unique not only id (file name) and name (across siblings) of the entity , but also remove it's uuid from the file. TeamCity will generate new uuid automatically.

#### Build logs storage

The location of the build logs in the internal format stored under [TeamCity Data Directory](teamcity-data-directory.md) has changed. The build log files in internal format are now stored under hidden build artifacts.Namely, the location has changed from `system/messages/CHyy/xxyy.*` to `system/artifacts/<PROJECT EXTERNAL ID>/<BUILD CONFIGURATION NAME>/xxyy/.teamcity/logs/buildLog.*`.

Old build logs are migrated to the new location on TeamCity server startup ([TW-37362](http://youtrack.jetbrains.com/issue/TW-37362)). To avoid this migration, `teamcity.skip.logs.migration` internal property should be set __before__ server startup.


[//]: # (Internal note. Do not delete. "Upgrade Notesd333e2251.txt")    


#### Builds re-indexing after upgrade

On the first server start after upgrade from a version prior to 9.0, the server will reindex all builds for the purpose of builds search functionality and NuGet feeds. During the indexing time, some builds will not be available in the search results and in NuGet feeds. The server can also behave in less performant manner way during the indexing. `teamcity-server.log` has corresponding logging. On indexing finishing, there are "BuildIndexer (search) \- Finished re\-indexing builds" and "BuildIndexer (metadata) \- Finished re\-indexing builds" lines in the log.

#### Integration with issue trackers

__Since TeamCity 9.0__, the issue trackers are configured on the project level instead of the global server\-wide configuration.On the server upgrade, all existing issue tracker integrations are moved to the Root project, which makes them still accessible to all the projects on the server.

#### WebSocket connections and proxy servers

__Since 9.0__, TeamCity tries to establish WebSocket connections between the browser and the server for the UI updates. If you have a proxy server (like nginx) in front of the TeamCity web UI, make sure that the proxy is [configured](how-to.md) properly to support WebSocket connections.

If the proxy is misconfigured or does not support the WebSocket protocol, a server health item will be shown for TeamCity system administrators. In this case TeamCity will use plain old polling for the UI updates as before.

#### REST API

REST API uses version 9.0. Previous versions of API are still available under `/app/rest/8.1`, `/app/rest/7.0`, `/app/rest/6.0` URLs.
* Change bean: the `webLink` attribute is renamed to `webUrl` to match other beans ([TW-34398](http://youtrack.jetbrains.com/issue/TW-34398)).
* Sub\-elements representing empty collections in some of the beans are no longer included into responses (used to be included as an empty tag in XML).
* the builds `changes` element does not include the "count" attribute by default (for performance reasons), count can still be included by providing fields parameter like: "fields=$long,changes(count,href)"
* The `/app/rest/agents` request now returns all the authorized agents by default (used to include unauthorized connected agents as well)
* queued builds now have the `id` attribute instead of the `taskId` attribute (they are the same for new builds since TeamCity 9.0)

__Build tags\-related changes__   
The `/app/rest/builds/<buildLocator>/tags` build request now returns a different XML: `<tags count="1"><tag name="TAG"/></tags>` instead of `<tags><tag>TAG</tag></tags>`.   
The same applies to the `/app/rest/buildTypes/<buildTypeLocator>/<buildTypeLocator>/buildTags` request.   
The same change in the structure also applies to the build's entity nested "tags" element.

To create a tag, there is an old way to post a plain\-text tag name to the `app/rest/builds/<buildLocator>/tags` URL.   
When sending POST or PUT XML or JSON requests to the URL, the new XML format is to be used (`<tag name="TAG"/></tags>` instead of `<tag>TAG</tag>`).

#### Handling tests with the same name within a build

In TeamCity 9.0, multiple tests with the same name within the same build are considered a single test with an invocation count. If any of these test runs fail, the whole test is considered failed in the build. The related issue is [TW-24212](https://youtrack.jetbrains.com/issue/TW-24212).

This change results in drop of test number counters in builds which have multiple runs for the same test. If you have a build failure condition which relies on test number in the build, this change may affect you.

If you need the tests to be treated as separate ones, consider running them in the test suites with different names or otherwise changing the test/running logic to change the full test name displayed in TeamCity.

#### Database-related changes

The national character sets (nchar, nvarchar, nclob types) for text fields are now supported in MS SQL databases used by TeamCity. It is recommended to use the Microsoft native JDBC driver, as jTDS JDBC driver does not support the nchar and nvarchar characters. If you still use jTDS, please [migrate](setting-up-an-external-database.md).

Upon upgrade and entering the normal working status, TeamCity starts a background process to move the entries from the vcs\_changes database table to vcs\_change table. This process is transparent and you can continue working with the server as usual. It has negligible impact on the server performance and the only affected logic is the projects import feature (it is recommended to be used only with backups taken after the process is completed). The progress of the process can be seen on the Backup section in the server administration, along with "TeamCity is currently optimizing VCS\-related data in the database for better backup/restore performance" message.   
The other important thing is that the data copying increases the size of the raw database storage.   
If this is an issue for your case (e.g. it might be with Microsoft SQL Server database with set database size limit), it is recommended to ensure the database size limit is twice the current size before the upgrade. It is possible to perform database\-specific procedures to shrink the storage to match the actually stored data after the VCS changes migration process finishes.

#### VCS Root-related changes

The Git and Mercurial VCS roots no longer provide the ability to specify a custom clone path on the server for new VCS roots. If you need this ability, set the following internal properties to `true` for git and mercurial respectively: `teamcity.git.showCustomClonePath`, `teamcity.hg.showCustomClonePath`.

#### Visual Studio Addin

The TeamCity Add\-in installed as a part of [ReSharper Ultimate ](https://www.jetbrains.com/dotnet/) will remove the pre\-bundle products versions: TeamCity and ReSharper versions prior to 9.0, dotCover prior to 3.0, dotTrace prior to 6.0.

Besides, it will not use the settings provided by the 8.1 version. The traditional add\-in downloaded from TeamCity server can still use settings from previous version.

#### Other

TeamCity agent installation via the Java web start installation package is no longer available.

## Changes from 8.1.1 to 8.1.4

No noteworthy changes.

## Changes from 8.1 to 8.1.1

#### Command Line Runner

The change in behavior introduced in 8.1 (see [below](#Known+issue+with+Command+Line+Runner)) has been fixed. Command line runners using "Executable with parameters" option which were created/changed with TeamCity 8.1can expose a change in behavior with the upgrade. The recommended approach is to switch to "Custom script" option instead of "Executable with parameters" in command line runner.

#### Separate download for VSTest.Console runner

VSTest console runner is no longer bundled with TeamCity and is available as a separate plugin. For download details, see the [plugin page](https://confluence.jetbrains.com/display/TW/VSTest.Console+Runner)

## Changes from 8.0.6 to 8.1

#### Known issue with creating MS SQL database with integrated security

When installing TeamCity anew and creating an MS SQL database with integrated security using the database setup UI, you may receive an error. We are planning to resolve it in the next bugfix, meanwhile use the following workaround:
1. After receiving the error, stop the TeamCity server.
2. Start the TeamCity server.
3. On the database configuration screen, fill out the required information. __Do not use the Refresh button.__ Make sure the information specified is correct.
4. Continue with the configuration steps.

If for some reason the workaround above does not resolve the problem, do the following:

1. Start the TeamCity server, approve a new database creation, configure the MS SQL access with a login and password, without [integrated security](setting-up-an-external-database.md).
2. Ensure that TeamCity works properly.
3. Stop the TeamCity server.
4. Modify the [Setting up an External Database](setting-up-an-external-database.md) file: configure MS SQL connection string to use integrated security, and remove the login and password.
5. Start the TeamCity server again.

#### Known issue with VSTest.Console runner

A new "VSTest.Console" runner which first appeared in TeamCity 8.1 is in experimental state and is not recommended for production use at this time. It will not be present in TeamCity 8.1.x by default (will be available as a separate download).

#### Known issue with PowerShell runner

PowerShell runner plugin is broken in 8.1. Fix is available, please follow instructions in [issue comment](http://youtrack.jetbrains.com/issue/TW-21554#comment=27-676676).

#### Known issue with Command Line Runner

Command line runner using "Executable with parameters" option can process quotes (") and percentage signs (%) in a bit different way then in previous TeamCity versions (see details in the [issue](http://youtrack.jetbrains.com/issue/TW-35087)). To switch back to the previous (8.0) behavior you may specify command.line.run.as.script=false configuration parameter in a build configuration or in a project. The issue is fixed in 8.1.1.   
The recommended approach is to switch to "Custom script" option instead of "Executable with parameters" in command line runner.

#### Memory Settings

If you have not [switched to 64 bit JVM](installing-and-configuring-the-teamcity-server.md) yet and use `-Xmx1300` memory setting for the server and the server is running on Windows, consider decreasing the setting to `-Xmx1200` as otherwise you might encounter "Native memory allocation (malloc) failed" JVM crash. See the [recommended memory settings](installing-and-configuring-the-teamcity-server.md) for details.

#### Actions menu

Some actions has moved under the "Actions" button available at the top\-right of the page, near Run button.   
These include:"Label this build sources" on Changes tab of a build,   
"Pause", "Copy", "Move", "Delete", "Associate with Template", "Extract Template", "Extract Meta\-Runner" on build configuration settings administration page,   
"Copy", "Move", "Delete", "Archive", "Bulk edit IDs" on project settings administration page.

#### Create Maven build configuration is not available by default

Action "Create Maven build configuration" is no longer available. Most of its functionality is covered by create project from URL and create VCS root from URL pages.

#### triggeredBy parameter from GroovyPlug plugin

The `build.triggeredBy` and `build.triggeredBy.username` configuration parameters provided by the [plugin](https://confluence.jetbrains.com/display/TW/Groovy+plug) added by the plugin are now [available](predefined-build-parameters.md) without the plugin under teamcity.build.triggeredBy and teamcity.build.triggeredBy.username names respectively. Consider migrating to the latter set of parameters in your settings if you used the plugin's ones.

#### Shared Resources build feature

If the build takes lock on all values of a resource with custom values, these values are provided as lock values in build parameters. Corresponding issue: [TW-29779](http://youtrack.jetbrains.com/issue/TW-29779)

#### TeamCity Disk Space Watcher

The following [internal properties](configuring-teamcity-server-startup-properties.md) define free disk space thresholds on the TeamCity server machine:
* `teamcity.diskSpaceWatcher.threshold` set to 500 Mb by default displays a warning on all the pages of the TeamCity Web UI.
* `teamcity.pauseBuildQueue.diskSpace.threshold` set to 50 Mb by default pauses the build queue.
The `teamcity.diskSpaceWatcher.softThreshold` property is removed.

#### PowerShell

The PowerShell plugin now uses the version that was specified in the UI as the `-Version` command line argument when executing scripts. Corresponding issue: [TW-33472](http://youtrack.jetbrains.com/issue/TW-33472)

#### REST API

The latest version of the API has not changed, it is still "8.0" while there are changes in the API detailed below. If you find this inconvenient for your REST API usages, please comment in the corresponding [issue](http://youtrack.jetbrains.com/issue/TW-35227).

Entities returned in the response of REST API requests might now exclude attributes/elements with empty/default values. This is relevant for boolean fields with "false" value and empty collections. The recommended approach is to make sure the client code assumes "false" as a value for not present boolean attributes/elements.

"projectName" of buildType node now contains full project name (with the names of the parent projects) instead of the short name of the project.

In the lists of builds, "startDate" attribute is not longer included in the "build" node. It has become an element instead of attribute to match the full build data representation. If your REST API usage is affected, check [a way](http://youtrack.jetbrains.com/issue/TW-35032#comment=27-704059) to get that element in a request for the list of builds.

Requests /app/rest/buildTypes/XXX/parameters/YYY and /app/rest/projects/XXX/parameters/YYY now support "text/plain" and "application/xml" responses. To get plain text response (which was the only supported way before 8.1) you will need to supply "Accept: text/plain" header to the request.

Password properties of the VCS roots are now included into the responses, just without values.

CCTray\-format XML (`app/rest/cctray/projects.xml`) does not include paused build configurations now.

Response to the experimental request `/app/rest/buildTypes/XXX/investigations` has changed the format and got additional fields to cover tests and problem investigations. There is an internal property `rest.beans.buildTypeInvestigationCompatibility` to include removed sub\-items. Please let us know via [support email](https://confluence.jetbrains.com/display/TW/Feedback) if you need to use the internal property.

#### Eclipse plugin

Dropped support of Subversion 1.4\-1.6. Now only Subversion 1.7\-1.8 working copies formats supported.

## Changes from 8.0.5 to 8.0.6

No noteworthy changes.

## Changes from 8.0.4 to 8.0.5

No noteworthy changes.

## Changes from 8.0.3 to 8.0.4

#### First Cleanup

First Cleanup after server upgrade might take a bit more time then regularly if there are many builds on the server. Following cleanups will then run a bit faster then in previous versions.

## Changes from 8.0 to 8.0.3

No noteworthy changes.

## Changes from 7.1.x to 8.0

#### Project and Build Configuration IDs

This version introduces user\-assignable IDs for projects and build configurations. This new ID is now used instead of internal id (projectN and btNNN) in at least:
* URLs of the web pages and artifact downloads
* in REST API
* project IDs are also used in directory names on the server under `<TeamCity Data Directory>\system\artifacts` instead of project names used prior to TeamCity 8.0

If you used any of the above, please, verify if you are affected by the change.   
Learn more about IDs at [Identifier](identifier.md).

On upgrade, all the projects get automatically generated IDs based on their names.   
Build configuration IDs are set to be equal to internal (btNNN) ids and can be later changed from the Administration UI via the __Regenerate ID__ or __Bulk Edit IDs__ actions.

Please note that the names of the projects and build configurations are no longer unique server\-wide (are only unique within the direct parent project) and can contain any symbols which might be relevant if you used these in directory or file names.

#### Project settings format on disk

The format of the project settings storage on the disk under `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>/config` has been changed.   
If you used any tools to read or update `project-config.xml` files, you will need to update the tools. It is recommended to use REST API or TeamCity open API (Java) to make changes so that the tools are not hugely affected by the format change.

#### Build Configuration templates

In version 8.0 build configuration templates support project hierarchy and TeamCity uses new rules:
* The TeamCity administration UI limits the use of templates only to those from the current project and its parents. On copying a project or a build configuration, the templates which do not belong to the target project or one of its parents are automatically copied.
* TeamCity no longer allows attaching a build configuration to a template if the template does not belong to the current project or one of its parents.
* Before version 8.0 it was possible to extract templates from a build configuration of one project to an unrelated project or to associate a build configuration in one project with a template in another. After upgrade to TC 8.0, such templates will become inaccessible in the current project. To reuse build configuration templates from an unrelated project, it is recommended to manually move them into the common parent project (or the Root project if you want them to be globally available).
#### JVM-originated agent parameters (os.arch and others)

The agent no longer reports system properties which come from the agent JVM: `system.os.arch, system.os.name, system.os.version, system.user.home, system.user.name, system.user.timezone, system.user.language, system.user.country, system.user.variant, system.path.separator, system.file.encoding, system.file.separator`).All the aforementioned parameters are now reported as configuration parameters with the `teamcity.agent.jvm.` prefix instead.If you used any of the parameters, make sure you update them to the new values.

#### IntelliJ IDEA project runner

IntelliJ IDEA project runner now uses IntelliJ IDEA's external make tool to build projects. Since this tool requires Java 1.6 to work, IntelliJ IDEA project runner now requires Java 1.6 (at least) too.

#### Clean-up for build configurations with feature branches

Build configurations with feature branches now process clean\-up rules per\-branch which can result in more builds preserved during clean\-up than in previous versions. See [details](clean-up.md).

#### Team Foundation Server integration

TFS now prefers Team Explorer 2012 to Team Explorer 2010 (if both are installed) for TFS operations


[//]: # (Internal note. Do not delete. "Upgrade Notesd333e2777.txt")    




#### Compatibility with YouTrack

If you use JetBrains YouTrack and use its TeamCity integration features, please note that only YouTrack version 4.2.4 and later are compatible with TeamCity 8.0.   
If you need earlier YouTrack versions to work with TeamCity 8.0, please [let us know](https://confluence.jetbrains.com/display/TW/Feedback).


[//]: # (Internal note. Do not delete. "Upgrade Notesd333e2793.txt")    




#### REST API

__External ids__ There are changes in the API related to the new external ids for project/build types/templates as well as other changes.   
The old API compatible with TeamCity 7.1 is still provided under "/app/rest/7.0" URL.

If you used URLs with locators having "id" for projects, build configuration or templates (like `.../app/rest/projects/id:XXX` or `.../app/rest/buildTypes/id:XXX`), please, update the locators to one of the following:
* (recommended) "id:EXTERNAL\_ID" (you can get the external ID in web UI URLs or via a request to `.../app/rest/projects/internalId:OLD\_ID/id`
* just "ANY\_ID" to find the entity either by its internal, external id or name (use with caution: you can find more than you expect)
* "internalId:INTERNAL\_ID" to find the entity by the internal idYou can also use the "/app/rest/7.0/" URL prefix instead of "/app/rest/" to work with 7.0\-version of REST API which still uses internal IDs except for finish build trigger properties.

Also, it is possible to set the internal property `rest.compatibility.allowExternalIdAsInternal=true` to turn on the compatibility mode so that id:xxx locators will search also by the internal id. Note that this will be dropped in the future versions of TeamCity and is not recommended for use.

__Other Changes__   
Requests for builds ".../builds/&lt;locator&gt;/..." and ".../builds?locator=&lt;locator&gt;" no longer return personal and canceled builds by default. To include those, make sure you add ",personal:any,canceled:any" to the locators.

The "relatedIssues" element of the build entity no longer contains a full list of related issues. It has only the "href" attribute whose value can be used to get the related issues via a separate request.   
There is also an internal property "rest.beans.build.inlineRelatedIssues" which can be set to `true` to return the "relatedIssues" node back for compatibility. See [TW-20025](http://youtrack.jetbrains.com/issue/TW-20025) for details. Also, the ".../builds/xxx/related\-issues" URL is renamed to ".../builds/xxx/relatedIssues".

The "source\_buildTypeId" property is dropped from snapshot and artifact dependency nodes. Instead, the "source\-buildType" sub\-element is added with a reference to the build type.   
Creating dependencies is still supported with the "source\_buildTypeId" property, but is deprecated. There is an internal property "rest.compatibility.includeSourceBuildTypeInDependencyProperties" which can be set to `true` to include the "source\_buildTypeId" property back.

In version 8.0 VCS roots support project hierarchy:
* When creating a VCS root, the `project` element should always be provided now. The element supports the `locator` attribute to specify the project.
* the `shared` attribute is dropped from the VCS root: after upgrade, such VCS roots are attached to the root project (with the "\_Root" ID) and become globally available.
* when copying projects and build configurations, the `shareVCSRoots` attribute is no longer present. To make the VCS root available to projects and build configurations, move it to the parent/root project and then proceed with the copying.

It is recommended to create projects hierarchy which corresponds to organizational/settings sharing structure and move the VCS roots to most nested umbrella projects. Users then can be granted "Create / delete VCS root" role in the project to be able to edit VCS roots. Please note that users can edit a VCS root only if it is used in the projects they have "Edit project" permission for.

The "template" attribute in a build configuration template node is renamed to "templateFlag".

PUT for /users/&lt;locator&gt;/roles and /userGroups/&lt;locator&gt;/roles now accepts list of roles as it should and replaces existing roles instead of accepting single riles and adding it.

Many of PUT and POST requests which used to return nothing now return the entities created.

#### Open API changes

See [details](https://confluence.jetbrains.com/display/TCD18/Open+API+Changes)

#### Shared Resources plugin

If you used the [Shared Resources plugin](https://confluence.jetbrains.com/display/TW/TeamCity+Shared+Resources) with TeamCity 7.1.x, make sure to remove it as it is now bundled. See the [upgrade instructions](https://confluence.jetbrains.com/display/TW/TeamCity+Shared+Resources).

#### Queue Manager plugin

If you used the [QueueManager plugin](https://confluence.jetbrains.com/display/TW/TeamCity+Queue+Manager), make sure to remove it as it is now bundled. See the [upgrade instructions](https://confluence.jetbrains.com/display/TW/TeamCity+Queue+Manager)

#### Bundled Maven

Maven bundled with TeamCity upgraded to version 3.0.5.

#### HTTPS connections from agents to server

If your agents connect to the TeamCity server by HTTPS protocol, and after upgrade agents fail to connect with error messages like:javax.net.ssl.SSLHandshakeException: Received fatal alert: handshake\_failure

then you should change Tomcat SSL connector configuration, i.e. add the following attribute to SSL connector and restart TeamCity server:sslEnabledProtocols="TLSv1,SSLv3,SSLv2Hello"

The issue only manifests when the server runs under Java 1.7.

See also:
* [http://mail-archives.apache.org/mod_mbox/tomcat-users/201302.mbox/%3C512559F7.4080001@gmail.com%3E](http://mail-archives.apache.org/mod_mbox/tomcat-users/201302.mbox/%3C512559F7.4080001@gmail.com%3E)
* [http://youtrack.jetbrains.com/issue/TW-30221#comment=27-561843](http://youtrack.jetbrains.com/issue/TW-30221#comment=27-561843)
## Changes from 7.1.4 to 7.1.5

__teamcity.build.branch__ parameter semantics has changed, see [http://youtrack.jetbrains.com/issue/TW-23699#comment=27-448002](http://youtrack.jetbrains.com/issue/TW-23699#comment=27-448002)

## Changes from 7.1.3 to 7.1.4

No noteworthy changes.


[//]: # (Internal note. Do not delete. "Upgrade Notesd333e2962.txt")    




## Changes from 7.1.2 to 7.1.3

No noteworthy changes.

Please check up\-to\-date list of [known regressions](http://youtrack.jetbrains.com/issues/TW?q=Affected+versions%3A+%7BFaradi+7.1.3+%2824266%29%7D+tag%3Aregression) for the version in our issue tracker.

## Changes from 7.1.1 to 7.1.2

__Possible issues with hg server\-side checkout__   
There is a known issue with 7.1.2 release: [TW-24405](http://youtrack.jetbrains.com/issue/TW-24405) which can reproduce when server\-side checkout, labeling or file content viewing are used for Mercurial repository.If you experience the error with message "abort: destination 'hg1' is not empty", please install the patch attached to the issue.

__Other known issues__   
Please also check a list of [known regressions](http://youtrack.jetbrains.com/issues/TW?q=Affected+versions%3A+%7BFaradi+7.1.2+%2824170%29%7D+tag%3Aregression) for the version in our issue tracker.

## Changes from 7.1 to 7.1.1

No noteworthy changes.

## Changes from 7.0.x to 7.1

__Windows service configuration__   
Since version 7.1, TeamCity uses its own service wrapping solution for the TeamCity server as opposed to that of default Tomcat one in previous versions.This changes the way TeamCity service is configured (data directory and server startup options including memory settings) and makes it unified between service and console startup.   
Please refer to the updated [section](configuring-teamcity-server-startup-properties.md) on configuring the server startup properties.

Agent windows service started to use OS\-provided environment variables. Once Agent server (and JVM) are x86 processes, agent will report x86 environment variables. The change may affect your CPU bitness checks. See [MSDN Blog](http://blogs.msdn.com/b/david.wang/archive/2006/03/26/howto-detect-process-bitness.aspx) on how to check if machine supports x64 by reported environment variables

__Default location for TeamCity Data Directory when installed with Windows installer__   
This is only relevant for fresh TeamCity installations with Windows installer. Existing settings are preserved if you upgrade an existing installation.   
Windows installer now uses `%ALLUSERSPROFILE%\JetBrains\TeamCity` location as default one for [TeamCity Data Directory](teamcity-data-directory.md). In TeamCity 7.0 and previous versions that used to be `%USERPROFILE%\.BuildServer`.

__Windows domain login module__   
When TeamCity server runs under Windows and Windows domain user authentication is used, TeamCity now uses another library (Waffle) to talk to the Windows domain.Under Linux the behavior is unchanged: jCIFS library is used as it were.

Unless you specified specific settings for jCIFS library in ntlm\-config.properties file, your installation should not be affected.   
If you experience any issues with login into TeamCity with your Windows username/password after upgrade, please provide details [to us](https://confluence.jetbrains.com/display/TW/Feedback). In the mean time you can switch to using old jCIFS library. For this, add `teamcity.ntlm.use.jcifs=true` line into [internal properties file](configuring-teamcity-server-startup-properties.md).   
Please note that jCIFS library approach can be depricated in future versions of TeamCity, so the property specification is not recommended if you can go without it.

__Checkout directory change for Git and Mercurial__   
Build configurations that have either Git or Mercurial VCS roots and use default checkout directory will perform clean checkout upon upgrade. The clean checkout will be triggered by changed default checkout directory name. Further builds will reuse the checkout directory more aggressively (all builds using different branches but using the same VCS root will use the same directory). This affects agent\- and server\-side checkouts.

__Perforce agent checkout workspace names change__   
Build configurations using Perforce agent\-side checkout will perform clean checkout once after server upgrade. This is related to changed names for automatically generated Perforce workspaces.

__SVN revision format__   
For changes, detected in external repositories, SVN revision got format `NNN_MMM:EXTUUID_CHANGEDATE`, where `NNN` \- revision of the main repository, `MMM` \- revision of externals repository, `EXTUUID` \- UUID of externals repository, `CHANGEDATE` \- change timestamp. This change may affect plugins/REST api clients which use revision of the last build change somehow.

__Eclipse IDE plugin compatibility__   
Since TeamCity 7.1, Eclipse version 3.3 (Europa) is no longer supported by TeamCity Eclipse plugin.Eclipse 3.8 and Eclipse 4.2 (Juno) are now supported.

__Default schema when Microsoft SQL Server is used as an external database__   
Starting with version 7.1 TeamCity works only with a single database schema unlike previous versions when it could work with tables in any schemas of the database server.

TeamCity\-related tables should now be located in the database schema which is set as default one for the database user used by TeamCity to connect to the database.

This change may require reconfiguration of the database to set default schema for the user used by TeamCity server to connect to the database.

Please check that all TeamCity\-related tables are located in the default user's schema before performing the upgrade. (e.g. [using](http://blog.sqlauthority.com/2009/06/17/sql-server-list-schema-name-and-table-name-for-database/) the 'sys.tables' view)

If the default user's schema is not set right, TeamCity can report "TeamCity database is empty or doesn't exist. If you proceed, a new database will be created." message on the first start of newer TeamCity.

To change user's default schema, use the '[alter user](http://msdn.microsoft.com/en-us/library/ms176060.aspx)' SQL command.

For the default schema description, see the "Default Schemas" section in the [corresponding documentation ](http://msdn.microsoft.com/en-us/library/ms190387.aspx).

__Open API changes__   
See [details](https://confluence.jetbrains.com/display/TCD18/Open+API+Changes)

## Changes from 7.0.1 to 7.0.4

No noteworthy changes.

## Changes from 7.0 to 7.0.1

__HTML report tabs URLs Change__   
If you use direct links for build\-level or project\-level [report tabs](including-third-party-reports-in-the-build-results.md), please update the links as they will [change](http://youtrack.jetbrains.com/issue/TW-20462#comment=27-302397) after upgrade. The change is necessary to make the feature more reliable.

## Changes from 6.5.x to 7.0

__(Known issue) Build can hang or produce memory error for NUnit and other .Net test runners__   
Affected are: .Net test runners (NUnit, MSTest, MSpec) as well as TeamCity NUnit console launcher.   
Reproduces when path to test assemblies has several deep paths without wildcards ("\*").

Visible outcome: build hangs or fails with OutOfMemoryException error after "Starting ...JetBrains.BuildServer.NUnitLauncher.exe" link in the build log.

The issue ([TW-20482](http://youtrack.jetbrains.com/issue/TW-20482)) is fixed and the fix will be included in the next release.   
Patch with a fix is [available](http://youtrack.jetbrains.com/issue/TW-20482#comment=27-302393).

__Minimum Supported Project JDK for Ant Runner__   
Starting with this version Ant runner requires minimum of JDK 1.4 in __runtime__ build part (was 1.3 previously). This means that you will not be able to use TeamCity Ant runner if your project uses JDK 1.3 for compilation or tests running.For projects that require JDK 1.3 you can use command\-line runner instead and configure "XML report processing" build feature to parse test reports.

  __Supported Java for Server and Agent__   
  Starting with this version the following requirements
* TeamCity __server__ should be run with JRE 1.6 or above (was 1.5 previously). TeamCity .exe distribution is already bundled with appropriate Java. For .tar.gz or .war TeamCity distributions you might need to install and configure server [manualy](installing-and-configuring-the-teamcity-server.md).
* TeamCity __agent__ should be run with JRE 1.6 or above (was 1.5 previously). Agent .exe distribution is already bundled with appropriate Java. If you used .zip agent distribution or installed the TeamCity agent with TeamCity version 5.0 or earlier, you might need [manual steps](setting-up-and-running-additional-build-agents.md). If you run TeamCity 6.5.x, please check "Agents" page of your existing TeamCity server: the page will have a yellow warning in case any of the connected agents are running JDK less than 1.6.

<note>

If any of your agents are running under JDK version less than 1.6, the agents will fail to upgrade and will stop running on the server upgrade. You will need to recover them manually by installing JDK 1.6 and making sure the agents will [use it](setting-up-and-running-additional-build-agents.md).
</note>

__Project/Template parameters override__   
In TeamCity 7.0 project parameters have higher priority than parameters defined in template, i.e. if there is a parameter with some name and value in the project and there is parameter with the same name and different value in template of the same project, value from the project will be used. This was not so in TeamCity 6.5 and was [changed](http://youtrack.jetbrains.com/issue/TW-17247) to be more flexible when template belongs to anohter project.Build configuration parameters have the highest priority, as usual.

__Support for Sybase is discontinued__   
  From this version support for Sybase as external database is shifted back into "experimental" state.   
  The reason for this decision is that it does not seem like the database is actively used with TeamCity, and supporting it requires a significant effort from TeamCity team which otherwise can be directed to improving more important areas of the product.   
  While it should be still [possible](https://confluence.jetbrains.com/display/TCD65/Setting+up+an+External+Database), we do not recommend using Sybase as an external database and we are not planning to provide support for the Sybase\-related issues.   
  Please consider using one of the other [databases supported](supported-platforms-and-environments.md). If you use Sybase, please migrate to another database before upgrading TeamCity.

__REST API Changes__
* Several objects got additional attributes and sub\-elements (e.g. BuildType, VcsRoot). Please check that your parsing code still works. _/buildTypes/_ path: BuildType object dropped runParameters field (as well as _/&lt;locator&gt;/runParameters_ path is dropped) in favor of _steps_ collection and _/&lt;locator&gt;/steps/_ path.
* A [bug](http://youtrack.jetbrains.net/issue/TW-17478) fixed which resulted in non\-array JSON representation of single element arrays for some resources. Please check if your code is affected.
* in build object, "dependency\-build" element is renamed to "snapshot\-dependencies", revisions/revision/vcs\-root is renamed to revisions/revision/vcs\-root\-intance (and it points to resolved VCS root instance now), revisions/revision/display\-version is renamed to "version".
* in buildType object, "vcs\-root" element is renamed to "vcs\-root\-entries"

Old version of the REST API is available via `/app/rest/6.0/...` URL in TeamCity 7.0. Please update your REST\-using code as future versions of TeamCity might drop support for 6.0 protocol.

__Minimum version of supported Tomcat__   
If you use TeamCity .war distribution, please note that Tomcat 5.5 is no longer supported. Please update Tomcat to version 6.0.27 or above (Tomcat 7 is recommended).

__Swabra__

swabra.handle.exe.path and handle.exe.path configuration parameters are no longer supported for providing path to Sysinternals handle.exe on agents. See the [plugin page](build-files-cleaner-swabra.md) for the details.

__Open API Changes__

Classes from __jetbrains.buildServer.messages.serviceMessages__ package like __jetbrains.buildServer.messages.serviceMessages.BuildStatus__ no longer depend on __jetbrains.buildServer.messages.Status__ class. To make your code compatible with TeamCity 6.0 \- 7.0 you can use __jetbrains.buildServer.messages.serviceMessages.ServiceMessage#asString__ methods, for example:


```Shell

ServiceMessage.asString("buildStatus", new HashMap<String, String>() {{
  put("text", "Errors found");
  put("status", "FAILURE");
}});

```



See also [Open API Changes](https://confluence.jetbrains.com/display/TCD18/Open+API+Changes)

## Changes from 6.5.4 to 6.5.6

No noteworthy changes

## Changes from 6.5.4 to 6.5.5

(Known issue infex in 6.5.6) .NET Duplicates finder may stop working, the patch is available, please see this comment: [http://youtrack.jetbrains.net/issue/TW-18784#comment=27-261174](http://youtrack.jetbrains.net/issue/TW-18784#comment=27-261174)

## Changes from 6.5.3 to 6.5.4

No noteworthy changes

## Changes from 6.5.3 to 6.5.4

No noteworthy changes

## Changes from 6.5.2 to 6.5.3

No noteworthy changes

## Changes from 6.5.1 to 6.5.2

__Maven runner__   
Working with MAVEN\_OPTS has changed again. Hopefully for the last time within the 6.5.x iteration. (see [http://youtrack.jetbrains.net/issue/TW-17393](http://youtrack.jetbrains.net/issue/TW-17393))   
Now TeamCity acts as follows:
1. If MAVEN\_OPTS is set TeamCity takes JVM arguments from MAVEN\_OPTS
2. If "JVM command line parameters" are provided in the runner settings, they are taken instead of MAVEN\_OPTS and __MAVEN\_OPTS is overwritten with this value to propagate it to nested Maven executions__.

Those who after upgrading to 6.5 had problems of not using MAVEN\_OPTS and who had to copy its value to the "JVM command line parameters" to make their builds work, now don't need to change anything in their configuration. Builds will work the same way they do in 6.5 or 6.5.1.

## Changes from 6.5 to 6.5.1

__(Fixed known issue) Long upgrade time and slow cleanup under Oracle__

## Changes from 6.0.x to 6.5

__(Known issue) Long upgrade time and slow cleanup under Oracle__   
On first upgraded server start the database structures are converted and this can take a long time (hours on a large database) if you use Oracle external database ([TW-17094](http://youtrack.jetbrains.net/issue/TW-17094)). This is already fixed in 6.5.1.

__Agent JVM upgrade__   
  With this version of TeamCity we added semi\-automatic upgrade of JVM used by the agents. If there is a Java 1.6 installed on the agent, and the agent itself is still running under the Java 1.5, TeamCity will ask to switch agent to Java 1.6. All you need is to review that detected path to Java is correct and confirm this switch, the rest should be done automatically. The operation is per\-agent, you'll have to make it for each agent separately. Note that we recommend to switch to Java 1.6, as at some point TeamCity will not be compatible with Java 1.5. Make sure newly selected java process has same firewall rules (i.e. port 9090 is opened to accept connections from server)

__IntelliJ IDEA Coverage data__   
Coverage data produced by IntelliJ IDEA coverage engine bundled with TeamCity 6.5 can only be loaded in IntelliJ IDEA 10.5\+. Due to coverage data format changes older versions of IntelliJ IDEA won't be able to load coverage from the server.

__IntelliJ IDEA 8 is not supported__   
Plugin for IntelliJ IDEA no longer supports IntelliJ IDEA 8.

__Unsupported MySQL versions__   
Due to [bugs](http://youtrack.jetbrains.net/issue/TW-16508#comment=27-219601) in MySQL 5.1.x TeamCity no longer supports MySQL versions in range 5.1 \- 5.1.48. TeamCity won't start with appropriate message if unsupported MySQL version is detected. Please upgrade your MySQL server to version 5.1.49 or later.

__Finish build properties are displayed__   
Finished builds now display all their properties used in the build on "Parameters" tab. This can potentially expose names and values of parameters from other builds (those that the given build uses as artifact or snapshot dependency). Please make sure this is acceptable in your environment. You can also manage users who see the tab with "View build runtime parameters and data" permissions which is assigned "Project Developers" role by default.

__PowerShell runner is bundled__   
If you installed [PowerShell plugin](https://confluence.jetbrains.com/display/TW/PowerShell) manually, please remove it form `.BuildServer/plugins` as a fresh version is now bundled with TeamCity.

__Changed settings location__
* XML test reporting settings are moved from runner settings into a dedicated [build feature](adding-build-features.md).
* "Last finished build" artifact dependency on a build which has snapshot dependency is automatically converted into dedicated "Build from the same chain" source build setting.

__Responsibility is renamed to Investigation__   
A responsibility assigned for a failing build configuration or a test is now called investigation. This is just a terminology change to make the action more neutral.   
If you have any email processing rules for TeamCity investigation assignment activity, please check is they need updating to use new text patterns.

__REST API Changes__   
Several objects got additional attributes and sub\-elements (e.g. "startDate" in reference to a build, "personal" in a change). Please check that your parsing code still works.

__Cleaning Non\-default Checkout Directories__   
In previous releases, if you have specified build [checkout directory](build-checkout-directory.md) explicitly using absolute path, TeamCity would not clean the content of the directory to free space on the disk.   
This is no longer the case.   
So if you have absolute path specified for the checkout directory and you need the directory to be present on agent for other build or for the machine environment, please set `system.teamcity.build.checkoutDir.expireHours` [property](build-checkout-directory.md) to "never" and re\-run the build. Please take into account that using [custom checkout directory](build-checkout-directory.md) is not recommended.

If you are using one of [system.teamcity.build.checkoutDir.expireHours](build-checkout-directory.md) properties and it is set to "never" to prevent the checkout directory from automatic deletion, the directory might be deleted once after TeamCity upgrade. Running the build in the build configuration once after the upgrade (and within 8 days from the previous build) will ensure that the directory preserves the "protected" behavior and will not be automatically removed by TeamCity.

__Free disk space__   
This release exposes [Free disk space feature](free-disk-space.md) in UI that was earlier only available via setting build configuration properties.   
While the old properties still work and take precedence, it is highly recommended to remove them and specify the value via "Disk Space" build feature instead. Future TeamCity versions might stop to consider the properties specified manually.

__Command line runner__   
`@echo off` which turns off command\-echoing is added to scripts provided by "Custom script" runner parameter. To enable command\-echoing add `@echo on` to the script.

__Windows Tray Notifier__   
You will need to upgrade windows tray notifier by uninstalling it and installing it again. Unfortunately, auto\-upgrade will not work due to issues in old version of Windows Tray Notifier.

__Maven runner__
* In earlier TeamCity versions Maven was executed by invoking the 'mvn' shell script. You could specify some parameters in MAVEN\_OPTS and some in UI. Maven build runner created its own MAVEN\_OPT by concatenating these two (`%MAVEN\_OPTS%\+jvmArgs`). In this case, if some parameter was specified twice \- in MAVEN\_OPTS and in UI, only the one specified in MAVEN\_OPTS was effective. Starting with TeamCity 6.5 Maven runner forms direct java command. While this approach solves many different problems, it also means that MAVEN\_OPTS isn't effective anymore and all JVM command line parameters should be specified in build runner settings instead of MAVEN\_OPTS.
* Those who had to manually setup surefire XML reporting for Maven release builds in TeamCity 6.0.x because otherwise tests weren't reported, now can forget about that. Since TeamCity 6.5 surefire tests run by release:prepare or release:perform goals are automatically detected. So don't forget to switch surefire XML reporting off in the build configuration settings to avoid double\-reporting!

__Email sending settings__   
Please check email sending settings are working correctly after upgrade (via Test connection on Administration &gt; Server Configuration &gt; EMail Notifier). If no authentication is needed, make sure login and password fields are blank. Non\-blank fields may cause email sending errors if SMTP server is not expecting authentication requests.

__XML Report Processing__   
Tests from Ant JUnit XML reports can be reported twice (see [TW-19058](http://youtrack.jetbrains.net/issue/TW-19058)), as we no longer automatically ignore TESTS\-xxx.xml report.   
To workaround this avoid using \*.xml mask and specify more concrete rules like TEST\-\*.xml or alike that will not match report with name starting with "TESTS\-"

__Open API Changes__   
Several return types have changes in TeamCity open API, so plugins might need recompilation against new TeamCity version to continue working.  
Also, some API was deprecated and will be discontinued in later releases. It is recommended to update plugins not to use deprecated API.   
See also [Open API Changes](https://confluence.jetbrains.com/display/TCD18/Open+API+Changes)

## Changes from 6.0.2 to 6.0.3

No noteworthy changes

## Changes from 6.0.1 to 6.0.2

__Maven and XML Test Reporting Load CPU on Agent__   
If you use Maven or XML test reporter and your build is CPU\-intensive, you might find important the [known issue](http://youtrack.jetbrains.net/issue/TW-15335). Patch is available, fixed in the following updates.

## Changes from 6.0 to 6.0.1

No noteworthy changes

## Changes from 5.1.x to 6.0

__Visual Studio Add\-in and Perforce__   
There is critical bug in TeamCity 6.0 VS Add\-in when Perforce is enabled. This can cause Visual Studio hangs and crashes. The fixed add\-in version is [available](ftp://ftp.intellij.net/pub/.teamcity/TW-14765/vsAddinInstaller.zip). (related [issue](http://youtrack.jetbrains.net/issue/TW-14765)). The issue is fixed in TeamCity 6.0.1.

__TFS checkout on agent__   
TFS checkout on agent might refuse to work with errors. Patch is available, see the [comment](http://youtrack.jetbrains.net/issue/TW-14804#comment=27-185725). Related [issue](http://youtrack.jetbrains.net/issue/TW-14804). The issue is fixed in TeamCity 6.0.1.

__Error Changing Priority class__   
You may encounter a browser error while changing priority number of a priority class. A patch is available in a related [issue](http://youtrack.jetbrains.net/issue/TW-14816). The issue is fixed in TeamCity 6.0.1.

 

__IntelliJ IDEA Compatibility__   
IntelliJ IDEA 6 and 7 are no longer supported in TeamCity plugin for IntelliJ IDEA.

Also, if you plan to upgrade to IntelliJ IDEA X (or other JetBrains IDE) please review this [known issue](known-issues.md).

__Build Failure Notifications__   
TeamCity 6.0 differentiates between build failure occurred while running a build script and one occurred while preparing for the build. The errors occurring in the latter case are called "failed to start" errors and are hidden by default from web UI (see "Show canceled and failed to start builds" option on Build Configuration page).

Since TeamCity 6.0, there is a separate notification rule "The build fails to start" which applies for "failed to start" builds. All the rest build failure notifications relate to build script\-related failures.

Please note that on upgrade, all users who had "The build fails" notification on, will automatically get "The build fails to start" option to preserve old behavior.

__Properties Changes__   
`teamcity.build.workingDir` property should no longer be used in non\-runner settings. For backward compatibility, the property is supported in non\-runner settings and is resolved to the working directory of the first defined build step.

__Swabra and Build Queue Priorities Plugins are Bundled__   
If you have installed the plugins previously, please remove them (typically form `.BuildServer/plugins`) before starting upgraded TeamCity version.

__Maven runner__   
Java older than 1.5 is no longer supported by the agent part of Maven runner. Please make sure you specify 1.6\+ JVM in Maven runner settings or ensure JAVA\_HOME points to such JVM.

__NUnit and MSTest Tests__   
If you had NUnit or MSTest tests configured in TeamCity UI (sln and MSBuild runners), the settings are extracted form the runners and converted to a new runner of corresponding type.

Please note that implementation of tests launching has changed and this affected relative paths usage: in TeamCity 6.0 the working directory and all the UI\-specified wildcards are resolved based on the build's [checkout directory](build-checkout-directory.md), while they used to be based on the directory containing .sln file. Simple settings are converted on TeamCity upgrade, but you might need to verify the runners contain appropriate settings.

__"%" Escaping in the Build Configuration Properties__   
Now, two percentage signs (%%) in values defined in Build Configuration settings are treated as escape for a single percentage sign. Your existing settings are converted on upgrade to preserve functioning like in previous versions. However, you might need to review the settings for unexpected "%" sign\-related issues.

__.Net Framework Properties are Reported as Configuration Parameters__   
In previous TeamCity versions, installed .Net Frameworks, Visual Studios and Mono were reported as System Properties of the build agents.   
This made the properties available in the build script.In order to reduce number of TeamCity\-specific properties pushed into the build scripts, the values are now reported via Configuration Parameters (that is, without "system." prefix) and are not available in the build script by default. They still be used in the Build Configuration settings via %\-references by their previous names, just without "system." prefix.

__Ipr runner is deprecated in favor of IntelliJ IDEA Project runner__   
Runner for IntelliJ IDEA projects was completely rewritten. It is not named "IntelliJ IDEA Project" runner. Previously available Ipr runner is also preserved but is marked as deprecated and will be removed in one of the further major releases of TeamCity. It is highly recommended to migrate your existing build configurations to the new runner.   
Please note that the new runner uses different approach to run tests: you need to have a shared Run Configuration created in IntelliJ IDEA and reference it in the runner settings.

__Cleanup for Inspection and Duplicates data__    
Starting from 6.0 Inspection and Duplicates reports for the builds are cleaned when build is cleaned from history, not when build's artifacts are cleaned as it used to be.

__Inspection and Duplicates runners require Java 1.6__   
"Inspections" and "Duplicates (Java)" runners now require Java JDK 1.6. Please ensure Java 1.6 is installed on relevant agents and check it is specified in the "JDK home path" setting of the runners.

__XML Report Validation__   
If you had invalid settings of "XML Report Processing" section of the build runners, you might find the Build Configurations reporting "Report paths must be specified" messages upon upgrade. In this case, please go to the runner settings and correct the configuration. (related [issue](http://youtrack.jetbrains.net/issue/TW-14821))

__Open API Changes__   
See [Open API Changes](https://confluence.jetbrains.com/display/TCD18/Open+API+Changes) Several jars in `devPackage` were reordered, some moved under `runtime` subdirectory. Please update your plugin projects to accommodate for these changes.

__REST API Changes__   
Several objects got additional attributes and sub\-elements. Please check that your parsing code still works.

__Perforce Clean Checkout__   
All builds using Perforce checkout will do a clean checkout after server upgrade. Please note that this can impose a high load on the server in the first hours after upgrade and server can be unresponsive while many builds are in "transferring sources" stage.

 

## Changes from 5.1.2 to 5.1.3

__Path to executable in Command line runner__   
The [bug](http://youtrack.jetbrains.net/issue/TW-11840) was fully fixed. The behavior is the same as in pre\-5.1 builds.

## Changes from 5.1.1 to 5.1.2

Jabber notification sending errors are displayed in web UI for administrators again (these messages were disabled in 5.1.1). If you do not use Jabber notifications, please pause the Jabber notifier on the Jabber settings server settings page.

## Changes from 5.1 to 5.1.1

__Path to executable in Command line runner__   
The [bug](http://youtrack.jetbrains.net/issue/TW-11840) was partly fixed. The behavior is the same as in pre\-5.1 builds except for the case when you have the working directory specified and have the script in both checkout and working directory. The script from the working directory is used.

__Path to script file in Solution runner and MSBuild runner__   
The [bug](http://youtrack.jetbrains.net/issue/TW-11854) was fixed. The behavior is the same as in pre\-5.1 builds.

## Changes from 5.0.3 to 5.1

<tip>

If you plan to upgrade from version 3.1.x to 5.1, you will need to modify some dtd files in `<TeamCity Data Directory>/config` before upgrade, read more in the issue: [TW-11813](http://youtrack.jetbrains.net/issue/TW-11813#comment=27-148589)
</tip>

<tip>

NCover 3 support may not work. See [TW-11680](http://youtrack.jetbrains.net/issue/TW-11680#comment=27-148573)
</tip>

__Notification templates change__   
Since 5.1, TeamCity uses [new template engine](customizing-notifications.md) (Freemarker) to generate notification messages. New default templates are supplied and customizations to the templates made prior to upgrading are no longer effective.

If you customized notification templates prior to this upgrade, please review the new notification templates and make changes to them if necessary. Old notification templates are copied into `<TeamCity Data Directory>/config/_trash/_notifications` directory. Hope, you will enjoy the new templates and new extended customization capabilities.

__External database drivers location__   
JDBC drivers can now be placed into `<TeamCity Data Directory>/lib/jdbc` directory instead of `WEB-INF/lib`. It is recommended to use the new location. See details at [Setting up an External Database](setting-up-an-external-database.md).

PostgresSQL jdbc driver is no more bundled with TeamCity installation package, you will need to [install](setting-up-an-external-database.md) it yourself upon upgrade.

__Database connection properties__   
Database connection properties template files have changed their names and are placed into `database.<database-type>.properties.dist` files under `<TeamCity Data Directory>/config` directory. They follow [.dist files convention](teamcity-data-directory.md).

It is recommended to review your `database.properties` file by comparing it with the new template file for your database and remove any options that you did not customize specifically.

__Default memory options change__   
We changed the default [memory option](installing-and-configuring-the-teamcity-server.md) for PermGen memory space and if you had `-Xmx` JVM option changed to about 1.3G and are running on 32 bit JVM, the server may fail to start with a message like: `Error occurred during initialization of VM Could not reserve enough space for object heap Could not create the Java virtual machine.` 

On this occasion, please consider either:
* switching to 64 bit JVM. Please consider the [note](installing-and-configuring-the-teamcity-server.md).
* reducing PermGen memory via `-XX:MaxPermSize` [JVM option](configuring-teamcity-server-startup-properties.md) (to previous default 120m)
* reducing heap memory via `-Xmx` [JVM option](configuring-teamcity-server-startup-properties.md)

__Vault Plugin is bundled__   
In this version we bundled [SourceGear Vault VCS plugin](https://confluence.jetbrains.com/display/TW/Vault) (with experimental status). Please make sure to uninstall the plugin from .BuildServer/plugins (just delete plugin's zip) if you installed it previously.

__Path to executable in Command line runner__ A [bug](http://youtrack.jetbrains.net/issue/TW-11840) was introduced that requires changing the path to executable if working directory is specified in the runner.The bug is partly fixed in 5.1.1 and fully fixed in 5.1.3.

__Path to script file in Solution runner and MSBuild runner__   
A [bug](http://youtrack.jetbrains.net/issue/TW-11854) was introduced that requires changing the path to script if working directory is specified in the runner. The bug is fixed in 5.1.1.

__Open API Changes__   
See [Open API Changes](https://confluence.jetbrains.com/display/TCD18/Open+API+Changes)

## Changes from 5.0.2 to 5.0.3

No noteworthy changes.

<tip>

There is a known issue with .NET duplicates finder: [TW-11320](http://youtrack.jetbrains.net/issue/TW-11320)   
Please use the patch attached to the issue.
</tip>

## Changes from 5.0.1 to 5.0.2

__External change viewers__   
The `relativePath` variable is now replaced with relative path of a file _without_ checkout rules. The previous value can be accessed via `relativeAgentPath`. More information at [TW-10801](http://jetbrains.net/tracker/issue/TW-10801).

## Changes from 5.0 to 5.0.1

No noteworthy changes.

## Changes from 4.5.6 to 5.0

__Pre\-5.0 Enterprise Server Licenses and Agent Licenses need upgrade__   
With the version 5.0, we announce changes to the upgrade policy: Upgrade to 5.0 is not free. Every license (server and agent) bought since 5.0 will work with any TeamCity version released within one year since the license purchase. Please review the detailed information at [Licensing and Upgrade](http://www.jetbrains.com/teamcity/buy/index.jsp) section of the official site.

__Bundled plugins__   
If you used standalone plugins that are now bundled in 5.0, do not forget to remove the plugins from `.BuildServer/plugins` directory.The newly bundled plugins are:
* Mercurial
* Git (JetBrains)
* REST API (was provided with YouTrack previously)

__Other plugins__   
If you use any plugins that are not bundled with TeamCity, please make sure you are using the latest version and it is compatible with the 5.0 release. e.g. You will need the latest version of [Groovy plug](https://confluence.jetbrains.com/display/TW/Groovy+plug) and other properties\-providing extensions.Pre\-5.0 notifier plugins may lack support for per\-test and assignment responsibility notifications.

__Obsolete Properties__   
The system property "build.number.format" and environment variable "BUILD\_NUMBER\_FORMAT" are removed. If you need to use build number format in your build (let us know why), you can define build number format as `%system.<property name>%` and define &lt;property name&gt; system property in the build configuration (it will be passed to the build then).

__Oracle database__   
If you use TeamCity with Oracle database, you should add an addition privilege to the TeamCity Oracle user. In order to do it, log in to Oracle as user SYS and perform

```Shell

grant execute on dbms_lock to <TeamCity_User>;

```



__PostgreSQL database__   
TeamCity 5.0 supports PostrgeSQL version 8.3\+.   
So if the version of your PostgreSQL server is less than 8.3 then it needs to be upgraded.

__Open API Changes__ See [Open API Changes](https://confluence.jetbrains.com/display/TCD18/Open+API+Changes)

## Changes from 4.5.2 to 4.5.6

No noteworthy changes.

## Changes from 4.5.1 to 4.5.2

Here is a critical issue with Rake runner in 4.5.2 release. Please see [TW-8485](http://jetbrains.net/tracker/issue2/TW-8485) for details and a fixing patch.

## Changes from 4.5.0 to 4.5.1

No noteworthy changes.

## Changes from 4.0.2 to 4.5

__Default User Roles__   
The roles assigned as default for new users will be moved to "All Users" groups and will be effectively granted to all users already registered in TeamCity.

__Running builds during server restart__   
Please ensure there are no running builds during server upgrade.If there are builds that run during server restart and these builds have test, the builds will be canceled and re\-added to build queue ([TW-7476](http://jetbrains.net/tracker/issue/TW-7476)).

__LDAP settings rename__   
If you had LDAP integration configured, several settings will be automatically converted on first start of the new server. The renamed settings are:
* `formatDN` — is renamed to `teamcity.auth.formatDN`
* `loginFilter` — is renamed to `teamcity.auth.loginFilter`
## Changes from 4.0.1 to 4.0.2

__Increased first cleanup time__   
The first server cleanup after the update can take significantly more time. Further cleanups should return to usual times. During this first cleanup the data associated with deleted build configuration is cleaned. It was not cleaned earlier because of a bug in TeamCity versions 4.0 and 4.0.1.

## Changes from 4.0 to 4.0.1

__"importData" service message arguments id__ argument renamed to __type__ and __file__ to __path__. This change is backward\-compatible. See [Using Service Messages](fxcop.md) section for examples of new syntax.

## Changes from 3.1.2 to 4.0

__Initial startup time__   
On the very first start of the new version of TeamCity, the database structure will be upgraded. This process can increase the time of the server startup. The first startup can take up to 20 minutes more then regular one. This time depends on the size of your builds history, average number of tests in a build and the server hardware.

__Users re\-login will be forced after upgrade__   
Upon upgrade, all users will be automatically logged off and will need to re\-login in their browsers to TeamCity web UI. After the first login since upgrade, __Remember me__ functionality will work as usual.

__Previous IntelliJ IDEA versions support__   
IntelliJ IDEA plugin in this release is no longer compatible with IntelliJ IDEA 6.x versions. Supported IDEA versions are 7.0.3 and 8.0.

__Using VCS revisions in the build__   
`build.vcs.number.N` system properties are replaced with `build.vcs.number.<escaped VCS root name>` properties (or just `build.vcs.number` if there is only one root). If you used the properties in the build script you should update the usages manually or switch compatibility mode on. References to the properties in the build configuration settings are updated automatically. Corresponding environment variable has been affected too. [Read more](configuring-build-parameters.md).

__Test suite__   
Due to the fact that TeamCity started to handle tests suites, the tests with suite name defined will be treated as new tests (thus, test history can start from scratch for these tests.)

__Artifact dependency pattern__   
Artifact dependencies patterns now support [Ant-like wildcards](https://confluence.jetbrains.com/display/TCD4/Wildcards).   
If you relied on `""` pattern to match directory names, please adjust your pattern to use `"/"` instead of single `"*"`.   
If you relied on the `""` pattern to download only the files without extension, please update your pattern to use `"."` for that.

__Downloading of artifacts with help of Ivy__   
If you downloaded artifacts from the build scripts (like Ant build.xml) with help of Ivy tasks you should modify your ivyconf.xml file and remove all statuses from there except "integration". You can take the ivyconf.xml file from the following page as reference: [http://www.jetbrains.net/confluence/display/TCD4/Configuring+Dependencies](http://www.jetbrains.net/confluence/display/TCD4/Configuring+Dependencies)

__Browser caches (IE)__   
To force Internet Explorer to use updated icons (i.e. for the Run button) you may need to force page reload (Ctrl\+Shift\+R) or delete "Temporary Internet Files".

## Changes from 3.1.1 to 3.1.2

No noteworthy changes.

## Changes from 3.1 to 3.1.1

No noteworthy changes.

## Changes from 3.0.1 to 3.1

__Guest User and Agent Details__

Starting from version 3.1, the Guest user does not have access to the agent details page. This has been done to reduce exposing potentially sensitive information regarding the agents' environment. In the Enterprise Edition, the Guest user's roles can be edited at the __Users and Groups__ page to provide the needed level of permission.

__StarTeam Support__

__Working Folders in Use__

Since version 3.1 when checking out files from a StarTeam repository TeamCity builds directory structure on the base of the working folder names, not just folder names as it was in earlier versions. So if you are satisfied with the way TeamCity worked with StarTeam folders in version 3.0, ensure the working folders' names are equal to the corresponding folder names (which is so by default).

Also note, that although StarTeam allows using absolute paths as working folders, TeamCity supports relative paths only and doesn't detect absolute paths presence. So be careful and review your configuration.

__StarTeam URL Parser Fixed__

In version 3.0 a user must have followed a wrong URL scheme. It was like _starteam://server:49201/project/view/rootFolder/subfolder/..._ and didn't work when user tried to refer a non\-default view. In version 3.1 the native StarTeam URL parser is utilized. This means you now don't have to specify the root folder in the URL, and the previous example should look like _starteam://server:49201/project/view/subfolder/..._

## Changes from 3.0 to 3.0.1

__Linux Agent Upgrade__
* Due to an issue with Agent upgrade under Linux, Agent auxiliary Launcher processes may have been left running after agent upgrades. Versions 3.0.1 and up fix the issue. To get rid of the stale running processes, after automatic agent upgrade, please stop the agent (via `agent.sh kill` command) and kill any running `java jetbrains.buildServer.agent.Launcher` processes and start the agent again.
## Changes from 2.x to 3.0

__Incompatible changes__

Please note that TeamCity 3.0 introduces several changes incompatible with TeamCity 2.x:
* __build.working.dir__ system property is renamed to __teamcity.build.checkoutDir__. If you use the property in you build scripts, please update the scripts.
* `runAll.bat` script now accepts a required parameter: __start__ to start server and agent, __stop__ to stop server and agent.
* Under Windows, `agent.bat` script now accepts a required parameter: __start__ to start agent, __stop__ to stop agent. Note that in this case agent will be stopped only after it becomes idle (no builds are run). To force immediate agent stopping, use `agent.bat stop force` command that is available under both Windows and Linux (`agent.sh stop force`). Under Linux you can also use `agent.sh stop kill` command to stop agents not responding to `agent.sh stop force`.

__Build working directory__

Since TeamCity 3.0 introduces ability to configure VCS roots on per\-Build Configuration basis, rather then per\-Project, the default directory in which build configuration sources are checked out on agent now has generated name. If you need to know the directory used by a build configuration, you can refer to `<agent home>/work/directory.map` file which lists build configurations with the directory used by them. See also [Build Checkout Directory](build-checkout-directory.md)

__User Roles when upgrading from TeamCity 1.x/2.x/3.x Professional to 3.x Enterprise__

When upgrading from TeamCity 1.x/2.x/3.x Professional to 3.x Enterprise for the first time TeamCity's accounts will be assigned the following [roles](role-and-permission.md) by default:
* _Administrators_ become System Administrators
* _Users_ become Project Developers for all of the projects
* The _Guest_ account is able to view all of the projects
* _Default user_ roles are set to Project Developer for all of the projects

## Changes from 1.x to 2.0

__Database Settings Move__   
Move your database settings from the `<TeamCity installation folder>/ROOT/WEB-INF/buildServerSpring.xml` file to the `database.properties` file located in the TeamCity configuration data directory (`<TeamCity Data Directory>/config`).

  

.NET inspection and duplicates
  inherited

 

<tip>

If you were using the TeamCity\-GitHub [third-party plugin](https://github.com/milgner/TeamCityGithub) prior to TeamCity 10.0, you can safely [remove](installing-additional-plugins.md) it: the built\-in TeamCity integration will detect the existing connection to GitHub issue tracker and pick up your settings automatically.
</tip>

 
`teamcity.TruncateIgnoreReasonConverter.copyReasons`:
   
```Shell

 Error
   collecting changes 
  for
   VCS repository 
  ...
  

  Failed
   to collect TFS changes 
  -
   
  From
   version x 
  is
   greater 
  then
   current version y 
```
  
  
The [bug with temp tool folders](https://youtrack.jetbrains.com/issue/TW-46648) introduced in the previous version has been fixed.





__  __

__See also:__

 

__Administrator's Guide__: [Licensing Policy](licensing-policy.md)
