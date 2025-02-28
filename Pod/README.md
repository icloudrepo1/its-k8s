## How to create pod
============================

create a file : `mypod.yml`

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

