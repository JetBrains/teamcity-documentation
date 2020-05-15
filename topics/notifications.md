[//]: # (title: Notifications)
[//]: # (auxiliary-id: Notifications)

The _Notifications_ build feature is responsible for sending notifications about the build statuses and events to external services. Currently, the feature comprises __Email Notifier__ and __Slack Notifier__.

This feature compliments the functionality of [project-level notifications](subscribing-to-notifications.md) that can be assigned to a particular user or user group.   
The Notifications feature allows configuring _notifications per build configuration_. This approach does not require referencing a specific TeamCity user and works better for group notifications.

To set up similar notifications for several build configurations, use a [build configuration template](build-configuration-template.md).

## Common Settings

For each type of Notifier you can select [events to watch](subscribing-to-notifications.md#What+Will+Be+Watched) and configure a [branch filter](branch-filter.md). Note that if no branch filter is configured, you will receive notifications about the default branch only.

## Email Notifier

Email Notifier requires entering a target email only. For example, you can specify an email list address, and all its subscribers will be receiving build notifications automatically.

## Slack Notifier

Slack Notifier relies on a [Slack connection](#Configuring+Slack+Connection), preconfigured in the parent project's settings, and requires an ID of a channel or user who will be receiving notifications.

<tip>

Start typing the user ID, and TeamCity will suggest it automatically based on your connection settings. Alternatively, you can copy this ID from your Slack user profile options.

</tip>

### Configuring Slack Connection

To configure a connection to Slack, go to __Project Settings | Connections__ and click __Add Connection__. In the opened form select the _Slack_ type and give a connection any convenient name.

To be able to integrate TeamCity with Slack, you need to create a respective [Slack app](https://api.slack.com/apps) with the following scopes: `channels:read`, `chat:write`, `im:read`, `im:write`, `users:read`, `team:read`, `groups:read`.

When configuring the app in Slack, set the __Redirect URL__ in __OAuth & Permissions | App Management__ to `<teamcity-server-address>`.

After creating the app, enter its parameters in the _Add connection_ form in TeamCity:
* _Client ID_ and _Secret_ from the app's __Basic Information__ page
* [bot user token](https://api.slack.com/docs/token-types#bot)


<seealso>
        <category ref="user-guide">
            <a href="subscribing-to-notifications.md">Subscribing to Notifications</a>
        </category>
        <category ref="admin-guide">
            <a href="notifications.md">Customizing Notifications</a>
        </category>
</seealso>