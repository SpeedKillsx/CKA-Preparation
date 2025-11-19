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