[//]: # (title: TeamCity 2021.1.4 Release Notes)
[//]: # (auxiliary-id: TeamCity 2021.1.4 Release Notes)

__Build: 92954__  
__08 October 2021__
{instance="tc"}

### Feature

[**TW-72955**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72955) — Allow running Python step on any version (AnyPython agent parameter support)  
[**TW-47419**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-47419) — ECDSA and ED25519 SSH keys support

### Bug

[**TW-73082**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73082) — Finish build triggering may be unexpected with reverse.dep.\* parameters added for all dependencies  
[**TW-73435**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73435) — IllegalArgumentException: duplicate key in BuildStatisticsMetrics.loadMetricIds  
[**TW-73407**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73407) — SSH upload + auth method &quot;SSH agent&quot; do not work with OpenSSH 8.8  
[**TW-73350**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73350) — Teamcity upgrade to 2021.1 failed.  
[**TW-73339**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73339) — TeamCity 2021.1.x cannot connect to OpenSSH 8.8 server (Git plugin)  
[**TW-73295**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73295) — Too many &#39;gitHubPasswordAuthUsageRemove&#39; multi node events  
[**TW-73293**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73293) — VMware plugin can provision vms in endless loop ignoring all limits  
[**TW-72964**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72964) — agent fails to detect docker-compose installation - Failed to parse version: Docker Compose version v2.0.0-rc.2  
[**TW-73039**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73039) — IllegalStateException error can happen in case when build attempts to publish two files under the same paths  
[**TW-73021**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73021) — Build queue pausing/resuming may not be saved on the multi-node setup with different OSs  
[**TW-73003**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73003) — Perforce checkout on agent always runs `p4 trust`, even for non-SSL connections  
[**TW-60688**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-60688) — A system problem reported by Commit Status Publisher is not cleared after the build feature is either deleted or disabled in the configuration  
[**TW-72812**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72812) — TeamCity agent does not detect .NET some configuration properties like MSBuildTools2.0\_x86\_Path under Windows XP  
[**TW-70290**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-70290) — Select test with several invocations selects only first invocation  
[**TW-72631**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72631) — Agents screen: redirect loop after the network connection was lost  
[**TW-53615**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-53615) — TeamCity can't import SSH private keys saved in the new (non-PEM) OpenSSH format

### Exception

[**TW-73371**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73371) — Lets encrypt certificate always error! is urgent  
[**TW-72947**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-72947) — ClassNotFoundException in teamcity-server.log  
[**TW-36123**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-36123) — Code Coverage fails with EOFException

### Performance Problem

[**TW-73208**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73208) — Lots of memory used by VcsConnectionStatusHolder instances if reported VCS error is large and VCS root affects many configurations  
[**TW-73004**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73004) — Repeating usage of getCheckoutPropertiesHash in LinearRevisionCalculator

### Task

[**TW-73054**](https://youtrack.jetbrains.com/oauth?state=%2Fissue%2FTW-73054) — Update git and perforce in agents` Linux images

### Security Problem

1 security problem has been fixed.
