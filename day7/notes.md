# Day 7 : Pod in Kubernetes

## Imperative :
### Create an NGINX container POD using the declarative way:
```shell
kubectl run nginx-pod --image nginx:latest
```
### Check pods inside a cluster : 
You can check the available pods inside the cluster using :
```shell
kubectl get pods
``` 
### Get POD description (information) : 
```shell
kubectl describe pod <pod-name> 
```
### Acces the pod outside the cluster : 
Let's take the created nginx pod as a reference. Generally, nginx is exposed on port 80. If you try to acess this port in your machine (host), you will get an error. <br>
You need to make your pod acessible outside the cluster, the simple way is to do a **Port-Forward**. 

```shell
kubectl port-forward nginx-pod 8080:80
```
Tape : localhost:8080 and you we'll get acces to the pod (application)
