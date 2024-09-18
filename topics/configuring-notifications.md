[//]: # (title: Configuring Notifications)
[//]: # (auxiliary-id: Configuring Notifications)

TeamCity can notify users about various events:
* in a web browser
* via email
* via Slack
* in an IDE

This article explains how notifications work in TeamCity and shows how to set them up on your server.

## Notification Levels

Notifications in TeamCity can be configured:

<table>

<tr><td></td><td></td></tr>

<tr>

<td>

__[Per user](#Configuring+User-level+Notifications)__

</td><td>

Notification rules are configured in the user profile settings. These rules can also be set at a user group level, so they are applied to all the users in the group.

A user-level notification rule can be assigned to a specific project or multiple projects and build configurations.

These notifications are available for IDEs, browsers, Slack, and email.

</td>


</tr>

<tr>

<td>

__[Per build configuration](#Configuring+Build-level+Notifications)__

</td><td>

Requires configuring the [Notifications](notifications.md) build feature.

Build-level notifications work best for notifying many people at once about events in a specific build configuration. You can specify an email list address or a Slack channel ID, and whoever is subscribed to this list or channel will be notified when the selected build events occur.

Currently, these notifications are available only for email and Slack.

</td>

</tr>

</table>

## Configuring User-level Notifications

### Email Notifications

TeamCity Email Notifier relies on the email address specified in your user profile.

To set up email notifications on a TeamCity server, the System Administrator needs to configure the SMTP server in __Administration | Email Notifier__. You can find the values of the expected parameters in you email server's settings.
{product="tc"}

See the [example configuration for Google Mail](setting-up-google-mail-as-notification-server.md).
{product="tc"}

### Browser Notifications

TeamCity Browser Notifier can show notifications directly in your web browser. It works as a browser extension. See the details about it in [this article](browser-notifier.md).

### Slack Notifications

TeamCity Slack Notifier requires signing in to Slack and relies on the Slack connection configured in the project settings. Read more on how to configure the connection to Slack [here](configuring-connections.md#Slack), or watch a video tutorial:

<video src="https://youtu.be/d_Xuw7kkp4c"
title="TeamCity tutorial — How to integrate TeamCity and Slack"/>

### IDE Notifications

TeamCity IDE Notifier requires installing the TeamCity plugin in your IDE. For the details on installing TeamCity IDE plugins, refer to [Installing Tools](installing-tools.md).

## Configuring Build-level Notifications

To set up notifications on a build configuration level, you need to add the [Notifications](notifications.md) build feature to this build configuration. This feature has two types:

* [Email Notifier](notifications.md#Email+Notifier) sends emails per events in this build configuration. Similarly to the [user-level email notifications](#Email+Notifications), it uses the SMTP server settings configured in __Administration | Email Notifier__.
{product="tc"}
* [Email Notifier](notifications.md#Email+Notifier) sends emails per events in this build configuration.
  {product="tcc"}
* [Slack Notifier](notifications.md#Slack+Notifier) posts events to a selected Slack channel. Similarly to the [user-level email notifications](#Slack+Notifications), requires configuring a connection to Slack.

## Configuring Notification Rules

After you configure your preferred notification methods, it is time to choose which events will be triggering notifications.

For example, if the SMTP server is configured in __Administration | Email Notifier__, TeamCity will be sending emails according to the default rule: if a user has an email address added in their settings, TeamCity will email them whenever the build containing their changes fails.

Users can manage their own notification rules in their profile settings. Project Administrators can manage notification rules for a user or a user group in, respectively, __Administration | Users | User's settings__ or __Administration | Groups | Group's settings__.

Read more about the composition and processing of notification rules in [this article](adding-notification-rules.md).

## Changing Notification Templates
{product="tc"}

Project Administrators can change the default notifications' format by customizing their text templates. See the detailed instruction in [this article](customizing-notification-templates.md).
