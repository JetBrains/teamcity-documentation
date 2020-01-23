[//]: # (title: Integrating TeamCity with Docker)
[//]: # (auxiliary-id: Integrating TeamCity with Docker)


TeamCity comes with Docker Support, implemented as a bundled [plugin](https://plugins.jetbrains.com/plugin/10062-docker-support).

On this page:

<tag-list of="chapter" mode="tree" depth="4"/>

## Requirements

The integration requires [Docker](https://docs.docker.com/engine/installation/) installed on the build agents. To use the [Docker Compose](docker-compose.md) build runner, install [Docker Compose](https://docs.docker.com/compose/install/) as well.

<chunk include-id="reqs-supported-env">

## Supported Environments

TeamCity Docker Support can run on Mac, Linux, and Windows build agents. It uses the `docker` executable on the build agent machine, so it should be runnable by the build agent user. 

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

TeamСity-Docker integration provides the following features which facilitate working with Docker under TeamCity:

### Docker Support Build Feature

<include src="docker-support.md" include-id="docker-support"/>

### Docker Connection for a Project

<include src="configuring-connections-to-docker.md" include-id="docker-connection"/>

### Docker Runner

<include src="docker.md" include-id="docker-runner"/>

### Docker Command
<include src="docker.md" include-id="docker-command"/>

### Docker Compose Runner

<include src="docker-compose.md" include-id="docker-compose"/>

### Docker Wrapper

TeamCity provides the Docker Wrapper extension for [Command Line](command-line.md), [Maven](maven.md), [Ant](ant.md), and [Gradle](gradle.md) runners. Each of the supported runners has the dedicated Docker settings section.

#### Docker Settings

<include src="docker-wrapper.md" include-id="docker-settings"/>

##### How It Works

<include src="docker-wrapper.md" include-id="docker-settings-how"/>

##### Environment Variables Handling

<include src="docker-wrapper.md" include-id="docker-settings-env-var"/>

### Docker Disk Space Cleaner

Docker Disk Space Cleaner is an extension to the [Free Disk Space](free-disk-space.md) build feature ensuring a certain amount of disk space for a build.   

TeamCity performs regular clean-up of Docker images, related to TeamCity:
* The TeamCity agent tracks Docker images tagged or pulled during builds (the list of images is stored in the `buildAgent/system/docker-used-images.dat` file). 
* During clean-up / freeing disk space, TeamCity agent tries to remove these images if they were not used within 3 days, 1 day, 0 on subsequent attempts to free disk space.  

Besides that, TeamCity cleans local Docker Caches using the command:

* __Since 2018.1.2__: `docker system prune --volumes`. Works for Docker v.17.06.1 or later.
* __Before 2018.1.2__: `docker system prune -a`

### Service Message to Report Pushed Image

If TeamCity (for some reason) cannot determine that an image has been pushed, a user can send a special [Service Message](build-script-interaction-with-teamcity.md) to report this information to the TeamCity server:


```Shell

##teamcity[dockerMessage type='dockerImage.push' value='<full_image_tag>,size:<size in bytes>,digest:<hash>']

```

For example:

```Shell

##teamcity[dockerMessage type='dockerImage.push' value='myRegistry/repo-test:17,size:2632,digest:sha256:8dc5a195c3dcdc7c288d16288ff3f9ab1d8a5a230e09afb9c8dc9215e861aa55']
```

__ __