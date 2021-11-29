[//]: # (title: Install TeamCity Server on Linux or macOS)
[//]: # (auxiliary-id: Install TeamCity Server on Linux or macOS)

>Before installing the TeamCity server, you might want to estimate your [system requirements](system-requirements.md) and read about [supported platforms](supported-platforms-and-environments.md).

## Download TeamCity Server

Go to the [JetBrains website](http://www.jetbrains.com/teamcity/download/) and download the __.tar.gz distribution__ with the "portable" version of the TeamCity server.

Or, you can install it from a __Docker image__. All the information related to the TeamCity Server Docker images is described on [Docker Hub](https://hub.docker.com/r/jetbrains/teamcity-server/).

Specifics of the `.tar.gz` distribution:
* It includes a Tomcat version tested to work fine with the respective version of TeamCity. You can use an [alternative Tomcat version](how-to.md#Install+Non-Bundled+Version+of+Tomcat), but other combinations are not guaranteed to work correctly.
* It is possible to configure the installation by changing the startup script and JRE options.
* It comes bundled with a build agent distribution and a startup script which allows for easy TeamCity server evaluation with one agent.
* It comes bundled with the devPackage for the [TeamCity plugin development](https://plugins.jetbrains.com/docs/teamcity/developing-teamcity-plugins.html).

## Install from tar.gz

>We recommend running the TeamCity server under a dedicated user account.

It is highly recommended running the TeamCity server under a dedicated user account.

To install the TeamCity server, unpack the `TeamCity<version number>.tar.gz` archive. You can use the `tar xfz TeamCity<version number>.tar.gz` command under Linux \* and WinZip, WinRar, or similar utility under Windows.

>\* We advise using GNU tar or the `gtar xfz` command for unpacking. For example, Solaris 10 tar is reported to truncate long file names and may cause `ClassNotFoundException` when using the server after unpacking.

Ensure that JRE or JDK are installed and the `JAVA_HOME` environment variable is pointing to the Java installation directory (see [recommended Java versions](supported-platforms-and-environments.md#TeamCity+Server)).
