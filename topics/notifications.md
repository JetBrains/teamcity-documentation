[//]: # (title: Notifications)
[//]: # (auxiliary-id: Notifications)

The _Notifications_ [build feature](adding-build-features.md) is responsible for sending notifications about build statuses and events to external services. Currently, the feature provides __Email Notifier__ and __Slack Notifier__.

This feature adds to the functionality of [project-level notifications](subscribing-to-notifications.md) that can be assigned to a particular user or user group, but it allows configuring _notifications per build configuration_. This approach does not require referencing a specific TeamCity user and works better for group notifications.

>To set up similar notifications for several build configurations, use a [build configuration template](build-configuration-template.md).

## Common Settings

For each type of Notifier you can select [events to watch](subscribing-to-notifications.md#What+Will+Be+Watched) and configure a [branch filter](branch-filter.md). Note that if no branch filter is configured, you will receive notifications about the default branch only.

## Email Notifier

The Email Notifier feature requires entering a target email only. For example, you can specify an email list address, and all its subscribers will be receiving build notifications automatically.

Note that TeamCity Email Notifier relies on the SMTP server settings configured in __Administration | Server Administration__.

## Slack Notifier

The Slack Notifier feature relies on a [Slack connection](#Configuring+Slack+Connection) that should be preconfigured in the parent project's settings.

After configuring this connection, go to the settings of the build configuration you want to receive notifications for. In __Build Features__, add the _Notifications_ feature and select _Slack Notifier_. Choose the created connection and enter the ID of a channel or user who will be receiving notifications.

>Start typing the user ID, and TeamCity will automcomplete it. Alternatively, you can copy this ID from your Slack user profile options (__Profile | More | Copy member ID__).

Configure the [common settings](#Common+Settings) to choose the cases to be notified about.

### Configuring Slack Connection

The TeamCity integration with Slack requires creating a [Slack app](https://api.slack.com/apps) with the following [bot token scopes](https://api.slack.com/scopes): `channels:read`, `chat:write`, `im:read`, `im:write`, `users:read`, `team:read`, `groups:read`. You can add these in __Features | OAuth & Permissions | Scopes__.   
To ensure your TeamCity server can connect to Slack, specify all the possible endpoint addresses of the server as __Redirect URLs__ in __Features | OAuth & Permissions__. In most cases, it would be enough to specify the Server URL set in __[Global Settings](configuring-server-url.md)__. However, if you access the server bypassing the proxy, the authentication in Slack might not work unless the server's IP address is also specified in __Redirect URLs__.

>See this [Basic app setup](https://api.slack.com/authentication/basics) guide for more details.

Now you can return to TeamCity and configure a connection to Slack. Go to __Project Settings | Connections__ and click __Add Connection__. In the opened form, select the _Slack_ type and give a connection any convenient name.

Enter the app parameters:
* _Client ID_ and _Secret_ from the app's __Basic Information__ page
* a [bot user token](https://api.slack.com/docs/token-types#bot) of your app

Save the connection and proceed with [adding the Notifier feature](#Slack+Notifier).

<seealso>
        <category ref="user-guide">
            <a href="subscribing-to-notifications.md">Subscribing to Notifications</a>
        </category>
</seealso>