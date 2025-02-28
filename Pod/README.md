## How to create pod
============================

### 1. create a file : `vi mypod.yml`

```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod1
  labels:
    app: my-app1
spec:
  containers:
    - name: my-container1
      image: nginx:latest
      ports:
        - containerPort: 80
```

Press-ESC and :wq

### 2. Apply yml file


```
kubectl apply -f mypod.yml
```

### 3. Verify pod status


```
kubectl get pods
```

### 4. Describe pod


```
kubectl describe pod my-pod1
```

### 5. Access running container


```
kubectl exec -it my-pod1 -- /bin/sh
```

### 5. To delete pod

```
kubectl delete pod my-pod1
```
