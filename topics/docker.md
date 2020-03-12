[//]: # (title: Docker)
[//]: # (auxiliary-id: Docker)

TeamCity comes with built-in [Docker integration](integrating-teamcity-with-docker.md), which includes the Docker runner (formerly Docker Build), a runner for Docker commands.

<include src="integrating-teamcity-with-docker.md" include-id="reqs-supported-env"/>

<chunk include-id="docker-runner">

The Docker runner supports the `build`, `push`, and `tag` Docker commands.

When creating TeamCity projects / build configurations from a repository URL, the runner is offered as build step during auto-detection, provided a Dockerfile is present in the VCS repository.

</chunk>

 
## Docker Command

<chunk include-id="docker-command">

Here is the list of the Docker commands. Depending on the selected command, the settings below will vary.

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

build

</td>

<td>

Dockerfile source

</td>

<td>

Depending on the selected source, the settings below will vary. The available options include File, a URL or File content.

</td></tr><tr>

<td>

Path to file

</td>

<td>

_Available if File is selected as the source_. Specify the path to the Docker file. The path should be relative to the [checkout directory](build-checkout-directory.md).


</td></tr><tr>

<td>

Context folder

</td>

<td>

_Available if File is selected as the source_. Specify the context for the Docker build. If blank, the enclosing folder for Dockerfile will be used.

</td></tr><tr>

<td>

URL to file

</td>

<td>

_Available if URL is selected as the source_. The URL can refer to three kinds of resources: Git repositories, pre-packaged tarball contexts, and plain text files. See the [Docker documentation](https://docs.docker.com/engine/reference/commandline/build/#extended-description) for details.

</td></tr><tr>

<td>

File Content

</td>

<td>

_Available if the file cis selected as the source_. You can enter the content of the Dockerfile into the field.

</td></tr><tr>

<td>

Image platform

</td>

<td>

Select \<Any\> (default), Linux or Windows.

</td></tr><tr>

<td>

Image name:tag

</td>

<td>

Provide a newline-separated list of image name:tag(s)


</td></tr><tr>

<td>

Additional arguments for the `build` command

</td>

<td>

Supply additional arguments to the docker build command. See [Docker documentation](https://docs.docker.com/engine/reference/commandline/build/) for details.

</td></tr><tr>

<td rowspan="2">

push

</td>

<td>

Remove image from agent after push

</td>

<td>

If selected, TeamCity will remove the image with `docker rmi` at the end of the step

</td></tr><tr>

<td>

Image name:tag

</td>

<td>

Provide a newline-separated list of image name:tag(s)

</td></tr><tr>

<td rowspan="3">

other

</td>

<td>

Command name

</td>

<td>

Docker sub-command, like `push` or `tag`. For run, use [Docker Wrapper](docker-wrapper.md)

</td></tr><tr>

<td>

Working directory

</td>

<td>

</td></tr><tr>

<td>

Additional arguments for the command

</td>

<td>

Additional arguments that will be passed to the Docker command.

</td></tr></table>

### Running Docker via sudo

You can enforce starting Docker commands on a TeamCity agent via `sudo`. Add the `teamcity.docker.use.sudo=true` setting in the [build agent configuration file](build-agent-configuration.md) or as an agent's system property. On an agent start, the TeamCity agent log will inform that the `sudo` prefix is used to run Docker commands.

To configure the `sudoers` file for the `sudo` command, use [`visudo`](https://www.sudo.ws/man/1.8.17/visudo.man.html) as follows:

```Shell
buildagentuser ALL=(ALL) NOPASSWD:SETENV:<full_path_to_docker>

```

We recommend removing (or commenting out) the `Defaults requiretty` line from the `sudoers` file to prevent the [problem with `docker login`](https://youtrack.jetbrains.com/issue/TW-60990).

</chunk>
 
__ __