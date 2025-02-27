Using Minikube
===================

### 1. Create 1 EC2 instance


ami -> ubuntu-22
instance types -> t2.medium
key pair -> k8s.pem
vpc -> default vpc
az -> 1a
sg -> ubuntu-SG (ssh,http,https = ANYWHERE)


### 2. Connect k8s-master instance & install docker - minikube


cd downloads
ssh -i "k8s.pem" ubuntu@ec2-13-201-15-57.ap-south-1.compute.amazonaws.com

```
sudo apt update -y
sudo apt install docker.io -y
docker --version
```


[go to minikube & paste below curl command]----visit

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
```

### 3. To check minikube installation status

```
minikube version
```

### 3. Allow permission

```
sudo usermod -aG docker $USER && newgrp docker
minikube start --driver=docker
```

### 4. Install k8s cli tool

```
sudo snap install kubectl --classic
```

### 5. Now check cluster info

kubectl get nodes
kubectl cluster-info
kubectl get po -A

====================END=================================================================
