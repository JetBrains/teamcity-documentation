[//]: # (title: Change State)
[//]: # (auxiliary-id: Change State)

This article lists visual indicators that appear depending on a detected build change, or commit.

<table><tr>

<td>

Icon

</td>

<td>

State

</td>

<td>

Description

</td></tr><tr>

<td>

![pending.gif](pending.gif)

</td>

<td>

pending

</td>

<td>

The change is scheduled to be integrated into a build that is currently in the build queue.

Even if a change has been successfully integrated into a build, the change will appear as pending when it is scheduled to be added to a different build.

</td></tr><tr>

<td>

![running_green_transparent.png](running_green_transparent.png)

</td>

<td>

running successfully

</td>

<td>

The change is being integrated into a build that is running successfully.

</td></tr><tr>

<td>

![success_small.gif](success_small.png)

</td>

<td>

successful

</td>

<td>

The change was integrated into a build that finished successfully.

</td></tr><tr>

<td>

![running_red_transparent.gif](running_red_transparent.png)

</td>

<td>

running and failing

</td>

<td>

The change is being integrated into a build that is failing.

</td></tr><tr>

<td>

![error_small.gif](error_small.gif)

</td>

<td>

failed

</td>

<td>

The change was integrated into a build that failed.

</td></tr></table>

## Personal Change States

<table><tr>

<td>

Icon

</td>

<td>

State

</td>

<td>

Description

</td></tr><tr>

<td>

![personalPending.gif](personalPending.gif)

</td>

<td>

pending

</td>

<td>

The change is scheduled to be integrated into a personal build that is currently in the build queue.

</td></tr><tr>

<td>

![personalRunning_green.gif](personalRunning_green.png)

</td>

<td>

running successfully

</td>

<td>

The change is being integrated into a personal build that is running successfully.

</td></tr><tr>

<td>

![personalSuccess.gif](personalSuccess.gif)

</td>

<td>

successful

</td>

<td>

The change was integrated into a personal build that finished successfully.

</td></tr><tr>

<td>

![personalRunning_red.gif](personalRunning_red.png)

</td>

<td>

running and failing

</td>

<td>

The change is being integrated into a personal build that is failing.

</td></tr><tr>

<td>

![personalCrashed.gif](personalCrashed.gif)

</td>

<td>

failed

</td>

<td>

The change was integrated into a personal build that failed.

</td></tr></table>

 <seealso>
        <category ref="concepts">
            <a href="managing-builds.md">Build Configuration Status</a>
            <a href="build-state.md">Build State</a>
            <a href="change.md">Change</a>
        </category>
        <category ref="user-guide">
            <a href="viewing-user-changes-in-builds.md">Viewing Your Changes</a>
        </category>
</seealso>