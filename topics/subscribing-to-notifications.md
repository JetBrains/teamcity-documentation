[//]: # (title: Subscribing to Notifications)
[//]: # (auxiliary-id: Subscribing to Notifications)

TeamCity provides a wide range of notification possibilities to keep developers informed about the status of their projects. Notifications can be sent by email and instant messages, or can be displayed in the IDE (with the help of TeamCity plugins) or in the web browser. Notifications can also be received via Atom/RSS syndication feeds.

Notifications in TeamCity can be sent:
* __Per user__, according to the user's configured notification rules. These rules can also be set at the user-group level, so they are applied to all the users in the TeamCity group.  
  You can create a user-level notification rule for a specific project or multiple projects and build configurations. These notifications are also available in a wider range of software than the build-level ones: IDE, browsers, Slack, Jabber, and email.
* __Per build configuration__, with the [Notifications](notifications.md) build feature. Currently, these notifications are available only for email and Slack.   
  Choose build-level notifications if you want to notify many people at once about a specific build configuration. You can enter an email list address or a Slack channel ID, and whoever is subscribed to this list or channel will be notified when the selected build events occur.

This article explains how to configure user-level notifications and describes the events that can trigger notifications. To read about build-level notifications, refer to the [respective article](notifications.md).

>You can [customize notification templates](customizing-notifications.md).
> 
{product="tc"}

## Subscribing to User-level Notifications

TeamCity allows you to flexibly adjust the notification rules, so that you receive notifications only on the events you are interested in. To subscribe to user-level notifications:

1\. In the upper right corner of the screen, click the arrow next to your username, and select __My Settings &amp; Tools__ from the drop-down menu. Open the __Notification Rules__ tab.

2\. Click the required notifications type and configure the required settings: 
 * __Email Notifier__*: relies on the email address specified in __My Settings &amp; Tools | General__.

>Note that TeamCity comes with a default notification rule. It will send you an email notification if a build with your changes has failed. This rule starts working after you enter the email address.

* __IDE Notifier__: the required TeamCity plugin must be installed in your IDE. For the details on installing TeamCity IDE plugins, refer to [Installing Tools](installing-tools.md).
* __Jabber Notifier__*: expects entering a Jabber account name. Note that instead of Jabber you can specify your Google Talk account here if this option is [configured by the System Administrator](setting-up-google-mail-and-google-talk-as-notification-servers.md#Google+Talk).
{product="tc"}
* __Slack Notifier__: requires signing in to Slack and relies on the Slack connection configured in the project settings. Read more on how to configure the connection to Slack [here](configuring-connections.md#Slack).
* (obsolete) __Windows Tray Notifier__: [Windows Tray Notifier](windows-tray-notifier.md) must be installed.
{product="tc"}
* __Browser Notifier__: aims at replacing Windows Tray Notifier and requires installing the [web browser extension](browser-notifier.md).

3\. Click __Add new rule__ and specify the rule in the dialog. The notification rules are comprised of two parts: [what you will watch](#What+Will+Be+Watched) and [which events you will be notified about](#Which+Events+Will+Trigger+Notifications). See the details below.

<note product="tc">

* Email and Jabber notifications are sent only if the System Administrator has configured the SMTP and Jabber servers respectively in __Administration | Server Administration__. System Administrators can also [change the templates](customizing-notifications.md) used for notifications.

</note>

### What Will Be Watched

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

Make sure your [Version Control Username](managing-users-and-user-groups.md#VCS+Usernames) Settings are correct.

</note>

* __My favorite builds only__ — limit notifications to your [favorite builds](favorite-build.md).

</td></tr><tr>

<td>

System wide events

</td>

<td>

Select to be notified about investigations assigned to you.

</td></tr></table>

### Which Events Will Trigger Notifications

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

Check this option to receive notifications on the test [mute](investigating-and-muting-build-failures.md#Muting+Failed+Tests) status change in the affected build configurations.

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

If there are more than one parent with their own rules set, the inherited rules are processed top\-bottom corresponding to the order they are presented on the Notification Rules user profile tab (alphabetically).

## Unsubscribing and Overriding Existing Rules

You may already have some notification rules configured by your System Administrator for the user group you're included in.
* To unsubscribe from or override group notifications, add your own rule with the same watched builds and different notification events.
* To unsubscribe from all events, add a rule with the corresponding watched builds and no events selected.

## Customizing RSS Feed Notifications

TeamCity allows obtaining information about the finished builds or about the builds with the changes of particular users via RSS. You can customize the RSS feed from the TeamCity Tools sidebar of __My Settings &amp; Tools__ page using the [Syndication Feed](syndication-feed.md) section (click __customize__ to open the __Feed URL Generator__ options) or from the home page of a build configuration. TeamCity produces a URL to the syndication feed on the basis of the values specified on the Feed URL Generator page.

#### Feed URL Generator Options

<table><tr>

<td>

Option

</td>

<td>

Description

</td></tr><tr>

<td>

__Select Build Configurations__

</td>

<td>

</td></tr><tr>

<td>

List build configurations

</td>

<td>

Specify which build configurations to display in the __Select build configurations or projects__ field. The following options are available:

* __With the external status enabled__: if this option is selected, the next field shows the list of build configurations with the [Enable status widget option set](configuring-general-settings.md#Enable+Status+Widget), and build configurations visible to everybody without authentication.
* __Available to the Guest user__: Select this option to show the list of build configurations, which are visible to a user with the Guest permissions.
* __All__: Select this option to show a complete list of build configurations; HTTP authorization is required. Selecting this option enables the "Feed authentication settings" field. If the external status for the build configuration is not enabled, the feed will be available only for authorized users.


</td></tr><tr>

<td>

Select build configurations or projects

</td>

<td>

Use this list to select the build configurations or projects you want to be informed about via a syndication feed.

</td></tr><tr>

<td>

__Feed Entries Selection__

</td>

<td>

</td></tr><tr>

<td>

Generate feed items for

</td>

<td>

Specify the events to trigger syndication feed generation. You can opt to select builds, changes or both.

</td></tr><tr>

<td>

Include builds

</td>

<td>

Specify the types of builds to be informed about:

* All builds
* Only successful
* Only failed

</td></tr><tr>

<td>

Only builds with changes of the user

</td>

<td>

Select the user whose changes you want to be notified about. You can get a syndication feed about the changes of all users, yours only, or of a particular user from the list of users registered to the server.

</td></tr><tr>

<td>

__Other Settings__

</td>

<td>

The following options are available only if __All__ is selected in the __List build configurations__ section.

</td></tr><tr>

<td>

Feed Authentication Settings

</td>

<td>

__Include credentials for HTTP authentication__ — check this box to specify the username and password for automatic authentication. If this option is not checked, you will have to enter your user name and password in the authorization dialog box of your feed reader.

_TeamCity User_, _Password_ — configurable when the ... option is checked. Type the username and password which will be used for HTTP authorization.   

</td></tr><tr>

<td>

__Copy and Paste the URL into Your Feed Reader or Subscribe__

</td>

<td>

This field displays the URL generated by TeamCity on the basis of the values specified above. You can either copy and paste it to your feed reader or click the __Subscribe__ link.

</td></tr></table>

#### Additional Supported URL Parameters

In addition to the URL parameters available in the [Feed URL Generator](#Feed+URL+Generator+Options), the following parameters are supported:

[//]: # (Internal note. Do not delete. "Subscribing to Notificationsd301e584.txt")    

<table><tr>

<td>

Parameter Name

</td>

<td>

Description

</td></tr><tr>

<td>

`itemsCount`

</td>

<td>

A number; limits the number of items to return in a feed. Defaults to 100.

</td></tr><tr>

<td>

`sinceDate`

</td>

<td>

A negative number; specifies the number of minutes. Only builds finished within the specified number of minutes from the moment of feed request will be returned. Defaults to 5 days.

</td></tr><tr product="tc">

<td>

`template`

</td>

<td>

A name of the custom template used for rendering the feed (`<template_name>`). The file `<TeamCity Data Directory>\config\<template_name>.ftl` should be present on the server. See the [corresponding section](customizing-notifications.md) on the file syntax.

</td></tr></table>

By default, the feed is generated as an Atom feed. Add `&feedType=rss_0.93` to the feed URL to get the feed in RSS 0.93 format.

#### Example

Get builds from the TeamCity server located at [`http://teamcity.server:8111`](http://teamcity.server:8111) address, from the build configuration with ID `bt1`, limit the builds to the those started with the last hour but no more than 200 items:


```Shell
http://teamcity.server:8111/httpAuth/feed.html?buildTypeId=bt1&itemsType=builds&sinceDate=-60&itemsCount=200

```

<seealso>
        <category ref="admin-guide">
            <a href="customizing-notifications.md" product="tc">Customizing Notifications</a>
            <a href="notifications.md">Notifications</a>
        </category>
</seealso>