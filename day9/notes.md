# Kubernetes Service:

## What is a service :
It is a resource that permits the communication between pods inside the cluster, or be exposed outside it.

## Why use Services in K8S :
 1. Stable communication between pods
 2. Charge repartition
 3. Expose the application outside the cluster.
 4. Mantain the communication between services even if some pod go down.


## Example of Service :
```yaml
kind: Service
apiVersion: v1
metadata:
  name: nginx-service
  labels:
    env: dev
spec:
  selector:
    env: dev
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30010

```

The Service selects the pods with the specified `selector` and will expose the application on a port and redirect it to another (*targerport*). 
*nodePort* exope the pod on a specific port on the Node.


## Types of Services :

1. **NodePort**: This Service will expose the pod on a specific port on the Node, it is used to try the application during the dev before pushing it to the production environment. But there is no charge repartition.

2. **ClusterIP**: The default type for a service in K8S if no type is specified. This service make the communication possible between pods inside the cluster.

3. **LoadBalancer**: Another service type that uses a load balancer from cloud

4. **ExternalDNS**


# Task :

1. Create a Service named myapp of type ClusterIP that exposes port 80 and maps to the target port 80.
```yaml
kind: Service
apiVersion: v1
metadata:
  name: myapp
spec:
  selector:
    env: cluster
  type: ClusterIP
  ports:
    - port: 80
      targetPort: 80

```

2. Create a Deployment named myapp that creates 1 replica running the image nginx:1.23.4-alpine. Expose the container port 80.
```yaml
kind: Deployment
apiVersion: apps/v1
metadata:
  name: myapp
  labels:
    app: myapp
spec:
  selector:
    matchLabels:
      app: myapp
  replicas: 1
  template:
    metadata:
      name: nginx
      labels:
        app: myapp
        env: cluster
    spec:
      containers:
        - name: nginx
          image: nginx:1.23.4-alpine
          ports:
            - containerPort: 80

```
3. Scale the Deployment to 2 replicas.

```bash
kubectl scale --replicas=2 deploy/myapp
```
4. Create a temporary Pod using the image busybox and run a wget command against the IP of the service.

```bash
kubectl run tempmyapp --rm -it --image=busybox -- sh

kubectl get svc/myapp
wget <service-ip>
```

5. Run a wget command against the service outside the cluster.
Cannot reach the pod outside the cluster.
6. Change the service type so the Pods can be reached outside the cluster.
```yaml
kind: Service
apiVersion: v1
metadata:
  name: myapp
spec:
  selector:
    env: cluster
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30010
```
```bash
kubectl apply -f <yaml-file>
```

Go to web navigator : localhost:30010

7. Run a wget command against the service outside the cluster.
```bash
wget localhost:30010
```
8. Discuss: Can you expose the Pods as a service without a deployment?
Yes, you just match the label of the pod with service selector
9. Discuss: Under what condition would you use the service types LoadBalancer, node port, clusterIP, and external?

**ClusterIP**:
- Internal communication only
- Microservices inside the cluster
- Most common type

**NodePort**:
- For local development or simple external access
- Exposes app on each node at a port
- Not production-friendly by itself

**LoadBalancer**:
- Cloud environments
- Public access with managed load balancer
- Production-friendly

**ExternalName**:
- Maps a service to an external DNS name
- No load balancing, no proxying

