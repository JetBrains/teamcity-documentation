[//]: # (title: Integrating TeamCity with Docker)
[//]: # (auxiliary-id: Integrating TeamCity with Docker)

TeamCity integration with Docker includes:
* The [Docker](docker.md) _build runner_ to launch Docker commands and create Docker images during a build.
* The [Docker Compose](docker-compose.md) _build runner_ to start services with the help of the [Docker Compose tool](https://docs.docker.com/compose/) during a build.
* The [Docker Wrapper](docker-wrapper.md) _extension_ to execute build steps inside a Docker container. Available for multiple runners.
* The [Docker Support](docker-support.md) _build feature_ to automatically sign in to a Docker registry before starting a build. This feature also adds the __Docker Info__ tab of __Build Results__ with the information about the images published to the Docker registry during the build.

You can learn more details about the listed tools in the dedicated Help articles, linked above. The following article contains information common to these tools.

>This page is about TeamCity instruments for integrating builds with Docker. If you want to learn how to run Docker inside a build agent container and read other information about the TeamCity Agent Docker images, read our documentation in [Docker Hub](https://hub.docker.com/r/jetbrains/teamcity-agent/).

## Requirements

The integration requires [Docker](https://docs.docker.com/engine/installation/) to be installed on the [build agents](build-agent.md). To use the [Docker Compose](docker-compose.md) build runner, you also need to install [Docker Compose](https://docs.docker.com/compose/install/).

TeamCity periodically checks if Docker is available on active build agents. Based on the `docker.server.version` and `docker.version` variables received from the agents, TeamCity distributes builds that use Docker only between agents with the installed Docker engine.   
If a [build configuration](managing-builds.md) uses the [Docker runner](docker.md) or the [Docker Wrapper extension](docker-wrapper.md), TeamCity automatically adds the `docker.server.version` [agent compatibility requirement](configuring-agent-requirements.md) for this configuration.

## Supported Environments

TeamCity Docker Support can run on Windows, Linux, and macOS build agents. It uses the `docker` executable on the build agent machine, so it should be runnable by the build agent user.

<note>

* On Linux, the integration will run if the installed Docker is detected.
* On Windows, the integration works for Linux and Windows container modes.
* On macOS, the official [Docker support for Mac](https://docs.docker.com/docker-for-mac/install/) should be installed for the user running the build agent.

</note>

## Parameters Reported by Agent

During the build, the build agent can report the following Docker-related parameters:

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

The [Docker CLI](https://docs.docker.com/engine/reference/commandline/docker/) version.

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

The [Docker Engine](https://docs.docker.com/engine/reference/commandline/dockerd/) version.

</td></tr><tr>

<td>

`docker.server.osType`

</td>

<td>

The Docker Engine OS platform. Supported values: `linux` or `windows`.

</td></tr></table>

>If you are using the [Command Line](command-line.md) build step (and not the specific Docker steps), these parameters can be used as [agent requirements](agent-requirements.md) to ensure your build is run only on the agents with Docker installed.

## Docker Disk Space Cleaner

Docker Disk Space Cleaner is an extension to the [Free Disk Space](free-disk-space.md) build feature ensuring a necessary amount of disk space for a build.

TeamCity regularly cleans up its related Docker images which were tagged/pulled:
* in a build with the [Docker Support](docker-support.md) build feature, __or__
* in a [Docker](docker.md) or [Docker Compose](docker-compose.md) build step, __or__
* in a build step with the enabled [Docker Wrapper](docker-wrapper.md) extension.

For such builds,  

* A TeamCity agent tracks Docker images tagged or pulled during the builds (the list of images is stored in the `buildAgent/system/docker-used-images.dat` file).
* During clean-up / freeing disk space, the TeamCity agent tries to remove these images if they have not been used within 3 days (or 1 or 0 days, on subsequent attempts to free the disk space).

Besides, TeamCity cleans local Docker Caches using the `docker system prune --volumes` command. Works for Docker v.17.06.1 or later.

## Service Message to Report Pushed Image

If, for some reason, TeamCity cannot determine that an image has been pushed, a user can send a special [service message](service-messages.md) to report this information to the TeamCity server:

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
* Paid Docker subscription users: 5000 pulls per 24 hours

A regular TeamCity agent stores a once pulled image in its cache. This allows running an indefinite number of builds using the same pulled image on a regular basis.   
However, there are few cases to consider:
* If you are using cloud agents, all required images will be downloaded every time a new cloud agent is launched.
* If the _Pull image explicitly_ option is enabled in the build step settings, the image will be downloaded in every new build run, even on a local agent. We recommend that you disable this option to prevent reaching the rate limit.
* During the [Free Disk Space](free-disk-space.md) stage of the build, TeamCity may clean up old unused Docker images from the local cache.

If previously your builds were accessing Docker Hub anonymously, you can double the number of allowed pulls by creating a Free Docker user profile and configuring a [Docker connection](configuring-connections-to-docker.md) in your TeamCity project. TeamCity agents will be able to use this connection to authenticate in Docker Hub before each build.

<seealso>
        <category ref="admin-guide">
            <a href="configuring-connections-to-docker.md">Configuring Connections to Docker</a>
            <a href="docker-compose.md">Docker Compose runner</a>
            <a href="docker.md">Docker runner</a>
            <a href="docker-support.md">Docker Support feature</a>
            <a href="docker-wrapper.md">Docker Wrapper extension</a>
        </category>
</seealso>
