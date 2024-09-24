[//]: # (title: Start TeamCity Server)
[//]: # (auxiliary-id: Start TeamCity Server)

## Start Server

If TeamCity is installed using the `.exe` or `.tar.gz` distributions, it can be started and stopped by the `teamcity-server` scripts located in the [`<TeamCity Home>`](teamcity-home-directory.md)`/bin` directory. The scripts accept `run` (run in the same console), `start` (start a new detached process and exit from the script), and `stop` commands. To restart TeamCity, send `stop` and then, after it stops, send `start`.

__(evaluation only) To start/stop the TeamCity server and one default agent at the same time__, use the `runAll` script, for example:
* Use `runAll.bat start` to start the server and default agent.
* Use `runAll.bat stop` to stop the server and default agent.

__To start/stop the TeamCity server only__, use the `teamcity-server` scripts and pass the required parameters. Start the script without parameters to see the usage instructions. The `teamcity-server` scripts support the following options for the `stop` command:
* `stop n` — sends the stop command to the TeamCity server and waits up to `n` seconds for the process to end.
* `stop n -force` — sends the stop command to the TeamCity server, waits up to `n` seconds for the process to end, and terminates the server process if it did not stop.

>The TeamCity server will restart automatically if the server process exits (crashes or is killed) without invoking the `teamcity-server stop` script.

If a TeamCity server is installed as a Windows service, follow the usual procedure of starting and stopping services.

If you need to pass special properties to the server, refer to [this article](server-startup-properties.md).

You can configure autostart of TeamCity on your machine by the means of the operating systems (see the [example for macOS](how-to.md#Autostart+TeamCity+Server+on+macOS)).

## Launch TeamCity UI

The TeamCity UI can be accessed via a web browser. The default addresses are [`http://localhost/`](http://localhost/){nullable="true"} for the `exe` distribution and [`http://localhost:8111/`](http://localhost:8111/){nullable="true"} for the `tar.gz` distribution. See how to [change the server port](configure-server-installation.md#Changing+Server+Port), if necessary.

If you cannot access the TeamCity UI after a successful installation, please refer to the [troubleshooting section](#Troubleshoot+TeamCity+Installation).

## Troubleshoot TeamCity Installation

If the TeamCity UI cannot be accessed, check the following:
* The _TeamCity Server_ service is running (if you installed TeamCity as a Windows service).
* The TeamCity server process (Tomcat) is running (a Java process run in the [`<TeamCity Home>`](teamcity-home-directory.md)`/bin` directory).
* Any warnings in the console output if you run the server from a console.
* The `teamcity-server.log` file and other files in the [`<TeamCity Home>`](teamcity-home-directory.md)`\logs` directory for error messages.

One of the most common issues with the server installation is using a port that is already used by another program. See [how to change the default port](configure-server-installation.md#Changing+Server+Port).
