[//]: # (title: Notifications)
[//]: # (auxiliary-id: Notifications)

The _Notifications_ [build feature](adding-build-features.md) is responsible for sending notifications about build statuses and events to external services. Currently, the feature provides __Email Notifier__ and __Slack Notifier__.

This feature adds to the functionality of [user-level notifications](configuring-notifications.md) that can be assigned to a particular user or user group, but it allows configuring _notifications per build configuration_. This approach does not require referencing a specific TeamCity user and works better for group notifications.

>To set up similar notifications for several build configurations, use a [build configuration template](build-configuration-template.md).

## Email Notifier

To configure email notifications for a build configuration:

1. Enter a recipient email address. If you need to specify multiple addresses, enter each following address on a new line.
2. Configure a [branch filter](branch-filter.md). If it is not configured, you will receive notifications about the default branch only.
3. Select [events to watch](adding-notification-rules.md#Which+Events+Will+Trigger+Notifications).

>To customize the notification texts, you can modify [notification templates](customizing-notification-templates.md).
>
{instance="tc"}

Note that TeamCity Email Notifier relies on the SMTP server settings configured in __Administration | Email Notifier__.

> Starting with version 2023.05, you can also utilize [Service Messages](service-messages.md#Sending+Custom+Email+Messages) to send custom email messages from inside build steps.
> 
{type="tip" instance="tc"}

## Slack Notifier

The Slack Notifier feature relies on a [Slack connection](configuring-connections.md#Slack) that should be preconfigured in the parent project's settings.

After configuring the connection, go to the settings of the build configuration you want to receive notifications for:
1. In __Build Features__, add the _Notifications_ feature and select _Slack Notifier_.
2. Choose the created connection.
3. Enter the ID of a channel or user who will be receiving notifications.   
   > Start typing the user ID, and TeamCity will autocomplete it. Alternatively, you can copy this ID from your Slack user profile options (__Profile | More | Copy member ID__).
4. Select the message format. Slack Notifier does not currently support custom notification templates. You can select the verbose format to choose what information to display in notifications, or utilize [Service Messages](service-messages.md#Sending+Custom+Slack+Messages) to send completely custom strings.
5. Configure a [branch filter](branch-filter.md). If it is not configured, you will receive notifications about the default branch only.
6. Select [events to watch](adding-notification-rules.md#Which+Events+Will+Trigger+Notifications).

> Watch our **video tutorial** on how to [integrate TeamCity with Slack](https://www.youtube.com/watch?v=d_Xuw7kkp4c).

<seealso>
        <category ref="user-guide">
            <a href="adding-notification-rules.md">Subscribing to Notifications</a>
        </category>
</seealso>