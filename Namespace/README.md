## k8s namespace
==============================


In Kubernetes, a namespace is a way to divide cluster resources between multiple users or applications. 

Namespaces are useful for organizing resources, managing access, and isolating environments within a single Kubernetes cluster.

Namespaces provide a logical separation for resources, making it easier to manage resources for different projects, teams, or environments (like dev, test, and prod).

Namespaces in Kubernetes offer a way to logically Separate development, testing, and production environments within a single cluster.


#### Default Namespaces in Kubernetes


Kubernetes includes a few default namespaces :-

`default`  The default namespace where resources are created if no namespace is specified.

`kube-system`  Holds system-related components and resources managed by Kubernetes (e.g., core components like kube-dns).

`kube-public` A namespace that's readable by all users, used for public resources.

`kube-node-lease`  Used to manage node heartbeat information, improving performance in large clusters.


#### 1. List All Namespaces


```
kubectl get namespaces
```


#### 2. Create a Namespace


```
kubectl create namespace dev-namespace
```


#### 3. Creating Resources in a Namespace


vi my-pod2.yaml


```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  namespace: dev-namespace
spec:
  containers:
    - name: dev-container-2
      image: nginx

```

```
kubectl apply -f my-pod2.yaml -n dev-namespace
```

#### 4. View Resources in a Specific Namespace


```
kubectl get pods -n dev-namespace
```


#### 5. Delete a Namespace


```
kubectl delete namespace dev-namespace
```
