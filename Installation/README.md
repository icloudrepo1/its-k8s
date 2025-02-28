Using Minikube
===================

### 1. Create 1 EC2 instance


ami  ----->  ubuntu-22

instance types  ---->  t2.medium

key pair  ---->  k8s.pem

sg   ----->  ubuntu-SG ( alltraffic = ANYWHERE )


### 2. Connect k8s-master instance & install docker - minikube


Coonect your instance and run below commands

```
sudo apt update -y
sudo apt install docker.io -y
docker --version
```


Go to `https://minikube.sigs.k8s.io/docs/start/?arch=%2Flinux%2Fx86-64%2Fstable%2Fbinary+download`

```
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64

```

### 3. To check minikube installation status

```
minikube version
```

### 3. Allow permission

```
sudo usermod -aG docker $USER && newgrp docker
```

### 4. Start minikube service

```
minikube start --driver=docker
```

### 5. Install k8s cli tool

```
sudo snap install kubectl --classic
```

### 6. Now check cluster info

```
kubectl get nodes
```

```
kubectl cluster-info
```

```
kubectl get po -A
```



====================END=================================================================



Using KUBEADM
===================


create 1 or 2 ec2 instances 
----------------------------

ubuntu-22
k8s-key.pem
az-1a
ssh, http,https anywhere


crt master
-------------

ubuntu-22
k8s-key.pem
az-1b
ssh,http,https anywhere

connect master
------------------

sudo su -
apt update
apt install docker.io -y
systemctl start docker
systemctl enable docker

sudo apt-get install -y apt-transport-https ca-certificates curl gpg

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl

kubeadm init

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml

kubeadm token create --print-join-command


connect worker
-----------------

sudo su -
apt update
apt install docker.io -y
systemctl start docker
systemctl enable docker

sudo apt-get install -y apt-transport-https ca-certificates curl gpg

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl



paste token ex:-
================

kubeadm join 172.31.12.115:6443 --token h2v9ur.edpe7q7za3fke7aa --discovery-token-ca-cert-hash sha256:6c0acbd0dc1a9839b78e9a53685aa2f1f7535a96999e65acae2848a73123ede7 --v=5
