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