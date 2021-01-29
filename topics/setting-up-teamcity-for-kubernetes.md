[//]: # (title: Setting Up TeamCity for Kubernetes)
[//]: # (auxiliary-id: Setting Up TeamCity for Kubernetes)

Since version 2020.1, the [Kubernetes Support](https://plugins.jetbrains.com/plugin/9818-kubernetes-support) plugin is bundled with TeamCity. The plugin adds the Kubernetes type of [cloud profile](agent-cloud-profile.md) and allows running [build agents](build-agent.md) in your Kubernetes cluster.

<note>

If you were using the Helm build runner, included in the external Kubernetes Support plugin, note that the built-in integration does not comprise the Helm runner. Refer to our [upgrade notes](upgrade-notes.md#Bundled+Kubernetes+Support+plugin+does+not+contain+Helm+runner) for more details.

</note>

## Requirements

TeamCity integration with Kubernetes does not depend on the `kubectl` tool and thus does not require installing it in your cluster.

Make sure the TeamCity user is allowed to perform writing operations in the [Kubernetes namespace](#kuber-namespace) used by TeamCity agents.

You might also require to configure the following privileges for your Kubernetes user role:
* Pods: `get`, `create`, `list`, `delete`.
* Deployments: `list`, `get` – if you want to create an agent [from a deployment](#Use+pod+template+from+deployment).
* Namespaces: `list` – to allow TeamCity to suggest the namespaces available on your server.

## Kubernetes Cloud Profile Configuration

To establish the integration with Kubernetes, you need to create a dedicated cloud profile in TeamCity. Open the settings of the required project and, under the __Cloud Profiles__ section, click __Create new profile__.

For general cloud profile options, refer to the [Agent Cloud Profile](agent-cloud-profile.md#Specifying+Profile+Settings) article. To configure options available only for the _Kubernetes_ cloud type, refer to the table below:

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

Specify the URL of the [Kubernetes API server](https://kubernetes.io/docs/concepts/overview/components/#kube-apiserver).

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

<anchor name="kuber-namespace"/>

Kubernetes namespace

</td>
<td>

Specify a required [Kubernetes namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/). Leave empty to use the [default namespace](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/#viewing-namespaces).

</td>

</tr>

<tr>

<td>

Authentication strategy

</td>
<td>

Select the required authentication strategy.

Depending on the selected strategy, the set of extra options will vary. Refer to the [Kubernetes documentation](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#authentication-strategies) for details on available options.

>The _Token_ field accepts any token supported by Kubernetes.

</td>

</tr>

</table>

## Adding Kubernetes Cloud Image

After configuring the general Kubernetes settings, you can proceed with [adding a new build agent image](agent-cloud-profile.md#Adding+Agent+Image).

Click __Add image__ and configure its options:

<table>

<tr>
<td>Option</td>
<td>Description</td>
</tr>

<tr>

<td>

Pod specification

</td>

<td>

Select one of the three types described [below](#Use+pod+template+from+deployment).

</td>

</tr>

<tr>

<td>

Agent pool

</td>

<td>

Assign the launched instances to a specific [agent pool](agent-pool.md).

</td>

</tr>

<tr>

<td>

Agent name prefix

</td>

<td>

(Optional) Show the same prefix for all instances of this image, when displaying them in TeamCity.

</td>

</tr>

<tr>

<td>

Max number of instances

</td>

<td>

(Optional) Set this number if you want to limit how many instances are launched based on this image.

</td>

</tr>

</table>

>To run a build agent in Kubernetes, TeamCity will create a dedicated pod in your cluster.

### Use pod template from deployment

Select this option to run a new pod based on a specific [deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) created in your Kubernetes cluster. If you are using Kubernetes Dashboard, you can find the list of available deployments under __Workloads | Deployments__.

You can create a simple deployment from the latest TeamCity agent image as follows:

```Shell
create deployment simpledep --image="jetbrains/teamcity-agent:latest"
deployment.apps/simpledep created
```

### Run single container

Select this option to run a single [container](https://kubernetes.io/docs/concepts/containers/overview/) based on any given agent Docker image from [Docker Hub](https://hub.docker.com/). You'll need to specify:

<table>

<tr>
<td>Option</td>
<td>Description</td>
</tr>

<tr>

<td>

Docker image name

</td>

<td>

Name of the Docker image, similarly to the one used with the `docker run` command (for example, `jetbrains/teamcity-agent/`).

</td>

</tr>

<tr>

<td>

Image pull policy

</td>

<td>

The policy of [updating the image](https://kubernetes.io/docs/concepts/containers/images/#updating-images).

</td>

</tr>

<tr>

<td>

Command

</td>

<td>

(Optional) Set the [command](https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/#notes) run by the container.

</td>

</tr>

<tr>

<td>

Command arguments

</td>

<td>

(Optional) Set the command arguments.

</td>

</tr>

</table>


### Use custom pod template

Enter your own [pod template](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/#pod-templates) in a YAML format. This option is recommended for accessing images located in private repositories, as it allows you to specify authentication parameters inside the template code.

Example of a simple template:

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: teamcity-agent
spec:
  restartPolicy: Never
  containers:
    - name: teamcity-agent
      image: jetbrains/teamcity-agent
      resources:
        limits:
          memory: "2Gi"
```

<seealso>
        <category ref="concepts">
            <a href="agent-cloud-profile.md">Agent Cloud Profile</a>
        </category>
        <category ref="installation">
            <a href="teamcity-integration-with-cloud-solutions.md">TeamCity Integration with Cloud Solutions</a>
        </category>
</seealso>
