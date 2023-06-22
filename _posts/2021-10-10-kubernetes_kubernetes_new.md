---
layout: post
title: Install Kubernetes
category: Kubernetes
tag: kubernetes
---

### For the master node

1. 
```
sudo apt-get update
sudo apt-get upgrade
```

2. considering that `docker` is already instaled

3. Add Kubernetes Signing Key: Kubernetes packages are signed with a key to ensure they're genuine.

```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
```

4. Add the Kubernetes repository: Kubernetes isn't in the standard Ubuntu repositories, so you'll need to add its repo.

```
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
```

5. Install Kubernetes: Now you can install Kubernetes itself.

```
sudo apt-get install kubeadm 
```

6. Disable swap
```
sudo swapoff -a
```

7. Initialize Kubernetes on the Master Node:
```
sudo kubeadm init
```

If the below error comes up:
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

8. Install network plugin
```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

=============================================

### For the worker node

1. 
```
sudo apt-get update
sudo apt-get upgrade
```

2. considering that `docker` is already instaled

3. Add Kubernetes Signing Key: Kubernetes packages are signed with a key to ensure they're genuine.

```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
```

4. Add the Kubernetes repository: Kubernetes isn't in the standard Ubuntu repositories, so you'll need to add its repo.

```
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
```

5. Install Kubernetes: Now you can install Kubernetes itself.

```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

6. Join into master
```
sudo kubeadm join ----
```

> To see the token of master again:
```
kubeadm token create --print-join-command
```

> If the below error comes up:
```
[preflight] Running pre-flight checks
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...
[kubelet-check] Initial timeout of 40s passed.
error execution phase kubelet-start: error uploading crisocket: Unauthorized
To see the stack trace of this error execute with --v=5 or higher
```
```
sudo systemctl enable docker
sudo systemctl enable kubelet
systemctl daemon-reload
systemctl restart docker
systemctl restart kubelet
```

Then, again: 
```
sudo kubeadm join ----
```

> If the existing `kubelet` config and port error come up:
```
sudo kubeadm reset
```