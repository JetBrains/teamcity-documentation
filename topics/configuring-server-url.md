[//]: # (title: Configuring Server URL)
[//]: # (auxiliary-id: Configuring Server URL)

The server URL configured in the Administration UI (on the __Administration | Global Settings__ page) is used by the server to generate links to the server  when the URL cannot be derived from any other parameter. These cases include Notifications (email, Jabber, and so on) and some other actions performed not within a web request. All generated links will be prefixed by this URL. 

Make sure the server is accessible by the URL specified.

In most cases TeamCity correctly autodetects its own URL and sets it as the __Server URL__. However, sometimes auto-detection is not possible/correct (for example, when the TeamCity server is running behind the Apache proxy). For such cases you can specify the server URL using one of the options: 
* on the __Administration |  Global Settings__ page  
* in the `<[TeamCity Data Directory](teamcity-data-directory.md)>/config/main-config.xml` file using the following format (no server restart is required after the change):

```Shell
    
<server rootURL="http://some.host.com:port">
</server>
        
```
