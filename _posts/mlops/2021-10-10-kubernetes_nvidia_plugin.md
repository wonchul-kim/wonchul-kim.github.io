---
layout: post
title: Install Nvidia Plugin
category: MLOps
tag: kubernetes
---

## To use GPUs in kubernetes

### For worker node,

#### Step 1: Install containerd: 
```cmd
sudo apt-get update
sudo apt-get install containerd
```

#### Step 2: Create the containerd configuration file:
```cmd
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
sudo nano /etc/containerd/config.toml
```

The part that will be changed is:

```toml
	[plugins]
		...
		[plugins."io.containerd.grpc.v1.cri"]
			...
			[plugins."io.containerd.grpc.v1.cri".containerd]
			default_runtime_name = "nvidia" #### changed ----------------------

			...
			[plugins."io.containerd.grpc.v1.cri".containerd.runtimes]
				[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.nvidia] #### add the entire below ------
					privileged_without_host_devices = false
					runtime_engine = ""
					runtime_root = ""
					runtime_type = "io.containerd.runc.v2"
					[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.nvidia.options]
						BinaryName = "/usr/bin/nvidia-container-runtime"'
```

#### step 3: Restart contenerd
```cmd
sudo systemctl restart containerd
```

#### step 4: When running kubernetes with docker, edit the config file which is usually present at /etc/docker/daemon.json to set up nvidia-container-runtime as the default low-level runtime:
```yaml
{
	"default-runtime": "nvidia",
	"runtimes": {
		"nvidia": {
			"path": "/usr/bin/nvidia-container-runtime",
			"runtimeArgs": []
		}
	}
}
```

#### step 5: Restart docker
```cmd
sudo systemctl restart docker
```

### For master node,
#### step 6: Run nvidia-plugin in master node.
```cmd
kubectl apply -f nvidia-plugin.yml
```

```yaml
# nvidia-plugin.yml

apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nvidia-device-plugin-daemonset
  namespace: kube-system
spec:
  selector:
    matchLabels:
      name: nvidia-device-plugin-ds
  updateStrategy:
    type: RollingUpdate
  template:
    metadata:
      labels:
        name: nvidia-device-plugin-ds
    spec:
      tolerations:
      - key: node-role.kubernetes.io/master # - key: nvidia.com/gpu
        operator: Exists
        effect: NoSchedule
      priorityClassName: "system-node-critical"
      containers:
      - image: nvcr.io/nvidia/k8s-device-plugin:v0.14.0
        name: nvidia-device-plugin-ctr
        env:
          - name: FAIL_ON_INIT_ERROR
            value: "true"
          - name: MIG_STRATEGY
            value: "none"
          - name: NVIDIA_DRIVER_ROOT
            value: "/"
          - name: PASS_DEVICE_SPECS
            value: "false"
          - name: DEVICE_LIST_STRATEGY
            value: "envvar"
          - name: DEVICE_ID_STRATEGY
            value: "uuid"

        securityContext:
          allowPrivilegeEscalation: false
          capabilities:
            drop: ["ALL"]
        volumeMounts:
        - name: device-plugin
          mountPath: /var/lib/kubelet/device-plugins
        - name: dev
          mountPath: /dev
        - name: nvml
          mountPath: /usr/lib/x86_64-linux-gnu

      volumes:
      - name: device-plugin
        hostPath:
          path: /var/lib/kubelet/device-plugins
      - name: dev
        hostPath:
          path: /dev
      - name: nvml
        hostPath:
          path: /usr/lib/x86_64-linux-gnu
```

> If we have worker nodes, we have to do step1 ~ step5 (not step6) on every nodes manually. Step6 is only for the master node.
