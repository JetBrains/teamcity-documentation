[//]: # (title: Docker Support)
[//]: # (auxiliary-id: Docker Support)

The _Docker Support_ [build feature](adding-build-features.md) allows automatically signing in to a Docker registry before the build start.

Adding this feature:
* enables the Docker events' monitoring: such operations as `docker pull` and `docker run` will be detected by TeamCity;
* adds the __Docker Info__ tab to the [__Build Results__](working-with-build-results.md) page. The tab provides information on Docker-related operations.

The feature also allows:
* cleaning up the Docker images;
* automatically log in to an authenticated registry before the build and log out of it after the build.

These two options require configuring a [connection to a Docker registry](configuring-connections-to-docker.md):

<img src="docker-support.png" width="750" alt="Docker Support build feature"/>

>_Docker Support_ is a part of the TeamCity-Docker integration toolset. Refer to [this page](integrating-teamcity-with-docker.md) for information on software requirements, supported environments, and other common aspects of this integration.

## Docker Images Clean-up

If you have a build configuration which publishes images, you need to remove them at some point. You can select the corresponding option and instruct TeamCity to remove the images published by a certain build when the build itself is [cleaned up](clean-up.md).  
It works as follows: when an image is published, TeamCity stores the information about the registry of the images published by the build. When the [server clean-up](clean-up.md) is run and it deletes the build, all the configured connections are searched for the address of this registry, and the images published by the build are cleaned up using the credentials specified in the found connection.

## Docker Registry Automatic Login/Logout

If you need to log in to a registry requiring authentication before a build, select the corresponding option and a connection to Docker configured in the __Project Settings__. Automatic logout will be performed after the build finishes.

>[See also](integrating-teamcity-with-docker.md#Conforming+with+Docker+download+rate+limits) how to use this functionality to double the number of pulls allowed to a Free Docker Hub user profile.

<seealso>
        <category ref="admin-guide">
            <a href="integrating-teamcity-with-docker.md">Integrating TeamCity with Docker</a>
            <a href="configuring-connections-to-docker.md">Configuring Connections to Docker</a>
            <a href="docker-compose.md">Docker Compose runner</a>
            <a href="docker.md">Docker runner</a>
            <a href="docker-wrapper.md">Docker Wrapper extension</a>
        </category>
</seealso>