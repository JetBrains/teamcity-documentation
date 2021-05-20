[//]: # (title: REST API)
[//]: # (auxiliary-id: REST API)

>This document is no longer supported. Please see the new version of the REST API documentation [here](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html).
>
{type="warning"}

TeamCity provides a REST API for integrating external applications and creating script interactions with the TeamCity server.

REST API is an open-source [plugin](https://github.com/JetBrains/teamcity-rest) bundled __since TeamCity 5.0__.

TeamCity's REST API allows accessing resources (entities) via URL paths. To use the REST API, an external application makes an HTTP request to the TeamCity server and parses the response.

This page describes principles of the REST API authentication, the structure of requests, and supported HTTP methods.

Refer to the __[REST API Reference](rest-api-reference.md)__ page for a list of the most used data entity requests.   
See also [request examples](rest-api-reference.md#Request+Examples).
 
## General Usage Principles

<note>

Make sure you read through this section before using the API.

</note>
 
This documentation is not a complete guide and will only provide some initial knowledge useful for working with the API.

The URL examples on this page assume that your TeamCity server web UI is accessible via the [`http://teamcity:8111`](http://teamcity:8111) URL.
 
You can start by opening [`http://teamcity:8111/app/rest/server`](http://teamcity:8111/app/rest/server) URL in your browser: this page will give you several pointers to explore the API.
 
Use [`http://teamcity:8111/app/rest/application.wadl`](http://teamcity:8111/app/rest/application.wadl) to get the full list of supported requests and names of parameters. This is the primary source of discovery for the supported requests and their parameters. The same data is also exposed in the [Swagger](https://swagger.io/tools/open-source/getting-started/) format via the [`http://teamcity:8111/app/rest/swagger.json`](http://teamcity:8111/app/rest/swagger.json) endpoint.
 
For the list of supported [locator dimensions](#Locator) included into the error response, use the `$help` locator.
 
Experiment and read the error messages returned: for the most part they should guide you to the right requests.
 
Suppose you want to know more on the agents and see (in the `/app/rest/server` response) that there is an `/app/rest/agents` URL.
* Try the `/app/rest/agents/` request: see the authorized agent list, get the `default` way of linking to an agent from the agent's element `href` attribute.
* Get individual agent details via the `/app/rest/agents/id:10` URL (obtained from `href` for one of the elements of the previous request).
* If you send a request to `/app/rest/agents/$help`, or `/app/rest/agents/aaa:bbb` (supplying unsupported locator dimension), you will get the list of the supported dimensions to find an agent via the agent's [locator](#Locator).
* Most of the attributes of the returned agent data (`name`, `connected`, `authorized`) can be used as the `<field_name>` in the `app/rest/agents/<agentLocator>/<field_name>` request. Moreover, if you issue a request to the `app/rest/agents/id:10/test` URL, you will get a list of the supported fields in the error message.
 
## REST Authentication

You can authenticate in the REST API in the following ways:
* The preferred way to access REST API is by using the [token-based HTTP authentication](configuring-authentication-settings.md#Token-Based+Authentication). Provide your personal TeamCity access token generated on __My Settings &amp; Tools | Access Tokens__ in the HTTP header `Authorization: Bearer <token-value>`. For example:
    ```Shell
    curl --header "Authorization: Bearer <token-value>" http://teamcity:8111/app/rest/builds
    ```
* Using basic HTTP authentication (it can be slow with certain authentications, see below). Provide a valid TeamCity username and password with the request. You can force basic auth by including `httpAuth` before the `/app/rest` part, e.g. [`http://teamcity:8111/httpAuth/app/rest/builds`](http://teamcity:8111/httpAuth/app/rest/builds).
* Using access to the server as a [guest user](guest-user.md) (if enabled) include `guestAuth` before the `/app/rest` part, e.g. [`http://teamcity:8111/guestAuth/app/rest/builds`](http://teamcity:8111/guestAuth/app/rest/builds).
* If you are checking REST `GET` requests from within a browser, and you are logged in to TeamCity in the browser, you can just use `/app/rest` URL, e.g. [`http://teamcity:8111/app/rest/builds`](http://teamcity:8111/app/rest/builds).

Authentication can be slow when a basic HTTP authentication with a non-built-in module is used. If you want to use basic HTTP authentication instead of token-based one, consider applying the [session reuse approach](http://youtrack.jetbrains.com/issue/TW-14209#comment=27-485445) for reusing authentication between sequential requests.
 
If you perform a request from within a TeamCity build, for a limited set of build-related operations (like downloading artifacts) you can use values of [`teamcity.auth.userId/teamcity.auth.password`](artifact-dependencies.md#Build-level+authentication) system properties as basic credentials (within TeamCity settings you can reference them as `%system.teamcity.auth.userId%` and `%system.teamcity.auth.password%`).
 
Within a build, the request for current build details can look like:
 
```Shell
curl -u "%system.teamcity.auth.userId%:%system.teamcity.auth.password%" "%teamcity.serverUrl%/httpAuth/app/rest/builds/id:%teamcity.build.id%"
 
```
 
## Superuser access
 
You can use the [super user account](super-user.md) with REST API: just provide no username and the generated password logged into the server log.
 
## REST API Versions
 
As REST API evolves from one TeamCity version to another, there can be incompatible changes in the protocol.
 
Under the [`http://teamcity:8111/app/rest/`](http://teamcity:8111/app/rest/) or [`http://teamcity:8111/app/rest/latest`](http://teamcity:8111/app/rest/latest) URL the latest version is available.

Earlier versions might be available under the [`http://teamcity:8111/app/rest/<version>`](http://teamcity:8111/app/rest/<version>) URL. Our general policy is to supply TeamCity with at least one previous version. The REST API protocol version is the TeamCity version where this protocol was first introduced.   
The latest legacy version of the protocol is 2018.1: use `2018.1` instead of `<version>` to access it. Other bundled versions are: `2017.2`, `2017.1`, `10.0`.
 
Breaking changes in the API are described in the related [Upgrade Notes](upgrade-notes.md) section. Note that additions to the objects returned (such as new XML attributes or elements) are not considered major changes and do not cause the protocol version to increment. Also, the endpoints marked with `Experimental` comment in `application.wadl` may change without a special notice in future versions.
 
<note>
 
The examples on this page use the `/app/rest` relative URL; replace it with the one containing the version if necessary.
</note>
 
## URL Structure
 
The general structure of the URL in the TeamCity API is [`http://teamcityserver:port/<authType>/app/rest/<apiVersion>/<restApiPath>?<parameters>`](http://teamcityserver:port/<authType>/app/rest/<apiVersion>/<restApiPath>?<parameters>)
 
where
* `teamcityserver` and `port` define the server name and the port used by TeamCity. This page uses [`http://teamcity:8111/`](http://teamcity:8111/) as an example URL.
* `<authType>` (optional) is the [authentication type](#REST+Authentication) to be used, this is generic TeamCity functionality.
* `app/rest` is the root path of the TeamCity REST API
* `<apiVersion>` (optional) is a reference to a specific version of REST API.
* `<restApiPath>?<parameters>` is the REST API part of the URL.  
A typical way to get multiple items is to use `<restApiPath>` in the form of `.../app/rest/<items>` (for example, `.../app/rest/builds`). These URLs regularly accept the [locator](#Locator) parameter which can filter the items returned. Individual items can regularly be addressed by a URL in the form of `.../app/rest/<items>/<item_locator>`. This URL always returns a single item. If the `<item_locator>` locator matches several items, the first one is returned. Both multiple and single items requests regularly support the [fields](#Full+and+Partial+Responses) parameter.
 
### Locator
 
In a number of places, you can specify a filter string which defines what entities to filter/affect in the request. This string representation is referred to as _locator_ in the scope of the TeamCity REST API.
 
The locators formats can be:
* single value: text without the following symbols: `,:-( )`
* dimension allowing you to filter entities using multiple criteria: `<dimension1>:<value1>,<dimension2>:<value2>,<dimension3>:(<dimension3.1>:<value3.1>,<dimension3.2>:<value3.2>)`   
Note that nested locators should be enclosed in parentheses. See entity descriptions in the [REST API reference](rest-api-reference.md) for the most popular locator descriptions. If in doubt what a specific locator supports, send a request with "$help" as the locator value. In the response, you will get a textual description of what the locator supports. If a request with invalid locators is sent, the error messages often hint at the error and list the supported locator dimensions as well.
 
<note>
 
If the single value contains the `,` symbol, it should be enclosed into parentheses: `(<value>)`. The value of a dimension can also be encoded as Base64url ("URL and Filename safe type base64" from RFC4648) and sent as `<dimension>:($base64:<base64-encoded-value>)` instead of `<dimension>: <value>`.
</note>
 
Examples:
 
* Use [`http://teamcity:8111/app/rest/projects`](http://teamcity:8111/app/rest/projects) to get the list of projects.
* Use [`http://teamcity:8111/app/rest/projects/<projectsLocator>`](http://teamcity:8111/app/rest/projects/<projectsLocator>), for example [`http://teamcity:8111/app/rest/projects/id:RESTAPIPlugin`](http://teamcity:8111/app/rest/projects/id:RESTAPIPlugin) (example ID is used) to get the full data for the REST API Plugin project.
* Use [`http://teamcity:8111/app/rest/buildTypes/id:bt284/builds?locator=<buildLocator>`](http://teamcity:8111/app/rest/buildTypes/id:bt284/builds?locator=<buildLocator>), for example [`http://teamcity:8111/app/rest/buildTypes/id:bt284/builds?locator=status:SUCCESS,tag:EAP`](http://teamcity:8111/app/rest/buildTypes/id:bt284/builds?locator=status:SUCCESS,tag:EAP) (example IDs are used) to get builds.
* Use [`http://teamcity:8111/app/rest/builds/?locator=<buildLocator>`](http://teamcity:8111/app/rest/builds/?locator=<buildLocator>) to get builds by build locator.
 
### Supported HTTP Methods

* __`GET`__: retrieves the requested data. For example, `.../app/rest/entities` usually retrieves a list of entities, `.../app/rest/entities/<entity locator>` retrieves a single entity
* __`POST`__: creates the entity from the payload submitted. To create a new entity, one regularly needs to post a single entity data (i.e. as retrieved via GET) to the `.../app/rest/entities` URL. When posting XML, be sure to specify the `Content-Type: application/xml` HTTP header.
* __`PUT`__: updates the data from the payload submitted. Accepts the same data format as retrieved via GET request to the same URL. Supported for some entities for URLs like `.../app/rest/entities/<entity locator>`
* __`DELETE`__: removes the data at the URL, for example, for the `.../app/rest/entities/<entity locator>`
 
## Logging
 
You can get details on errors and REST request processing in the `logs\teamcity-rest.log` [server log](teamcity-server-logs.md).

If you get an error in response to your request and want to investigate the reason, look into [rest-related server logs](#Logging).
 
To get details about each processed request, turn on debug logging (for example, set Logging Preset to `debug-rest` on the [Administration/Diagnostics](teamcity-monitoring-and-diagnostics.md#Debug+Logging) page or modify the Log4J `jetbrains.buildServer.server.rest` category).
 
## CORS Support
{id="CORS-support" auxiliary-id="CORS Support"}
 
The TeamCity REST API can be configured to allow [cross-origin requests](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing) using the `rest.cors.origins` [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties).
 
To allow requests from a page loaded from a specific domain, add the page address (including the __protocol and port__, __wildcards are not supported__) to the comma-separated [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties) `rest.cors.origins`. For example, `rest.cors.origins=http://myinternalwebpage.org.com:8080,https://myinternalwebpage.org.com`.

<note>

Since version 2020.1, TeamCity uses CSRF tokens to improve the security of REST API integration mechanisms. This change will affect the behavior of your existing custom integration scripts __only__ if you had enabled the `rest.cors.origins` internal property and if these scripts rely on CORS in writing operations. Refer to [upgrade notes](upgrade-notes.md#Limitation+of+CORS+support+for+writing+operations) for details.

</note>
 
To enable support for a [preflight OPTIONS request](https://youtrack.jetbrains.com/issue/TW-27606):
1. Add the `rest.cors.optionsRequest.allowUnauthorized=true` [internal property](configuring-teamcity-server-startup-properties.md#TeamCity+internal+properties).
2. __Restart__ the TeamCity server.
3. Use the `/app/rest/latest` URL for the requests.  Do not use `/app/rest`, do not use the `httpAuth` prefix.
If that does not help, enable debug [logging](#Logging) and look for related messages. If there are none, capture the browser traffic and messages to investigate the case.
 
## API Client Recommendations
 
When developing a client using the REST API, consider the following recommendations:
* Make the root REST API URL configurable (for example, allow specifying an alternative for `app/rest/<version>` part of the URL). This will allow directing the client to another version of the API if necessary.
* Ignore (do not error out) item's attributes and sub-items which are unknown to the client. New sub-items are sometimes added to the API without version change and this will ensure the client is not affected by the change.
* Set large (and make them configurable) request timeouts. Some API calls can take minutes, especially on a large server.
* Use HTTP sessions to make consecutive requests (use the `TCSESSIONID` cookie returned from the first authenticated response instead of supplying raw credentials all the time). This saves time on authentication which can be significant for external authentication providers.
* Beware of partial answers when requesting list of items: some requests are paged by default. Value of the `count` attribute in the response indicate the number of the items on the current page and there can be more pages available. If you need to process more (or all) items, read and process the `nextHref` attribute of the response entity for items collections. If the attribute is present it means there might be more items when queried by the URL provided. Related locator dimensions are `count` (page limit) and `lookupLimit` (depth of search). Even when the returned `count` is 0, it does not mean there are no more items if there is the "nextHref" attribute present.
* Do not increase the `lookupLimit` value in the locators without a second thought. Doing so has the direct effect of loading the server more and may require increased amounts of CPU and memory. It is assumed that those increasing the default limit understand the negative consequences for the server performance.
* Do not abuse the ability to execute automated requests for TeamCity API: do not query the API too frequently and restrict the data requested to only that necessary (using due [locators](#Locator) and specifying necessary [fields](#Full+and+Partial+Responses)). Check the server behavior under load from your requests. Make sure not to repeat the request frequently if it takes time to process the request.

## Response Formats
 
The TeamCity REST APIs returns HTTP responses in the following formats according to the HTTP "Accept" header:
 
<table>
 
<tr>
 
<td>
Format
 
</td>
 
<td>
 
Response Type
 
</td>
 
<td>
 
HTTP "Accept" header value
 
</td>
 
</tr>
 
<tr>
 
<td>
 
plain text
 
</td>
 
<td>
 
single-value responses
 
</td>
 
<td>
text/plain
 
</td></tr><tr>
 
<td>
 
XML
 
</td>
 
<td>
 
complex value responses
 
</td>
 
<td>
 
application/xml
 
</td>
 
</tr>
 
<tr>
 
<td>
JSON
 
 
</td>
 
<td>
complex value responses
 
 
</td>
 
<td>
application/json
 
 
</td></tr></table>
 
### Full and Partial Responses
 
By default, when a list of entities is requested, only basic fields are included into the response. When a single entry is requested, all the fields are returned. The complex field values can be returned in full or basic form, depending on a specific entity.
 
It is possible to change the set of fields returned for XML and JSON responses for the majority of requests.This is done by supplying the __fields__ request parameter describing the fields of the top-level entity and sub-entities to return in the response. An example syntax of the parameter is: `field,field2(field2_subfield1,field2_subfield1)`. This basically means "include `field` and `field2` of the top-level entity and for `field2` include `field2_subfield1` and `field2_subfield1` fields". The order of the fields specification plays no role.
 
Examples:

```Shell
http://teamcity.jetbrains.com/app/rest/buildTypes?locator=affectedProject:(id:TeamCityPluginsByJetBrains)&fields=buildType(id,name,project)

```

```Shell
http://teamcity.jetbrains.com/app/rest/builds?locator=buildType:(id:bt345),count:10&fields=count,build(number,status,statusText,agent,lastChange,tags,pinned)
 
```
 
At this time, the response can sometimes include the fields/elements not specifically requested. This can change in the future versions, so it is recommended to specify all the fields/elements used by the client.
