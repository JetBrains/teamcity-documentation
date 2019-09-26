[//]: # (title: Visual SourceSafe)
[//]: # (auxiliary-id: Visual SourceSafe)
<note>

__Since TeamCity 2018.1__, the Visual SourceSafe plugin is no longer bundled with TeamCity and is available as a [separate download](https://plugins.jetbrains.com/plugin/10902-vcs-support-vss).
</note>

### Notes and Limitations
* TeamCity supports Microsoft Visual SourceSafe 6.0 and 2005 (_English versions_ only).
* Microsoft Visual SourceSafe only works if the TeamCity server is installed on a computer running a Windows® operating system.
* Make sure the TeamCity server process is run by a user that has permission to access the VSS databases.

TeamCity has the following limitations with Visual SourceSafe:
* Shared (not branched) files cannot be checked out.
* Comments for add and delete operations for file and directories are not shown in VSS 6.0. All such operations will have "No Comment" instead of a real VSS comment. (This limitation is imposed by the VSS API, which makes it impossible to retrieve comments with acceptable performance).
* The timestamps on VSS check in are driven by local time on client computer. Therefore if the time on clients and build server are not synchronized, TeamCity cannot properly order check\-ins. There is also problem with timezone, however it was already addressed in VSS client version 2005, when configured properly. To avoid these problems follow the recommendations:
  * Sync client machines via timeserver: [http://support.microsoft.com/kb/131715/EN-US](http://support.microsoft.com/kb/131715/EN-US).
  * Set timezone for VSS database: Start VSS admin &gt; Tools &gt; Options &gt; TimeZone and pick one.
  * Use VSS 2005.

__ __