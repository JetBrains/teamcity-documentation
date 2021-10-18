[//]: # (title: Notifier)
[//]: # (auxiliary-id: Notifier)

TeamCity supports the following notifiers:

<table><tr>

<td>

Notifier


</td>

<td>

Description


</td></tr><tr>

<td>

__Email Notifier__


</td>

<td>

Notifications regarding specified events are sent via email. Email Notifier can operate on a [user](subscribing-to-notifications.md#Subscribing+to+User-level+Notifications) and [build configuration](notifications.md) level.

</td></tr>

<tr>

<td>

__Slack Notifier__


</td>

<td>

Notifications regarding specified events are sent to the Slack messenger. Slack Notifier can operate on a [user](subscribing-to-notifications.md#Subscribing+to+User-level+Notifications) and [build configuration](notifications.md) level.

</td></tr>

<tr>

<td>

__IDE Notifier__


</td>

<td>

Displays the status of the specified build configurations and/or the status of the user changes in the user's IDE. Requires a corresponding TeamCity plugin: IntelliJ Platform plugin, Addin for Visual Studio or Eclipse Plugin; which can be downloaded from the [My Settings&amp;Tools](subscribing-to-notifications.md) page. Refer to the [Installing Tools](installing-tools.md) section. 


</td></tr><tr product="tc">

<td>

__Jabber Notifier__ (needs installing a [dedicated plugin](https://plugins.jetbrains.com/plugin/17722-notifier-jabber-xmpp))

</td>

<td>

Notifications regarding specified events are sent via Jabber.


</td></tr></table>

You can configure the notifier settings, create, change, and delete notification rules in __My Settings \& Tools | [Notification Rules](subscribing-to-notifications.md)__.

 <seealso>
        <category ref="user-guide">
            <a href="subscribing-to-notifications.md">Subscribing to Notifications</a>
        </category>
        <category ref="admin-guide" product="tc">
            <a href="customizing-notifications.md">Customizing Notifications</a>
        </category>
</seealso>