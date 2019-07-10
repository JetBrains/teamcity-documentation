[//]: # (title: Configuring Connections to Docker)
[//]: # (auxiliary-id: Configuring Connections to Docker)

It is possible to configure a Docker connection in TeamCity if you need [Docker support](docker-support.md) build feature enabling you to
* log in to an authenticated registry before running a build / log out after the build 
* clean up the published images after the build. 


<chunk include-id="docker-connection">



The __Project Settings__ | __Connections__ page allows you to configure a connection to [docker.io](http://docker.io/) (default) or a private Docker registry. More than one connection can be added to the project. The connection will be available in all the subprojects and build configurations of the current project.

#### Registry Address Format

By default, [https://docker.io](https://docker.io/) is used.

To connect to a registry, use the following format: `[http(s)://]hostname:port`.

If the protocol is not specified, the connection over `https` is used by default.

#### Connecting to Private Cloud Registry

* TeamCity supports the Azure container registry storing Docker images; you can authenticate using [Service principal](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-authentication#service-principal) (the principal ID and password are used as connection credentials) or [Admin account](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-authentication#admin-account).
* __Since TeamCity 2018.1__, Amazon Elastic Container Registry (AWS ECR) is supported: specify the AWS region and your AWS Security Credentials when configuring the connection.

#### Connecting to Insecure Registry

To connect to an insecure registry:
1. Configure all TeamCity agents where Docker is installed to work with insecure repositories as stated [Docker documentation](https://docs.docker.com/registry/insecure/#deploying-a-plain-http-registry). This is sufficient to allow the connection to the private registry over http.
2. To connect to an insecure registry over https with a self\-signed certificate, in addition to the step above, import the self\-signed certificate to the JVM of the TeamCity server as described [here](using-https-to-access-teamcity-server.md#Configuring+client+JVM+for+trusting+server+certificate). You can consult the Docker documentation on [using self-signed certificates](https://docs.docker.com/registry/insecure/#using-self-signed-certificates). 

</chunk> 



 