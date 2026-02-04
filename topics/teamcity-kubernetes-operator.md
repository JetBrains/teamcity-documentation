# Kubernetes Operator: Deploy a TeamCity Server in a Kubernetes Cluster

[Kubernetes](https://kubernetes.io) provides a number of features and tools that helps you deploy stable, reliable, and scalable applications with zero downtime updates.


## Manual Deployment


```yaml
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
          ports:
            - containerPort: 8111
          volumeMounts:
            - name: teamcity-data
              mountPath: /data/teamcity_server/datadir
      volumes:
        - name: teamcity-data
          emptyDir: {}
---
apiVersion: v1
kind: Service
metadata:
  name: teamcity-server
spec:
  selector:
    app: teamcity-server
  ports:
    - port: 80
      targetPort: 8111
  type: ClusterIP
```