[//]: # (title: Installation Quick Start)
[//]: # (auxiliary-id: Installation Quick Start)

This page covers the __evaluation__ setup of a TeamCity server with a default build agent running on the same machine for the most popular operating systems.

To get a general idea of how to install TeamCity for evaluation, you can watch our video tutorial:

<video href="5Akqy-vEFr0"
       title="TeamCity Installation and initial setup"/>

For more details about the production setup, see the [Installation and Upgrade](installation-and-upgrade.md) section.

<note>

__[For production purposes](installing-and-configuring-the-teamcity-server.md#Configuring+Server+for+Production+Use)__, we recommend setting up the TeamCity server and agent on separate machines.
</note>

You can also use the [TeamCity Server](https://hub.docker.com/r/jetbrains/teamcity-server/) and [TeamCity Build Agent](https://hub.docker.com/r/jetbrains/teamcity-agent/) Docker images.


## Download TeamCity

__[Download](https://www.jetbrains.com/teamcity/download/) the free Professional Edition of TeamCity__, the full-featured TeamCity bundled with 3 build agents with a limit of 100 build configurations.   
After evaluation, you can __switch to the Enterprise Edition__: [Licensing Policy](licensing-policy.md) provides details. The pricing is available on the [JetBrains website](https://www.jetbrains.com/teamcity/buy/).

Earlier versions are available [here](previous-releases-downloads.md) page.

## Install TeamCity

### on Windows

Run the downloaded `.exe` file and follow the instructions of the TeamCity Setup wizard. The TeamCity web server and one build agent will be installed on the same machine.

During installation, you can configure:
* [TeamCity Home Directory](teamcity-home-directory.md) where TeamCity will be installed
* whether the TeamCity server and agent will run as Windows services    
   <img src="installAsWinServicepng.png" alt="TeamCity server and agent as Windows services"/>
   
* ports:      
   * Server port: `8111` is the default port, which can be already used by other applications (for example, Skype). Change the server port if it is already in use. In the example below, we've set the port to `7777`.
   * Agent port: `9090` is the default for incoming connections from the server. If this port is already in use, you'll be asked to change it by setting the `ownPort` property to a different value.   
   <img src="configure-agent-port.png" alt="Configure a build agent port" width="450"/>

If the TeamCity server is installed as a Windows service, follow the [usual procedure](https://bit.ly/2yJF87R) of starting and stopping services.

Otherwise, to start/stop the TeamCity server and one default agent at the same time, use the `runAll` script, provided in the `<TeamCity Home>\bin` directory:

* to __start__ the server and the default agent, use
    ```Shell
    .\runAll.bat start
    ```
* to __stop__ the server and the default agent, use
    ```Shell
    .\runAll.bat stop
    ```

If you did not change the default port (`8111`) during the installation, the TeamCity web UI can be accessed at [`http://localhost`](http://localhost/) in a web browser running on the same machine where the TeamCity server is installed. Otherwise, use `http://localhost:<port>` ([`http://localhost:8111`](http://localhost:8111/) in our case).

### on Linux and OS X

1. Make sure you have Java 8 installed (for example, use bundled [Amazon Corretto](https://aws.amazon.com/corretto/)).   
   Open a command-line terminal and run the following command:   
    ```Shell
    java -version
    ```
2. Make sure the `JAVA_HOME` environment variable is pointing to the Java installation directory. Run the following command in the command-line terminal:   
    ```Shell
    echo $JAVA_HOME
    ```
3. Use the `TeamCity<version number>.tar.gz` archive to unpack TeamCity bundled with Tomcat servlet container.   
   Unpack the archive. For example, under Linux use   
   ```Shell
   tar xfz TeamCity<version number>.tar.gz
   ```
   The archive can be used on Windows as well.   
4. After the archive is unpacked, the TeamCity web server and one build agent will be available on the current machine.   
__Note__ that for [production purposes](installing-and-configuring-the-teamcity-server.md#Configuring+Server+for+Production+Use) it is recommended to set up the TeamCity server and agent on separate machines.

__To start/stop the TeamCity server and one default agent at the same time__, use the `runAll` script, provided in the `<TeamCity Home>/bin` directory.

* to __start__ the server and the default agent, use
    ```Shell
    ./runAll.sh start
    ```
* to __stop__ the server and the default agent, use
    ```Shell
    ./runAll.sh stop
    ```

By default, TeamCity runs on [`http://localhost:8111/`](http://localhost:8111/) and has one registered build agent that runs on the same computer. If another application uses the same port as the TeamCity server, the TeamCity server (Tomcat server) will not start with the "_Address already in use_" errors in the server logs or server console.

To change the server port, locate the `<TeamCity Home>/conf/server.xml` and modify the port in the `<Connector>` XML node, for example:

```Shell
<Connector port="8111" ...>

```

## Start TeamCity for the First Time

On the first TeamCity start:
1. Review the [location of the TeamCity Data Directory](teamcity-data-directory.md#Configuring+the+Location), where all the configuration information is stored. Click __Proceed__.
2. TeamCity stores build history, users, build results, and some runtime data in an SQL database and allows you to select the database type.   
   For now, keep the default internal database. Click __Proceed__.   
   <img src="default-DB.png" alt="Select the database type" width="450"/>
   It will take some time for TeamCity to configure the necessary components.
3. On the next screen, accept the License Agreement to proceed with the launch. Click __Continue__.
4. TeamCity displays the __Create Administrator Account__ page. Specify the administrator credentials and click __Create Account__.  

You are signed in to TeamCity: now you can configure your user settings and [create and run your first build](configure-and-run-your-first-build.md).
