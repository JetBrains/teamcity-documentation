[//]: # (title: Browser Notifier)
[//]: # (auxiliary-id: Browser Notifier)

The TeamCity _Browser Notifier_ extension allows receiving real-time notifications about build statuses and events.   
The extension is available for Mozilla Firefox, Opera, and Google Chrome (including all Chromium-based browsers such as Microsoft Edge).

<note instance="tc">

Browser Notifier aims at replacing the obsolete Windows Tray Notifier and automatically uses all rules configured for it, if any. The __Your Profile | Notification Rules | Windows Tray Notifier__ tab in TeamCity is renamed to __Browser Notifier__.

We recommend switching to the new Notifier as it covers the Windows Tray Notifier functionality and is overall more convenient. However, if you are using Mozilla Firefox or Opera, note that Browser Notifier will work only when your browser is open.

</note>

## Main Features

* Notifications appear directly in a browser where TeamCity itself is run.
* The extension works instantly after enabling it in a browser: it automatically detects an active TeamCity session and seamlessly authenticates on the server.
* By click, each notification redirects to the related TeamCity page.
* Multiple parallel TeamCity servers are supported.
* Notification rules are easily customizable and work similarly to other [rules](adding-notification-rules.md#What+Will+Be+Watched) in TeamCity.
* History of notifications is available in the OS notification area. The extension works similarly on Windows, macOS, and Linux.

## Enabling Browser Notifier

Follow these steps to start using the Notifier:
1. Download and enable the extension for your browser:   
   * [Google Chrome](https://chrome.google.com/webstore/detail/teamcity-notifier/miolcigeeebinhdbihpodaajenfoggjl)
   * [Microsoft Edge](https://microsoftedge.microsoft.com/addons/detail/joojdhbnigbkaeaohmookbghmlfejcpm)
   * [Mozilla Firefox](https://addons.mozilla.org/en-US/firefox/addon/teamcity-notifier/)
   * [Opera](https://addons.opera.com/en/extensions/details/teamcity-notifier/)
2. Open your TeamCity server and sign in to it.   
The extension will automatically detect the TeamCity session and change the icon from the inactive ![browser-notifier-inactive.png](browser-notifier-inactive.png) to active state ![browser-notifier-active.png](browser-notifier-active.png). Click the icon and check that notifications are enabled.
3. The Notifier relies on your [custom notification rules](adding-notification-rules.md#What+Will+Be+Watched) configured in TeamCity. If no rules are specified, you won’t be able to receive any notifications. To add or modify a rule, click __Edit rules__ opposite the server URL. You will be redirected to the __Your Profile | Notification Rules | Browser Notifier__ tab of your TeamCity user settings.

## Working with Browser Notifier

After detecting an active TeamCity session, the Notifier will be monitoring the server and reporting events in real time similarly to how regular notifications work in your OS and browser.   
Chromium-based browsers (such as Google Chrome and Microsoft Edge) are able to send notifications even when working in a background mode after closing. Mozilla Firefox and Opera send notifications only when open.

Click the notification to get to the related TeamCity build page.

The history of notifications is available in the OS notification area.

If you are signed in to multiple TeamCity servers in the same browser, the extension will remember each detected session and automatically disable/enable notifications for it when you — respectively — sign off/into this server. You can manage all sessions in the Notifier menu or in its __Options__.
