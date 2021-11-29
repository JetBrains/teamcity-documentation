[//]: # (title: TeamCity Disk Space Watcher)
[//]: # (auxiliary-id: TeamCity Disk Space Watcher)

The TeamCity server regularly checks for free disk space on the server machine (in the TeamCity Data Directory) and displays a warning on all the pages of the web UI is the free disk space falls below a certain threshold. If the space continues to decrease and reaches a certain limit, the build queue is paused.

The thresholds can be changed by modifying the following [internal properties](server-startup-properties.md) values in __KB__:

<table><tr>

<td>

Property


</td>

<td>

Default


</td>

<td>

Description


</td></tr><tr>

<td>

`teamcity.diskSpaceWatcher.threshold`


</td>

<td>

512000 (500 MB)


</td>

<td>

displays a warning


</td></tr><tr>

<td>

`teamcity.pauseBuildQueue.diskSpace.threshold`


</td>

<td>

51200 (50 MB)


</td>

<td>

pauses the build queue


</td></tr></table>

You can use [Disk Usage](disk-usage.md) report to evaluate what projects use the most of the disk space and adjust the [clean-up](teamcity-data-clean-up.md) rules if required. 


<seealso>
        <category ref="admin-guide">
            <a href="free-disk-space.md">Free disk space on agent</a>
            <a href="disk-usage.md">[Disk Usage</a>
            <a href="teamcity-data-clean-up.md">Clean-up</a>
        </category>
</seealso>
