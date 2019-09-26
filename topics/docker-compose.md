[//]: # (title: Docker Compose)
[//]: # (auxiliary-id: Docker Compose)

TeamCity-[Docker integration](integrating-teamcity-with-docker.md) includes the Docker Compose runner.

<include src="integrating-teamcity-with-docker.md" include-id="reqs-supported-env"/>

## Docker Compose

<chunk include-id="docker-compose">

The runner allows starting [Docker Compose](https://docs.docker.com/compose/) build services and shutting down those services at the end of the build.

The Docker Compose runner supports one or several [Docker Compose YAML file(s)](https://docs.docker.com/compose/compose-file/compose-file-v2/) with a description of the services to be used during the build. The path to the `docker-compose.yml` file(s) should be relative to the [checkout directory](build-checkout-directory.md). When specifying several files, separate them with a space.

The executed commands are:

```Shell

# The commands are executed with the current working directory, where the docker-compose file resides.
docker-compose -f <docker-compose.yml> [-f <docker-compose2.yml>] up -d

# At the end of the build, for each docker compose build step the build agent will run:
docker-compose -f <docker-compose.yml> [-f <docker-compose2.yml>] down -v
```


If the checkbox __pull image explicitly__ is enabled, `docker-compose pull` will be run before the `docker-compose up` command.

When using Docker Compose with images which support [HEALTHCHECK](https://docs.docker.com/engine/reference/builder/#healthcheck), TeamCity will wait for the `healthy` status of all containers that support this parameter.

If the start of Docker Compose was successful, the TeamCity agent will register the `TEAMCITY_DOCKER_NETWORK` environment variable containing the name of the Docker Compose default network. This network will be passed transparently to the [Docker Wrapper](integrating-teamcity-with-docker.md#Docker+Wrapper) when used in some build runners. 

</chunk>

__ __