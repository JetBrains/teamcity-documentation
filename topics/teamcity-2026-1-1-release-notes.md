[//]: # (title: TeamCity 2026.1.1 Release Notes)
[//]: # (help-id: TeamCity 2026.1.1 Release Notes)

**Build 222577, 29 May 2026**

### Bug


* [**TW-100620**](https://youtrack.jetbrains.com/issue/TW-100620) — "Force virtual host addressing" checkbox doesn't work
* [**TW-100946**](https://youtrack.jetbrains.com/issue/TW-100946) — .NET builds with qualifier agent requirements starting with "Exists" cannot find compatible agents if there are incompatible agents in the same pool
* [**TW-100761**](https://youtrack.jetbrains.com/issue/TW-100761) — Perforce failing on newlines in changelist description
* [**TW-78597**](https://youtrack.jetbrains.com/issue/TW-78597) — Agent alternate IP address ignored by TeamCity
* [**TW-99613**](https://youtrack.jetbrains.com/issue/TW-99613) — Working better with non-default branches using MCP
* [**TW-100760**](https://youtrack.jetbrains.com/issue/TW-100760) — Rake build steps are broken (plugin is damaged)
* [**TW-100744**](https://youtrack.jetbrains.com/issue/TW-100744) — Authorize virtual agents action should validate agent pool for cloud agents
* [**TW-100212**](https://youtrack.jetbrains.com/issue/TW-100212) — S3 Uploads fail with 403 status code
* [**TW-100985**](https://youtrack.jetbrains.com/issue/TW-100985) — Build-scoped tokens UX: add descriptions and docs lin
* [**TW-100573**](https://youtrack.jetbrains.com/issue/TW-100573) — Properties for key preserving on rotation (time.min and time.days) are not working 
* [**TW-100962**](https://youtrack.jetbrains.com/issue/TW-100962) — TeamCity cleanup does not remove cancelled multi-node tasks
* [**TW-97236**](https://youtrack.jetbrains.com/issue/TW-97236) — Creation flow: handle Unauthorized exception in the UI
* [**TW-96205**](https://youtrack.jetbrains.com/issue/TW-96205) — AWS SDK2: remove "(SDK Attempt Count: 1)" from the errors, return back the error code and service name
* [**TW-100707**](https://youtrack.jetbrains.com/issue/TW-100707) — CONNECT: 401 on access to K8s via proxy with login and password
* [**TW-87045**](https://youtrack.jetbrains.com/issue/TW-87045) — No reaction after activation of the security patch (it is unclear whether the installation has actually started)
* [**TW-100713**](https://youtrack.jetbrains.com/issue/TW-100713) —  Build cannot resolve dep. parameters, if it has an optional artifact dependency on another skipped conditional dependency
* [**TW-92752**](https://youtrack.jetbrains.com/issue/TW-92752) — “Store password and API tokens outside of VCS” gets hidden when you disable UI-managed project settings.
* [**TW-99122**](https://youtrack.jetbrains.com/issue/TW-99122) — Perforce shelve trigger starts a build on excluded stream if feature branches support is disabled
* [**TW-100650**](https://youtrack.jetbrains.com/issue/TW-100650) — VCS trigger doesn't trigger build on empty branch
* [**TW-98678**](https://youtrack.jetbrains.com/issue/TW-98678) — Failed DSL compilation due to invalid cache after moving a Perforce label 
* [**TW-100234**](https://youtrack.jetbrains.com/issue/TW-100234) — Failed to resolve artifact dependency in multinode setup with external artifact storage
* [**TW-99808**](https://youtrack.jetbrains.com/issue/TW-99808) — Build features UI: placeholder in the search field refers to non-existing items
* [**TW-100476**](https://youtrack.jetbrains.com/issue/TW-100476) — All compatible agents are outdated and cannot be upgraded” is shown even though some agents are currently undergoing the upgrade process
* [**TW-95156**](https://youtrack.jetbrains.com/issue/TW-95156) — Deadlock on attempt to save data to test_metadata table
* [**TW-100377**](https://youtrack.jetbrains.com/issue/TW-100377) — Excessive `/user` calls during repository listing in BitBucket Cloud connection


### Pipeline Enhancements

* [**TW-94668**](https://youtrack.jetbrains.com/issue/TW-94668) — Pipeline is broken after project copying
* [**TW-99898**](https://youtrack.jetbrains.com/issue/TW-99898) — Pipeline steps description is not aligned with steps if Dockerfile is used in some of these steps
* [**TW-100063**](https://youtrack.jetbrains.com/issue/TW-100063) — `/app/pipeline` MCP requests do not work when user has active 2FA
* [**TW-99861**](https://youtrack.jetbrains.com/issue/TW-99861) — Limit Job ID and name lengths
* [**TW-95617**](https://youtrack.jetbrains.com/issue/TW-95617) — Pipeline YAML parse error should contain location in the YAML
* [**TW-97441**](https://youtrack.jetbrains.com/issue/TW-97441) — RAM Amount condition for pipeline agent requirements doesn't work

### Performance Problem

* [**TW-100970**](https://youtrack.jetbrains.com/issue/TW-100970) — Too big thread name with lots of "commit" strings
* [**TW-97583**](https://youtrack.jetbrains.com/issue/TW-97583) — Duplicated IDs in the where clause of the SQL query can cause the PostgreSQL planner to switch to a full table scan
* [**TW-100629**](https://youtrack.jetbrains.com/issue/TW-100629) — Saving settings on Windows can be slow
* [**TW-100864**](https://youtrack.jetbrains.com/issue/TW-100864) — Repeating changes collection operations holds a reference to a previous operation via this$0
* [**TW-98141**](https://youtrack.jetbrains.com/issue/TW-98141) — PageExtensionsInterceptor is called for non-WEB POST requests
* [**TW-100626**](https://youtrack.jetbrains.com/issue/TW-100626) — Slow loading of VCS leads to UI slowness


### Security

Two security problems have been fixed.
To learn more about fixed vulnerabilities directly related to TeamCity, check out our [Security Bulletin](https://www.jetbrains.com/privacy-security/issues-fixed/?product=TeamCity&version=2026.1.1).

Security bulletins are typically published few days after the release date.


