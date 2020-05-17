[//]: # (title: Setting Up TeamCity for Kubernetes)
[//]: # (auxiliary-id: Setting Up TeamCity for Kubernetes)

Since version 2020.1, the [Kubernetes Support](https://plugins.jetbrains.com/plugin/9818-kubernetes-support) plugin is bundled with TeamCity. The plugin adds the Kubernetes type of [cloud profile](agent-cloud-profile.md) and allows running [build agents](build-agent.md) in your K8S cluster.

<note>

If you were using the Helm build runner, included in the external Kubernetes Support plugin, note that the built-in integration does not comprise the Helm runner. Refer to our [upgrade notes](upgrade-notes.md#Bundled+Kubernetes+Support+plugin+does+not+contain+Helm+runner) for more details.

</note>

## Requirements

TeamCity integration with Kubernetes does not depend on the `kubectl` tool and thus does not require installing it in a cluster.

Make sure the TeamCity user is allowed to perform writing operations in the Kubernetes namespace specified in the cloud profile settings.

## Kubernetes Cloud Profile Configuration

For general cloud profile options, refer to the [Agent Cloud Profile](agent-cloud-profile.md#Specifying+Profile+Settings) page.

To configure options available for the _Kubernetes_ cloud type, refer to the table below:

<table>

<tr>

<td>Option</td>
<td>Description</td>

</tr>

<tr>

<td>

Kubernetes API server URL

</td>
<td>

Specify the URL of the [API server](https://kubernetes.io/docs/concepts/overview/components/#kube-apiserver).

</td>

</tr>

<tr>

<td>

Certificate Authority (CA)

</td>
<td>

Enter the content of the [CA certificate](https://kubernetes.io/docs/concepts/cluster-administration/certificates/) for your cluster.

</td>

</tr>

<tr>

<td>

Kubernetes namespace

</td>
<td>

Specify a [Kubernetes namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/). Leave empty to use the [default namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/#viewing-namespaces).

</td>

</tr>

<tr>

<td>

Authentication strategy

</td>
<td>

Select the required authentication strategy.

Depending on the selected strategy, the set of extra options will vary. Refer to the [Kubernetes documentation](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#authentication-strategies) for details on available options.

</td>

</tr>

</table>

## Adding Kubernetes Cloud Image

To run a build agent in Kubernetes, TeamCity needs to create a separate pod in your cluster.

When [adding an image](agent-cloud-profile.md#Adding+Agent+Image) to the Kubernetes cloud profile, you can choose one of the three pod specification types:

* __Use pod template from deployment__   
Run a new pod based on a [deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) created in your cluster.
* __Run single container__   
Run a single [container](https://kubernetes.io/docs/concepts/containers/overview/) based on any given agent Docker image.
* __Use custom pod template__   
Enter your own [pod template](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/#pod-templates) in a YAML format. This option is recommended for accessing images located in private repositories: you can specify authentication parameters inside the template code.

<seealso>
        <category ref="concepts">
            <a href="agent-cloud-profile.md">Agent Cloud Profile</a>
        </category>
        <category ref="installation">
            <a href="teamcity-integration-with-cloud-solutions.md">TeamCity Integration with Cloud Solutions</a>
        </category>
</seealso>