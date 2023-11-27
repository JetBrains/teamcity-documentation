[//]: # (title: TeamCity 2020.1.5 Release Notes)
[//]: # (auxiliary-id: TeamCity 2020.1.5 Release Notes)


__Build: 78938__

### Bug

* [TW-67984](https://youtrack.jetbrains.com/issue/TW-67984) — Corrupted custom data storage produces endless stream of warnings like "storage is corrupted and will be re-created" in the teamcity-server log
* [TW-67648](https://youtrack.jetbrains.com/issue/TW-67648) — TeamCity failed to terminate an AWS instance
* [TW-67692](https://youtrack.jetbrains.com/issue/TW-67692) — Node trigger responsibilities may get stale if node has been unexpectedly terminated
* [TW-67751](https://youtrack.jetbrains.com/issue/TW-67751) — \[IDEA Runner] jetbrains/buildServer/agent/ideaRunner/ExternalMakeMessageHandlerAntTaskExtension : Unsupported major.minor version 52.0
* [TW-67799](https://youtrack.jetbrains.com/issue/TW-67799) — Copy-paste typo in TeamCity REST API reference
* [TW-67856](https://youtrack.jetbrains.com/issue/TW-67856) — K8SCloudParamsXmlConverter should support custom profile name
* [TW-67889](https://youtrack.jetbrains.com/issue/TW-67889) — 'Nodes time difference' health report shows wrong time diff between nodes

### Performance Problem

* [TW-34572](https://youtrack.jetbrains.com/issue/TW-34572) — Opening Custom Build Dialog hang when the build configuration depends on many other build configurations (fetching builds tags is slow)
* [TW-67481](https://youtrack.jetbrains.com/issue/TW-67481) — Slow processing of VCS trigger with triggering rules if branch disappears for some reason and then appears again
* [TW-67881](https://youtrack.jetbrains.com/issue/TW-67881) — RemoteStatusManager.getFilteredGZippedSummaryByVcsUris can be slow because of inefficient PersonalSupportBatchServiceImpl.checkForParents method



### Task

* [TW-67644](https://youtrack.jetbrains.com/issue/TW-67644) — Add an ability to explicitly specify SameSite=None attribute for session cookies




### Security Problem

2 security-related issues have been fixed.