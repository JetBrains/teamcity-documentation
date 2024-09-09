[//]: # (title: Setting Up TeamCity for Kubernetes)
[//]: # (auxiliary-id: Setting Up TeamCity for Kubernetes)

Thanks to the [Kubernetes Support](https://plugins.jetbrains.com/plugin/9818-kubernetes-support) plugin, TeamCity can run [build agents](build-agent.md) in your Kubernetes cluster.

<note instance="tc">

If you were using the Helm build runner, included in the external Kubernetes Support plugin, note that the built-in integration does not comprise the Helm runner. Refer to our [upgrade notes](upgrade-notes.md#Bundled+Kubernetes+Support+plugin+does+not+contain+Helm+runner) for more details.

</note>

## Requirements

TeamCity integration with Kubernetes does not depend on the `kubectl` tool and thus does not require installing it in your cluster.

Make sure the TeamCity user is allowed to perform writing operations in the [Kubernetes namespace](#kuber-namespace) used by TeamCity agents.

You might also require to configure the following privileges for your Kubernetes user role:
* Pods: `get`, `create`, `list`, `delete`.
* Deployments: `list`, `get` — if you want to create an agent pod using [a deployment configuration](#Use+pod+template+from+deployment).
* Namespaces: `get`, `list` — to allow TeamCity to suggest the namespaces available on your server.

Here is an example of setting up all required permissions via [Kubernetes RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/):

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: teamcity:manage-agents
rules:
- apiGroups: [""]
  resources: ["namespaces"]
  verbs: ["list", "get"]
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "create", "list", "delete"]
- apiGroups: ["extensions", "apps"]
  resources: ["deployments"]
  verbs: ["list", "get"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: teamcity:manage-agents
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: teamcity:manage-agents
subjects:
  # proper RoleBinding subject depends on your Authentication strategy
  # use one of examples below

  # if you use OIDC/Certificate auth strategies
  - kind: User
    name: teamcity

  # if you use Service account
  - kind: ServiceAccount
    name: teamcity
```

## Kubernetes Cloud Profile Configuration

To establish the integration with Kubernetes, you need to create a dedicated cloud profile in TeamCity. Open the settings of the required project and, under the __Cloud Profiles__ section, click __Create new profile__.

The table below lists options specific for Kubernetes cloud profiles.

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

>The _Token_ strategy accepts any token types supported by Kubernetes.

</td>

</tr>

</table>

## Adding Kubernetes Cloud Image

After configuring the general Kubernetes settings, you can proceed with adding a new build agent image.

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

Assign the launched instances to a specific [agent pool](configuring-agent-pools.md).

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

Select this option to run a new pod based on a specific [deployment configuration](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) created in your Kubernetes cluster. If you are using Kubernetes Dashboard, you can find the list of available deployments under __Workloads | Deployments__.

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

Name of the Docker image, similarly to the one used with the `docker run` command (for example, `jetbrains/teamcity-agent`).

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

Enter your own [pod template](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/#pod-templates) in a YAML format. Use this option when you need to specify additional configuration for a launched pod, such as resources' requests/limits or credentials. This option is alternative to the _template from deployment_ specification and allows editing the configuration directly in the TeamCity UI/DSL.

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

## Kotlin DSL

To create and setup k8s profiles and images, create [KubernetesCloudProfile](https://www.jetbrains.com/help/teamcity/kotlin-dsl-documentation/root/kubernetes-cloud-profile/index.html?query=KubernetesCloudProfile) and [KubernetesCloudImage](https://www.jetbrains.com/help/teamcity/kotlin-dsl-documentation/root/kubernetes-cloud-image/index.html) class instances under the project's `features` group. Set an image `profileId` property to the profile `id` value to assign this image to the target profile.


```Kotlin
project {
    // ...
    features {
        // ...
        kubernetesCloudProfile {
            id = "kube-1"
            name = "K8S Agents"
            description = "EKS"
            serverURL = "https://myteamcityserver.com"
            terminateIdleMinutes = 30
            apiServerURL = "https://123.gr7.eu-west-1.eks.amazonaws.com"
            caCertData = "abc"
            authStrategy = eks {
                accessId = "aws_access_id"
                secretKey = "aws_key"
                clusterName = "my-k8s-cluster"
            }
        }
        kubernetesCloudImage {
            id = "PROJECT_EXT_10"
            profileId = "kube-1"
            agentPoolId = "21"
            agentNamePrefix = "k8s-singlec"
            maxInstancesCount = 5
            podSpecification = runContainer {
                dockerImage = "jetbrains/teamcity-agent"
            }
        }
        kubernetesCloudImage {
            id = "PROJECT_EXT_11"
            profileId = "kube-1"
            agentPoolId = "21"
            agentNamePrefix = "k8s-pspec"
            maxInstancesCount = 5
            podSpecification = customTemplate {
                customPod = """
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
                """.trimIndent()
            }
        }
    }
}
```

See [KubernetesCloudProfile](https://www.jetbrains.com/help/teamcity/kotlin-dsl-documentation/root/kubernetes-cloud-profile/index.html?query=KubernetesCloudProfile) and [KubernetesCloudImage](https://www.jetbrains.com/help/teamcity/kotlin-dsl-documentation/root/kubernetes-cloud-image/index.html) for more examples and API descriptions.


<seealso>
        <category ref="concepts">
            <a href="agent-cloud-profile.md">Agent Cloud Profile</a>
        </category>
        <category ref="installation">
            <a href="teamcity-integration-with-cloud-solutions.md">TeamCity Integration with Cloud Solutions</a>
        </category>
</seealso>
