[//]: # (title: Configure Server Installation)
[//]: # (auxiliary-id: Configure Server Installation)

## Changing Server Port

If another application uses the same port as the TeamCity server, the server won't be able to start. This will result in "_Address already in use_" errors in the server logs or server console.

If you install a server from `.exe`, you can customize the port in the installation wizard.

To change the port of the installed/unpacked server, open the `<[TeamCity Home Directory](teamcity-home-directory.md)>/conf/server.xml` file and set another number in the not commented `<Connector>` XML node:

```XML
<Connector port="8111" ...

```

To apply the changes, [restart the server](start-teamcity-server.md) by.

If you change the port of an operational TeamCity server, you also need to change it in all the stored URLs of the server (browser bookmarks, agents' `serverUrl` [properties](configure-agent-installation.md), URL in users' IDEs, the _Server URL_ setting on the __Administration | Global Settings__ page).  
If you run another Tomcat server on the same machine, you might also need to change other service ports of the Tomcat server (search for `port=` in the `server.xml` file).

If you want to use the HTTPS protocol, it should be enabled separately. The process is specific to the web server used (by default, Tomcat). See notes on how to [configure HTTPS for TeamCity web UI](how-to.md#Configure+HTTPS+for+TeamCity+Web+UI).

## Changing Server Context

By default, the TeamCity server is accessible under the root context of the server address (for example, [`http://localhost:8111/`](http://localhost:8111/){nullable="true"}). To make it available under a nested path instead (for example, [`http://localhost:8111/teamcity/`](http://localhost:8111/teamcity/){nullable="true"}), you need to:
1. Stop the TeamCity server.
2. Rename the `<[TeamCity Home Directory](teamcity-home-directory.md)>\webapps\ROOT` directory to `<[TeamCity Home Directory](teamcity-home-directory.md)>\webapps\teamcity`.
3. [Start the TeamCity server](start-teamcity-server.md).

>After this change, [automatic update](upgrading-teamcity-server-and-agents.md#Automatic+Update) will be disabled for your installation and you will have to upgrade TeamCity [manually](upgrading-teamcity-server-and-agents.md#Manual+Update).
> 
{style="note"}

<anchor name="InstallingandConfiguringtheTeamCityServer-SettingUpMemorysettingsforTeamCityServer"/>

## Configure Memory Settings for TeamCity Server

TeamCity Server has the main process which can also launch child processes. Child processes use available memory on the machine. This section covers the memory settings of the main TeamCity server process only, as it requires special configuration.

As a JVM application, the TeamCity main server process utilizes only memory available to the JVM. The required memory depends on the JVM bitness (64- or 32-bit). The memory used by JVM usually consists of: _heap_ (configured via `-Xmx`) and _metaspace_ (limited by the amount of available native memory), internal JVM (usually tens of MB), and OS-dependent memory features like memory-mapped files. TeamCity mostly depends on the heap memory. To configure the heap memory size:

<tabs>

<tab title="On Windows">

1. Stop the TeamCity server.
2. Run the `sysdm.cpl` command and navigate to **Advanced | Environment Variables**.
3. Check if the `TEAMCITY_SERVER_MEM_OPTS` entry exists under the System variables section, and if it's not, add it.
4. Set this property to the `Xmx<Size>` value. For example, `TEAMCITY_SERVER_MEM_OPTS=-Xmx3g`. See the paragraph below for more examples.
5. Restart your server machine.
6. [Start TeamCity server](start-teamcity-server.md) after your server reboots.

</tab>


<tab title="On Linux and macOS">

1. Stop the TeamCity server.
2. Open the `/etc/environment` file.
3. Check if the `TEAMCITY_SERVER_MEM_OPTS` line exists in the file, and if it's not, add it.
4. Set this property to the `Xmx<Size>` value. For example, `TEAMCITY_SERVER_MEM_OPTS=-Xmx3g`. See the paragraph below for more examples.
5. Save the file and restart your server machine.
6. [Start TeamCity server](start-teamcity-server.md) after your server reboots.

</tab>

</tabs>


Possible values for the `TEAMCITY_SERVER_MEM_OPTS` variable:

* __Minimum setting__: `-Xmx1024m` for 64-bit Java (bundled), and `-Xmx750m` for 32-bit Java.
* __Recommended setting for medium server__: `-Xmx2048m` for 64-bit Java, `-Xmx1024m` for 32-bit Java. Greater settings with the 32-bit Java can cause `OutOfMemoryError` with "_Native memory allocation (malloc) failed_" JVM crashes or "Unable to create new native thread" messages.
* __Recommended setting for a large server__ (64-bit Java should be used): `-Xmx4g`. This setting should be suitable for an installation with up to two hundreds of agents and thousands of build configurations. Custom plugins might require increasing the value defined via the `Xmx` parameter.
* __Maximum setting for large-scale server__ (64-bit Java should be used): `-Xmx10g`. Greater values can be used for larger TeamCity installations. However, generally it is not recommended to use values greater than `10g` without consulting TeamCity support.

The `ReservedCodeCacheSize=640m` attribute is set by default for new server installations. 
If the attribute was specified before TeamCity 2022.04.4, you'll have to update it manually after upgrading.

If an `OutOfMemory` error occurs or you consistently see a memory-related warning in the TeamCity UI, it means you need to increase the setting to the next level.

>__Tips__:
>* A rule of thumb is that the 64-bit JVM should be assigned twice as much memory as the 32-bit for the same application. If you [switch to the 64-bit JVM](how-to.md#Update+from+32-bit+to+64-bit+Java) (for example, on upgrading TeamCity), make sure to adjust the memory setting (`-Xmx`) accordingly. It does not make sense to switch to 64-bit if you dedicate less than the double amount of memory to the application.
>* Large TeamCity installations might benefit from fine-tuning of the memory settings. The amount of memory dedicated to the TeamCity server JVM should not regularly exceed 60% of the total available physical memory on the machine (to allow for nested process and OS-level caches usage). Also, with heaps (`–Xmx`) set to more than 8 GB, if the machine has many CPU cores (for example, more than 8) and current CPU usage is below 60%, enabling G1 JVM garbage collector via the `-XX:+UseG1GC` JVM option might reduce the length of stop-the-world GC pauses.  
   > The recommended approach is to start with the default settings and monitor the used memory on the __Administration | Diagnostics__ page. If the server uses more than 80% of memory consistently without drops for tens of minutes, that is probably a signal to increase the `-Xmx` memory value by another 20%.

[//]: # (Internal note. Do not delete. "Installing and Configuring the TeamCity Serverd172e1122.txt")

## Configure TeamCity Data Directory

The default placement of the TeamCity Data Directory can be changed. See [this article](teamcity-data-directory.md) for details.

## Configuring Server for Production Use

An out-of-the-box TeamCity server installation is suitable for evaluation purposes. For production use, you will need to perform additional configuration. It typically includes these steps:
* Check that the server is using a [proper server port](#Changing+Server+Port) and configure [access via HTTPS](how-to.md#Configure+HTTPS+for+TeamCity+Web+UI).
* Make sure the TeamCity server URL and [email server](set-up-notifications.md#Email+Notifier) settings are correct.
* Configure the server process for OS-dependent [autostart](start-teamcity-server.md) on machine reboot.
* Use a reliable storage for [TeamCity Data Directory](teamcity-data-directory.md).
* Use an [external database](set-up-external-database.md).
* Configure [recommended memory settings](#Configure+Memory+Settings+for+TeamCity+Server). Use "maximum settings" for active or growing servers.
* Plan for regular [backups](teamcity-data-backup.md).
* Plan for regular [upgrades](upgrading-teamcity-server-and-agents.md) to the latest TeamCity releases.
* Consider adding the `teamcity.installation.completed=true` line into the `<[TeamCity Data Directory](teamcity-data-directory.md)>\conf\teamcity-startup.properties` file — this will prevent the server from creating an administrator user if no such user is found.
* Make sure to read the [notes on configuring the server for performance](system-requirements.md#Configuring+TeamCity+Server+for+Performance) and [security notes](security-notes.md).




