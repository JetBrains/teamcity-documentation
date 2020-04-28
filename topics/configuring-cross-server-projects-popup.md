[//]: # (title: Configuring Cross-Server Projects Popup)
[//]: # (auxiliary-id: Configuring Cross-Server Projects Popup)

TeamCity Projects popup allows browsing projects and build configurations on multiple servers, so that when a project or a build configuration is selected, the page is opened on the right server.

The projects popup uses separate REST API requests to get the list of projects to display. If peer servers are configured, REST API calls are made to them as well.   
For these REST API calls to work, the servers [need to be configured](rest-api.md#CORS+Support) to allow CORS requests from all of their peers. Besides, they must be accessible from the user's browser and the user must be logged in on those servers.

## Configuring Popup

The dedicated UI on the __Administration | Nodes configuration__ page allows configuring linked servers feeding the projects popup:
1. Specify the TeamCity server URL and click __Add Server.__
2. Click the test connection button. You have to be logged in to do that.
If the connection is successful, you will see the corresponding server node added to the projects popup.

## CORS Configuration

Provided a popup on the server A is to display projects from the server B, Server B is required to have [CORS configured](rest-api.md#CORS+Support) to trust all the URLs of server A which can be used by the users to access server A.

When configuring the popup on both servers, they need to have [CORS configured](rest-api.md#CORS+Support) to trust all the URLs of one another. If a third server is added, it has to be added to the other two.

## Server Versions Compatibility

It is possible to configure the cross\-server projects popup between servers running different TeamCity versions.

__ __