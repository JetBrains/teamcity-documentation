[//]: # (title: Docker Wrapper)
[//]: # (auxiliary-id: Docker Wrapper)

The _Docker Wrapper_ extension allows running a build step inside the specified Docker image.

The extension is available for the following [build runners](build-runner.md):
* [Command Line](command-line.md)
* [Maven](maven.md)
* [Ant](ant.md)
* [Gradle](gradle.md)
* [.NET](net.md)
* [Python](python.md)
* [PowerShell](powershell.md)
* [C# Script](c-script.md)
  
Each of the supported runners has the dedicated Docker settings section.

>_Docker Wrapper_ is a part of the TeamCity-Docker integration toolset. Refer to [this page](integrating-teamcity-with-docker.md) for information on software requirements, supported environments, and other common aspects of this integration.

## Docker Settings

In the _Docker Settings_ section of the build step settings, you can specify a Docker image which will be used to run the build step. Once the image is specified, the following options become available.

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

Specify a Docker image name as stated in [Docker Hub](https://hub.docker.com/). TeamCity will start a container from the specified image and will try to run this build step within this container.

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

Allows specifying additional options for the `docker run` command. The default argument is `--rm`, but you can provide more, for instance, add an additional volume mapping.

>In this field, you cannot reference environment variables using the `%\env.FOO_BAR%` syntax because TeamCity does not pass environment variables from a build agent into a Docker container.  
If you need to reference an environment variable on an agent, define the configuration parameter `system.FOO_BAR=env_var_value` in [`buildAgent.properties`](configure-agent-installation.md) and reference it via `%\system.FOO_BAR%`.

</td></tr></table>

## How Docker Wrapper Works
{id="how-it-works-1"}

Technically, the command of the build runner is wrapped in a shell script, and this script is executed inside a Docker container with the `docker run` command. All the details about the started process, text of the script, and so on, are written into the build log (the [Verbose mode](build-log.md#Viewing+Build+Log) enables viewing them).

The [build checkout directory](build-checkout-directory.md) and most build agent directories are mapped inside the Docker process.

If the process environment contains the `TEAMCITY_DOCKER_NETWORK` environment variable set by the previous [Docker Compose](docker-compose.md) build step, this network is passed to the started `docker run` command with the `--network` switch.
                                     
## Restoring File Ownership on Linux

At the end of each build step performed inside a Docker wrapper, a build agent runs the `chown` command to restore the access of the `buildAgent` user to the checkout directory. This is done to prevent a potential problem when the files from a Docker container are created with the `root` ownership and cannot be removed by the build agent later.

By default, a TeamCity agent uses the `busybox` image from Docker Hub to run the `chown` command. You can specify an alternative image name with the `teamcity.internal.docker.busybox` parameter, either in the [`buildAgent.properties`](configure-agent-installation.md) file or in the [build configuration parameters](configuring-build-parameters.md).

>You may want to disable restoring the file ownership. For instance, if the `userns-remap` package is used for handling ownership of files created under Docker. For this, add the `teamcity.docker.chown.enabled=false` configuration parameter to the [`buildAgent.properties`](configure-agent-installation.md) file. As a result, TeamCity will not try to restore permissions of the files at the end of the build.

## Environment Variables Handling

TeamCity passes environment variables from the [build configuration](build-configuration.md) into the Docker process, but it does not pass environment variables from the [build agent](build-agent.md), as they may not be relevant to the Docker container environment. The list of the passed environment variables can be seen in the [Verbose mode](build-log.md#Viewing+Build+Log) in the build log.

## Setting Image Entrypoint

If a Docker image does not define an [`ENTRYPOINT`](https://docs.docker.com/engine/reference/builder/#entrypoint), you can use still run a container with an `ENTRYPOINT` from the command line:
1. Add a [Command Line](command-line.md) build step.
2. Set the _Run_ mode to _Executable with parameters_.
3. In the _Command executable_ field, specify the full path to the `ENTRYPOINT` in the target Docker container.
4. In _Docker Settings_, specify the name of the Docker container.

TeamCity will start the specified Docker image with the defined `ENTRYPOINT`.

<seealso>
        <category ref="admin-guide">
            <a href="integrating-teamcity-with-docker.md">Integrating TeamCity with Docker</a>
            <a href="configuring-connections-to-docker.md">Configuring Connections to Docker</a>
            <a href="docker-compose.md">Docker Compose runner</a>
            <a href="docker.md">Docker runner</a>
            <a href="docker-support.md">Docker Support feature</a>
        </category>
</seealso>
