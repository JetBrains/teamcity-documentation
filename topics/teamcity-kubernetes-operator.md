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