[//]: # (title: TeamCity 2018.2.4 Release Notes)
[//]: # (auxiliary-id: TeamCity 2018.2.4 Release Notes)


__Build: 61678__

### Feature

* [TW-50731](https://youtrack.jetbrains.com/issue/TW-50731) — Check free disk space before running auto-upgrade
* [TW-59845](https://youtrack.jetbrains.com/issue/TW-59845) — Update the integration package in dotnet cli plugin to 1.0.7

### Usability Problem

* [TW-23286](https://youtrack.jetbrains.com/issue/TW-23286) — Test names should not be shortened if there is enough browser window size

### Bug

* [TW-52863](https://youtrack.jetbrains.com/issue/TW-52863) — Swabra throws StackOverflowError and ignores it
* [TW-57420](https://youtrack.jetbrains.com/issue/TW-57420) — Enforced settings. Overridden build steps should be marked in UI.
* [TW-57422](https://youtrack.jetbrains.com/issue/TW-57422) — Incorrect build step status when the same template is used as default and enforced.
* [TW-57793](https://youtrack.jetbrains.com/issue/TW-57793) — Uncaught TypeError: Cannot read property 'computeOrder' of undefined on trying to reorder Charts in Experimental UI.
* [TW-57794](https://youtrack.jetbrains.com/issue/TW-57794) — Duplicated Add New Chart buttons in Experimental UI.
* [TW-57823](https://youtrack.jetbrains.com/issue/TW-57823) — Error While Applying Patch starting build
* [TW-57918](https://youtrack.jetbrains.com/issue/TW-57918) — Build log issues while "Resolving artifact dependency"
* [TW-59273](https://youtrack.jetbrains.com/issue/TW-59273) — Corrupted builds when collecting changes is performed on VCS node
* [TW-59477](https://youtrack.jetbrains.com/issue/TW-59477) — Docker images cleanup doesn't work for docker hub
* [TW-59509](https://youtrack.jetbrains.com/issue/TW-59509) — Umlauts in Project Statistics are not encoded properly
* [TW-59527](https://youtrack.jetbrains.com/issue/TW-59527) — Corrupted "Configure Build Agent Properties" Java window layout during agent installation under Windows 10 (bad bundled Java)
* [TW-59529](https://youtrack.jetbrains.com/issue/TW-59529) — Running builds aren't shown on the build configuration page if there are no finished builds displayed on the page
* [TW-59555](https://youtrack.jetbrains.com/issue/TW-59555) — Nodes information page produces errors with than one running builds node
* [TW-59563](https://youtrack.jetbrains.com/issue/TW-59563) — Branch filter is reset on the Statistics page and all branches are displayed when Range is changed on Project/Build configuration Statistics page.
* [TW-59566](https://youtrack.jetbrains.com/issue/TW-59566) — On Changes page, personal builds with empty commit message do not show "personal build" icon
* [TW-59570](https://youtrack.jetbrains.com/issue/TW-59570) — Permissions are not checked in BuildCustomizer when a new build promotion is created
* [TW-59600](https://youtrack.jetbrains.com/issue/TW-59600) — Incorrect warning is shown to users of IE 11
* [TW-59615](https://youtrack.jetbrains.com/issue/TW-59615) — The calculated statistic value 'serverSideBuildFinishing' is less than 0
* [TW-59644](https://youtrack.jetbrains.com/issue/TW-59644) — dotnet build step hangs
* [TW-59646](https://youtrack.jetbrains.com/issue/TW-59646) — Branches tab: VCS problem/system problem is not displayed for non-default branch when "&lt;All branches>" is selected
* [TW-59648](https://youtrack.jetbrains.com/issue/TW-59648) — Internal REST request to embed build data into page can fail calling method isCommitted: UnsupportedOperationException: Not implemented in jetbrains.buildServer.controllers.fakes.FakeHttpServletResponse
* [TW-59661](https://youtrack.jetbrains.com/issue/TW-59661) — TFS Braches: partial merge commits aren't shown on the graph (with checkout rules)
* [TW-59662](https://youtrack.jetbrains.com/issue/TW-59662) — Build can be redirected to the main node from the running builds node without due reason
* [TW-59666](https://youtrack.jetbrains.com/issue/TW-59666) — TFS plugin does not use proxy settings if nonProxyHosts contains wildcards
* [TW-59676](https://youtrack.jetbrains.com/issue/TW-59676) — "Cannot find a node" error on many UI requests (VCS node is in use)
* [TW-59698](https://youtrack.jetbrains.com/issue/TW-59698) — Error regarding "teamcity.build.branch" parameter when running custom builds
* [TW-59703](https://youtrack.jetbrains.com/issue/TW-59703) — java.lang.IllegalArgumentException: Cannot find a node &lt;num> can occur on attempt to create a history build with a non DAG based VCS root (setup with VCS node)
* [TW-59716](https://youtrack.jetbrains.com/issue/TW-59716) — Deadlock on finished build eviction from builds cache
* [TW-59720](https://youtrack.jetbrains.com/issue/TW-59720) — Git LFS checkout fails on agent with error "StrictHostKeyChecking=no not accessible: No such file or directory"
* [TW-59741](https://youtrack.jetbrains.com/issue/TW-59741) — Nunit-Console 3.10 tool is not supported.
* [TW-59751](https://youtrack.jetbrains.com/issue/TW-59751) — Project expand is not working in Edge
* [TW-59793](https://youtrack.jetbrains.com/issue/TW-59793) — TeamCity NuGet credential provider causing FileNotFoundException when executing dotnet publish step
* [TW-59805](https://youtrack.jetbrains.com/issue/TW-59805) — Race condition in git-plugin JSchClient
* [TW-59813](https://youtrack.jetbrains.com/issue/TW-59813) — EC2 instance can be left on unauthorized page in case of spot instance request and short idle timeout
* [TW-59815](https://youtrack.jetbrains.com/issue/TW-59815) — Prefer agent start timeout to idle timeout
* [TW-59820](https://youtrack.jetbrains.com/issue/TW-59820) — Deadlock on build finish if build failure condition has option to stop build on failed check enabled
* [TW-59871](https://youtrack.jetbrains.com/issue/TW-59871) — NCover Code Coverage fails with error \`The system cannot find the path specified [java.io](http://java.io).IOException: The system cannot find the path specified\`

### Performance Problem

* [TW-59202](https://youtrack.jetbrains.com/issue/TW-59202) — Unauthorized agents page is slow if there are many such agents
* [TW-59590](https://youtrack.jetbrains.com/issue/TW-59590) — 'Build messages queue is full' error caused by SSH key 'View usages' UI action

### Security Problem

3 security problems have been fixed.

### Cosmetics

* [TW-55106](https://youtrack.jetbrains.com/issue/TW-55106) — Don't show filters if there are no builds in history
