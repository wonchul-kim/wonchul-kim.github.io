---
layout: post
title: Kubernetes
category: mlops
tag: kubernetes
---


# How to install Kubernetes on Ubuntu 

* OS: Ubuntu 20.04 LTS

## install docker

* https://docs.docker.com/engine/install/ubuntu/


```
sudo apt-get update
```

```
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

```
sudo apt-get update
```

```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

```
systemctl status docker
```
```
sudo docker run hello-world
```

## install kubernetes

* https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

With root mode, 
```
swapoff -a && sed -i '/swap/s/^/#/' /etc/fstab

cat <<EOF > /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sysctl --system

systemctl stop firewalld 
systemctl disable firewalld

sudo apt-get update 
sudo apt-get install -y apt-transport-https ca-certificates curl

#sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

#echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
#sudo apt-get install -y kubeadm=1.18.19-00 kubelet=1.18.19-00 -

curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.18.20/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client

sudo apt-mark hold kubelet kubeadm kubectl

systemctl restart kubelet
```

* https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/

#### for master node

```
kubeadm init
```
> need to save tokens!!!

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

```
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | 
base64 | tr -d '\n')"

kubectl get pod --all-namespaces
kubectl get nodes
```

#### for worker nodes

master node에서 저장한 token 그대로 붙여서 사용


#### setting kubectl autocomplete

* https://kubernetes.io/docs/reference/kubectl/cheatsheet/

```
source <(kubectl completion bash) # setup autocomplete in bash into the current shell, bash-completion package should be installed first.
echo "source <(kubectl completion bash)" >> ~/.bashrc # add autocomplete permanently to your bash shell.
```


## etc.

* not to use ui
```
systemctl isolate multi-user.target
```

* issue:

```
vim /etc/docker/daemon.json
```
then add
```
{
  "exec-opts": ["native.cgroupdriver=systemd"]
}
```
```
systemctl restart docker
systemctl status docker
```

* connect to kubernetes nodes(master & nodes)

```
ssh -p [port] [node-hostname]@127.0.0.1
```

* master와 worker의 설정을 다시하고자 할 때, 
```
kubeadm reset
```
을 하고, 다시 kubeadm으로 설치


* to downgrade kubernetes
```
sudo apt-get install -y kubectl=1.18.19-00 kubeadm=1.18.19-00 kubelet=1.18.19-00 --allow-downgrades --allow-change-held-packages
```

* to uninstall kubernetes

```
kubeadm reset
sudo apt-get purge kubeadm kubectl kubelet kubernetes-cni kube*   
sudo apt-get autoremove  
sudo rm -rf ~/.kube
```

