# Day 8 : Kubernetes Deployment,Replication Controller and ReplicaSet  

## What is a Deployment :

A Kubernetes Deployment is a resource that can deploy and manage a set of PODs using a declarative way (ex: Yaml file). <br>
The Deployment maintains the PODs healthy, where he starts (run) replications for the application (use replication controller) to maintains the service.

## Advantages of using a Deployment : 
Creating pods using Deployment helps in many things :
    1. Ensure the disponibility of the PODs.
    2. Scalability
    3. Replicas are defined, so it mantains the availability of the application.
    4. Save the history of deployments.
    5. Ensure progressive update (rolling update)

## ReplicaSet :

It ensures that the defined number of replicas is always available. <br>

```shell
Example : 
If replicas = 3; the system will always provide 3 replicas.
```
The recent version of kubernetes doesn't need to declare it , it will be used automatically.


## Replication Controller :
An older version of replicaSet that are built for the same job, but ReplicationController is not used nowadays. (Even in Kubernetes documentation, it is recommanded to use a ReplicaSet)


# Pratical part :

## Create ReplicationController:

```shell

kind: ReplicationController
apiVersion: v1
metadata:
  name: nginx
  labels:
    name: demo-rc
spec:
  replicas: 3
  
  template:
    metadata:
      name: nginx
      labels:
        env: demo
        version: 1.0.0
        type: frontend
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
              hostPort: 8080
```