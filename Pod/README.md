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
kubectl describe pod mypod1
```

### 5. Access running container


```
kubectl exec -it mypod1 -- /bin/sh
```

### 5. To delete pod

```
kubectl delete pod mypod1
```
