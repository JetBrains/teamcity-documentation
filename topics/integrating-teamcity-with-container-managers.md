[//]: # (title: Integrating TeamCity with Container Managers)
[//]: # (help-id: Integrating TeamCity with Docker and Podman)

TeamCity integrates with container managers (Docker and Podman) to support a wide range of operations.

<deflist type="full">

<def title="Run steps in containers">

Most build steps include a [**Container settings**](container-wrapper.md) section where you can specify the container in which the step should run.

<img src="dk-container-settings-in-steps.png" width="706" alt="Container settings in steps"/>

> If this step is part of a [pipeline](create-and-edit-pipelines.md), use the **Run in Docker** toggle in the step settings.
>
{style="tip"}

If a configuration has multiple steps that need to run in the same container, use the [](run-in-docker.md) build feature instead of configuring each step separately.

Both approaches require a build agent with Docker or Podman installed. TeamCity can use either tool to run steps inside containers.

</def>

<def title="Pull, build, and upload images">

The [Docker](docker.md) build step can run custom `docker ...` commands, including `build` and `push`. The [](docker-compose.md) build step lets you run multi-container Docker applications.

To perform these actions on build agents that have Podman installed instead of Docker, use the generic [](command-line.md) build step.

</def>

<def title="Run TeamCity server and agents as Docker images">

For more information, see the following pages:

* [teamcity-docker-images on GitHub](https://github.com/JetBrains/teamcity-docker-images/tree/master?tab=readme-ov-file#-docker-images)
* [jetbrains/teamcity-server image description on Docker Hub](https://hub.docker.com/r/jetbrains/teamcity-server)
* [jetbrains/teamcity-agent image description on Docker Hub](https://hub.docker.com/r/jetbrains/teamcity-agent/)

</def>

</deflist>

In classic build configurations, these integrations require a corresponding [Docker Registry](configuring-connections-to-docker.md) connection assigned to the [](docker-support.md) build feature. In addition to selecting the registry connection used by the configuration, this build feature adds the __Container Info__ tab to __Build Results__, where you can view information about images published to a registry during the build.

<img src="dk-containerInfoTab.png" width="706" alt="Container Info tab"/>

In pipelines, you only need to configure the corresponding item in the [](pipeline-settings.md#Integrations) section.

## Compatibility and Requirements

### Environment Requirements

To run a Docker- or Podman-related operation, a build agent must be able to use the `docker` or `podman` executable.

Docker can be installed on Linux, Windows, and macOS build agents.

* On Linux, the Docker Registry Connections integration will run if the installed Docker is detected.
* On Windows, the integration works for Linux and Windows container modes.
* On macOS, the official [Docker support for Mac](https://docs.docker.com/docker-for-mac/install/) should be installed for the user running the build agent.

Podman is a tool designed primarily for Linux. To run on Windows and macOS, Podman requires an installed Linux virtual machine ("Podman machine"). See the following article to learn more about installing Podman on different operating systems: [Podman Installation Instructions](https://podman.io/docs/installation).

* Specify container registry domains in the `registries.conf` file on your build agent machine. For example: `unqualified-search-registries = ["docker.io"]`. Podman and TeamCity require this list of registries to:

  * Resolve full container addresses when a `podman ...` command uses a short image name (for example, `podman pull ubi8` instead of `podman pull registry.access.redhat.com/ubi8:latest`).
  * Successfully log in to a registry if your build configuration has the [](docker-support.md) build feature.
  
  See the following article to learn more about the `registries.conf` file: [How to manage Linux container registries](https://www.redhat.com/sysadmin/manage-container-registries).

* If you're running Linux build agents configured to [start automatically](start-teamcity-agent.md#Automatic+Agent+Start+Under+Linux) via systemd, enable [user lingering](https://www.freedesktop.org/software/systemd/man/loginctl.html) to keep a user session running: `loginctl enable-linger my_agent_user`.



### Tools

* Install [Docker](https://docs.docker.com/engine/installation/) or [Podman](https://podman.io) on your agent machines. For Windows and macOS agents, additionally requires a [Podman machine](https://podman.io/docs/installation).

* To use the [Docker Compose](docker-compose.md) build runner, you also need to install [Docker Compose](https://docs.docker.com/compose/install/).

### Build Parameters and Agent Requirements

TeamCity checks the following [parameters](configuring-build-parameters.md) to identify the available agent software:

* `container.engine` — returns "docker", "podman" or "docker,podman", depending on which container manager is installed on the agent machine. If both Docker and Podman are installed, TeamCity uses Docker by default.

* `teamcity.default.сontainer.engine` — set this property to either `docker` or `podman` to always use the desired tool. This property allows you to override the default logic that forces TeamCity to use Docker when both tools are available. You can set this property as a [configuration parameter](configuring-build-parameters.md) or as an individual agent's setting (in the [buildAgent.properties](configure-agent-installation.md) file).

* `docker.server.version`, `docker.version` — return versions of installed [Docker Engine](https://docs.docker.com/engine/reference/commandline/dockerd/)  and [Docker CLI](https://docs.docker.com/engine/reference/commandline/docker/), respectively.

* `podman.version` — stores the version of installed Podman software.

* `docker.server.osType`, `podman.osType` — return the type of the agent OS. Supported values are "linux" (for both Linux and macOS agents) and "windows".

* `dockerCompose.version` — returns the Docker Compose file version, if the [Docker Compose](docker-compose.md) build step is used.

Depending on the exact Docker/Podman integration you utilize, TeamCity specifies different [agent compatibility requirements](configuring-agent-requirements.md). For instance, if your [build configuration](managing-builds.md) uses the [Docker runner](docker.md), this configuration can only run on agents that meet the `docker.server.version exists` requirement.


> If you are using the [Command Line](command-line.md) build step instead of the [](docker.md) runner, these parameters can be used as [agent requirements](configuring-agent-requirements.md) to ensure your build is run only on the agents with Docker installed.
> 
{style="tip"}




## Docker Disk Space Cleaner

Docker Disk Space Cleaner is an extension to the [Free disk space](free-disk-space.md) build feature ensuring a necessary amount of disk space for a build.

TeamCity regularly cleans up its related Docker images which were tagged/pulled:
* in a build with the [](docker-support.md) build feature, __or__
* in a [Docker](docker.md) or [](docker-compose.md) build step, __or__
* in a build step with the enabled [](container-wrapper.md) extension.

For such builds,  

* A TeamCity agent tracks Docker images tagged or pulled during the builds (the list of images is stored in the `buildAgent/system/docker-used-images.dat` file).
* During clean-up / freeing disk space, the TeamCity agent tries to remove these images if they have not been used within 3 days (or 1 or 0 days, on subsequent attempts to free the disk space).

Besides, TeamCity cleans local сaches using the `docker/podman system prune --volumes` command. For Docker installations, this clean-up works for v.17.06.1 or later.

## Service Message to Report Pushed Image

If, for some reason, TeamCity cannot determine that an image has been pushed, a user can send a special [service message](service-messages.md) to report this information to the TeamCity server:

```Shell

##teamcity[dockerMessage type='dockerImage.push' value='<full_image_tag>,size:<size in bytes>,digest:<hash>']

```

For example:

```Shell

##teamcity[dockerMessage type='dockerImage.push' value='myRegistry/repo-test:17,size:2632,digest:sha256:8dc5a195c3dcdc7c288d16288ff3f9ab1d8a5a230e09afb9c8dc9215e861aa55']
```

## Conforming with Docker Download Rate Limits

Since November 1st 2020, Docker Hub introduces [download rate limits](https://docs.docker.com/docker-hub/download-rate-limit/) for public image pulls.

If your TeamCity builds use [](container-wrapper.md) or [Docker Compose](docker-compose.md) to pull images from Docker Hub, make sure these pulls do not exceed the following limits:

* Anonymous Docker users: 100 pulls per 6 hours
* Docker users with the Free plan: 200 pulls per 6 hours

If you have a Team or Pro Docker account, the number of pulls stays unlimited.

A regular TeamCity agent stores a once pulled image in its cache. This allows running an indefinite number of builds using the same pulled image on a regular basis.   
However, there are few cases to consider:
* If you are using cloud agents, all required images will be downloaded every time a new cloud agent is launched.
* If the _Force pull on each run_ option is enabled in the build step settings, the image will be downloaded in every new build run, even on a local agent. We recommend that you disable this option to prevent reaching the rate limit.
* While [freeing disk space](free-disk-space.md) for the build, TeamCity may clean up old unused Docker images from the local cache.

If previously your builds were accessing Docker Hub anonymously, you can double the number of allowed pulls by creating a Free Docker user profile and configuring a [Docker connection](configuring-connections-to-docker.md) in your TeamCity project. TeamCity agents will be able to use this connection to authenticate in Docker Hub before each build.

<seealso>
        <category ref="admin-guide">
            <a href="configuring-connections-to-docker.md">Configuring Connections to Docker</a>
            <a href="docker-compose.md">Docker Compose runner</a>
            <a href="docker.md">Docker runner</a>
            <a href="docker-support.md">Docker Support feature</a>
            <a href="container-wrapper.md">Container Wrapper extension</a>
        </category>
</seealso>