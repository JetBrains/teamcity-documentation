[//]: # (title: Docker)
[//]: # (auxiliary-id: Docker)

The _Docker_ [build runner](build-runner.md) allows launching the `build`, `push`, and `tag` [Docker](https://www.docker.com/) commands inside your build.

>For the `run` command, use [Docker Wrapper](docker-wrapper.md).

If a [Dockerfile](https://docs.docker.com/engine/reference/builder/) is present in your VCS repository and you create a TeamCity project based on this repository, TeamCity will [autodetect it](configuring-build-steps.md#Autodetecting+build+steps) and offer creating a build step using this runner.

## Common Settings

This runner is a part of the TeamCity-Docker integration toolset. Refer to [this page](integrating-teamcity-with-docker.md) for information on software requirements, supported environments, and other common aspects of this integration.

Available step execution policies are described [here](configuring-build-steps.md#Execution+policy).

## Docker Command

The runner provides the following settings, depending on the selected command:

<table><tr>

<td>

Command

</td>

<td>

Parameter

</td>

<td>

Description

</td></tr>

<tr>

<td rowspan="8">

`build`

</td>

<td id="Docker_build" auxiliary-id="Docker build">

Dockerfile source

</td>

<td>

Depending on the selected source, the settings below will vary. Available options are _File_, _URL_, and _File content_.

</td></tr><tr>

<td>

Path to file

</td>

<td>

Available for the _File_ source type:

Specify the path to the [Dockerfile](https://docs.docker.com/engine/reference/builder/). The path should be relative to the [build checkout directory](build-checkout-directory.md).

</td></tr><tr>

<td>

Context folder

</td>

<td>

Available for the _File_ source type:

Specify the [context](https://docs.docker.com/engine/reference/commandline/build/#extended-description) for the `docker build`. If blank, the parent directory of the Dockerfile will be used.

</td></tr><tr>

<td>

URL to file

</td>

<td>

Available for the _URL_ source type:

The URL can refer to one of the three kinds of resources: Git repositories, prepackaged tarball contexts, and plain text files. See the [Docker documentation](https://docs.docker.com/engine/reference/commandline/build/#extended-description) for details.

</td></tr><tr>

<td>

File Content

</td>

<td>

Available for the _File Content_ source type:

You can enter the content of the [Dockerfile](https://docs.docker.com/engine/reference/builder/) into the field.

</td></tr><tr>

<td>

Image platform

</td>

<td>

Select \<Any\> (default), Linux, or Windows.

</td></tr><tr>

<td>

Image name:tag

</td>

<td>

Provide a newline-separated list of image name:[tag(s)](https://docs.docker.com/engine/reference/commandline/tag/).

</td></tr><tr>

<td>

Additional arguments for the `build` command

</td>

<td>

Supply additional arguments to the `docker build` command. See the [Docker documentation](https://docs.docker.com/engine/reference/commandline/build/) for details.

</td></tr><tr>

<td rowspan="2">

`push`

</td>

<td>

Remove image from agent after push

</td>

<td>

If selected, TeamCity will remove the image with `docker rmi` at the end of the step.

</td></tr><tr>

<td>

Image name:tag

</td>

<td>

Provide a newline-separated list of image name:[tag(s)](https://docs.docker.com/engine/reference/commandline/tag/).

</td></tr><tr>

<td rowspan="3">

other

</td>

<td>

Command name

</td>

<td>

Docker sub-command, like `push` or `tag`. For the `run` command, use [Docker Wrapper](docker-wrapper.md).

</td></tr><tr>

<td>

Working directory

</td>

<td>

Specify the [build working directory](build-working-directory.md) if it differs from the [checkout directory](build-checkout-directory.md).

</td></tr><tr>

<td>

Additional arguments for the command

</td>

<td>

Additional arguments that will be passed to the `docker` command.

</td></tr></table>

### Running Docker via sudo

You can enforce starting Docker commands on a TeamCity agent via `sudo`. Add the `teamcity.docker.use.sudo=true` setting in the [build agent configuration file](configure-agent-installation.md) or as an agent's system property. On the agent start, the TeamCity agent log will inform you that the `sudo` prefix is used to run Docker commands.

To configure the `sudoers` file for the `sudo` command, use [`visudo`](https://www.sudo.ws/man/1.8.17/visudo.man.html) as follows:

```Shell
buildagentuser ALL=(ALL) NOPASSWD:SETENV:<full_path_to_docker>

```

We recommend removing (or commenting out) the `Defaults requiretty` line from the `sudoers` file to prevent the [problem with `docker login`](https://youtrack.jetbrains.com/issue/TW-60990).
 
<seealso>
        <category ref="admin-guide">
            <a href="integrating-teamcity-with-docker.md">Integrating TeamCity with Docker</a>
            <a href="configuring-connections-to-docker.md">Configuring Connections to Docker</a>
            <a href="docker-support.md">Docker Support feature</a>
            <a href="docker-compose.md">Docker Compose runner</a>
            <a href="docker-wrapper.md">Docker Wrapper extension</a>
        </category>
</seealso>