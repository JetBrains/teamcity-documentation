[//]: # (title: Using HTTPS to access TeamCity server)
[//]: # (auxiliary-id: Using HTTPS to access TeamCity server)

This document describes how to configure Java applications to use HTTPS for communicating with the server.

<note>

If you need to connect the TeamCity server to a service behind a self\-signed certificate (for example, Git) or if you need to connect a TeamCity agent to the TeamCity server using the self\-signed certificate, use [trusted certificates configuration](uploading-ssl-certificates.md).
</note>

We assume that you have [already configured HTTPS](how-to.md#Configure+HTTPS+for+TeamCity+Web+UI) in your TeamCity web server. The most common and recommended approach for this is to set up a reverse proxy server like Nginx or Apache that provides HTTPS access for HTTP\-only TeamCity server's Tomcat port. In the setup make sure that the reverse proxy has correct configuration as per [Set Up TeamCity behind a Proxy Server](how-to.md#Set+Up+TeamCity+behind+a+Proxy+Server) section.

## Accessing the server via HTTPS

__If your certificate is valid__ (i.e. it was signed by a well known Certificate Authority like Verisign), then TeamCity clients should work with HTTPS without any additional configuration. All you have to do is use `https://` links to the TeamCity server instead of `http://`.

__If your certificate is not valid (is self\-signed):__ (i.e. is not signed by a known Certificate Authority and likely to result in "PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target" error message)
* To enable HTTPS connections from the TeamCity [Visual Studio Addin](visual-studio-addin.md) and [Windows Tray Notifier](windows-tray-notifier.md), point your Internet Explorer to the TeamCity server using `https://` URL and import the server certificate into the browser. After that, the Visual Studio Addin and Windows Tray Notifier should be able to connect by HTTPS.
* To enable HTTPS connections from Java clients (TeamCity Agents, IntelliJ IDEA, Eclipse, and so on), see the [section below](#Configuring+JVM) for configuring the JVM installation used by the connecting application.

## Configuring JVM

### Configuring client JVM for trusting server certificate

__If your certificate is valid__ (that is it was issues and signed by a well-known Certificate Authority like Verisign), then the Java clients should work with HTTPS without any additional configuration. To use Let's Encrypt\-issued certificates, make sure to upgrade the JVM used by the client to the latest.

__If your certificate is not valid (is self\-signed):__

To enable HTTPS connections from Java clients, you need to install the server certificate (or your organization's certificate the server's certificate is signed by) into the JVM as a trusted certificate. These are generic Java application steps (not TeamCity\-specific):
* save the CA Root certificate of the server's certificate to a file in one of the [supported formats](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html#keytool_option_importcert) (the file is referred as &lt;cert file&gt; below). This can be done in a browser by inspecting certificate data and exporting it as Base64 encoded X.509 certificate.
* locate the JRE used by the process. The best way to get the path to the proper Java installation is to look up the command line of the running process. * If there is a JDK installed (like for IntelliJ IDEA), &lt;path to JRE installation&gt; should be &lt;path to used JDK&gt;/jre
 * For TeamCity agent or server installed under Windows, the default location for &lt;path to JRE installation&gt; is &lt;TeamCity installation path&gt;/jre
* import the server certificate into the default JRE installation keystore using JVM's `keytool` tool:


```Plain Text
keytool -importcert -file <cert file> -keystore <path to JRE installation>/lib/security/cacerts
```



By default, Java keystore is protected by password "`changeit`" which you need to type on prompt

* restart the JVM
### Configuring JVM for authentication with client certificate

If you have implemented a custom authentication based on client certificates in front of TeamCity server, you will need to make the JVM clients correctly authenticate with their certificate.

__Importing client certificate__

If you need to use a client certificate to access a server via https (for example, from IntelliJ IDEA, Eclipse or the build agents), you will need to add the certificate to the Java keystore and supply the keystore to the JVM used by the connecting process.

1\. If you have your certificate in a __p12__ file, you can use the following command to convert it to a Java keystore. Make sure you use `keytool` from JDK 1.6\-1.8: earlier versions may not understand p12 format.

```Plain Text
keytool -importkeystore -srckeystore <path to your .p12 certificate> -srcstoretype PKCS12 -srcstorepass <password of your p12 certificate> -destkeystore <path to keystore file>  -deststorepass <keystore password> -destkeypass <keystore password> -srcalias 1
```


This commands extracts the certificate with the alias "1" from your .p12 file and adds it to Java keystore You should know &lt;path to your .p12 certificate&gt; and &lt;password of your p12 certificate&gt; and you can provide new values for &lt;path to keystore file&gt; and &lt;keystore password&gt;.

Here, `keypass` should be equal to `storepass` because only `storepass` is supplied to the JVM, and if `keypass` is different, the following error may accur: "java.security.NoSuchAlgorithmException: Error constructing implementation (algorithm: Default, provider: SunJSSE, class: com.sun.net.ssl.internal.ssl.DefaultSSLContextImpl)".

__Importing root certificate to organize a chain of trust__

If your certificate is not signed by a trusted authority, you will also need to add the root certificate from your certificate chain to a trusted keystore and supply this trusted keystore to JVM.

2\. You should first extract the root certificate from your certificate. You can do this from a web browser if you have the certificate installed, or you can do this with the [OpenSSL](http://openssl.org/) tool using the command:


```Plain Text
openssl.exe pkcs12 -in <path to your .p12 certificate> -out <path to your certificate in .pem format>

```

You should know `<path to your .p12 certificate>` and its password (to enter it when prompted). You should specify new values for &lt;path to your certificate in .pem format&gt; and for the pem pass phrase when prompted.

3\. Then you should extract the root certificate (the root certificate should have the same issuer and subject fields) from the pem file (it has text format) to a separate file. The file should look like:


```Plain Text
-----BEGIN CERTIFICATE-----
MIIGUjCCBDqgAwIBAgIEAKmKxzANBgkqhkiG9w0BAQQFADBwMRUwEwYDVQQDEwxK
...
-----END CERTIFICATE-----

```



Let's assume its name is &lt;path to root certificate&gt;.

4\. Now import the root certificate to the trusted keystore with the command:


```Plain Text
keytool -importcert -trustcacerts -file <path to root certificate> -keystore <path to trust keystore file> -storepass <trust keystore password>
```

Here you can use new values for &lt;trust keystore path&gt; and &lt;trust keystore password&gt; (or use existing trust keystore).

<note>

In some scenarios, TeamCity builds may fail with the "_SSL certificate problem: unable to get local issuer certificate_" error __on Windows agents__ if they have the __`curl`__ library installed. This happens if only a self-signed certificate, but not a root certificate, is used by a VCS root of the current build configuration. To solve the problem, upload the [root certificate](#Configuring+JVM+for+authentication+with+client+certificate) as well.

</note>

__Starting the connecting application JVM__

Now you need to pass the following parameters to the JVM when running the application:


```Plain Text
-Djavax.net.ssl.keyStore=<path to keystore file>
-Djavax.net.ssl.keyStorePassword=<keystore password>
-Djavax.net.ssl.trustStore=<path to trust keystore file>
-Djavax.net.ssl.trustStorePassword=<trust keystore password>
```

For IntelliJ IDEA, you can add the lines into the `bin\idea.exe.vmoptions` file (one option per line). For the TeamCity build agent, see [agent startup properties](configuring-build-agent-startup-properties.md).

 

## Useful tools in analyzing certificates issues

Online HTTPS server configuration analysis: [https://www.ssllabs.com/ssltest/analyze.html](https://www.ssllabs.com/ssltest/analyze.html)[SSLPoke](https://gist.github.com/4ndrej/4547029) Java class

__ __