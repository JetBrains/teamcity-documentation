[//]: # (title: Container Wrapper)
[//]: # (auxiliary-id: Container Wrapper;Docker Wrapper)

The _Container Wrapper_ extension allows running a build step inside the specified Docker/LXC image. Images are pulled via `docker pull` or `podman pull` commands, depending on which container manager is installed on the agent that runs the build.

> To configure the _Container Wrapper_ extension in Kotlin DSL, see the Docker example in [MavenBuildStep](https://www.jetbrains.com/help/teamcity/kotlin-dsl-documentation/buildSteps/maven-build-step/index.html).
>
{style="note"}

TeamCity can pull containers anonymously (if images are publicly available) or after logging into a registry (for private registries or to avoid DockerHub penalties for anonymous downloads). If you need TeamCity to authorize to a registry before pulling an image, configure the _Docker Support_ build feature, as follows:
1. In your project settings, select **Connections** from the sidebar and follow the instructions in [](configuring-connections-to-docker.md) to add new Docker or Podman connections to your project.
2. In your build configuration settings, follow the instructions in [](docker-support.md) to configure the **Docker Support** build feature, adding the connections created in the previous step.

The extension is available for the following [build runners](build-runner.md):

* [Command Line](command-line.md)
* [Maven](maven.md)
* [Ant](ant.md)
* [Gradle](gradle.md)
* [.NET](net.md)
* [Python](python.md)
* [PowerShell](powershell.md)
* [C# Script](c-script.md)
* [Node.js](nodejs.md)


>_Container Wrapper_ is a part of the TeamCity-Docker/Podman integration toolset. Refer to this documentation article for information on software requirements, supported environments, and other common aspects of this integration: [](integrating-teamcity-with-container-managers.md).

## Container Settings

In the _Container Settings_ section of the build step settings, you can specify an image which will be used to run the build step. Once the image is specified, the following options become available.

<table><tr>

<td>

Setting

</td>

<td>

Description

</td></tr><tr>

<td>

Run step within container

</td>

<td>

The image name as stated in [Docker Hub](https://hub.docker.com/) or other registry. TeamCity will start a container from the specified image and will try to run this build step within this container.

For example, `ruby:2.4` will run the step within the Ruby container, version 2.4.

If an agent that runs the build has Podman installed instead of Docker, use either full image names (for example, `docker.io/library/alpine:latest` instead of `alpine:latest`), or ensure the registry domain is specified in the [registries.conf](integrating-teamcity-with-container-managers.md#Environment+Requirements) file on your build agent machine. See also: [How to manage Linux container registries](https://www.redhat.com/sysadmin/manage-container-registries).

</td></tr><tr>

<td>

Image platform

</td>

<td>

Select &lt;Any&gt; (default), Linux, or Windows. Note that Windows images are not supported by Podman.

</td></tr><tr>

<td>

Pull image explicitly

</td>

<td>

If enabled, the image will be pulled from the Hub repository via `docker pull <imageName>`/`podman pull <imageName` before the `docker run`/`podman run` command is launched.

</td></tr><tr>

<td>

Additional run arguments

</td>

<td>

Allows specifying additional options for the `docker run` and `podman run` commands. The default argument is `--rm`, but you can provide more, for instance, add an additional volume mapping.

>If you intend to utilize [environment variables](configuring-build-parameters.md#Environment+Variables) in this field (for example, `%\env.FOO%`), note that TeamCity passes to containers only those variables that are declared in build configurations and projects. Agent-specific environment variables declared in the [buildAgent.properties](configure-agent-installation.md) file are not passed to containers.
> 
> If you need a parameter declared in this file, use this approach:
>1. Define a system property (`system.FOO=BAR`) in [buildAgent.properties](configure-agent-installation.md).
>2. Add a [build configuration parameter](configuring-build-parameters.md) with the name `system.FOO` and value `%\system.FOO%`.
>3. Reference the system property as `%\system.FOO%` in the "Additional run arguments" field.
>4. If you need to use the same value as an environment variable in your build script, you can also add a build configuration parameter with the name `env.FOO` and value `%\system.FOO%`.
>
{style="note"}

</td></tr></table>

## How Container Wrapper Works
{id="how-it-works-1"}

Technically, the command of the build runner is wrapped in a shell script, and this script is executed inside a container with the `docker run` or `podman run` command. To view the details about the started process, text of the script, and so on, check the build log in [Verbose mode](build-log.md#Viewing+Build+Log).

The Container Wrapper maps paths to the [build checkout directory](build-checkout-directory.md) and other agent directories like <path>[buildAgent/work](agent-work-directory.md)</path>, so that all these directories have the same location on a build agent and inside a [](container-wrapper.md).

If the process environment contains the `TEAMCITY_DOCKER_NETWORK` environment variable set by the previous [Docker Compose](docker-compose.md) build step, this network is passed to the started `docker run` command with the `--network` switch.
                                     
## Restoring File Ownership on Linux

Build agents that use Docker execute the `chown` command at the end of each step running inside a container to restore the `buildAgent` user's permissions to access the checkout directory. This action prevents potential issues related to build agents being unable to remove no longer needed container files that were created with the `root` ownership. Agents using Podman do not perform this step.

By default, a TeamCity agent uses the `busybox` image from Docker Hub to run the `chown` command. You can specify an alternative image name with the `teamcity.internal.docker.busybox` parameter, either in the [`buildAgent.properties`](configure-agent-installation.md) file or in the [build configuration parameters](configuring-build-parameters.md).

>You may want to disable restoring the file ownership. For instance, if the `userns-remap` package is used for handling ownership of files created under Docker. For this, add the `teamcity.docker.chown.enabled=false` configuration parameter to the [`buildAgent.properties`](configure-agent-installation.md) file. As a result, TeamCity will not try to restore permissions of the files at the end of the build.

## Environment Variables Handling

TeamCity passes environment variables from the [build configuration](managing-builds.md) into the Docker or Podman process, but it does not pass environment variables from the [build agent](build-agent.md), as they may not be relevant to the container environment. The list of the passed environment variables can be seen in the [Verbose mode](build-log.md#Viewing+Build+Log) in the build log.

## Setting Image Entrypoint

If a Docker image does not define an [`ENTRYPOINT`](https://docs.docker.com/engine/reference/builder/#entrypoint), you can use still run a container with an `ENTRYPOINT` from the command line:
1. Add a [Command Line](command-line.md) build step.
2. Set the _Run_ mode to _Executable with parameters_.
3. In the _Command executable_ field, specify the full path to the `ENTRYPOINT` in the target container.
4. In _Docker Settings_, specify the name of the container.

TeamCity will start the specified Docker image with the defined `ENTRYPOINT`.

## Setting Container User

If your step creates or accesses files or folders on local storage, ensure these actions are performed under the correct user with sufficient permissions. To do this, add `--user=<value>` to **Additional run arguments** of the runner.

```Kotlin
steps {
    script {
        // ...
        dockerImage = "python:windowsservercore-ltsc2022"
        dockerRunParameters = "--user=1001"
    }
}
```

The host UID can be retrieved via the `env.UID` parameter (`--user=%\env.UID%`).

<seealso>
        <category ref="admin-guide">
            <a href="integrating-teamcity-with-container-managers.md">Integrating TeamCity with Docker</a>
            <a href="configuring-connections-to-docker.md">Configuring Connections to Docker</a>
            <a href="docker-compose.md">Docker Compose runner</a>
            <a href="docker.md">Docker runner</a>
            <a href="docker-support.md">Docker Support feature</a>
        </category>
</seealso>
