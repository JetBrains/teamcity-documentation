# Darwin Information Typing Architecture

## Project and Configuration Settings



<var name="tab-name" value="tab-name"/>
<snippet id="open-project-settings-tab">Open <a href="project-administrator-guide.md#Edit+and+View+Modes">project settings</a> and navigate to the <b>%tab-name%</b> settings tab.</snippet>


<var name="configuration-tab-name" value="configuration-tab-name"/>
<snippet id="open-configuration-settings-tab">Open <a href="project-administrator-guide.md#Edit+and+View+Modes">configuration settings</a> and navigate to the <b>%configuration-tab-name%</b> settings tab.</snippet>


## Project Features

<var name="tab-name" value="tab-name"/><var name="button-name" value="button-name"/>
<snippet id="open-settings-create-new-entity">Open the <b>%tab-name%</b> settings tab and click the <b>%button-name%</b> button.</snippet>


## Connections

<snippet id="create-new-connection">Click <b>Add Connection</b>. Note that connections can be used only in their parent projects and their subprojects. If you want a connection to be available globally, add it to the <b>Root</b> project.</snippet>

<var name="connection-type" value="connection-type"/>
<snippet id="choose-connection-type">Select "%connection-type%" in the <b>Connection Type</b> drop-down menu.</snippet>

<var name="unique-url-sample" value="unique-url-sample"/>
<snippet id="connections-unique-callback-URL">
Ensure the <b>Enable unique callback URL</b> setting is enabled to generate a unique ID added to your callback URL. This setting bolsters the security of your setup by mitigating the risk of mix-up attacks: attacks utilizing malicious authorization servers that impersonate real auth servers to trick a victim client into leaking an authorization code (token). Using the <code>%unique-url-sample%</code> URL format ensures an attacker cannot hand-craft an address acknowledged by TeamCity.

<note>
<ul>
<li>Whenever you toggle this setting on or off, the callback URL changes. Update OAuth settings on the VCS side accordingly.</li>
<li>IDs are unique for every connection, including copies of existing connections. If you clone a connection with this setting enabled, remember to update your VCS OAuth settings.</li>
</ul>
</note>
</snippet>


<snippet id="test-and-save-connection">Click <b>Test connection</b> to verify TeamCity can access your resources, and save your new connection.</snippet>


## Auth Methods


<snippet id="rat-single"><b>Refreshable access tokens</b> are short-lived tokens acquired by TeamCity from a required VCS provider via existing OAuth connections (as opposed to static PAT tokens issued manually by users on a VCS hosting side). See the following article for more information on generating and using refreshable tokens: <a href="manage-access-tokens.md"></a>.</snippet>


## K8S

<snippet id="kubernetes-settings-api-server-url">Specify the URL of the <a href="https://kubernetes.io/docs/concepts/overview/components/#kube-apiserver">Kubernetes API server</a>.</snippet>

<snippet id="kubernetes-settings-certificate-authority">Enter the content of the <a href="https://kubernetes.io/docs/concepts/cluster-administration/certificates/">CA certificate</a> for your cluster.</snippet>


<snippet id="kubernetes-settings-namespace">Specify a required <a href="https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/">Kubernetes namespace</a>. Leave empty to use the <a href="https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/#viewing-namespaces">default namespace</a>.</snippet>

<snippet id="kubernetes-settings-auth-strategy">

Select the required authentication strategy. Depending on the selected strategy, the set of additional options will vary. Refer to the <a href="https://kubernetes.io/docs/reference/access-authn-authz/authentication/#authentication-strategies">Kubernetes documentation</a> for details on available options.

<note>The <b>Token</b> strategy accepts any token types supported by Kubernetes.</note>

</snippet>

<snippet id="kubernetes-settings-proxy-url">

Specify the URL of your proxy server in the `protocol://address:port` format.

</snippet>

<snippet id="kubernetes-settings-proxy-credentials">

Enter the proxy server credentials.

</snippet>

<snippet id="kubernetes-settings-proxy-noproxy">

