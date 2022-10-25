[//]: # (title: Configuring HTTPS Access to TeamCity Server)
[//]: # (auxiliary-id: Configuring HTTPS Access to TeamCity Server)

TeamCity lets you easily configure HTTPS access to the server.

## Configuring HTTPS Settings

To enable HTTPS access to your server, you need an SSL certificate and a private key. 
TeamCity supports `.pem` certificates and RSA keys of the `PKCS#8` format. 

If you do not have a certificate, there are a couple of options available to you: 

- For a public facing server, you can generate a new trusted certificate. One of the possibilities is [Certbot](https://certbot.eff.org/pages/about). 
Please follow [these instructions](https://certbot.eff.org/instructions).

- For a non-public facing server, you can use an existing certificate or generate a new one following these instructions (TODO). 
 
If you use a self-signed certificate, make sure your clients are [configured to trust it](using-https-to-access-teamcity-server.md#Accessing+the+server+via+HTTPS).

When a certificate is available, configure HTTPS settings on your TeamCity server.

1. Go to **Administration | Server Administration | HTTPS Settings**. The settings can be configured via the UI or a script.
2. Upload the certificate.
3. Upload the RSA key.
4. Now you can specify the port for HTTPS connections. By default, TeamCity suggests port 443. You may need to change the port number.

<tip>

Make sure that the specified port is not already occupied and the TeamCity server process has access to it. For example, if you’re running the TeamCity server on Unix under an unprivileged account for security reasons, Unix allows access to ports below 1024 to the root user only.

</tip>

Before saving the specified settings, TeamCity will check access. If access is denied, TeamCity will show an error and will not save the invalid settings.


You can also automate configuring these HTTPS settings using a script, which should contain the following:

```Shell
curl '&lt;TeamCity_URL>/app/https/settings/uploadCertificate' -X POST -H 'Accept: application/json' -H 'Authorization: Bearer TOKEN' -F certificate=@PATH_CERT -F key=@PATH_KEY -F port=XXXX'`
```

The `TOKEN` here is your [personal token](configuring-your-user-profile.md#Managing+Access+Tokens) with the `Change HTTPS settings` permission.

5. Save your settings. 
  TeamCity will track the validity of your certificate and will start displaying a warning 30 days before expiration. After the certificate expires, an error will be displayed. 
  
>In a multi-node server configuration, each node’s settings are independent and you’ll need to upload a certificate to each of the nodes. 


6. After the settings are saved, you’ll be presented with a list of HTTPS redirect options. Select one of the radio buttons:
* **Disabled** (default). HTTPS redirection is not enforced and all clients can use both HTTP and HTTPS. This is the least secure option.
* **Only browser requests**. Use this option to force all users connecting via a browser to use HTTPS. Requests from agents and custom scripts will not be redirected. 
* This option can be suitable if you have a secure isolated infrastructure, or if you only have local agents connecting to the TeamCity server. 
* It is also useful for a transition period, when you can [configure your agents](how-to.md#Configure+TeamCity+Agent+to+Use+Proxy+To+Connect+to+TeamCity+Server) to connect to TeamCity via HTTPS.
* **Enable for all requests**. All TeamCity clients will be redirected to HTTPS. 

 > **Before enabling this option, make sure that all agents and custom scripts support redirection**. If any of TeamCity clients do not support redirects, their setting must be updated. All agents as well as the images for cloud agents must be updated to version 2022.10 or later. The agents launched from an earlier version may not be able to connect to the server after the redirection.

After HTTPS settings are saved, the page will display the **Remove** button below the certificate information.

<warning>
Be warned that if you have HTTPS redirects configured, removing the certificate may lead to the complete inability to access your server.
</warning>



