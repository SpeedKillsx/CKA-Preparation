# Day 7 : Pod in Kubernetes

## Imperative :
Imperative way means use commands for creating/managing kubernetes objects.
### Create an NGINX container POD using the declarative way:
```shell
kubectl run nginx-pod --image nginx:latest
```

## Declarative : 
Imperative way means use configuration files for creating/managing kubernetes objects.

### Create a POD :
```yaml
kind: Pod
apiVersion: v1
metadata:
    - name: nginx-pod-yaml
spec:
    - containers:
      name: nginx
      images: nginx:latest
      ports:
        - containerPort: 80
          hostPort: 8080
```
RUN : 
```shell
kubectl apply -f <yaml-file>
```
To check the available version of an object :
```shell
kuebctl explain pod
```


## POD basic commands : 
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

### Update an object inside cluster : 
There is multiple ways to do that : 
1. Update the config yaml file and run the commads : 

```shell
kubectl delete pod nginx-pod
kubectl apply -f <yaml-file>
```
2. Using command line : 
````shell
kubectl edit pod <nginx-pod>
```
