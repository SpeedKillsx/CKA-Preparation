# Day 6 : Kubernetes Multi Node Cluster with KIND
## KIND installation :
Go to the KIND's web page : [text](https://kind.sigs.k8s.io/docs/user/quick-start)

## Kubernetes Version :
I used the Kube version that is setup at CKA exam : 1.34.0


## Kubectl commands:
### Create a cluster:
```powershell
kind create cluster --image <kubernetes-image> --name <cluster-name>
```
*  *Note* : It will create a cluster with one node by default 

### Find cluster created inside KIND:
```powershell
kind get clusters
```

### Find Nodes
```powershell
kubectl get nodes
``` 
**FLOW** : 
1. Kubectl send a request to apiserver.
2. Apiserver will authentificate, validate the request.
3. Apiserver asks the ETCD and retrieve the nodes.
4. The response is redirected to the client.

### Delete Cluster:
```powershell
kind delete cluster --name <cluster-name>
```
### Create a cluster with multiple nodes :
By default, the created cluster has one node. <br>
To add new nodes, it is possible by applying a configuration file. <br>

```yaml
kind: Cluster
apiVersion : kind.x-k8s.io/v1alpha4
nodes: 
   - role: control-plane
   - role: worker
   - role: worker
```

Then run command :
```powershell
kind create cluster --image <kubernetes-image> --name <cluster-name> --config <config-file>
```
### Get information about the cluster : 
```powershell
kubectl cluster-info --context <cluster-name>
```

### Get all context : 
```powershell
kubectl config get-contexts
```
### Get current context :
```powershell
kubectl config current-context
```
### Delete context :

```powershell
kubectl config delete-context <context-name>
```

### Switch to another context :
```powershell
kubectl config use-context <cluster-name>
```

### Show cluster nodes : 

```powershell 
kubectl get nodes
```

**Or specify the cluster inside the command**

```powershell
   kubectl get nodes --context <cluster-name>
```
### Get configuration for kubernetes:
```powershell
kubectl config view 
```