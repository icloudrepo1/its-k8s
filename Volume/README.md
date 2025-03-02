persisent volume
==================

vi pv.yml


[paste below code]

apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-todo-app
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: "/tmp/data"


:wq


kubectl apply -f pv.yml


vi pvc.yml


[paste below code]

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-todo-app
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi


:wq


kubectl apply -f pvc.yml



vi deployment.yml


[paste below code]


apiVersion: apps/v1
kind: Deployment
metadata:
  name: todo-app-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: todo-app
  template:
    metadata:
      labels:
        app: todo-app
    spec:
      containers:
        - name: todo-app
          image: mudit097/django-todo
          ports:
            - containerPort: 8000
          volumeMounts:
            - name: todo-app-data
              mountPath: /app
      volumes:
        - name: todo-app-data
          persistentVolumeClaim:
            claimName: pvc-todo-app

:wq



kubectl apply -f deployment.yml
kubectl get pv
kubectl get pvc
kubectl describe pvc pvc-todo-app


kubectl get pods
kubectl exec -it <pod-name> /bin/bash


cd /app/
echo "Hello" > /app/myfile.txt
ls


kubectl get pods
kubectl delete pod <pod-name>

kubectl get pods
docker ps

docker exec -it <pod-name> /bin/bash
cd /app/
ls
cat myfile.txt
