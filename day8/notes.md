## Repication Controller:
I created a basic RecplicationController using this yaml file :
```yaml
kind: ReplicationController
apiVersion: v1
metadata:
  name: nginx-rc
  labels:
    name: test-replication-controller
    version: 1.0.0
    type: demo
spec:
  replicas: 3
  template:
    metadata:
      name: nginx
      labels:
        name: nginx-replication
        type: demo
    spec:
      containers:
        - name: nginx-pod
          image: nginx:latest
          ports:
            - containerPort: 80

```
<br>

**My Personnal Notes**:
1- If you have only one node inisde the cluser (which was my case) and you want to expose each node on same pod (exemple : 80) you will get one of your replication in *Pending* State.

2- You must delete the created replication to apport modification. Why ?? **Because Replication Controller do not change the state of an already created pod** This is why Kuberenetes documentation recommand to use **Deployments**. 
3- A **ReplicationController** manages only the pods that he creates


## ReplicaSet :

New resource that manages the pods better than the *ReplicationController*, it manages all the pods that matches a labels.


Example :
```yaml
kind: ReplicaSet
apiVersion: apps/v1
metadata:
  name: nginx-rs
  labels:
    name: replication-set-nginx
    type: demo
    version: 1.0.0
spec:
  replicas: 3
  template:
    metadata:
      name: nginx
      labels:
        env: test
    spec:
      containers:
        - image: nginx:latest
          name: nginx-rs
          ports:
            - containerPort: 80
  selector:
    matchLabels:
      env: test
```
<br>

### Update the number of replicas : 

1) kubectl edit rs nginx-rs
2) from yaml file
3) kubectl scale --replicas = <number-replicas> rs/nginx-rs


## Deployment:
A kubernetes resource that manages pods and save the history.


```yaml

kind: Deployment
apiVersion: apps/v1
metadata:
  name: nginx-deploy
  labels:
    env: deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      env: deploy
  template:
    metadata:
      labels:
        env: deploy
    spec:
      containers:
        - image: nginx:latest
          name: nginx
          ports:
            - containerPort: 80

```

### Update deployment image :
1) from yaml
2) imperative way:
```shell
kubectl set image deploy/nginx-deploy\
nginx=nginx:<image-version>
```
*Note: But the Yaml file steels the same, we changed the live object on kubernetes*

### Check rollouts history:

```shell
kubectl rollout history deploy/<deployment-name>
```
### Undo Changes:
```shell
kubect rollout undo deploy/<deploymeny-name>
```

# Task Day 8 :
## ReplicaSet:
YAML FILE :

```yaml
kind: ReplicaSet
apiVersion: apps/v1
metadata:
  name: nginx-test-rs
  labels:
    env: task
spec:
  replicas: 3
  selector:
    matchLabels:
      env: task
  template:
    metadata:
      name: nginx
      labels:
        env: task
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80


```


2) Update the replicas using yaml file to 4 :

```yaml
kind: ReplicaSet
apiVersion: apps/v1
metadata:
  name: nginx-test-rs
  labels:
    env: task
spec:
  replicas: 4
  selector:
    matchLabels:
      env: task
  template:
    metadata:
      name: nginx
      labels:
        env: task
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80


```


3) Update the replicas using command line file to 6 :

```shell
kubectl scale --replicas=6 rs/nginx-test-rs
```

## Deployment:

1. Create a Deployment named nginx with 3 replicas. The Pods should use the nginx:1.23.0 image and the name nginx. The Deployment uses the label tier=backend. The Pod template should use the label app=v1.

```yaml
kind: Deployment
apiVersion: apps/v1
metadata:
  name: nginx
  labels:
    tier: backend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: v1
  template:
    metadata:
      name: nginx
      labels:
        tier: backend
        app: v1
    spec:
      containers:
        - image: nginx:1.23.0
          name: nginx
          ports:
            - containerPort: 80


```
2. List the deployments:
```shell
kubectl get deployments
```

3. Update the image to nginx:1.23.4.

```shell
kubectl set image deploy/nginx \
nginx=nginx:1.23.4
```
4. Verify changes :
a-
```shell
kubectl get deploy/nginx -o wide
```

b- 
```shell
kubectl rollout history deploy/nginx
```

c- 
```shell
kubectl rollout status deploy/nginx
```

5. Assign the change cause "Pick up patch version" to the revision.
```shell
kubectl annotate deployment nginx kubernetes.io/change-cause="Pick up patch version" --overwrite
```

6. Scale the deployment to 5 replicas
```shell
kubectl scale --replicas=5 deploy/nginx
```
7. Rollout History :
kubectl rollout history deploy/nginx

8. kubectl rollout undo deploy/nginx
9. kubectl describe deploy/nginx
