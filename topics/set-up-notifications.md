[//]: # (title: Set Up Notifications)
[//]: # (auxiliary-id: Set Up Notifications)

You can integrate TeamCity with various external systems and be notified about the build events:
* in a browser
* via email
* via Slack
* in an IDE

In this guide, we show how to quickly configure email, browser, and Slack notifications. For other methods and in-depth details, see [this article](configuring-notifications.md).

## Configure Notifiers

### Email Notifier

First, the TeamCity server administrator should integrate TeamCity with the email service provider used in your organization. This can be done in __Administration | Email Notifier__ and requires entering parameters like SMTP host and service email.

The email notifier will send notifications to the addresses specified in each user's profile, according to their [preferences](#Subscribe+to+Notifications).

In addition to notifications per user, you can configure mailing lists per build configuration. This way, when a certain event occurs in a certain build configuration, TeamCity will report it to the respective service email address, specified by you. You can control the final distribution stages of this notification on your email provider's site.  
To configure such behavior for a build configuration, you need to add a [Notifications](notifications.md#Email+Notifier) build feature.

### Browser Notifier

The TeamCity Notifier extension is available for the following desktop browsers:
* [Google Chrome](https://chrome.google.com/webstore/detail/teamcity-notifier/miolcigeeebinhdbihpodaajenfoggjl)
* [Microsoft Edge](https://microsoftedge.microsoft.com/addons/detail/joojdhbnigbkaeaohmookbghmlfejcpm)
* [Mozilla Firefox](https://addons.mozilla.org/en-US/firefox/addon/teamcity-notifier/)
* [Opera](https://addons.opera.com/en/extensions/details/teamcity-notifier/)

When installed, it automatically detects an active TeamCity session if your TeamCity server is open in the browser. The extension displays notifications according to your [preferences](#Subscribe+to+Notifications).

Read more about this type of notifier [here](browser-notifier.md).

### Slack Notifier

To set up Slack notifications, you need to create a dedicated app on the Slack side and connect TeamCity to this app. We suggest that you read [this guide](configuring-connections.md#Slack) for the detailed instruction.

When configured, you will need to authenticate in Slack in __Your Profile | Notification Rules__ and specify the [rules](#Subscribe+to+Notifications) themselves. Then, you will be receiving build status notifications directly to private messages in Slack.

In addition to notifications per user, you can configure notifications per build configuration. This way, when a certain event occurs in a certain build configuration, TeamCity will report it to the respective Slack channel, specified by you.  
To configure such behavior for a build configuration, you need to add a [Notifications](notifications.md#Slack+Notifier) build feature.

## Subscribe to Notifications

All the notifiers behave according to notification rules. If you want to configure individual notification rules, go to __Your Profile | Notification Rules__. If you want to configure notification rules for a mailing list or Slack channel, they need to be specified in the settings of the respective build feature.

A notification rule comprises the following conditions:
* what projects to monitor
* what branches to monitor
* include all builds, or only builds with your changes, or/and only your favorite builds
* what events to report

The notification about a certain event is only sent if this event satisfies any of the configured rules.

Admins can configure these rules per [user group](creating-and-managing-user-groups.md), and its users will inherit these rules automatically.

## Customize Notifications
{instance="tc"}

You can change the text and structure of the notification messages by altering their templates. Read [our instructions](customizing-notification-templates.md) on this subject.