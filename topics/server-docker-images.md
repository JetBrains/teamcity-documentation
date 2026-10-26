[//]: # (title: Server Docker Images)
[//]: # (help-id: Server Docker Images)

Instead of installing the TeamCity server from a distribution package, you can run it from the official JetBrains **TeamCity Server** Docker image. JetBrains publishes images for Linux and Windows (including Windows Server Core and Nano Server variants) across major CPU architectures.

To start a server container, mount a host directory as the [TeamCity Data Directory](teamcity-data-directory.md) and publish the server port:

```Shell
docker run -it --name teamcity-server \
    -v <path to data directory>:/data/teamcity_server/datadir \
    -p 8111:8111 \
    jetbrains/teamcity-server
```

Always mount the data directory from the host. Otherwise, your projects, builds, and server settings are lost when the container is removed.

> Pin a specific version tag (for example, `jetbrains/teamcity-server:%product-version%`) instead of `latest`. Doing so keeps an accidental container restart from pulling a newer major version, and makes [upgrades](upgrading-teamcity-server-and-agents.md#manual-update-of-docker-image) an explicit action.
>
{style="note"}

A containerized server cannot use the [automatic update](upgrading-teamcity-server-and-agents.md#Automatic+Update). To move to a newer version, pull the new image and start a container from it against the same data directory, as described in [](upgrading-teamcity-server-and-agents.md#manual-update-of-docker-image).

| | Where to look |
|---|---|
| Image registry and available tags | [Docker Hub](https://hub.docker.com/r/jetbrains/teamcity-server/tags) |
| Installation instructions, environment variables, limitations, and customization options | [Docker Hub](https://hub.docker.com/r/jetbrains/teamcity-server) |
| The list of components installed in server images | [GitHub](https://github.com/JetBrains/teamcity-docker-images/blob/master/context/generated/teamcity-server.md) |
| Docker Compose samples (run a TeamCity server and agents together) | [GitHub](https://github.com/JetBrains/teamcity-docker-samples) |

<seealso>
        <category ref="admin-guide">
            <a href="agent-docker-images.md">Agent Docker Images</a>
            <a href="teamcity-kubernetes-operator.md">Kubernetes Operator</a>
        </category>
</seealso>
