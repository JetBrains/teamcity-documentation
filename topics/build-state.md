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

![build running animation](dk-build-running.gif)

</td>

<td>

running successfully

</td>

<td>

A build is running successfully.

</td></tr><tr>

<td>

<img src="buildSuccessful.png" width="16" alt="Build successful"/>

</td>

<td>

successful

</td>

<td>

A build finished successfully in all specified build configurations.

</td></tr><tr>

<td>

![running and failing.gif](dk-build-failing.gif)

</td>

<td>

running and failing

</td>

<td>

A build is failing.

</td></tr><tr>

<td>

<img src="buildFailed.png" width="16" alt="Build failed"/>

</td>

<td>

failed

</td>

<td>

A build failed at least in one specified build configuration.

</td></tr><tr>

<td>

<img src="cancelled.png" width="16" alt="Build cancelled"/>

</td>

<td>

cancelled

</td>

<td>

A build was cancelled.

</td></tr></table>

### Canceled/Stopped build

Stopping a running build results in the build status displayed as cancelled. You can stop a running build from the [__Build Results__](working-with-build-results.md), [__Build Configuration Home__](build-configuration-home-page.md) or using the __Stop__ option from the __Actions__ drop-down menu.

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

<img src="personalFinished.png" width="20" alt="Personal build successful"/>

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

<img src="personalFinishedFailed.png" width="20" alt="Personal build failed"/>

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

Builds which failed to start, that is which did not get to the point of launching the first build step are marked with the ![redSign.png](redSign.png) icon. It may be caused by a VCS repository being down when the build starts, or the inability to resolve artifact dependencies and so on. Such build status is often an indication of a configuration error and should usually be addressed by a build engineer rather than a developer.   
If such an error occurs, TeamCity:
* doesn't send build failed notification (unless you have subscribed to "the build fails to start" notification)
* doesn't associate pending changes with this build, that is the changes will remain pending, because they were not actually tested
* doesn't show such build as the last finished build on the overview page
* such builds will not affect the build configuration status and the status of developer changes
* shows a "configuration error" stripe for a build configuration with such a build


## Project and Build Configuration Icons

Build states affect the way TeamCity projects and build configurations are presented. Color-coded icons provide a bird's eye view of the build farm, enabling you to instantly spot problematic projects.

Project and configuration icons have three distinctive colors:

* **Gray** — corresponds to unknown (neither successful nor failure) project/configuration status. This typically happens when no recent builds exist.
    <img src="dk-icons-gray.png" width="660" alt="Gray project and configuration icons" style="block"/>

* **Green** — used for configurations whose last builds were successful and projects whose configurations are all green.
  <img src="dk-icons-green.png" width="660" alt="Green project and configuration icons" style="block"/>

* **Red** — used for configurations whose last builds failed and projects that have at least one red configuration.
  <img src="dk-icons-red.png" width="660" alt="Red project and configuration icons" style="block"/>


TeamCity uses builds of the **specific branch** to identify the project/configuration status. If a configuration's version selector points to the specific branch, builds of this branch affect the configuration status. Global views, such as project overview and side navigation bar, use configuration default branches instead.


For example, the configuration below has multiple successful builds in the "development" branch. The configuration's branch selector targets this specific branch, and the configuration's icon is green.

<img src="dk-confbuttons-green.png" width="706" alt="Green configuration icons"/>

The same configuration but in the "sandbox" branch has failed its last build, which turns the configuration icon red.

<img src="dk-confbuttons-red.png" width="706" alt="Red configuration icons"/>

However, since there were no builds in the default "main" branch, project icons on both screens above are gray. The same color is used for configuration icons when the version selector is not pointing to any branch.

<img src="dk-confbuttons-gray.png" width="706" alt="Gray configuration icons"/>


<seealso>
        <category ref="concepts">
            <a href="changing-build-configuration-status.md">Build Configuration Status</a>
            <a href="change.md">Change</a>
            <a href="change-state.md">Change State</a>
        </category>
        <category ref="user-guide">
            <a href="viewing-user-changes-in-builds.md">Viewing Your Changes</a>
        </category>
</seealso>