[//]: # (title: Build State)
[//]: # (auxiliary-id: Build State)

The build state icon appears next to each build under the expanded view of the build configuration on the __Projects__ page.

## Build States

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

![running_green_transparent.gif](running_green_transparent.gif)


</td>

<td>

running successfully


</td>

<td>

A build is running successfully.


</td></tr><tr>

<td>

![buildSuccessful.png](buildSuccessful.png)


</td>

<td>

successful


</td>

<td>

A build finished successfully in all specified build configurations.


</td></tr><tr>

<td>

![running_red_transparent.gif](running_red_transparent.gif)


</td>

<td>

running and failing


</td>

<td>

A build is failing.


</td></tr><tr>

<td>

![buildFailed.png](buildFailed.png)


</td>

<td>

failed


</td>

<td>

A build failed at least in one specified build configuration.


</td></tr><tr>

<td>

![cancelled.png](cancelled.png)


</td>

<td>

cancelled


</td>

<td>

A build was cancelled.


</td></tr></table>

### Canceled/Stopped build

Stopping a running build results in the build status displayed as cancelled. You can stop a running build from the [build results page](working-with-build-results.md), [build configuration home page](viewing-build-configuration-details.md) or using the __Stop__ option from the __Actions__ drop\-down.

When a build is started, the build process calls the runner process and listens to its output. The stop command kills the runner process, then the build process stops.

<note>

It is possible to configure your build so that it will continue executing build steps after the build was stopped. To do it, can add a build step with the __Always, even if build stop command was issued__ option selected. See [Configuring Build Steps](configuring-build-steps.md).
</note>

## Personal Build States

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

![personalRunning.gif](personalRunning.gif)


</td>

<td>

running successfully


</td>

<td>

A personal build is running successfully.


</td></tr><tr>

<td>

![personalFinished.png](personalFinished.png)


</td>

<td>

successful


</td>

<td>

A personal build has completed successfully for all specified build configurations.


</td></tr><tr>

<td>

![personalRunningFailing.gif](personalRunningFailing.gif)


</td>

<td>

running and failing


</td>

<td>

A personal build is running with errors.


</td></tr><tr>

<td>

![personalFinishedFailed.png](personalFinishedFailed.png)


</td>

<td>

failed


</td>

<td>

A personal build failed at least in one specified build configuration.


</td></tr></table>

## Hanging and Outdated Builds

TeamCity considers a build as _hanging_ when its run time significantly exceeds estimated average run time and the build did not send any messages since the estimation exceeded.

A running build can be marked as _Outdated_ if there is a build which contains more changes but it is already finished.Hanging and outdated builds appear with the icon ![attentionComment.png](attentionComment.png). Move the cursor over the icon to view a tooltip that displays additional information about the warning.

## Failed to Start Builds

Builds which failed to start, i.e. did not get to the point of launching the first build step are marked with the  ![redSign.png](redSign.png) icon. It may be caused by a VCS repository being down when the build starts, or the inability to resolve artifact dependencies and so on. Such build status is often an indication of a configuration error and should usually be addressed by a build engineer rather than a developer if there is such roles separation.   
If such an error occurs, TeamCity:
* doesn't send build failed notification (unless you have subscribed to "the build fails to start" notification)
* doesn't associate pending changes with this build, that is the changes will remain pending, because they were not actually tested
* doesn't show such build as the last finished build on the overview page
* such builds will not affect the build configuration status and the status of developer changes
* shows a "configuration error" stripe for a build configuration with such a build
  <seealso>
        <category ref="concepts">
            <a href="build-configuration.md">Build Configuration Status</a>
            <a href="change.md">Change</a>
            <a href="change-state.md">Change State</a>
        </category>
        <category ref="user-guide">
            <a href="viewing-your-changes.md">Viewing Your Changes</a>
        </category>
</seealso>