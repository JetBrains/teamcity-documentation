[//]: # (title: Notifications)
[//]: # (auxiliary-id: Notifications)

The _Notifications_ [build feature](adding-build-features.md) is responsible for sending notifications about build statuses and events to external services. Currently, the feature provides __Email Notifier__ and __Slack Notifier__.

This feature adds to the functionality of [user-level notifications](subscribing-to-notifications.md) that can be assigned to a particular user or user group, but it allows configuring _notifications per build configuration_. This approach does not require referencing a specific TeamCity user and works better for group notifications.

>To set up similar notifications for several build configurations, use a [build configuration template](build-configuration-template.md).

## Email Notifier

To configure email notifications for a build configuration:

1. Enter the target email. For example, you can specify an email list address, and all its subscribers will be receiving build notifications automatically.
2. Configure a [branch filter](branch-filter.md). If it is not configured, you will receive notifications about the default branch only.
3. Select [events to watch](subscribing-to-notifications.md#Which+Events+Will+Trigger+Notifications).

>To customize the notification texts, you can modify [notification templates](customizing-notifications.md).
>
{product="tc"}

Note that TeamCity Email Notifier relies on the SMTP server settings configured in __Administration | Server Administration__.

## Slack Notifier

The Slack Notifier feature relies on a [Slack connection](configuring-connections.md#Slack) that should be preconfigured in the parent project's settings.

After configuring the connection, go to the settings of the build configuration you want to receive notifications for:
1. In __Build Features__, add the _Notifications_ feature and select _Slack Notifier_.
2. Choose the created connection.
3. Enter the ID of a channel or user who will be receiving notifications.   
   >Start typing the user ID, and TeamCity will autocomplete it. Alternatively, you can copy this ID from your Slack user profile options (__Profile | More | Copy member ID__).
4. Select the message format. Slack Notifier does not currently support custom notification templates. However, if you select the verbose format, you will be able to choose what information to display in notifications.
5. Configure a [branch filter](branch-filter.md). If it is not configured, you will receive notifications about the default branch only.
6. Select [events to watch](subscribing-to-notifications.md#Which+Events+Will+Trigger+Notifications).

<seealso>
        <category ref="user-guide">
            <a href="subscribing-to-notifications.md">Subscribing to Notifications</a>
        </category>
</seealso>