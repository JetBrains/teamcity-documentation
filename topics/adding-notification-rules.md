[//]: # (title: Adding Notification Rules)
[//]: # (auxiliary-id: Adding Notification Rules;Subscribing to Notifications)

This article explains how to set up notification rules. To learn how enable the notification functionality on your TeamCity server, follow [these instructions](configuring-notifications.md).

Each notification rule consists of two parts: [what scope will be watched](#What+Will+Be+Watched) and [what events will trigger notifications](#Which+Events+Will+Trigger+Notifications). Users can create personal rules and inherit rules from their user group.

## Add Notification Rule

To add a new personal notification rule:
1. In the upper right corner of the screen, click your avatar and click __Profile__ in the drop-down menu.
2. In __Your Profile__, open the __Notification Rules__ tab.
3. Click the required notifications type and configure the required settings.
4. Click __Add new rule__ and specify the rule in the dialog.

## What Will Be Watched

<table>

<tr><td></td><td></td></tr>

<tr>

<td>

Events related to the following projects and build configurations

</td>

<td>

Select the projects / build configurations whose builds you want to monitor. Notification rules defined for a project will be propagated to its subprojects. To monitor all of the builds of all the projects' build configurations, select __Root project__. 

Use the following options for granular control over the notifications in the selected projects / build configurations:

* __Branch filter__ — select this option to receive alerts only on the builds from the specified branches. Read more in [Branch Filter](branch-filter.md).
* __Builds with my changes only__ — select this option to limit notifications to builds containing your changes only. 

<note>

Make sure your [Version Control username](creating-and-managing-users.md#VCS+Usernames) settings are correct.

</note>

* __My favorite builds only__ — limit notifications to your [favorite builds](build-actions.md#Add+Build+to+Favorites).

</td></tr><tr>

<td>

System-wide events

</td>

<td>

Select to be notified about investigations assigned to you.

</td></tr></table>

## Which Events Will Trigger Notifications

<table>

<tr><td></td><td></td></tr>

<tr>

<td>

__Build fails__

</td>

<td>

Check this option to receive notifications about all of _finished_ failed builds in the specified projects and build configurations. If an investigation for a build configuration was assigned to someone _while the build was in progress_, the failed build notification is sent only to that user.

Note that if __Builds with my changes only__ is selected in the __Watch__ area, TeamCity will notify you if a build with your changes fails, and will keep sending notifications on each 'incomplete' build afterwards until a successful one. An incomplete build is a finished build which failed with at least one of the following errors: 

* Execution timeout
* JVM crashed
* JVM Out of memory error
* Unable to collect changes
* Compilation error
* Artifacts publishing failed
* Unknown Failure reason

</td></tr><tr>

<td>

Only notify on the first failed build after successful

</td>

<td>

Check this option to be notified about only the first failed build after a successful build or the first build with your changes. When using this option, you will not be notified about subsequent failed builds.

</td></tr><tr>

<td>

Only notify on new build problem or new failed test

</td>

<td>

Check this option to be notified only if a build fails with a new build problem or a new failed test.

</td></tr><tr>

<td>

__Build is successful__

</td>

<td>

Check this option to receive notifications when a build of the specified projects and build configurations executed successfully.

</td></tr><tr>

<td>

Only notify on the first successful build after failed

</td>

<td>

Check this option to receive notifications when only the first successful build occurs after a failed build. Notifications about subsequent successful builds will _not_ be sent.

</td></tr><tr>

<td>

__The first error occurs__

</td>

<td>

Check this option to receive notifications about a "failing" build as soon as the first build error is detected, even before the build has finished.

</td></tr><tr>

<td>

__Queued build requires approval__

</td>

<td>

Check this option to receive notifications about a build requiring your approval. This option overrides the __Builds with my changes only__ option.

</td></tr><tr>

<td>

__Build starts__

</td>

<td>

Check this option to receive notifications as soon as a build starts.

</td></tr><tr>

<td>

__Build fails to start__

</td>

<td>

Check this option to receive notifications when a build [fails to start](build-state.md#Personal+Build+States).

</td></tr><tr>

<td>

__Build is probably hanging__

</td>

<td>

Check this option to receive notifications when TeamCity identifies a build as [hanging](configuring-general-settings.md#Hanging+Build+Detection).

</td></tr><tr>

<td id="investigation-is-updated">

__Investigation is updated__

</td>

<td>

Check this option to receive notifications on changing a build configuration or test investigation status, e.g. someone is investigating the problem, or problems were fixed, or the investigator changed.

</td></tr><tr>

<td>

__Tests are muted or unmuted__

</td>

<td>

Check this option to receive notifications on the test [mute](investigating-and-muting-build-failures.md) status change in the affected build configurations.

</td></tr><tr>

<td>

__Investigation assigned to me__

</td>

<td>

This option is available only if the __System wide events__ option is selected in the __Watch__ area. Check the option to be notified each time you start investigating a problem.

</td></tr></table>

## Rules Processing Order
[//]: # (AltHead: processingOrder)

TeamCity applies the notification rules in the order they are presented. TeamCity checks whether a build matches any notification rule, and sends a notification according to _the first matching_ rule; the further check for matching conditions for the same build is not performed. You can reorder the configured notification rules.

The user rules are applied first, then the group rules are applied.

The group rules are processed in hierarchical order: starting from child groups to parent ones.

If there are more than one parent with their own rules set, the inherited rules are processed top-to-bottom corresponding to the order they are presented on the Notification Rules user profile tab (alphabetically).

## Unsubscribing and Overriding Existing Rules

You may already have some notification rules configured by your System Administrator for the user group you're included in.
* To unsubscribe from or override group notifications, add your own rule with the same watched builds and different notification events.
* To unsubscribe from all events, add a rule with the corresponding watched builds and no events selected.

<seealso>
        <category ref="admin-guide">
            <a href="customizing-notification-templates.md" product="tc">Customizing Notifications</a>
            <a href="notifications.md">Notifications</a>
        </category>
</seealso>