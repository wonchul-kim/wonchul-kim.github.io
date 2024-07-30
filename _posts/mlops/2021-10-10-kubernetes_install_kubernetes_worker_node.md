---
layout: post
title: Install Kubernetes - worker node
category: MLOps
tag: kubernetes
---

## For the worker node

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

#### 4. Follow the [official site tutorial](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

- Update the apt package index and install packages needed to use the Kubernetes apt repository

```cmd
sudo apt-get update
# apt-transport-https may be a dummy package; if so, you can skip that package
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
```

- Download the public signing key for the Kubernetes package repositories. The same signing key is used for all repositories so you can disregard the version in the URL

```cmd
# If the directory `/etc/apt/keyrings` does not exist, it should be created before the curl command, read the note below.
sudo mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```

- Add the appropriate Kubernetes apt repository. Please note that this repository have packages only for Kubernetes 1.30; for other Kubernetes minor versions, you need to change the Kubernetes minor version in the URL to match your desired minor version (you should also check that you are reading the documentation for the version of Kubernetes that you plan to install).

```cmd
# This overwrites any existing configuration in /etc/apt/sources.list.d/kubernetes.list
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```

- Update the apt package index, install kubelet, kubeadm and kubectl, and pin their version

```cmd
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

#### 5. Disable swap
```
sudo swapoff -a
```

> DO NOT recover swap

#### 6. Join into master
```
sudo su
```

```
sudo kubeadm join ----
```

> To see the token of master again:
```
kubeadm token create --print-join-command
```

> If the below error comes up:

* case 1) 
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

* case 2)
```
[preflight] Running pre-flight checks
	[WARNING Swap]: swap is enabled; production deployments should disable swap unless testing the NodeSwap feature gate of the kubelet
error execution phase preflight: [preflight] Some fatal errors occurred:
	[ERROR CRI]: container runtime is not running: output: time="2023-06-29T17:34:37+09:00" level=fatal msg="validate service connection: CRI v1 runtime API is not implemented for endpoint \"unix:///var/run/containerd/containerd.sock\": rpc error: code = Unimplemented desc = unknown service runtime.v1.RuntimeService"
, error: exit status 1
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher
```

Need to delete the existing `kubelet` config:

```
sudo rm /etc/containerd/config.toml
sudo systemctl restart containerd
```

* case 3)
```
[preflight] Running pre-flight checks
error execution phase preflight: [preflight] Some fatal errors occurred:
	[ERROR FileAvailable--etc-kubernetes-kubelet.conf]: /etc/kubernetes/kubelet.conf already exists
	[ERROR FileAvailable--etc-kubernetes-pki-ca.crt]: /etc/kubernetes/pki/ca.crt already exists
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
To see the stack trace of this error execute with --v=5 or higher
```


Then, again: 
```
sudo kubeadm join ----
```


## References
- [Official Docs to install k8s](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
- https://diamond-goose.tistory.com/65