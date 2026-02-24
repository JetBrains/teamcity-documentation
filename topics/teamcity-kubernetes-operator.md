# Kubernetes Operator: Deploy a TeamCity Server in a Kubernetes Cluster

[Kubernetes](https://kubernetes.io) provides a number of features and tools that helps you deploy stable, reliable, and scalable applications with zero downtime updates.


## Manual Deployment

When deploying a server in a Kubernetes cluster, the following TeamCity features might affect your final setup and the overall approach.


<deflist type="full">

<def title="Multinode setup" id="multinode">

As with any continuously running processes, pods and individual containers are bound to fail or get evicted at some point. If your application runs in a single pod, these incidents lead to your entire workflow failing in case Kubernetes administrators are unable to spot and manually replace a failed pod in a timely fashion.

To offset this threat, facilitate pod self-healing, and ultimately ensure the integrity of your K8S workflows, Kubernetes supports replicas — identical instances or copies of the same pod running simultaneously on one or multiple nodes. When a pod fails, an available replica takes its place, and Kubernetes restarts a failed pod to restore the desired number of replicas.

TeamCity does not support completely identical instances of a server running simultaneously. When multiple instances are present, TeamCity treats them as parts of its [multinode setup](multinode-setup.md) and asks you to rank them as main or secondary nodes. This means you will need to create N deployments with different `TEAMCITY_SERVER_OPTS` environment variable values to assign correct [node responsibilities](multinode-setup.md#Responsibilities) to TeamCity nodes.

</def>

<def title="Shared resources">

In a production setup, you need to ensure all TeamCity nodes have access to:

* an [external database](set-up-external-database.md) that stores build history, users, build results, and more.
* a [data directory](teamcity-data-directory.md) that stores all configuration files, server settings, and other crucial data.

A data directory must be an NFS/SMB volume ensure its availability for multiple virtual machines (K8S nodes).

</def>

</deflist>


### Example 1: Test Setup

The following manifest illustrates a simple test setup with one TeamCity node and an external database.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: teamcity-server
spec:
  replicas: 1 #always 1
  selector:
    matchLabels:
      app: teamcity-server
  template:
    metadata:
      labels:
        app: teamcity-server
    spec:
      containers:
        - name: teamcity-server
          image: jetbrains/teamcity-server:latest
          env:
            - name: TEAMCITY_SERVER_OPTS
              value: -Dteamcity.server.rootURL=http://$(POD_NAME).$(POD_NAMESPACE)
            - name: TEAMCITY_DATA_PATH
              value: /data/teamcity_server/datadir
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  apiVersion: v1
                  fieldPath: metadata.name
            - name: POD_NAMESPACE
              valueFrom:
                fieldRef:
                  apiVersion: v1
                  fieldPath: metadata.namespace
          ports:
            - containerPort: 8111
          volumeMounts:
            - name: teamcity-data
              mountPath: /data/teamcity_server/datadir
      volumes:
        - name: teamcity-data
          emptyDir: {}
```

### Example 2: Two TeamCity Replicas on One Virtual Machine

The sample below implements a more stable solution with two replicas and an external database. However, to simplify the setup, both are running on the same virtual machine and using a local directory as [TeamCity data directory](teamcity-data-directory.md).

> This deployment declares two identical TeamCity nodes. As stated in the [Multinode setup](#multinode) block above, this behavior is not supported in TeamCity. The first time you access TeamCity UI, you will need to label one of the two replicas as the main node.
> 
{style="note"}

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  ports:
    - port: 3306
  selector:
    app: mysql
  clusterIP: None
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: mysql-initdb-config
data:
  init.sql: |
    CREATE DATABASE IF NOT EXISTS teamcity;
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
spec:
  selector:
    matchLabels:
      app: mysql
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: mysql
    spec:
      volumes:
        - name: mysql-initdb
          configMap:
            name: mysql-initdb-config
      containers:
        - image: mysql:8
          name: mysql
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: password
          ports:
            - containerPort: 3306
              name: mysql
          volumeMounts:
            - name: mysql-initdb
              mountPath: /docker-entrypoint-initdb.d
---
#based on database.properties
apiVersion: v1
data:
  connectionProperties.password: cGFzc3dvcmQ=
  connectionProperties.user: cm9vdA==
  connectionUrl: amRiYzpteXNxbDovL215c3FsLmRlZmF1bHQ6MzMwNi90ZWFtY2l0eQ==
kind: Secret
metadata:
  name: database-properties
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: teamcity-server
spec:
  replicas: 2
  selector:
    matchLabels:
      app: teamcity-server
  template:
    metadata:
      labels:
        app: teamcity-server
    spec:
      containers:
        - name: teamcity-server
          image: jetbrains/teamcity-server:latest
          env:
            - name: TEAMCITY_DATA_PATH
              value: /data/teamcity_server/datadir
            - name: TEAMCITY_DB_USER
              valueFrom:
                secretKeyRef:
                  key: connectionProperties.user
                  name: database-properties
            - name: TEAMCITY_DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  key: connectionProperties.password
                  name: database-properties
            - name: TEAMCITY_DB_URL
              valueFrom:
                secretKeyRef:
                  key: connectionUrl
                  name: database-properties
          ports:
            - containerPort: 8111
          volumeMounts:
            - name: teamcity-data
              mountPath: /data/teamcity_server/datadir
      volumes:
        - name: teamcity-data
          persistentVolumeClaim:
            claimName: teamcity-node-volume
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: teamcity-node-volume
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: standard
```

### Example 3: Multiple Nodes With Different Responsibilities

The sample below configures two separate deployment layers, for the main and secondary nodes respectively. The `env` section of each deployment sets the `TEAMCITY_SERVER_OPTS` environment variable to provide different responsibilities for each of these nodes.

```yaml
#database deployment
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  ports:
    - port: 3306
  selector:
    app: mysql
  clusterIP: None
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: mysql-initdb-config
data:
  init.sql: |
    CREATE DATABASE IF NOT EXISTS teamcity;
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
spec:
  selector:
    matchLabels:
      app: mysql
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: mysql
    spec:
      volumes:
        - name: mysql-initdb
          configMap:
            name: mysql-initdb-config
      containers:
        - image: mysql:8
          name: mysql
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: password
          ports:
            - containerPort: 3306
              name: mysql
          volumeMounts:
            - name: mysql-initdb
              mountPath: /docker-entrypoint-initdb.d
---
apiVersion: v1
data:
  connectionProperties.password: qwerty
  connectionProperties.user: johndoe
  connectionUrl: foobar
kind: Secret
metadata:
  name: database-properties
---
apiVersion: apps/v1
kind: Deployment #Deployment #1, main TeamCity node
metadata:
  name: teamcity-server-main
spec:
  replicas: 1 #always 1
  selector:
    matchLabels:
      app: teamcity-server
  template:
    metadata:
      labels:
        app: teamcity-server
    spec:
      containers:
        - name: teamcity-server
          image: jetbrains/teamcity-server:latest
          env:
            - name: TEAMCITY_SERVER_OPTS
              value: -Dteamcity.server.nodeId=main-node -Dteamcity.server.rootURL=http://$(POD_NAME).$(POD_NAMESPACE)
                -Dteamcity.server.responsibilities=MAIN_NODE,CAN_PROCESS_BUILD_MESSAGES,CAN_CHECK_FOR_CHANGES,CAN_PROCESS_BUILD_TRIGGERS,CAN_PROCESS_USER_DATA_MODIFICATION_REQUESTS
            - name: TEAMCITY_DATA_PATH
              value: /data/teamcity_server/datadir
            - name: TEAMCITY_DB_USER
              valueFrom:
                secretKeyRef:
                  key: connectionProperties.user
                  name: database-properties
            - name: TEAMCITY_DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  key: connectionProperties.password
                  name: database-properties
            - name: TEAMCITY_DB_URL
              valueFrom:
                secretKeyRef:
                  key: connectionUrl
                  name: database-properties
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  apiVersion: v1
                  fieldPath: metadata.name
            - name: POD_NAMESPACE
              valueFrom:
                fieldRef:
                  apiVersion: v1
                  fieldPath: metadata.namespace
          ports:
            - containerPort: 8111
          volumeMounts:
            - name: teamcity-data
              mountPath: /data/teamcity_server/datadir
      volumes:
        - name: teamcity-data
          persistentVolumeClaim:
            claimName: teamcity-node-volume
---
apiVersion: apps/v1
kind: Deployment #Deployment #2, secondary TeamCity node
metadata:
  name: teamcity-server-additional-node-0
spec:
  replicas: 1 #always 1
  selector:
    matchLabels:
      app: teamcity-server
  template:
    metadata:
      labels:
        app: teamcity-server
    spec:
      containers:
        - name: teamcity-server
          image: jetbrains/teamcity-server:latest
          env:
            - name: TEAMCITY_SERVER_OPTS
              value: -Dteamcity.server.nodeId=secondary-node-0 -Dteamcity.server.rootURL=http://$(POD_NAME).$(POD_NAMESPACE)
                -Dteamcity.server.responsibilities=CAN_PROCESS_BUILD_MESSAGES,CAN_CHECK_FOR_CHANGES,CAN_PROCESS_BUILD_TRIGGERS,CAN_PROCESS_USER_DATA_MODIFICATION_REQUESTS
            - name: TEAMCITY_DATA_PATH
              value: /data/teamcity_server/datadir
            - name: TEAMCITY_DB_USER
              valueFrom:
                secretKeyRef:
                  key: connectionProperties.user
                  name: database-properties
            - name: TEAMCITY_DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  key: connectionProperties.password
                  name: database-properties
            - name: TEAMCITY_DB_URL
              valueFrom:
                secretKeyRef:
                  key: connectionUrl
                  name: database-properties
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  apiVersion: v1
                  fieldPath: metadata.name
            - name: POD_NAMESPACE
              valueFrom:
                fieldRef:
                  apiVersion: v1
                  fieldPath: metadata.namespace
          ports:
            - containerPort: 8111
          volumeMounts:
            - name: teamcity-data
              mountPath: /data/teamcity_server/datadir
      volumes:
        - name: teamcity-data
          persistentVolumeClaim:
            claimName: teamcity-node-volume
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: teamcity-node-volume
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: standard
```


## Deploy TeamCity Server via Helm

[Helm](https://helm.sh) is the package manager for Kubernetes that lets you template, version, configure, and reuse raw YAML manifests. As a result, you avoid copy-pasting YAML, conveniently manage multiple environments (dev, staging, prod, and so on), and update/rollback deployments more safely.

These benefits also apply when deploying TeamCity. For example, the most suitable for real-life applications [sample 3](#Example+3%3A+Multiple+Nodes+With+Different+Responsibilities) above showcases the following issues:

* Duplication — main and secondary TeamCity deployments are ~90% identical.
* Hard-coded values — raw values for image tags, node IDs, responsibilities, storage size, and so on.
* Tight coupling — DB config, secrets, PVCs, and app deployments all mixed together.
* Poor scalability — creating any additional secondary node means copy-pasting another Deployment.

To eliminate these issues, you can break the manifest down into separate files. Your Helm Chart structure can look like the following:


```Text
teamcity/
├── Chart.yaml
├── values.yaml             # The majority of your edits will happen here
├── templates/
│   ├── mysql/              # The database template
│   │   ├── service.yaml
│   │   ├── deployment.yaml
│   │   └── configmap.yaml
│   ├── teamcity/           # Template for TeamCity nodes
│   │   ├── deployment.yaml
│   │   └── service.yaml
│   ├── secrets.yaml        # Sensitive data
│   ├── pvc.yaml            # The storage template
│   └── _helpers.tpl
```

<deflist type="full">

<def title="values.yaml">

This file encloses the majority of variable and unique values into a separate layer. You can modify these values (for example, the TeamCity Server image tag) without editing the global manifest.

```yaml
teamcity:
  image: jetbrains/teamcity-server
  tag: latest
  dataPath: /data/teamcity_server/datadir

  mainNode:
    enabled: true
    nodeId: main-node
    responsibilities:
      - MAIN_NODE
      - CAN_PROCESS_BUILD_MESSAGES
      - CAN_CHECK_FOR_CHANGES
      - CAN_PROCESS_BUILD_TRIGGERS
      - CAN_PROCESS_USER_DATA_MODIFICATION_REQUESTS

  secondaryNodes:
    - name: secondary-node-0
      responsibilities:
        - CAN_PROCESS_BUILD_MESSAGES
        - CAN_CHECK_FOR_CHANGES
        - CAN_PROCESS_BUILD_TRIGGERS
        - CAN_PROCESS_USER_DATA_MODIFICATION_REQUESTS

database:
  secretName: database-properties
```

You can easily scale your TeamCity setup by simply adding new entries under the `secondaryNodes` section without bloating the manifest with non-unique lines...

```yaml
secondaryNodes:
  - name: secondary-node-0
      responsibilities:
        ...
  - name: secondary-node-1
      responsibilities:
        ...
  - name: secondary-node-2
    responsibilities:
      ...
```

... and calling the `helm upgrade teamcity ./teamcity` command. 

</def>


<def title="templates/teamcity/deployment.yaml">

With the `values.yml` file in place, your core node deployment manifest can be reduced to the following:

```yaml
# ---------------------------------------------------------
# Main TeamCity node
# ---------------------------------------------------------
apiVersion: apps/v1
kind: Deployment
metadata:
  name: teamcity-server-main
spec:
  replicas: 1
  selector:
    matchLabels:
      app: teamcity-server
      node: main
  template:
    metadata:
      labels:
        app: teamcity-server
        node: main
    spec:
      containers:
        - name: teamcity-server
          image: {{ .Values.teamcity.image }}:{{ .Values.teamcity.tag }}
          env:
            - name: TEAMCITY_SERVER_OPTS
              value: >
                -Dteamcity.server.nodeId={{ .Values.teamcity.mainNode.nodeId }}
                -Dteamcity.server.rootURL=http://$(POD_NAME).$(POD_NAMESPACE)
                -Dteamcity.server.responsibilities={{ join "," .Values.teamcity.mainNode.responsibilities }}
            - name: TEAMCITY_DATA_PATH
              value: {{ .Values.teamcity.dataPath }}
            - name: TEAMCITY_DB_USER
              valueFrom:
                secretKeyRef:
                  name: {{ .Values.database.secretName }}
                  key: connectionProperties.user
            - name: TEAMCITY_DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: {{ .Values.database.secretName }}
                  key: connectionProperties.password
            - name: TEAMCITY_DB_URL
              valueFrom:
                secretKeyRef:
                  name: {{ .Values.database.secretName }}
                  key: connectionUrl
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: POD_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
          ports:
            - containerPort: 8111
          volumeMounts:
            - name: teamcity-data
              mountPath: {{ .Values.teamcity.dataPath }}
      volumes:
        - name: teamcity-data
          persistentVolumeClaim:
            claimName: teamcity-node-volume

# ---------------------------------------------------------
# Secondary TeamCity nodes (rendered from values.yaml)
# ---------------------------------------------------------
{{- range $index, $node := .Values.teamcity.secondaryNodes }}
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: teamcity-server-{{ $node.name }}
spec:
  replicas: 1
  selector:
    matchLabels:
      app: teamcity-server
      node: {{ $node.name }}
  template:
    metadata:
      labels:
        app: teamcity-server
        node: {{ $node.name }}
    spec:
      containers:
        - name: teamcity-server
          image: {{ $.Values.teamcity.image }}:{{ $.Values.teamcity.tag }}
          env:
            - name: TEAMCITY_SERVER_OPTS
              value: >
                -Dteamcity.server.nodeId={{ $node.name }}
                -Dteamcity.server.rootURL=http://$(POD_NAME).$(POD_NAMESPACE)
                -Dteamcity.server.responsibilities={{ join "," $node.responsibilities }}
            - name: TEAMCITY_DATA_PATH
              value: {{ $.Values.teamcity.dataPath }}
            - name: TEAMCITY_DB_USER
              valueFrom:
                secretKeyRef:
                  name: {{ $.Values.database.secretName }}
                  key: connectionProperties.user
            - name: TEAMCITY_DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: {{ $.Values.database.secretName }}
                  key: connectionProperties.password
            - name: TEAMCITY_DB_URL
              valueFrom:
                secretKeyRef:
                  name: {{ $.Values.database.secretName }}
                  key: connectionUrl
            - name: POD_NAME
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: POD_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
          ports:
            - containerPort: 8111
          volumeMounts:
            - name: teamcity-data
              mountPath: {{ $.Values.teamcity.dataPath }}
      volumes:
        - name: teamcity-data
          persistentVolumeClaim:
            claimName: teamcity-node-volume
{{- end }}
```

This single definition will remain unchanged regardless of the secondary TeamCity nodes number. Helm will automatically render this file into multiple Kubernetes deployments: one for the main node plus N secondary node deployments via `range` over the list declared in `values.yml` file.

</def>


<def title="templates/mysql/deployment.yaml">

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "teamcity.mysqlName" . }}
spec:
  strategy:
    type: Recreate
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: {{ .Values.mysql.image }}
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: {{ .Values.mysql.rootPassword | quote }}
          ports:
            - containerPort: 3306
```

</def>


<def title="templates/mysql/configmap.yaml">

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mysql-initdb-config
data:
  init.sql: |
    CREATE DATABASE IF NOT EXISTS {{ .Values.mysql.database }};
```

</def>


<def title="templates/pvc.yaml">

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: teamcity-node-volume
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: {{ .Values.persistence.size }}
  storageClassName: {{ .Values.persistence.storageClass }}
```

</def>

</deflist>


You can continue optimizing this setup via Go templates defined in `_helpers.tpl`. For example, you can create a common container config...


```yaml
{{- define "teamcity.containerBase" -}}
image: {{ .Values.teamcity.image }}:{{ .Values.teamcity.tag }}
ports:
  - containerPort: 8111
volumeMounts:
  - name: teamcity-data
    mountPath: {{ .Values.teamcity.dataPath }}
env:
  - name: TEAMCITY_DATA_PATH
    value: {{ .Values.teamcity.dataPath }}
  - name: POD_NAME
    valueFrom:
      fieldRef:
        fieldPath: metadata.name
  - name: POD_NAMESPACE
    valueFrom:
      fieldRef:
        fieldPath: metadata.namespace
{{ include "teamcity.dbEnv" . | indent 2 }}
{{- end -}}
```

...and a `TEAMCITY_SERVER_OPTS` builder...

```yaml
{{- define "teamcity.serverOpts" -}}
-Dteamcity.server.nodeId={{ .nodeId }}
-Dteamcity.server.rootURL=http://$(POD_NAME).$(POD_NAMESPACE)
-Dteamcity.server.responsibilities={{ join "," .responsibilities }}
{{- end -}}
```

...to make the `deployment.yaml` file much cleaner:

```yaml
containers:
  - name: teamcity-server
  {{ include "teamcity.containerBase" . | indent 4 }}
env:
  - name: TEAMCITY_SERVER_OPTS
    value: >
      {{ include "teamcity.serverOpts" (dict
        "nodeId" .Values.teamcity.mainNode.nodeId
        "responsibilities" .Values.teamcity.mainNode.responsibilities
      ) }}
```