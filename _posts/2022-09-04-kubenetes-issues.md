---
layout: post
title: kubernetes issues
category: mlops
tag: kubernetes
---


### 컴퓨터를 재부팅하고 kubernetes를 건드려보려고 할 때, 다음과 같이 kubernetes가 동작하지 않을 때:

#### 현상
```
kubectl get node
```
를 하니, 다음의 오류가 출력됨
```
The connection to the server 192.168.0.105:6443 was refused - did you specify the right host or port?
```

#### 해결법

1)
```
sudo -i
swapoff -a
exit
strace -eopenat kubectl version
```

2)
```
sduo systemctl restart kubelet
```

-------------------------------------------

