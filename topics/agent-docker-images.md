[//]: # (title: Agent Docker Images)

Instead of manually installing TeamCity agents and setting up required build software, you can do the following:

* Pull a required JetBrains "TeamCity Agent" Docker image. You can choose between a "minimal" (the basic agent image without any 3rd-party tools) and regular/full (bundled with multiple tools such as Git and .NET Runtime) Docker images.

* Execute the `docker run ...` command to start a container with a TeamCity agent running within.

    ```Shell
    docker run -e SERVER_URL="<url to TeamCity server>"  \ 
        -v <path to agent config folder>:/data/teamcity_agent/conf  \      
        jetbrains/teamcity-agent
    ```
  
Running TeamCity agents inside Docker container is a part of a broad TeamCity-Docker/Podman integration toolset. Refer to this documentation article for information on software requirements, supported environments, and other common aspects of this integration: [](integrating-teamcity-with-container-managers.md#Compatibility+and+Requirements).

> You can also start a TeamCity server inside a container. See instructions on this page for more information: [jetbrains/teamcity-server](https://hub.docker.com/r/jetbrains/teamcity-server).
>
{style="tip"}

> Since 3rd-party tool vendors may not provide binaries for every OS/architecture, some images may include fewer installed components compared to the others. For instance, ARM-based images do not include Perforce CLI (p4) and Windows images ship without Mercurial.
>
{style="note"}


|                                                                                                  | Regular Agent Image                                                                                           | Minimal Agent Image                                                                                                   |
|--------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| Image registries and available tags                                                              | [Docker Hub](https://hub.docker.com/r/jetbrains/teamcity-agent/tags)                                          | [Docker Hub](https://hub.docker.com/r/jetbrains/teamcity-minimal-agent/tags)                                          |
| Installation instructions, limitations, and customization options                                | [Docker Hub](https://hub.docker.com/r/jetbrains/teamcity-agent)                                               | [Docker Hub](https://hub.docker.com/r/jetbrains/teamcity-minimal-agent)                                               |
| The list of components installed in agent images                                                 | [GitHub](https://github.com/JetBrains/teamcity-docker-images/blob/master/context/generated/teamcity-agent.md) | [GitHub](https://github.com/JetBrains/teamcity-docker-images/blob/master/context/generated/teamcity-minimal-agent.md) |
| Docker Compose samples<br/>(allow you to run a TeamCity server and agents in a single container) |                             [GitHub](https://github.com/JetBrains/teamcity-docker-samples)                    ||

