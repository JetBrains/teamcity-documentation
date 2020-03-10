[//]: # (title: Docker Support)
[//]: # (auxiliary-id: Docker Support)

TeamCity comes with built-in [Docker integration](integrating-teamcity-with-docker.md), which includes the Docker Support build feature.

<chunk include-id="docker-support">

[Adding this build feature](adding-build-features.md) will enable Docker events monitoring: such operations as `docker pull` and `docker run` will be detected.   
The build feature adds the _Docker Info_ tab to the [build results](working-with-build-results.md) page providing information on Docker-related operations. It also provides the following options:
* ability to clean-up the images
* automatic login to an authenticated registry before the build and logout of it after the build

These options require a configured connection to a docker registry:

<img src="docker-support.png" width="750" alt="Docker Support build feature"/>

#### Clean-up of images

If you have a build configuration which publishes images, you need to remove them at some point. You can select the corresponding option and instruct TeamCity to remove the images published by a certain build when the build itself is [cleaned up](clean-up.md). It works as follows: when an image is published, TeamCity stores the information about the registry of the images published by the build. When the [server clean-up](clean-up.md) is run and it deletes the build, all the configured connections are searched for the address of this registry, and the images published by the build are cleaned up using the credentials specified in the connection found.

#### Automatic Login to/Logout of Docker Registry

If you need to log in to a registry requiring authentication before a build, select the corresponding option and a connection to Docker configured in the __Project Settings__.   
Automatic logout will be performed after the build finishes.

</chunk>

__ __