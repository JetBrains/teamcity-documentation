[//]: # (title: TeamCity Home Directory)
[//]: # (auxiliary-id: TeamCity Home Directory)

The _TeamCity Home Directory_ or the _TeamCity Installation Directory_ is the directory where the TeamCity server application files and libraries have been unpacked when TeamCity was [installed](installing-and-configuring-the-teamcity-server.md#Installing+TeamCity+Server). The location of the TeamCity Home directory is defined when you install the TeamCity server. The default directory suggested by the Windows [installation package](installing-and-configuring-the-teamcity-server.md#Installing+TeamCity+Server) is `C:\TeamCity`; however, TeamCity can be installed into any directory.
{product="tc"}

The _TeamCity Home Directory_ or the _TeamCity Installation Directory_ is the directory where the TeamCity server application files and libraries are stored. For TeamCity Cloud instances, this directory is fully operated by the TeamCity team.
{product="tcc"}

## Important Files and Directories
{product="tc"}

* `TeamCity-readme.txt` – description of the directory
* `BUILD_<number>` – TeamCity server application build number
* `Uninstall.exe` – used to uninstall the currently installed server

* __`/bin`__ – contains executable binary files and scripts (only available in TeamCity `.tar.gz` and `.exe` distributions)
     * `runAll.bat` – batch script to start/stop the server and a build agent from the console under Windows
     * `runAll.sh` – shell script to start/stop the server and a build agent under Linux/Unix
     * `teamcity-server.bat` – batch script to start/stop the server only from the console under Windows
     * `teamcity-server.sh` – shell script to start/stop the server only under Linux/Unix
     * `maintainDB.cmd` – Windows Command line script to [back up, restore, and migrate](creating-backup-via-maintaindb-command-line-tool.md) the server data between different databases
     * `maintainDB.sh` – shell script to [back up, restore, and migrate](creating-backup-via-maintaindb-command-line-tool.md) the server data between different databases
* __`/buildAgent`__ – [Build Agent Home Directory](agent-home-directory.md)
* __`/conf`__ – contains all configuration files for the TeamCity server 
    * `server.xml` – the main server configuration file
* __`/devPackage`__ – the [bundled development package](https://plugins.jetbrains.com/docs/teamcity/bundled-development-package.html) that can be used to start developing TeamCity plugins.
* __`/jre`__ – the bundled JRE installation directory
* __`/licenses`__ – licenses for the [third-party libraries](third-party-license-agreements.md) distributed with TeamCity
* __`/logs`__ – contains the [TeamCity server logs](teamcity-server-logs.md)
* __`/temp`__ – temporary folder
* __`/webapps`__ – TeamCity web application data
* __`/work`__ – standard [Tomcat folder](http://tomcat.apache.org/tomcat-7.0-doc/config/host.html#Standard_Implementation) where Tomcat writes cache files for every page it serves
