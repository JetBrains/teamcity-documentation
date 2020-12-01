[//]: # (title: Integrating TeamCity with Docker)
[//]: # (auxiliary-id: Integrating TeamCity with Docker)

TeamCity comes with Docker Support, implemented as a bundled [plugin](https://plugins.jetbrains.com/plugin/10062-docker-support).

>To learn how to run Docker inside an agent container and read other information about TeamCity agent Docker images, read our documentation in [Docker Hub](https://hub.docker.com/r/jetbrains/teamcity-agent/).

## Requirements

The integration requires [Docker](https://docs.docker.com/engine/installation/) installed on the build agents. To use the [Docker Compose](docker-compose.md) build runner, install [Docker Compose](https://docs.docker.com/compose/install/) as well.

Since version 2019.2.1, TeamCity periodically checks if Docker is available on active build agents. Based on the `docker.server.version` and `docker.version` variables received from the agents, TeamCity distributes builds that use Docker only between agents with the installed Docker engine.   
If a build configuration uses the [Docker runner](docker.md) or the [Docker Wrapper extension](docker-wrapper.md), TeamCity automatically adds the `docker.server.version` [agent compatibility requirement](configuring-agent-requirements.md) for this configuration.

<chunk include-id="reqs-supported-env">

## Supported Environments

TeamCity Docker Support can run on Windows, Linux, and macOS build agents. It uses the `docker` executable on the build agent machine, so it should be runnable by the build agent user. 

<note>

   * On Linux, the integration will run if the installed Docker is detected.
   * On Windows, the integration works for Linux and Windows container modes.
   * On macOS, the official [Docker support for Mac](https://docs.docker.com/docker-for-mac/install/) should be installed for the user running the build agent.

</note>

</chunk>

## Parameters Reported by Agent

During the build, the build agent reports the following parameters:

<table><tr>

<td>

Parameter

</td>

<td>

Description

</td></tr><tr>

<td>

`docker.version`

</td>

<td>

[Docker CLI](https://docs.docker.com/engine/reference/commandline/docker/) version

</td></tr><tr>

<td>

`dockerCompose.version`

</td>

<td>

The Docker Compose file version, if the [Docker Compose](docker-compose.md) build step is used.

</td></tr><tr>

<td>

`docker.server.version`

</td>

<td>

[Docker Engine](https://docs.docker.com/engine/reference/commandline/dockerd/) version

</td></tr><tr>

<td>

`docker.server.osType`

</td>

<td>

The Docker Engine OS platform, can have the `linux` or `windows` value.

</td></tr></table>

If you are using the [Command Line](command-line.md) build step (and not the TeamCity-provided Docker steps), these parameters can be used as [agent requirements](agent-requirements.md) to ensure your build is run only on the agents with Docker installed. 

## Features

The TeamСity-Docker integration provides the following features which facilitate working with Docker under TeamCity:

### Docker Support Build Feature

<include src="docker-support.md" include-id="docker-support"/>

### Docker Connection for Project

<include src="configuring-connections-to-docker.md" include-id="docker-connection"/>

### Docker Runner

<include src="docker.md" include-id="docker-runner"/>

### Docker Command
{id="docker-command-1"}

<include src="docker.md" include-id="docker-command"/>

### Docker Compose Runner

<include src="docker-compose.md" include-id="docker-compose"/>

### Docker Wrapper
{id="docker-wrapper-1"}

TeamCity provides the Docker Wrapper extension for [Command Line](command-line.md), [Maven](maven.md), [Ant](ant.md), [Gradle](gradle.md), and [Python](python.md) runners. Each of the supported runners has the dedicated Docker Settings section.

#### Docker Settings
{id="docker-settings-1"}

See the [Docker Wrapper](docker-wrapper.md) description.

### Docker Disk Space Cleaner

Docker Disk Space Cleaner is an extension to the [Free Disk Space](free-disk-space.md) build feature ensuring a certain amount of disk space for a build.   

TeamCity regularly cleans up its related Docker images which were tagged/pulled:
* in a build with the [Docker Support](docker-support.md) build feature, __or__
* in a [Docker](docker.md) or [Docker Compose](docker-compose.md) build step, __or__
* in a build step with the enabled [Docker Wrapper](docker-wrapper.md) extension

For such builds,  

* The TeamCity agent tracks Docker images tagged or pulled during builds (the list of images is stored in the `buildAgent/system/docker-used-images.dat` file). 
* During clean-up / freeing disk space, TeamCity agent tries to remove these images if they were not used within 3 days, 1 day, 0 on subsequent attempts to free disk space.  

Besides that, TeamCity cleans local Docker Caches using the command:

* __Since 2018.1.2__: `docker system prune --volumes`. Works for Docker v.17.06.1 or later.
* __Before 2018.1.2__: `docker system prune -a`

### Service Message to Report Pushed Image

If TeamCity (for some reason) cannot determine that an image has been pushed, a user can send a special [service message](service-messages.md) to report this information to the TeamCity server:


```Shell

##teamcity[dockerMessage type='dockerImage.push' value='<full_image_tag>,size:<size in bytes>,digest:<hash>']

```

For example:

```Shell

##teamcity[dockerMessage type='dockerImage.push' value='myRegistry/repo-test:17,size:2632,digest:sha256:8dc5a195c3dcdc7c288d16288ff3f9ab1d8a5a230e09afb9c8dc9215e861aa55']
```

## Conforming with Docker download rate limits

Since November 1st 2020, Docker Hub introduces [download rate limits](https://docs.docker.com/docker-hub/download-rate-limit/) for public image pulls.

If your TeamCity builds use [Docker Wrapper](docker-wrapper.md) or [Docker Compose](docker-compose.md) to pull images from Docker Hub, make sure these pulls do not exceed the following limits:

* Anonymous Docker users: 100 pulls per 6 hours
* Docker users with the Free plan: 200 pulls per 6 hours

If you have a Team or Pro Docker account, the number of pulls stays unlimited.

A regular TeamCity agent stores a once pulled image in its cache. This allows running an indefinite number of builds using the same pulled image on a regular basis.   
However, there are few cases to consider:
* If you are using cloud agents, all required images will be downloaded every time a new cloud agent is launched.
* If the _Pull image explicitly_ option is enabled in the build step settings, the image will be downloaded in every new build run, even on a local agent. We recommend that you disable this option to prevent reaching the rate limit.
* During the [Free Disk Space](free-disk-space.md) stage of the build, TeamCity may clean up old unused Docker images from the local cache.

If previously your builds were accessing Docker Hub anonymously, you can double the number of allowed pulls by creating a Free Docker user profile and configuring a [Docker connection](configuring-connections-to-docker.md) in your TeamCity project. TeamCity agents will be able to use this connection to authenticate in Docker Hub before each build.