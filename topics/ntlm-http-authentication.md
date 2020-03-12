[//]: # (title: NTLM HTTP Authentication)
[//]: # (auxiliary-id: NTLM HTTP Authentication)
The TeamCity NTLM HTTP authentication feature employs Integrated Windows Authentication and allows transparent/SSO login to the TeamCity web UI when using browsers/clients supporting NTLM and Negotiate HTTP authentications with NTLMv1, NTLMv1, NTLMv2 and Kerberos logic. The authentication is supported when the TeamCity server runs under Windows OS. Generally, it allows users to log in to the TeamCity server web UI using their NT domain account without the need to enter credentials manually.

## Configuration

The __NTLM HTTP__ module is configured on the __Administration | Authentication__ page under the "HTTP authentication modules" section.

<tip>

For information on configuring authentication the settings directly in the `<TeamCity Data Directory>/config/auth-config.xml`, refer to the [previous documentation version](https://confluence.jetbrains.com/display/TCD8/NTLM+HTTP+Authentication).
</tip>

You can enable NTLM login with any login module once the TeamCity username is the same as the Windows domain username or the Windows domain username is specified on the user profile.

<note>

NTLM HTTP authentication is supported only for TeamCity servers installed on __Windows__ machines. If you need it under other platforms, feel free to [contact us](mailto:teamcity-support@jetbrains.com) with details as to why.
</note>


[//]: # (Internal note. Do not delete. "NTLM HTTP Authenticationd226e47.txt")    

## Requirements
1. The authenticating user should be logged in to the client workstation with the domain account that is to be used for the authentication.
2. The user's web browser should support NTLM HTTP authentication (the TeamCity server URL should be a "trusted site", etc.)
## Enabling NTLM HTTP Authentication

After the NTLM HTTP authentication module is configured, users will see a link on the login screen which, when clicked, will force the browser to send the domain authentication data.

 You can force the server to announce NTLM HTTP authentication by specifying protocols in the "Force protocols" setting. This will make the server request domain authentication for any request to the TeamCity web UI. If the user's browser is run in the domain environment, the current user will be logged in automatically. If not, the browser will pop up a dialog asking for domain credentials.

Without this attribute, NTLM HTTP authentication will work only if the client explicitly initiates it (e.g. clicks the "Login using NT domain account" link on the login page), and in the usual case an unauthenticated user will be simply redirected to the TeamCity login page.


[//]: # (Internal note. Do not delete. "NTLM HTTP Authenticationd226e74.txt")    


The TeamCity server forces NTLM HTTP authentication only for Windows users by default. If you want to enable it for all users, set the following [internal property](configuring-teamcity-server-startup-properties.md):


```
teamcity.ntlm.ignore.user.agent=true

```



### NTLM login URLs

There are two more ways to force NTLM authentication for a certain connection (there is no need to set the `forceProtocols` attribute for this case):
* Send request to `<Your TeamCity server URL>/ntlmLogin.html` and TeamCity will initiate NTLM authentication and redirect you to the overview page.
* Send request to `<Your TeamCity server URL>/ntlmAuth/<path>` and TeamCity will initiate NTLM authentication and show you the &lt;path&gt; page (without redirect).

## Using NTLM HTTP Authentication Module with LDAP Authentication

When using LDAP authentication, it is possible to deny login for some users. The NTLM HTTP authentication module (as well as the Windows domain credentials authentication module) does not have such functionality, so it can be possible for some users to log in using Windows domain account even if they are not allowed to log in via LDAP. To solve this problem, you should enable the `Allow creating new users on the first login` option for the corresponding authentication module.

With this property set, a user will be able to log in via their NT domain account only if he/she already has an existing account in TeamCity (i.e. if he/she has already logged into TeamCity earlier via LDAP) with a TeamCity username which equals the Windows domain username or a custom NT domain username specified on the user's profile page.

## Configuring client

Depending on your environment, you may need to configure your client to make NTLM authentication work.

#### Internet Explorer

1. Open __Tools | Internet Options__.
2. On the __Advanced__ tab make sure the option __Security | Enable Integrated Windows Authentication__ is checked.
3. On the __Security__ tab select __Local Intranet | Sites | Advanced__ and add your TeamCity server URL to the list.

#### Google Chrome

On Windows, Chrome normally uses IE's behaviour, see more information [here](http://dev.chromium.org/developers/design-documents/http-authentication).

#### Mozilla Firefox
1. Type `about:config` in the browser's address bar.
2. Add your TeamCity server URL to the [network.automatic-ntlm-auth.trusted-uris](http://kb.mozillazine.org/Network.automatic-ntlm-auth.trusted-uris) property.

[//]: # (Internal note. Do not delete. "NTLM HTTP Authenticationd226e165.txt")    

## Troubleshooting

Helpful links:
* [http://waffle.codeplex.com/wikipage?title=Frequently%20Asked%20Questions](http://waffle.codeplex.com/wikipage?title=Frequently%20Asked%20Questions)
* [http://waffle.codeplex.com/discussions/254748](http://waffle.codeplex.com/discussions/254748)
* [http://waffle.codeplex.com/wikipage?title=Troubleshooting%20Negotiate&amp;referringTitle=Documentation](http://waffle.codeplex.com/wikipage?title=Troubleshooting%20Negotiate&amp;referringTitle=Documentation)

__ __