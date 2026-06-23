[//]: # (title: Docker)
[//]: # (help-id: Docker)

<primary-label ref="primary-step-config"/>

<show-structure for="chapter" depth="2"/>

The _Docker_ [build step](configuring-build-steps.md) allows launching `docker build`, `docker push`, `docker tag`, and other [Docker](https://www.docker.com/) commands inside your build.

> For the `run` command, see [](container-wrapper.md).
> 
{style="note"}

> To perform these actions on build agents that have Podman installed instead of Docker, use the generic [](command-line.md) build step.
> 
{style="tip"}


<include from="common-templates.md" element-id="docker-integration-note"><var name="docker-feature-name" value="Docker build step"/></include>


## Prerequisites

To push a newly built image to a Docker registry, you must authorize first.

1. <include from="common-templates.md" element-id="open-project-settings-tab"><var name="tab-name" value="Connections"/></include>
2. Add a new [Docker Registry connection](configuring-connections-to-docker.md) to your project.
3. In your [build configuration settings](project-administrator-guide.md#Edit+and+View+Modes), configure the [](docker-support.md) build feature using the connection created in the previous step.


## Common Settings



The build step provides the following settings, depending on the selected Docker command:

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

<td>

Dockerfile source
{id="Docker_build" help-id="Docker build"}

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

Docker sub-command, like `push` or `tag`. For the `run` command, use [](container-wrapper.md).

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

## Running Docker via sudo

You can enforce starting Docker commands on a TeamCity agent via `sudo`. Add the `teamcity.docker.use.sudo=true` setting in the [build agent configuration file](configure-agent-installation.md) or as an agent's system property. On the agent start, the TeamCity agent log will inform you that the `sudo` prefix is used to run Docker commands.

To configure the `sudoers` file for the `sudo` command, use [`visudo`](https://www.sudo.ws/man/1.8.17/visudo.man.html) as follows:

```Shell
buildagentuser ALL=(ALL) NOPASSWD:SETENV:<full_path_to_docker>

```

We recommend removing (or commenting out) the `Defaults requiretty` line from the `sudoers` file to prevent the [problem with `docker login`](https://youtrack.jetbrains.com/issue/TW-60990).


## Building Multi-Arch Images

The **other...** command option allows you to execute any custom `docker ...` command. For example, you can invoke [buildx](https://github.com/docker/buildx) commands to build multi-arch images.

1. Add a new **Docker** runner to your build configuration.
2. Switch its **Docker command** option to "other...".
3. Type "buildx" in the **Command name** field and "create --use" in **Additional arguments for the command**. TeamCity will combine these fields into a single `docker buildx create --use` command.
4. Repeat steps 1~3 to for each new command you want to execute. For example, you may want to add additional nodes (`docker buildx create --append --name mybuild <context_name>`) or call `docker buildx build <path> --platform linux/amd64,linux/arm64` to start building and image.


## Building Custom TeamCity Images

See the following articles for more information on using and building TeamCity server and agent images:

* [TeamCity Docker images](https://github.com/JetBrains/teamcity-docker-images/tree/master?tab=readme-ov-file#-docker-images)
* [Custom TeamCity Agent Images](https://github.com/JetBrains/teamcity-docker-images/blob/master/custom/README.md#custom-teamcity-agent-images)

<seealso>
        <category ref="admin-guide">
            <a href="integrating-teamcity-with-container-managers.md">Integrating TeamCity with Docker</a>
            <a href="configuring-connections-to-docker.md">Configuring Connections to Docker</a>
            <a href="docker-support.md">Docker Registry Connections feature</a>
            <a href="docker-compose.md">Docker Compose runner</a>
            <a href="container-wrapper.md">Container Wrapper extension</a>
        </category>
</seealso>