[//]: # (title: Executor Mode: External Kubernetes Integration)
[//]: # (help-id: Executor Mode: External Kubernetes Integration;Executor Mode: Agentless Kubernetes Integration;Kubernetes Executor)


<include from="setting-up-teamcity-for-kubernetes.md" element-id="k8s-integration-types"/>


This article explains the native integration approach. To learn about the traditional integration instead, refer to the [](setting-up-teamcity-for-kubernetes.md) topic.

## How It Works

1. Create a [Kubernetes connection](configuring-connections.md#Kubernetes) that will allow TeamCity to access your K8s cluster.
2. In project settings, go to the **Cloud Profiles** section and click **Create New Profile**.
3. Click the **Kubernetes** tile under the "Offload your tasks to external agents" section.
4. Set your K8s integration as follows:
    * **Connection** — choose a connection created in step 1.
    * **Server URL** — enter your TeamCity server URL or leave empty to use the URL specified on the server's [Global Settings](configuring-server-url.md) page.
    {instance="tc"}
    * **Server URL** — enter your TeamCity server URL or leave empty to use the default server URL.
    {instance="tcc"}
    * **Pod template** — choose a required pod configuration. See the [](#Pod+Templates) section for more information.
    * **Maximum number of builds** — enter the cluster capacity. When this capacity is reached, new builds will remain queued unless currently ongoing builds are finished.
    <!--* **Parameters** — enter the list of [parameters](configuring-build-parameters.md) (in the `name=value` format) that should be present on K8s containers. These parameters will be matched to [explicit agent requirements](configuring-agent-requirements.md) of queued builds. As a result, you can specify which builds can be run in your K8s cluster.
        
        For example, add the `k8s=yes` value to this field in order to offload builds with the `equals("k8s", "yes")` agent requirement.
        
        > Parameters written to pods are not accessible to builds running on them. That is, you cannot access these parameters within build processes (for example, by echoing their values) or view them on the [Build Results](build-results-page.md#Parameters+Tab) page or via the `/app/rest/builds/id:<Int32>?fields=properties(property)` REST API requests. 
        > 
        > These parameters are used solely to match executors with configuration requirements.
        > 
        {style="note"}
   -->
   
5. In your build configuration settings, specify [agent requirements](configuring-agent-requirements.md) and [step containers](container-wrapper.md) if needed.
6. Trigger a new build.
7. The TeamCity K8s executor collects a list of build steps with their parameters, generates a pod definition, and submits it to K8s cluster. Each build step runs in a separate container, which allows you to specify different [images](container-wrapper.md) for individual steps.
8. The K8s cluster allocates pods required to run a build and starts it.


## Cluster Permissions

Make sure the TeamCity user is allowed to perform writing operations in the Kubernetes namespace. Your Kubernetes user role must be configured with the following permissions:

* Pods: `get`, `create`, `list`, `delete`.
* Pod templates: `get`, `list`
* Namespaces: `get`, `list` — to allow TeamCity to suggest the namespaces available on your server.

The following sample illustrates all required permissions configured via [Kubernetes RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/):

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: teamcity:executor
rules:
  - apiGroups: [""]
    resources: [namespaces]
    verbs: ["get", "list"]
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get", "list", "create", "delete"]
  - apiGroups: [""]
    resources: [podtemplates]
    verbs: ["get","list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: teamcity:executor
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: teamcity:executor
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

## Pod Templates

The `podtemplates list` [permission](#Cluster+Permissions) enables TeamCity to access the list of [pod templates](https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/#pod-templates) stored in your cluster (under the same namespace as specified in the selected [Kubernetes connection](configuring-connections.md#Kubernetes). The retrieved templates are displayed in the **Pod Templates** drop-down menu.

The sample template below launches pods that have 2GB of memory and 25Gb of storage, and use a custom build agent image (see the [](#Special+Notes+and+Limitations) section). You can also explicitly declare [build parameters](configuring-build-parameters.md) in YAML markup. These parameters and their values are matched against explicit agent requirements to match compatible executors with queued builds.

```yaml
apiVersion: v1
kind: PodTemplate
metadata:
  name: my-template
  namespace: default
template:
  spec:
    containers:
      - name: template-container # see the limitations section
        image: johndoe/custom_agent_image:latest
        resources:
          limits:
            ephemeral-storage: 25Gi
            memory: 2Gi
          requests:
            ephemeral-storage: 25Gi
        env:
          - name: JDK_1_8
            value: /usr/local/openjdk-8
    nodeSelector:
      linux: arm64
```


## Agent Priority

If your build configuration has a mix of different agent options, TeamCity uses the following logic to delegate queued builds:

* Self-hosted agents have the highest priority
* If there is no free self-hosted agent compatible with a build, TeamCity looks for a suitable [cloud agent](teamcity-integration-with-cloud-solutions.md)
* If neither of these options are available, a build is offloaded to an external executor

See the following article to learn more about agent priorities: [](install-and-start-teamcity-agents.md#Agent+Priority).

## Licensing

Although Kubernetes-based builds do not occupy native TeamCity agents, their number is still limited by your [agent license](licensing-policy.md). The combined number of "native" and executor builds cannot exceed this licensed limit. When the limit is reached, new builds will remain queued with the "Maximum number of concurrent builds reached" message, waiting for a free agent slot.
{instance="tc"}

Although Kubernetes-based builds do not occupy native TeamCity agents, regular [self-hosted agent licensing policies](teamcity-cloud-subscription-and-licensing.md#cloud-self-hosted-agent-licensing) apply. The combined number of executor builds and builds running on self-hosted agents cannot exceed the number of purchased monthly slots. When the limit is reached, new builds will remain queued, waiting for a free slot.
{instance="tcc"}

[Detached builds](detaching-build-from-agent.md) and those spawned by [composite build configurations](composite-build-configuration.md) do not occupy agent slots and can run without restrictions.

## Special Notes and Limitations

* Currently, a project can use only one Kubernetes integration. We expect to support multiple executors per project (along with a mechanism to prioritize them) in future release cycles.
* A Kubernetes cluster acts as an external orchestrator that processes builds without using "classic" build agents connected to a TeamCity server. This leads to a "Build agent was disconnected while running a build" warning displayed when a build handled by an executor is running. As long as builds finish successfully, this warning does not indicate a misconfiguration or connectivity issue and can be disregarded. We expect to resolve this behavior in upcoming bug-fix releases.
* [Pod templates](#Pod+Templates) that specify custom container properties must have the "template-container" container names.

    ```yaml
    # ...
    template:
      spec:
        containers:
          - name: template-container
            image: johndoe/custom_agent_image:latest
    # ...
    ```
    
    Otherwise, the container will use default settings. For example, it will override the `image` property in favor of the standard "jetbrains/teamcity-agent:latest" image.
* Currently, the Kubernetes executor does not support Windows nodes. Builds handled by these nodes are stuck in the "Setting up resources" phase with pods displaying the `MountVolume.SetUp failed for volume "kube-api-access-sfhbc"` error. For that reason, builds designed to run under Windows cannot be delegated to Kubernetes executor.

    To avoid this issue for mixed clusters (with both Windows and Linux nodes), specify the required node in [pod templates](#Pod+Templates):

    ```yaml
    spec:
      containers:
      # ...
      nodeSelector:
        kubernetes.io/os: linux
    ```
  
* The [](docker.md) build step is not supported.
* The Docker inside Docker (DinD) setup is not supported.
* Pod initialization can stall while cleaning the "/agent/temp/.old" directory.
* Advanced [Container Wrapper](container-wrapper.md) are not available in build steps if the configuration's parent project has a configured Kubernetes Executor.