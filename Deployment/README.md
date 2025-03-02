## k8s deployment
=============================


### 1. Create `vi myapp-deployment.yml`

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myapp:latest
        ports:
        - containerPort: 8080
```


### 2. Apply

```
kubectl apply -f myapp-deployment.yml
```
