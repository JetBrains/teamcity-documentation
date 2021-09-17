[//]: # (title: Configuring Connections to Docker)
[//]: # (auxiliary-id: Configuring Connections to Docker)

A preconfigured Docker connection allows you to:
* sign in to an authenticated registry before running a build / sign out after the build;
* clean up the published images after the build.

>This type of connection is a part of the TeamCity-Docker integration toolset. Refer to [this page](integrating-teamcity-with-docker.md) for information on software requirements, supported environments, and other common aspects of this integration.

You can configure a _Docker Registry_ connection on the __Project Settings | Connections__ page. TeamCity supports connections to [docker.io](http://docker.io/) (default) or private Docker registries. More than one connection can be added to a project. The connection will be available in all the subprojects and build configurations of the current project.

>After configuring the Docker Registry connection for a TeamCity project, you need to select it when adding a [Docker Support](docker-support.md) feature to the respective build configuration.
> 
{type="note"}

<anchor name="ConfiguringConnectionstoDocker-RegistryAddressFormat"/>

## Registry Address Format

By default, [`https://docker.io`](https://docker.io/) is used.

To connect to a registry, use the following format: `[http(s)://]hostname:port`.

If the protocol is not specified, the connection over `https` is used by default.

## Connecting to Private Cloud Registry

TeamCity supports the Azure container registry storing Docker images. You can authenticate using the [Service principal](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-authentication#service-principal) (the principal ID and password are used as the connection credentials) or [Admin account](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-authentication#admin-account).

Amazon Elastic Container Registry (AWS ECR) is supported: specify the AWS region and your AWS Security Credentials when configuring the connection.

## Connecting to Insecure Registry

To connect to an insecure registry:
1. Configure all TeamCity agents where Docker is installed to work with insecure repositories as stated in the [Docker documentation](https://docs.docker.com/registry/insecure/#deploying-a-plain-http-registry). This is sufficient to allow the connection to the private registry over HTTP.
2. To connect to an insecure registry over HTTPS with a self-signed certificate, in addition to the step above, import the self-signed certificate to the JVM of the TeamCity server as described [here](using-https-to-access-teamcity-server.md#Configuring+client+JVM+for+trusting+server+certificate). You can consult the Docker documentation on [using self-signed certificates](https://docs.docker.com/registry/insecure/#using-self-signed-certificates).

## Running multiple agents with Docker on one machine

TeamCity supports the case when multiple agents are running parallel builds on the same machine and connect to a Docker registry during these builds. This setup requires using different Docker environments: the `docker logout` command executed at the end of the one build should not affect the parallel build on another agent.  
To configure it, you need to specify locations of each agent's `.docker` directory. For this, define the `env.DOCKER_CONFIG=%[teamcity.agent.home.dir](agent-home-directory.md)%/system/.docker` environment variable either as a [build configuration parameter](configuring-build-parameters.md) or in the [`buildAgent.properties`](build-agent-configuration.md) file of each agent.

<seealso>
        <category ref="admin-guide">
            <a href="integrating-teamcity-with-docker.md">Integrating TeamCity with Docker</a>
            <a href="docker.md">Docker runner</a>
            <a href="docker-compose.md">Docker Compose runner</a>
            <a href="docker-support.md">Docker Support feature</a>
            <a href="docker-wrapper.md">Docker Wrapper extension</a>
        </category>
</seealso>