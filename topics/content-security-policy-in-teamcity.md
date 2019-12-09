[//]: # (title: Content Security Policy in TeamCity)
[//]: # (auxiliary-id: Content Security Policy in TeamCity)

TeamCity implements additional HTTP security with the [Content-Security-Policy](https://content-security-policy.com/) (CSP) header. 

The header prohibits TeamCity pages from downloading external resources, with some whitelisted exceptions. Downloading from non-whitelisted resources will be blocked.

In some setups, you may need to allow downloading external resources. For example, when using analytics tools or when integrating TeamCity with external services via a plugin.

As a plugin developer, you can provide CSP directives via the [`ContentSecurityPolicyConfig`](http://javadoc.jetbrains.net/teamcity/openapi/current/jetbrains/buildServer/web/ContentSecurityPolicyConfig.html) OpenAPI interface.

As a server administrator, you can change the CSP header value via the [internal properties](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties):

- for TeamCity administration pages:   
    ```Plain Text
    teamcity.web.header.Content-Security-Policy.adminUI.protectedValue=<full_header_value>
 
    ```
- for other TeamCity pages:   
    ```Plain Text
    teamcity.web.header.Content-Security-Policy.protectedValue=<full_header_value>
 
    ```

<note>

The property must contain the full value of the CSP header, so change it with _extra caution_.

</note>

#### Adding Google Analytics via internal properties

For example, to allow Google Analytics you must change the values of the following directives in the CSP header:
 

* [`connect-src`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/connect-src) to allow loading Google Analytics URLs:

     ```Resource
     connect-src 'self' ws: wss: https://www.google-analytics.com www.google-analytics.com https://stats.g.doubleclick.net
  
     ```

 
* [`img-src`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/img-src) to allow loading images:    

     ```Resource
     img-src 'self' data: https://www.google-analytics.com www.google-analytics.com https://stats.g.doubleclick.net;
  
     ```
 
* [`script-src`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/script-src) to allow loading JavaScript:

     ```Resource
     script-src 'self' 'unsafe-inline' 'unsafe-eval' https://www.google-analytics.com
  
     ```

The internal properties must be set as follows:

```Resource
# For TeamCity administration pages:
teamcity.web.header.Content-Security-Policy.adminUI.protectedValue=frame-ancestors 'self'; default-src 'self' 'unsafe-inline'; script-src 'self' 'unsafe-inline' 'unsafe-eval' https://www.google-analytics.com; img-src 'self' data: https://www.google-analytics.com www.google-analytics.com https://stats.g.doubleclick.net; connect-src 'self' ws: wss: https://www.google-analytics.com www.google-analytics.com https://stats.g.doubleclick.net

# For other pages:
teamcity.web.header.Content-Security-Policy.protectedValue=frame-ancestors 'self'; default-src 'self' 'unsafe-inline'; script-src 'self' 'unsafe-inline' 'unsafe-eval' https://www.google-analytics.com; img-src 'self' data: https://www.google-analytics.com www.google-analytics.com https://stats.g.doubleclick.net; connect-src 'self' ws: wss: https://www.google-analytics.com www.google-analytics.com https://stats.g.doubleclick.net

```

__ __