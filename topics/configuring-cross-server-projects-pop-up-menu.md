[//]: # (title: Configuring Cross-Server Projects Pop-up Menu)
[//]: # (auxiliary-id: Configuring Cross-Server Projects Pop-up Menu)

The TeamCity _Projects_ pop-up menu allows browsing projects and build configurations on multiple servers, so that when a project or a build configuration is selected, the page is opened on the right server.

>To see the Projects menu, click the arrow button next to the _Projects_ link in the upper left corner, or use the "P" keyboard shortcut.
>
{type="tip"}

The Projects pop-up menu uses separate REST API requests to get the list of projects to display. If peer servers are configured, REST API calls are made to them as well.   
For these REST API calls to work, the servers [need to be configured](rest-api.md#CORS-support) to allow CORS requests from all of their peers. Besides, they must be accessible from the user's browser and the user must be logged in on those servers.

## Configuring Projects Pop-up Menu

The dedicated UI on the __Administration | Nodes configuration__ page allows configuring linked servers feeding the Projects pop-up menu:
1. Specify the TeamCity server URL and click __Add Server__.
2. Click the test connection button. You have to be logged in to do that.

If the connection is successful, you will see the corresponding server node added to the Projects pop-up menu.

## CORS Configuration

Provided a pop-up menu on the server A is to display projects from the server B, Server B is required to have [CORS configured](rest-api.md#CORS-support) to trust all the URLs of server A which can be used by the users to access server A.

When configuring the pop-up menu on both servers, they need to have [CORS configured](rest-api.md#CORS-support) to trust all the URLs of one another. If a third server is added, it has to be added to the other two.

## Server Versions Compatibility

It is possible to configure the cross-server Projects pop-up menu between servers running different TeamCity versions.
