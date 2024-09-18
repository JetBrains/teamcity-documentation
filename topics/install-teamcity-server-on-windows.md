[//]: # (title: Install TeamCity Server on Windows)
[//]: # (auxiliary-id: Install TeamCity Server on Windows)

>Before installing the TeamCity server, you might want to estimate your [system requirements](system-requirements.md) and read about [supported platforms](supported-platforms-and-environments.md).

> Since TeamCity does not require any elevated permissions, it is recommended to run the server under a regular user account (you can configure a dedicated account for TeamCity). Running the server under an Administrator/SYSTEM account is considered a security threat since potential attackers who exploit possible TeamCity vulnerabilities can gain unwanted access to your server machine.
> 
{style="warning"}

## Download TeamCity Server

Go to the [JetBrains website](https://www.jetbrains.com/teamcity/download/) and download a convenient TeamCity Server distribution:
* __.exe__: executable which provides the installation wizard for Windows platforms and allows installing the server as a Windows service.
* __.tar.gz__: archive with a "portable" version.

Or, you can install it from a __Docker image__. All the information related to the TeamCity Server Docker images is described on [Docker Hub](https://hub.docker.com/r/jetbrains/teamcity-server/).

The `.exe` and `.tar.gz` have the following specifics:
* They include a Tomcat version tested to work fine with the respective version of TeamCity. You can use an [alternative Tomcat version](how-to.md#Install+Non-Bundled+Version+of+Tomcat), but other combinations are not guaranteed to work correctly.
* On Windows, they provide better error reporting for some scenarios (like a missing Java installation).
* It is possible to configure the installation by changing the startup script and JRE options.
* They come bundled with a build agent distribution and a startup script which allows for easy TeamCity server evaluation with one agent.
* They come bundled with the devPackage for the [TeamCity plugin development](https://plugins.jetbrains.com/docs/teamcity/developing-teamcity-plugins.html).

## Install from Executable File

To install the TeamCity server, run the executable (`.exe`) file and follow the installation instructions.

The installation wizard will prompt you to install the TeamCity server and one build agent that can be run as a Windows service. If you opted to install the services, you can use the standard Windows _Services_ app to manage the service. Otherwise, use standard [scripts](start-teamcity-server.md).

Make sure the user account specified for the service has:
* the [log on as service](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc794944(v=ws.10)?redirectedfrom=MSDN) right
* write permissions for the [TeamCity Data Directory](teamcity-data-directory.md)
* write permissions for the [TeamCity Home Directory](teamcity-home-directory.md) (a directory where TeamCity has been installed)
* all the permissions necessary to work with external systems like VCSs

By default, the Windows service is installed under the `SYSTEM` account. To change it, use the Services applet (__Control Panel | Administrative Tools | Services__).

If you do not change the default port (`8111`) during the installation, the TeamCity UI will be accessed via [`http://localhost/`](http://localhost/){nullable="true"} in a web browser running on the same machine where the server is installed. Note that port 8111 can be already occupied by other programs. In this case, you can specify another port during the installation and use [`http://localhost:<port>/`](http://localhost:<port>/){nullable="true"} address in the browser.

If you want to edit the TeamCity server's service parameters, memory settings, or system properties after the installation, refer to [this article](server-startup-properties.md).

## Install from tar.gz

It is highly recommended running the TeamCity server under a dedicated user account.

To install the TeamCity server, unpack the `TeamCity<version number>.tar.gz` archive. You can use the `tar xfz TeamCity<version number>.tar.gz` command under Linux \* and WinZip, WinRar, or similar utility under Windows.

>\* We advise using GNU tar or the `gtar xfz` command for unpacking. For example, Solaris 10 tar is reported to truncate long file names and may cause `ClassNotFoundException` when using the server after unpacking.

Make sure that JRE or JDK are installed and the `JAVA_HOME` environment variable is pointing to the Java installation directory (see [recommended Java versions](supported-platforms-and-environments.md#TeamCity+Server)).