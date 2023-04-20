[//]: # (title: Setting up Google Mail as Notification Server)
[//]: # (auxiliary-id: Setting up Google Mail as Notification Server)

To set Google Mail as a notification server for TeamCity, configure these options in __Administration | Email Notifier__ page, set the options as described below:

<table><tr>

<td>

Setting

</td>

<td>

Value

</td></tr><tr>

<td>

SMTP host

</td>

<td>

`smtp.gmail.com`

</td></tr><tr>

<td>

SMTP port

</td>

<td>

465

</td></tr><tr>

<td>

Send email messages from

</td>

<td>

Email address to send notifications from.

</td></tr><tr>

<td>

SMTP login

</td>

<td>

Full username with domain part if you use Google Apps for domain

</td></tr><tr>

<td>

SMTP password

</td>

<td>

User's GMail password. If you get the "Username and Password not accepted" error when testing your connection, try replacing the user password with an [App Password](https://support.google.com/accounts/answer/185833?sjid=7857815404877033739-EU&authuser=2).

</td></tr><tr>

<td>

Secure connection

</td>

<td>

SSL

</td></tr></table>

See also: [Google help](https://mail.google.com/support/bin/answer.py?answer=13287) | [](notifications.md#Email+Notifier) | [Sending emails via Service Messages](service-messages.md#Sending+Custom+Email+Messages).