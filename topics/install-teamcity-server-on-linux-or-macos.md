[//]: # (title: Install TeamCity Server on Linux or macOS)
[//]: # (auxiliary-id: Install TeamCity Server on Linux or macOS)

>Before installing the TeamCity server, you might want to estimate your [system requirements](system-requirements.md) and read about [supported platforms](supported-platforms-and-environments.md).

## Download TeamCity Server

Go to the [JetBrains website](https://www.jetbrains.com/teamcity/download/) and download the __.tar.gz distribution__ with the "portable" version of the TeamCity server.

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

## Example: Installation using Ubuntu Linux

 1. Create a dedicated user account called `teamcity` to run TeamCity.
    
    ```Shell
    root@ubuntu:/# adduser teamcity
    ```

 2. Install `wget` to download tar.gz files.
    
    ```Shell
    root@ubuntu:/# apt update && apt install wget -y
    ```

 3. Download the Linux tar.gz file from the [TeamCity Downloads Page](https://www.jetbrains.com/teamcity/download/other.html).
    
    ```Shell
    # Switch to the /opt directory, where TeamCity will be unpacked
    root@ubuntu:/# cd /opt
    # Download the tar.gz file using wget
    root@ubuntu:/opt# wget https://download.jetbrains.com/teamcity/TeamCity-2022.10.1.tar.gz
    ```

 4. Unpack the tar.gz file.
    
    ```Shell
    root@ubuntu:/opt# tar xfz TeamCity-2022.10.1.tar.gz
    ```

 5. Ensure a compatible Java is installed or the `JAVA_HOME` environment variable stores the correct path to Java.
    
    ```Shell
    root@ubuntu:/opt# java -version
    root@ubuntu:/opt# echo $JAVA_HOME
    ```
    
    If no compatible Java is installed, install it like follows (the following example installs the [Amazon Corretto 11](https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/downloads-list.html) Java):

    ```Shell
    root@ubuntu:/opt# apt install java-common -y
    root@ubuntu:/opt# wget https://corretto.aws/downloads/latest/amazon-corretto-11-x64-linux-jdk.deb
    root@ubuntu:/opt# dpkg --install amazon-corretto-11-x64-linux-jdk.deb
    # verify the installation
    root@ubuntu:/opt# java -version
    openjdk version "11.0.18" 2023-01-17 LTS
    OpenJDK Runtime Environment Corretto-11.0.18.10.1 (build 11.0.18+10-LTS)
    OpenJDK 64-Bit Server VM Corretto-11.0.18.10.1 (build 11.0.18+10-LTS, mixed mode)
     ```

 6. Change the owner of the /opt/TeamCity directory to the `teamcity` user created in step 1.
    
    ```Shell
    root@ubuntu:/opt# chown -R teamcity:teamcity TeamCity
    ```

 7. Switch to the `teamcity` user.
    
    ```Shell
    root@ubuntu:/opt# su teamcity
    teamcity@ubuntu:/opt$
    ```

 8. Start the TeamCity server and its bundled build agent.
    
    ```Shell
    teamcity@ubuntu:/opt$ TeamCity/bin/runAll.sh start
    Spawning TeamCity restarter in separate process
    TeamCity restarter running with PID 2817
    Starting TeamCity build agent...
    Java executable is found: '/usr/lib/jvm/java-11-amazon-corretto/bin/java'
    Starting TeamCity Build Agent Launcher...
    Agent home directory is /opt/TeamCity/buildAgent
    Done [3230], see log at /opt/TeamCity/buildAgent/logs/teamcity-agent.log
    ```

 9. Navigate to `<ip address>:8111` in a web browser to complete the installation (see [TeamCity First Start](quick-setup-guide.md#TeamCity+First+Start)).
