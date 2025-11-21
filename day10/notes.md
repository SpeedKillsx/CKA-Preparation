# Day10: Namespaces

## What is a Namespace:
Namespaces are a resources that separates logically the cluster. It is usefull if you manage various project, so it offeres a logically sepration.

Some Namespaces are created by the cluster :
default: Default namespace where created resources goes
kube-node-lease: 
kube-public
kube-system          


## Create A namespace:

1. **Declarative way**:
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: demo
```

2. **Imperative way**:

```bash
kubectl create namespace demo
```

## Create a resource inside a 
Create a deployment in the created namespace.
```bash
kubectl create deploy nginx-demo -n demo --image=nginx --replicas=1
```

## Communication between pods:

We can reach pods that are in different namespaces using their IP but not with their services names. but it is possible with fully qualified domain name (FQDN).
Example :
I have a service `svc-test` in default namespace, i want to reach it from a pod in `demo` namespace.
```bash
kubectl exec -it nginx-demo-98d9dcdf8-67dqv -n demo -- sh

curl svc-test.default.svc.cluster.local
```

# Task:

1. Create two namespaces and name them ns1 and ns2:

    ```bash
    kubectl create namespace ns1
    kubectl create namespace ns2
    ```
2. Create a deployment with a single replica in each of these namespaces with the image as nginx and name as deploy-ns1 and deploy-ns2, respectively

```bash
kubectl create deploy deploy-ns1 --image=nginx --replicas=1 --namespace=ns1
kubectl create deploy deploy-ns2 --image=nginx --replicas=1 --namespace=ns2
```
3. Get the IP address of each of the pods (Remember the kubectl command for that?)

```bash
kubectl get pods -n ns1

NAME                          READY   STATUS    RESTARTS   AGE
deploy-ns1-85f4589c78-58kmt   1/1     Running   0          2m2s

kubectl describe pod deploy-ns1-85f4589c78-58kmt -n ns1| grep IP
IP:               10.244.1.8
IPs:
IP:           10.244.1.8

kubectl get pods -n ns2
NAME                          READY   STATUS    RESTARTS   AGE
deploy-ns2-59fccc89b5-thhws   1/1     Running   0          15m

kubectl describe pod deploy-ns2-59fccc89b5-thhws -n ns2 | grep IP
IP:               10.244.1.9
IPs:
IP:           10.244.1.9
```
You can also use :
```bash
kubectl get pods -o wide -n ns1
kubectl get pods -o wide -n ns2
```
4. Exec into the pod of deploy-ns1 and try to curl the IP address of the pod running on deploy-ns2:
In `ns1`:

```bash
kubectl exec -it deploy-ns1-85f4589c78-58kmt -n ns1 -- sh

# curl 10.244.1.9
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

```
In `ns2`

```bash
kubectl exec -it deploy-ns2-59fccc89b5-thhws -n ns2 -- sh
# curl 10.244.1.8
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

```
5. Now scale both of your deployments from 1 to 3 replicas.
`deploy-ns1`:

```bash
kubectl scale deploy/deploy-ns1 --replicas=3 -n ns1
deployment.apps/deploy-ns1 scaled

---------------------------------------
kubectl get pods -n ns1
NAME                          READY   STATUS    RESTARTS   AGE
deploy-ns1-85f4589c78-58kmt   1/1     Running   0          21m
deploy-ns1-85f4589c78-59tpv   1/1     Running   0          5s
deploy-ns1-85f4589c78-mw9bq   1/1     Running   0          5s
```
`deploy-ns2`:

```bash
kubectl scale deploy/deploy-ns2 --replicas=3 -n ns2
    deployment.apps/deploy-ns2 scaled
    ------------
    kubectl get pods -n ns2
    NAME                          READY   STATUS    RESTARTS   AGE
    deploy-ns2-59fccc89b5-qv525   1/1     Running   0          10s
    deploy-ns2-59fccc89b5-s5flk   1/1     Running   0          10s
    deploy-ns2-59fccc89b5-thhws   1/1     Running   0          23m    
```
6. Create two services to expose both of your deployments and name them svc-ns1 and svc-ns2

```bash
kubectl expose deploy deploy-ns1 --name=svc-ns1 --port=80 -n ns1
kubectl expose deploy deploy-ns2 --name=svc-ns2 --port=80 -n ns2
```
7. exec into each pod and try to curl the IP address of the service running on the other namespace:
**Namespace NS1**:
```bash
kubectl get svc -n ns1
NAME      TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
svc-ns1   ClusterIP   10.96.133.174   <none>        80/TCP    6m

kubectl exec -it deploy-ns2-59fccc89b5-thhws -n ns2 -- sh
# curl 10.96.133.174
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

```
**Namespace NS2**:
```bash
kubectl get svc -n ns2
NAME      TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
svc-ns2   ClusterIP   10.96.214.27   <none>        80/TCP    7m17s


kubectl exec -it deploy-ns1-85f4589c78-58kmt -n ns1 -- sh
# curl 10.96.214.27
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
8. Now try curling the service name instead of IP. You will notice that you are getting an error and cannot resolve the host:
*Namespace NS1*:
```bash
kubectl exec -it deploy-ns1-85f4589c78-58kmt -n ns1 -- sh
# curl svc-ns2
curl: (6) Could not resolve host: svc-ns2
```
*Namespace NS2*

```bash
kubectl exec -it deploy-ns2-59fccc89b5-thhws -n ns2 -- sh
# curl svc-ns1
curl: (6) Could not resolve host: svc-ns1
```
9. Now use the FQDN of the service and try to curl again, this should work:

**Use FQDN to reach svc-ns2**

```bash
kubectl exec -it deploy-ns1-85f4589c78-58kmt -n ns1 -- sh
# cat /etc/resolv.conf
search ns1.svc.cluster.local svc.cluster.local cluster.local
nameserver 10.96.0.10
options ndots:5
# curl svc-ns2.ns2.svc.cluster.local
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

**Use FQDN to reach svc-ns1**
```bash
kubectl exec -it deploy-ns2-59fccc89b5-thhws -n ns2 -- sh
# cat /etc/resolv.conf
search ns2.svc.cluster.local svc.cluster.local cluster.local
nameserver 10.96.0.10
options ndots:5
# curl svc-ns1.ns1.svc.cluster.local
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
10. n the end, delete both the namespaces, which should delete the services and deployments underneath them:
```bash
kubectl delete ns ns1
namespace "ns1" deleted
PS F:\CKA-Preparation\day10> kubectl delete ns ns2
namespace "ns2" deleted

kubectl get ns
NAME                 STATUS   AGE
default              Active   47h
demo                 Active   140m
kube-node-lease      Active   47h
kube-public          Active   47h
kube-system          Active   47h
local-path-storage   Active   47h

```

All the resources that were in the two namespaces were deleted.