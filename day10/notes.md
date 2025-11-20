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

We can reach pods that are in different namespaces using their IP but not with their services names. but it is possible with fully qualified domain name.
Example :
I have a service `svc-test` in default namespace, i want to reach it from a pod in `demo` namespace.
```bash
kubectl exec -it nginx-demo-98d9dcdf8-67dqv -n demo -- sh

curl svc-test.default.svc.cluster.local
```