Specify the hosts that should be available directly, without routing through the proxy server. Typically, these are internal resources from your local network. Use comma to separate multiple entries; for example: `http://localhost,*.mydomain.com`.

</snippet>

## Docker

<var name="docker-feature-name" value="docker-feature-name"/>



<snippet id="docker-integration-note">
<note>
<b>%docker-feature-name%</b> is a part of the TeamCity-Docker/Podman integration toolset. Refer to this documentation article for information on software requirements, supported environments, and other common aspects of this integration: <a href="integrating-teamcity-with-container-managers.md"></a>.
</note>
</snippet>



## Java

<snippet id="java-deprecation-warning">
<warning>
Starting with version 2026.1, TeamCity servers and agents will not be able to start under Java versions older than 21. TeamCity build agents support Java 21 and newer, while the server supports only Java 21.

See <a href="java-21.md">this documentation article</a> for upgrade instructions.
{instance="tc"}

See [](configure-java-for-agent.md) for agent upgrade information.
{instance="tcc"}
</warning>
</snippet>


## Maven

<snippet id="maven-2-deprecation">Maven 2.x has reached end of life and is no longer supported by Apache (see the official [EOL announcement](https://maven.apache.org/maven-2.x-eol.html)). Accordingly, TeamCity 2026.1 and later will also drop support for Maven 2. Builds using this version may still run, but advanced features such as test reporting and incremental building will no longer be available.<br/><br/>Please switch your Maven build steps to any custom or bundled Maven 3.x version to use a fully supported Maven version.</snippet>

## Encryption

<var name="operation-name-ev" value="operation-name-ev"/>
<snippet id="env-imported-encryption-key-warning">
<warning>
Note that a TeamCity server cannot %operation-name-ev% from another server that uses custom encryption keys missing from this server. In this case you need to add all encryption keys from the source server to the target one. See the following article to learn more: <a href="teamcity-configuration-and-maintenance.md#encryption-settings" instance="tc">Encryption Settings</a>.
</warning>
</snippet>


## Steps

<snippet id="build-step-run-in-docker">
<p>This build step can run inside a container deployed by Docker or Podman.</p>

<procedure>
<tabs>

<tab title="Classic build configurations">
<p>Classic build configuration steps display a set of properties that allow you to specify the image name, platform, and additional run arguments. The <b>Pull image explicitly</b> ensures TeamCity pulls an image from the target container every time this step runs.</p>
<p><img src="dk-docker-container-settings.png"/></p>
<p>To point TeamCity to a registry where it should look for the specified image, add <a href="configuring-connections-to-docker.md">Docker/Podman connection</a> to your project. By default, this connection allows TeamCity to pull images from <a href="https://hub.docker.com">Docker Hub</a> in anonymous mode, but you can set it up for any container registry.</p>
<p>See the following article for more information: <a href="container-wrapper.md">Container Wrapper</a>.</p>
</tab>

<tab title="Pipelines">
<p>Toggle <b>Run in Docker</b> on to run a step inside a container. When enabled, this element displays two options.</p>
<p><img src="dk-run-in-docker-pipeline.png" width="706" thumbnail="true" alt="Run pipeline step in a container"/></p>
<ul>

<li><b>Docker image</b> — allows you to pull an image from a Docker or Podman registry. By default, TeamCity can pull Docker Hub images in anonymous mode. For other cases (private images, custom image registries, non-anonymous mode that ensures you do not violate Docker Hub rate limits), configure a <a href="pipeline-settings.md#Integrations">Docker integration</a> on a pipeline or job level.</li>

<li><b>Dockerfile</b> — allows you to build a custom image from a Dockerfile.</li>

</ul>

</tab>
</tabs>
</procedure>
</snippet>

<snippet id="step-settings-working-dir">The directory where the build step starts. By default, this is the same root directory where the agent checks out remote sources. See this topic for more information: <a href="build-working-directory.md">Build Working Directory</a>.</snippet>


<snippet id="step-settings-config-as-code">
The following snippets illustrate a customized build step in both [YAML](pipelines-yaml-syntax.md) (only pipelines) and [Kotlin DSL](kotlin-dsl.md) formats.
</snippet>