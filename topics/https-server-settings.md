[//]: # (title: Configuring HTTPS Access to TeamCity Server)
[//]: # (auxiliary-id: Configuring HTTPS Access to TeamCity Server)

The HTTPS protocol uses encryption for secure server-client communication over computer networks. If your TeamCity server is available by a public internet address, it is strongly recommended that you configure the HTTPS connection to significantly enhance the security.

To configure secure HTTPS access, you need a certificate. You can obtain and load it manually, or let TeamCity automatically issue a valid certificate via **Let's Encrypt**.

> HTTPS settings made via TeamCity UI affect the built-in Tomcat server configuration.
>
> If your TeamCity server is [behind a proxy](configuring-proxy-server.md#Set+Up+TeamCity+Server+Behind+Proxy), configure HTTPS on the proxy side.
Modifying the settings via a web UI may break your existing proxy configuration.
>
{type="warning"}





## Fetch Certificates from Let's Encrypt
{id="fetch-certificates-from-lets-encrypt"}

[Let's Encrypt](https://letsencrypt.org) is a non-profit Certificate Authority (CA) that provides TLS certificates trusted by all modern browsers. TeamCity can contact this CA to automatically issue a certificate for both your TeamCity server domain and if configured, the [artifacts isolation domain](teamcity-configuration-and-maintenance.md#artifacts-domain-isolation).

Refer to this article to learn how Let's Encrypt validates your domain ownership and issues certificates: [How it Works](https://letsencrypt.org/how-it-works/).

> This option is not available for [multinode setups](multinode-setup.md).
> 
{type="note"}

### Technical Information

*Certificate type:*&emsp;Multi-Domain SAN certificate<br/>
*Certificate shelf life:*&emsp;90 days<br/>
*Automatic renewal:*&emsp;30 days before expiration<br/>
*Challenge type:*&emsp;[HTTP-01](https://letsencrypt.org/docs/challenge-types/)

### Automatic Fetch

1. Navigate to **Administration | HTTPS Settings** and select the **Fetch from Let's Encrypt** option. Since Let's Encrypt needs to access specific endpoints to verify that you own the server and artifact isolation domains, these domains must be accessible over the internet. If your [server URL](configuring-server-url.md) points to a "localhost" address, you will see a corresponding error message.

2. Click the corresponding link to read Let's Encrypt Terms of Service, and click **Agree and fetch**.
    
    <img src="dk-https-letsencrypt.png" width="706" alt="Obtain certificate via Lets Encrypt"/>
    
    After CA verifies your identity, valid certificates will be issued and installed automatically.

3. Choose the required [redirect mode](#HTTPS+Redirect+Modes).

### Troubleshooting

If Let's Encrypt cannot verify the domain ownership and issue a certificate (for example, if a server has no access to the domain root or the 80 port used to serve challenge files is closed), TeamCity puts the process on hold and displays a text file content and a path.

<img src="dk-letsencrypt-noaccess.png" width="706" alt="Certificate instructions for system administrators"/>

A domain administrator/owner should manually place this file under the given location and ensure it is reachable over the internet. Then you can return to the **Administration | HTTPS Settings** page and click the **Proceed** button.

### Automatic Certificate Renewal

Certificates issued by Let's Encrypt are valid for 90 days. TeamCity attempts to renew certificates 30 days before they expire automatically. You can set a different threshold via the `teamcity.https.close.expiration.threshold.<units>=value` [internal property](server-startup-properties.md#TeamCity+Internal+Properties):

```Plain Text
teamcity.https.close.expiration.threshold.minutes=60
teamcity.https.close.expiration.threshold.days=40
```

If TeamCity is unable to re-issue a certificate, a corresponding message is shown in the [health report](server-health.md).



## Generate and Load Certificates Manually

If you do not wish to let TeamCity request certificates from Let's Encrypt, obtain and manually upload an SSL certificate and a private RSA key.

* A certificate must be a `.pem` file.
* A private key must be in `PKCS#1` or `PKCS#8` format and non-encrypted.

  > If your key has a password, you can utilize OpenSSL to remove it:
  >
  > ```Plain Text
  > openssl pkcs8 -topk8 -nocrypt -in [original.key] -out [new.key]
  > ```
  >
  {type="tip"}


### How to Obtain a Certificate

* For a public-facing server, manually generate a free certificate from a trusted authority ([Let's Encrypt](https://letsencrypt.org), [ZeroSSL](https://zerossl.com), and others). For example, you can use [Certbot](https://certbot.eff.org/pages/about). Another option is to purchase a certificate from a commercial CA such as [DigiCert](https://www.digicert.com/tls-ssl/tls-ssl-certificates) or [GoDaddy](https://www.godaddy.com/web-security/ssl-certificate).
* For a non-public-facing server, you can use an existing certificate or generate a new one locally. For example, you can follow [these instructions](https://www.ssl.com/how-to/manually-generate-a-certificate-signing-request-csr-using-openssl). Note that if you use a self-signed certificate, make sure your clients are [configured to trust it](using-https-to-access-teamcity-server.md#Accessing+the+server+via+HTTPS).

### Upload Files

Once you obtain a certificate and a private key:

1. Go to **Administration | HTTPS Settings**.
2. Select the **Upload** option.
3. Upload both files.
4. Choose the port for HTTPS connections. By default, TeamCity suggests port 443. You may need to change the port number.
  
    > Ensure that the specified port is not already occupied and the TeamCity server process has access to it. For example, if you’re running the TeamCity server on Unix under an unprivileged account for security reasons, Unix allows access to ports below 1024 to the root user only.
    >
    {type="tip"}

5. Click **Apply files** to let TeamCity check if the server URL is accessible. If access is denied, TeamCity shows an error and ignores the invalid settings.
  
6. Save your settings.
   
7. Choose the required [redirect mode](#HTTPS+Redirect+Modes).

> * In a multi-node server configuration, each node’s settings are independent. Upload a certificate to each of the nodes individually.
> * TeamCity tracks the validity of your certificate and starts showing a warning 30 days before the expiration date. After the certificate expires, an error will be displayed.
> 
{type="note"}


### Configure HTTPS Settings via a Script

You can also automate configuring these HTTPS settings using a script, which should contain the following:

```Shell
curl '<TeamCity_URL>/app/https/settings/uploadCertificate' -X POST -H 'Accept: application/json' -H 'Authorization: Bearer TOKEN' -F certificate=@PATH_CERT -F key=@PATH_KEY -F port=XXXX'`
```

The `TOKEN` here is your [personal token](configuring-your-user-profile.md#Managing+Access+Tokens) with the `Change HTTPS settings` permission.


## HTTPS Redirect Modes

After you have correctly configured the HTTPS access, TeamCity allows you to select one of the following redirect options:

<img src="dk-https-redirects.png" width="706" alt="Available HTTPS Redirect Options"/>

* **Disabled** (default). All clients can use both HTTP and HTTPS requests. This is the least secure option.
* **Only browser requests**. All users connecting via a browser must use HTTPS. Requests from agents and custom scripts can use HTTP.
  * This option can be suitable if you have a secure, isolated infrastructure, or if you only have local agents connecting to the TeamCity server.
  * It is also helpful for a transition period, when you can [configure your agents](how-to.md#Configure+TeamCity+Agent+to+Use+Proxy+To+Connect+to+TeamCity+Server) to connect to TeamCity via HTTPS.
* **Enable for all requests**. All TeamCity clients are redirected to HTTPS.

  > Before enabling this option, **ensure that you have updated the URL to HTTPS on all agents**. Otherwise, the agents that use HTTP may not be able to connect to the server.
  > 
  > You can view the URLs used by the agents to connect to the TeamCity server on **Agents | Overview | Parameters Report** page using `teamcity.serverUrl` in the filter.

  > This option may lead to the complete inability to access your TeamCity server after you remove an uploaded certificate.
  >
  {type="warning"} 





## Specify Available Encryption Protocols
{hidden="true"}

The default protocol TeamCity uses to communicate with clients is **TLS Version 1.2**.

To set a list of available protocols or to force TeamCity to use one specific protocol, add the `teamcity.https.use.protocols` [internal property](server-startup-properties.md#TeamCity+Internal+Properties) and set it to a required value using the common Tomcat syntax. See this page's "protocols" attribute description to view available values: [The HTTP Connector](https://tomcat.apache.org/tomcat-8.5-doc/config/http.html#SSL_Support_-_SSLHostConfig).

```Plain Text
teamcity.https.use.protocols=TLSv1.3
```

After you modify this property, restart your TeamCity server for the change to take effect.

<img src="dk-tls-protocols.png" width="706" alt="TeamCity using the TLS 1.3 Protocol"/>
