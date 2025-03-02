## What is pod ?
============================


In Kubernetes, a pod is the smallest and simplest unit that you can create or deploy. 

Pod is a very small unit that contains a container or multiple containers where the application is deployed. 

A pod represents a single instance of a running process in your cluster and encapsulates one or more tightly coupled containers. 

Typically, a pod will include just one container, but in some cases, multiple containers are used when they need to share resources or work closely together.

Each pod is assigned a unique IP address within the cluster, which allows containers within the pod to communicate with each other and with other pods.

When a pod dies, Kubernetes typically replaces it with a new pod that may have a different IP.

Pods can specify shared storage volumes that are accessible by any container within the pod. This is useful for sharing files between containers.

The "disposable" nature of pods enables Kubernetes to easily scale, manage, and replace instances as needed.

Pods are usually created and managed by higher-level controllers like Deployments, ReplicaSets, or StatefulSets. 

These controllers help ensure that a desired number of pods are running, scaling, and self-healing when failures occur.



### Create POD
-------------------------------


#### 1. vi dev-pod.yaml


```
apiVersion: v1
kind: Pod
metadata:
  name: pod1
  labels:
    app: my-app
spec:
  containers:
    - name: container1
      image: nginx
      ports:
        - containerPort: 80

```


In this example:

  - The pod is named pod1.
  - The pod contains one container running the nginx image.
  - The container exposes port 80.


#### 2. Create the pod


```
kubectl apply -f dev-pod.yaml
```


#### 3. Verify the pod's status


```
kubectl get pods
```

```
kubectl get pod pod1 -o wide

```

#### 4. Get detailed information about the pod 


```
kubectl describe pod pod1
```


#### 5. List all pods in the current namespace


```
kubectl get pods --all-namespaces
```

#### 6. Delete a Pod


```
kubectl delete pod pod1

```

#### 7. To copy a file from your local machine to a Kubernetes pod


```
kubectl cp example.txt pod1:/tmp/example.txt
```

If your pod is in a specific namespace

```
kubectl cp example.txt -n <namespace> pod1:/tmp/example.txt
```


To verify :-

`kubectl get pods -n <namespace>`

`kubectl exec -it pod1 -n <namespace> -- /bin/bash`

`ls`

`cd tmp`

`ls`

#### 8. To Copy a file from the pod to your local machine


```
kubectl cp pod1:/tmp/example.txt ./example.txt
```

If your pod is in a specific namespace

```
kubectl cp -n <namespace> pod1:/tmp/example.txt ./example.txt

```


#### 9. To go inside pod


```
kubectl exec -it <pod-name> -n <namespace> -- /bin/bash
```

------------------END--------------------------------------------
