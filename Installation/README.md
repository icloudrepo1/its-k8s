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


1. create TWO ubuntu ec2 instance
2. t2.medium
3. k8s-key.pem
4. ssh,all traffic ( anywhere )
5. launch instance
6. now connect , instance



## 1. Execute on Both "Master" & "Worker Node"

Run the following commands on both the master and worker nodes to prepare them for kubeadm.

```bash
# disable swap
sudo swapoff -a

# Create the .conf file to load the modules at bootup
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

# sysctl params required by setup, params persist across reboots
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

# Apply sysctl params without reboot
sudo sysctl --system

## Install CRIO Runtime
sudo apt-get update -y
sudo apt-get install -y software-properties-common curl apt-transport-https ca-certificates gpg

sudo curl -fsSL https://pkgs.k8s.io/addons:/cri-o:/prerelease:/main/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/cri-o-apt-keyring.gpg
echo "deb [signed-by=/etc/apt/keyrings/cri-o-apt-keyring.gpg] https://pkgs.k8s.io/addons:/cri-o:/prerelease:/main/deb/ /" | sudo tee /etc/apt/sources.list.d/cri-o.list

sudo apt-get update -y
sudo apt-get install -y cri-o

sudo systemctl daemon-reload
sudo systemctl enable crio --now
sudo systemctl start crio.service

echo "CRI runtime installed successfully"

# Add Kubernetes APT repository and install required packages
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update -y
sudo apt-get install -y kubelet="1.29.0-*" kubectl="1.29.0-*" kubeadm="1.29.0-*"
sudo apt-get update -y
sudo apt-get install -y jq

sudo systemctl enable --now kubelet
sudo systemctl start kubelet
```

---

## 2. Execute ONLY on "Master Node"

```bash
sudo kubeadm config images pull

sudo kubeadm init

mkdir -p "$HOME"/.kube
sudo cp -i /etc/kubernetes/admin.conf "$HOME"/.kube/config
sudo chown "$(id -u)":"$(id -g)" "$HOME"/.kube/config


# Network Plugin = calico
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/calico.yaml

kubeadm token create --print-join-command
```

- You will get `kubeadm token`, **Copy it**.
  <img src="https://raw.githubusercontent.com/faizan35/kubernetes_cluster_with_kubeadm/main/Img/kubeadm-token.png" width="75%">

---

## 3. Execute on ALL of your Worker Node's

1. Perform pre-flight checks

   ```bash
   sudo kubeadm reset pre-flight checks
   ```

2. Paste the join command you got from the master node and append `--v=5` at the end.

   ```bash
   sudo your-token --v=5
   ```

   > Use `sudo` before the token.

---

## 4. Verify Cluster Connection

**On Master Node:**

```bash
kubectl get nodes
```

   <img src="https://raw.githubusercontent.com/faizan35/kubernetes_cluster_with_kubeadm/main/Img/nodes-connected.png" width="70%">

---

## Optional: create pod inside worker node-1


#### 1. Go to your worker node-1 system and change your hostname

```bash
sudo hostnamectl set-hostname worker-1
```

#### 2. For Apply

```bash
sudo reboot
```

#### 3. Go to Master node and delete existing worker_node-1 ( ip add-type )


```bash
sudo kubectl delete node <worker_node-1_ip>
```

```bash
sudo kubectl get nodes
```


#### 4. Go to Worker node rejoin again


```bash
sudo kubeadm reset
```

```bash
sudo your-token --v=5
```

#### 5. Go to Master node and check node joined or not


```bash
sudo kubectl get nodes
```

#### 6. Go to Master node and create pod


```bash
vi mypodcrt.yml
```


```
apiVersion: v1
kind: Pod
metadata:
  name: pod1
spec:
  nodeSelector:
    kubernetes.io/hostname: worker-1
  containers:
    - name: container1
      image: nginx
      ports:
        - containerPort: 80

```


#### A. Create the pod


```
kubectl apply -f mypodcrt.yml
```


#### B. Verify the pod's status


```
kubectl get pods
```

```
kubectl get pod pod1 -o wide

```

===========================END=====================================================
