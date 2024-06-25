---
layout: post
title: Install Kubernetes - master node
category: MLOps
tag: kubernetes
---

## For the master node

#### 1. update
```
sudo apt-get update
sudo apt-get upgrade
```

#### 2. considering that `docker` is already instaled

#### 3. Add Kubernetes Signing Key: Kubernetes packages are signed with a key to ensure they're genuine.

```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
```

#### 4. Add the Kubernetes repository: Kubernetes isn't in the standard Ubuntu repositories, so you'll need to add its repo.

```
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
```

> If there has been error about that repo, you should follow the installation process in official docs. Refer the reference!!!

#### 5. Install Kubernetes: Now you can install Kubernetes itself.

```
sudo apt-get install kubeadm 
```

#### 6. Disable swap
```
sudo swapoff -a
```

#### 7-1. Initialize Kubernetes on the Master Node:
```
sudo kubeadm init
```

> If the below error comes up:
```
[init] Using Kubernetes version: v1.27.3
[preflight] Running pre-flight checks
error execution phase preflight: [preflight] Some fatal errors occurred:
    [ERROR CRI]: container runtime is not running: output: time="2023-06-22T17:37:50+09:00" level=fatal msg="validate service connection: CRI v1 runtime API is not implemented for endpoint \"unix:///var/run/containerd/containerd.sock\": rpc error: code = Unimplemented desc = unknown service runtime.v1.RuntimeService"
, error: exit status 1
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher
```

```
sudo rm /etc/containerd/config.toml
sudo systemctl restart containerd
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```


#### 7-2. To start using your cluster, 

When the initialization finished, the below content will come up.
<img src='/assets/mlops/kube-init.png'>

```cmd
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

#### 8. Install network plugin
```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```


## References
- [Official Docs to install k8s](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
- https://diamond-goose.tistory.com/65