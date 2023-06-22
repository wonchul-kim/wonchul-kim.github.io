---
layout: post
title: Install Kubernetes
category: Kubernetes
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

#### 1. swap off
```
swapoff -a && sed -i '/swap/s/^/#/' /etc/fstab
```
Or annotate it in `/etc/fstab`
```
#/swap.img      none    swap    sw      0       0
sudo swapoff -a
```

#### 2. let iptables see bridged traffic
```
cat <<EOF > /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sysctl --system
```

#### 3. disable firewall

systemctl stop firewalld 
systemctl disable firewalld

#### 4. install kubenetes
```
sudo apt-get update && sudo apt-get install -y apt-transport-https curl
```
```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```
```
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
```
```
sudo apt-get update
```

#### 5. install kubelet kubeadm kubectl 1.19.16
```
apt-get install -y kubelet=1.19.16-00 kubeadm=1.19.16-00 kubectl=1.19.16-00
apt-mark hold kubelet kubeadm kubectl
```

```
systemctl start kubelet
systemctl enable kubelet
```
------------------------------------------------------------------------------
## Set control-plane

#### 1. for master node
In the master computer, 
```
kubeadm init
```

> make sure to save tokens to add nodes into master

when you type the below, 
```
kubectl get nodes
```
you need permission and to do this, 
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

If you want to use the `kubectl` command with another user, need to do the same thing in that user

#### 2. install a pod network add-on
In master computer, 
```
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
kubectl get nodes
```

#### 3. for worker nodes
In node computer,

copy & paste the koten you saved in master computer.

------------------------------------------------------------------------------

## Etc.

* https://kubernetes.io/docs/reference/kubectl/cheatsheet/

```
source <(kubectl completion bash) # setup autocomplete in bash into the current shell, bash-completion package should be installed first.
echo "source <(kubectl completion bash)" >> ~/.bashrc # add autocomplete permanently to your bash shell.

source <(kubeadm completion bash) # setup autocomplete in bash into the current shell, bash-completion package should be installed first.
echo "source <(kubeadm completion bash)" >> ~/.bashrc # add autocomplete permanently to your bash shell.
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

