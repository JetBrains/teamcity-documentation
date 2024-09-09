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

Notifications regarding specified events are sent via email. Email Notifier can operate on both [user and build configuration levels](configuring-notifications.md).

</td></tr>

<tr>

<td>

__Slack Notifier__


</td>

<td>

Notifications regarding specified events are sent to the Slack messenger. Slack Notifier can operate on both [user and build configuration levels](configuring-notifications.md).

</td></tr>

<tr>

<td>

__IDE Notifier__


</td>

<td>

Displays the status of the specified build configurations and/or the status of the user changes in the user's IDE. Requires a corresponding TeamCity plugin: IntelliJ Platform plugin, Add-in for Visual Studio; which can be downloaded from the [My Settings&amp;Tools](adding-notification-rules.md) page. Refer to the [Installing Tools](installing-tools.md) section. 


</td></tr>
</table>

You can configure the notifier settings, create, change, and delete notification rules in __My Settings \& Tools | [Notification Rules](adding-notification-rules.md)__.

 <seealso>
        <category ref="user-guide">
            <a href="adding-notification-rules.md">Subscribing to Notifications</a>
        </category>
        <category ref="admin-guide" instance="tc">
            <a href="customizing-notification-templates.md">Customizing Notifications</a>
        </category>
</seealso>