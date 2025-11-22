# Day 11 : Multi Container POD
A Multi Container POD is a Pod that contains multiple containers. It is generally used if an application needs to run a treatement before starting or during it execution.

The container is dependent of the application , so if the application is down he goes down.

## Example :

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: "2025-11-22T20:15:11Z"
  generation: 1
  labels:
    app: nginx-deplyoment
  name: nginx-deplyoment
  namespace: default
  resourceVersion: "55111"
  uid: b07d80a6-4fa7-4717-bb9d-158850c87185
spec:
  progressDeadlineSeconds: 600
  replicas: 2
  revisionHistoryLimit: 10
  selector:
    matchLabels:
      app: nginx-deplyoment
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx-deplyoment
    spec:
      containers:
      - image: nginx
        imagePullPolicy: Always
        name: nginx
        env:
        - name: FIRSTNAME
          value: "amayas"
        command: ['sh', '-c', 'echo The app is running! && sleep 3600']
        ports:
        - containerPort: 80
          protocol: TCP
        resources: {}
      initContainers:
      - name: init-myservice
        image: busybox:1.28
        command: ['sh', '-c']
        args: ['until nslookup my-svc.default.svc.cluster.local;do echo waiting for service to be up;sleep 2; done']
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      schedulerName: default-scheduler
      securityContext: {}
      terminationGracePeriodSeconds: 30
status: {}

```

The **init-myservice** is a container that shares with the container `nginx`:
    1. Same resources
    2. Same Network
    3. Same POD

If you run 
```bash 
kubectl apply -f file.yaml
kubectl get pods
```

you 'll find that no pod is running, because the **init-myservice container** doesn't find a service my-svc. You'll need to create one before

```yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: "2025-11-22T20:19:20Z"
  labels:
    app: nginx-deplyoment
  name: my-svc
  namespace: default
  resourceVersion: "55551"
  uid: cb3ec7bd-58e0-4f86-90cb-d91639d78c9c
spec:
  clusterIP: 10.96.177.5
  clusterIPs:
  - 10.96.177.5
  internalTrafficPolicy: Cluster
  ipFamilies:
  - IPv4
  ipFamilyPolicy: SingleStack
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx-deplyoment
  sessionAffinity: None
  type: ClusterIP
status:
  loadBalancer: {}

```

Tape :
```bash
kubectl logs nginx-deplyoment-947d9b8d4-cqntn -c init-myservice
```
You will see the container show the server and the adress. The nginx container will also print `The app is running!`