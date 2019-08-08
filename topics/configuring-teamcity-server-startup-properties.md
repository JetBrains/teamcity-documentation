[//]: # (title: Configuring TeamCity Server Startup Properties)
[//]: # (auxiliary-id: Configuring TeamCity Server Startup Properties)

Various aspects of TeamCity behavior can be customized through a set options passed on a TeamCity server start. These options fall into two categories: affecting Java Virtual Machine (JVM) and affecting TeamCity behavior.

<note>

You do not need to specify any of the options unless you are advised to do by the TeamCity support team or you are confident in what you are doing.
</note>

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## TeamCity internal properties 

TeamCity server has some configuration properties that are visible in the UI only to the Server Administrators. These are normally meant for debugging, changing internal constants or enabling an experimental behavior. Please do not change the internal properties unless asked by TeamCity support.

If you have internal properties customized, make sure to note this when you turn to the TeamCity support.

You can review and edit internal properties in the TeamCity web UI: go to the __Administration | Server Administration | Diagnostics__ page, select the __Internal Properties__ tab and click __Edit internal properties__.   
Many properties do not require the server restart, but some do.

The properties are stored in the `<`[`TeamCity Data Directory`](teamcity-data-directory.md)`>/config/internal.properties` file. The file is a Java [properties file](http://en.wikipedia.org/wiki/.properties). Create the file and add a required property `<property name>=<property value>` on a separate line.

An alternative but obsolete way to add an internal property is to pass it as a `-D<name>=<value>` JVM option (see the [section below](#JVM+Options)).

### JVM Options

If you need to pass additional JVM options to a TeamCity server (for example, `-D` options mentioned at [Reporting Issues](reporting-issues.md) or any non\-`-D` options like `-X...`), the approach will depend on the way the server is run.    
If you are using the `.war` distribution, use the manual of your Web Application Server.    
In all other cases, please refer to the next section.

For general notes on the memory settings, please refer to [Setting Up Memory settings for TeamCity Server](installing-and-configuring-the-teamcity-server.md).

You will need to [restart](installing-and-configuring-the-teamcity-server.md) the server for the changes to take effect.

### Standard TeamCity Startup Scripts

If you run the server using the `runAll` or `teamcity-server` scripts or as a Windows service, you need to set the options via the OS __[environment variables](http://en.wikipedia.org/wiki/Environment_variable)__ passed to the TeamCity server process:
* `TEAMCITY_SERVER_MEM_OPTS` – server JVM memory options (for example, `-Xmx750m`)
* `TEAMCITY_SERVER_OPTS` – additional server JVM options (for example, `-Dteamcity.git.fetch.separate.process=false`)

Make sure the environment variables are set for the user whose account is used to run TeamCity or as global environment variables. You might need to reboot the machine after the environment change for the changes to have effect.

 __  __

__See also:__


__Concepts__: [TeamCity Data Directory](teamcity-data-directory.md)  

__Administrator's Guide__: [Configuring Build Agent Startup Properties](configuring-build-agent-startup-properties.md) 
