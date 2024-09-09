[//]: # (title: Accessing Server by HTTP)
[//]: # (auxiliary-id: Accessing Server by HTTP)

<!--[//]: # (Internal note. Do not delete. "Accessing Server by HTTPd2e3.txt")-->

<warning>

Use the TeamCity [REST API](https://www.jetbrains.com/help/teamcity/rest/teamcity-rest-api-documentation.html) for accessing the server from scripts. This section is __obsolete__ and preserved for __backward-compatibility__ with previous TeamCity versions and for some specific functionality.

</warning>

The examples below assume that your server web UI is accessible via <a href="http://teamcity.jetbrains.com:8111/" nullable="true">http://teamcity.jetbrains.com:8111/</a> URL.

The TeamCity server supports basic HTTP authentication allowing users to access certain web server pages and perform actions from various scripts. Consult the manual for the client tool/library on how to supply basic HTTP credentials when issuing a request.

Use the valid TeamCity server username and password to use basic HTTP authentication. Appropriate user permissions are required to perform the actions.   

<tip>

You may want to [configure](using-https-to-access-teamcity-server.md) the server to use HTTPS as username and password are passed in insecure form during basic HTTP authentication.

</tip>

To force using a basic HTTP authentication instead of redirecting to the login page if no credentials are supplied, prepend a path in the usual TeamCity URL with `/httpAuth`. For example:

```Shell
http://teamcity.jetbrains.com:8111/httpAuth/action.html?add2Queue=MyBuildConf

``` 

The HTTP authentication can be useful when [downloading build artifacts](patterns-for-accessing-build-artifacts.md#Obtaining+Artifacts+from+a+Build+Script) and triggering a build.

If you have _Guest_ user enabled, it can be used to perform the action too. Use `/guestAuth` before the URL path to perform the action on Guest user behalf. For example:


```Shell
http://teamcity.jetbrains.com:8111/guestAuth/action.html?add2Queue=MyBuildConf

``` 

<tip>

Make sure the user used to perform the authentication (or _Guest_ user) has appropriate role to perform the necessary operation.

</tip>

## Triggering a Build From Script

<warning>


Use the TeamCity [REST API](https://www.jetbrains.com/help/teamcity/rest/edit-build-configuration-settings.html#Manage+Build+Triggers) to trigger a build. The section below is __obsolete__ and preserved for compatibility purposes only.

</warning>
 
To trigger a build, send the `HTTP POST` request for the URL performing basic HTTP authentication:

```Shell

http://<server address>/httpAuth/action.html?add2Queue=<build configuration ID>
```



Some tools (for example, [Wget](https://www.gnu.org/software/wget/)) support the following syntax for the basic HTTP authentication:


```Shell
http://<user name>:<user password>@<server address>/httpAuth/action.html?add2Queue=<build configuration Id>

``` 

Example:

```Shell
http://testuser:testpassword@teamcity.jetbrains.com:8111/httpAuth/action.html?add2Queue=MyBuildConf

```
 
You can trigger a build on a specific agent passing additional `agentId` parameter with the agent's ID. You can get the agent Id from the URL of the Agent's details page: __Agents | \<agent name\>__. For example, you can infer that agent's Id equals "2", if its details page has the following URL:

```Shell
http://teamcity.jetbrains.com:8111/agentDetails.html?id=2

```
 
To trigger a build on two agents at the same time, use the following URL:

```Shell
http://testuser:testpassword@teamcity.jetbrains.com:8111/httpAuth/action.html?add2Queue=MyBuildConf&agentId=1&agentId=2

```

To trigger a build on all enabled and compatible agents, use `allEnabledCompatible` as agent ID:

```Shell
http://testuser:testpassword@teamcity.jetbrains.com:8111/httpAuth/action.html?add2Queue=MyBuildConf&agentId=allEnabledCompatible

``` 

### Triggering a Custom Build

TeamCity allows you to trigger a build with customized parameters. You can select particular build agent to run the build, define additional properties and environment variables, and select the particular sources revision (by specifying the last change to include in the build) to run the build with. These customizations will affect only the single triggered build and will not affect other builds of the build configuration.

To trigger a build on a specific change inclusively, use the following URL:

```Shell
http://testuser:testpassword@teamcity.jetbrains.com:8111/httpAuth/action.html?add2Queue=MyBuildConf&modificationId=11112

```

`modificationId` — modification/change internal id which can be obtained from the web diff URL.

To trigger a build with custom parameters (system properties and environment variables), use:

```Shell
http://testuser:testpassword@teamcity.jetbrains.com:8111/httpAuth/action.html?add2Queue=MyBuildConf&name=<full property name1>&value=<value1>&name=<full property name2>&value=<value2>

```
 
Where `<full property name>` is a full property name with system./env. prefix or no prefix to define configuration parameter. Note that previous TeamCity versions used different syntax for this action. That syntax is still supported for compatibility reason, though.

To move build to the top of the queue, add the following to the query string: `&moveToTop=true`

To run a personal build, add `&personal=true` to the query string.

To run a build on a feature branch:

```Shell
http://testuser:testpassword@teamcity.jetbrains.com/httpAuth/action.html?add2Queue=bt10&branchName=master

```
