[//]: # (title: Docker Wrapper)
[//]: # (auxiliary-id: Docker Wrapper)

TeamCity provides the Docker Wrapper extension for [Command Line](command-line.md), [Maven](maven.md), [Ant](ant.md), [Gradle](gradle.md), [.NET CLI (dotnet)](net-cli-dotnet.md), and [PowerShell](powershell.md) runners. This extension allows running a build step inside the specified Docker image. Each of the supported runners has the dedicated Docker settings section.

## Requirements

The integration requires [Docker](https://docs.docker.com/engine/installation/) installed on the build agents.

<include src="integrating-teamcity-with-docker.md" include-id="reqs-supported-env"/>

## Docker Settings

<chunk include-id="docker-settings">

In the _Docker Settings_ section of the build step settings, you can specify a Docker image which will be used to run the build step. Once an image is specified, all the following options become available.

<table><tr>

<td>

Setting

</td>

<td>

Description

</td></tr><tr>

<td>

Run step within Docker container

</td>

<td>

Specify a Docker image name as stated in the Docker Hub. TeamCity will start a container from the specified image and will try to run this build step within this container.

For example, `ruby:2.4` will run the step within the Ruby container, version 2.4.


</td></tr><tr>

<td>

Docker image platform

</td>

<td>

Select &lt;Any&gt; (default), Linux, or Windows.

</td></tr><tr>

<td>

Pull image explicitly

</td>

<td>

If enabled, the image will be pulled from the Hub repository via `docker pull <imageName>` before the `docker run` command is launched.


</td></tr><tr>

<td>

Additional docker run arguments

</td>

<td>

Allows specifying additional options for `docker run`. The default argument is `--rm`, but you can provide more, for instance, add an additional volume mapping.

<note>
     
In this field, you cannot reference environment variables 
using `%env.FOO_BAR%` syntax because TeamCity does not pass environment variables
from a build agent into a Docker container.

If you need to reference an environment variable on an agent, 
define the configuration parameter `system.FOO_BAR=env_var_value` in `buildAgent.properties`
and reference it via `%system.FOO_BAR%`.
     
 </note> 


</td></tr></table>

</chunk>

### How It Works

<chunk include-id="docker-settings-how">

Technically, the command of the build runner is wrapped in a shell script, and this script is executed inside a Docker container with the `docker run` command. All the details about the started process, text of the script, and so on, are written into the build log (the _Verbose_ mode enables viewing them).

The [checkout directory](build-checkout-directory.md) and most build agent directories are mapped inside the Docker process.

At the end of the build step with the Docker wrapper, a build agent runs the `chown` command to restore access of the `buildAgent` user to the checkout directory. This mitigates a possible problem when the files from a Docker container are created with the `root` ownership and cannot be removed by the build agent later.

If the process environment contains the `TEAMCITY_DOCKER_NETWORK` environment variable set by the previous [Docker Compose](docker-compose.md) build step, this network is passed to the started `docker run` command with the `--network` switch.

</chunk>

### Environment Variables Handling

<chunk include-id="docker-settings-env-var">

TeamCity passes environment variables from the [build configuration](build-configuration.md) into the Docker process, but it does not pass environment variables from the [build agent](build-agent.md), as they may not be relevant to the Docker container environment. The list of the passed environment variables can be seen in [Verbose mode](build-log.md#Viewing+Build+Log) in the build log.


 </chunk>

__ __