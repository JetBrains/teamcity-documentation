[//]: # (title: Setting up Google Mail and Google Talk as Notification Servers)
[//]: # (auxiliary-id: Setting up Google Mail and Google Talk as Notification Servers)
This section covers how to set up the Google Mail and Google Talk as notification servers when configuring the TeamCity server.

## Google Mail

On the __Administration | Email Notifier__ page set the options as described below:

<table><tr>

<td>

Property


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

E\-mail address to send notifications from.


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

User's GMail password


</td></tr><tr>

<td>

Secure connection


</td>

<td>

SSL


</td></tr></table>

(see also [Google help](https://mail.google.com/support/bin/answer.py?answer=13287))

## Google Talk

Thee settings were working with Google Talk and can still continue to work with Google Hangouts. However, since Google discontinues Jabber/XMPP support, they might stop working.

On the __Administration | Jabber Notifier__ page set the options as described below:

<table><tr>

<td>

Property


</td>

<td>

Value


</td></tr><tr>

<td>

Server


</td>

<td>

`talk.google.com`


</td></tr><tr>

<td>

Port


</td>

<td>

5222


</td></tr><tr>

<td>

Server user


</td>

<td>

Full username with domain part if you use Google Apps for domain


</td></tr><tr>

<td>

Server user password


</td>

<td>

User's GMail password


</td></tr><tr>

<td>

Use legacy SSL


</td>

<td>

no


</td></tr></table>

__ __