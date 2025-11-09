# What is Kubernetes
Kubernetes is a portable platform for managing containerized workloads (Projects), it is widely used in the industry due to his ecosystem.

# Kubernetes Component:
![alt](components-of-kubernetes.svg) <br>
<br>
A Kubernetes Cluster is composed of :<br>

    1. Master Node (Control Plane) 
    
    2. Worker Node

## Master Node:
Control Plane manage the lifecycle og the Kubernetes Cluster and make the global decision like Scheduling, as well as detecting and responding to cluster events like running a new instance of a POD if a Deployment replicas falls down.

### Control Plane Components:
1- **Api-Server**:  Core object in the Master Node, it recives all the requests (came from outside) that came from the user to the cluster's master Node. It represents the main entry inside the Kubernetes cluster. <br>

2- **Scheduler or Kube-Scheduler** : The Scheduler is responsible to schedule the nodes of the cluster. The request is sent to the `kube-apiserver` who will create the POD using a tool called *kubectl*. the scheduler will choose where the POD should be placed. An example : " A user wants to scale a pod", this request will be redirected to the scheduler to find the best node that matches a couple of conditions like : CPU availability / usage, PODs affinity/anti-affinity.

3- **Controller Manager** : It is a process that controls the state of the cluster. He ensures to move the current state of the cluster closer to the desired state. <br> Controller Manager is a group of process (controllers) but they are all compiled into a single binary and run in a single process.

There are many different types of controllers. Some examples of them are:

* **Node controller**: Responsible for noticing and responding when nodes go down.

* **Job controller**: Watches for Job objects that represent one-off tasks, then creates Pods to run those tasks to completion.

* **EndpointSlice controller**: Populates EndpointSlice objects (to provide a link between Services and Pods).

* **ServiceAccount controller**: Create default ServiceAccounts for new namespaces.

4- **ETCD** : A key-value Database that stores each information about the cluster. The database interacts only with the `kube-apiserver`. The `kube-apiserver` is the only component that may apply changes to the database.


### Node Controller :
Runs on every Node, it composed by :

1. **Kublet** : It is a component that receives instruction form the **Apiservice** and apply them inside the Node. <br>

    Example: The `api-service` send a request to the Node to delete a specific POD. The `kublet` recieves the request, deletes the POD then send a response according the request to the `api-service`.

2. **Kube-proxy** : Manage Networking inside the Node, to make the communication possible between the PODs inside the Node.

### Kubectl Command Workflow

1. **User runs a kubectl command**
   - Example: `kubectl create -f pod.yaml`

2. **kubectl sends a REST API request to the API Server**
   - Communicates over HTTPS using the Kubernetes API.

3. **API Server validates and processes the request**
   - Performs authentication, authorization, and validation.

4. **API Server interacts with etcd**
   - Writes or reads the cluster state depending on the request.

5. **Controllers and Scheduler react (if needed)**
   - Scheduler assigns Pods to Nodes.
   - Kubelet runs containers on the selected Node.

6. **API Server returns a response to kubectl**
   - `kubectl` displays the result to the user.
