---
layout: post
title: Kubernetes
category: mlops
tag: kubernetes-dashboard
---

#### install 
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.4.0/aio/deploy/recommended.yaml
```

#### service-account.yml
```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
EOF
```
```
kubectl create -f service-account.yml
```

#### cluster-role-binding.yml
```
cat <<EOF | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
EOF
```
```
kubectl create -f cluster-role-binding.yml
```
#### edit type as LoadBalancer
```
KUBE_EDITOR=vim kubectl edit service -n kubernetes-dashboard kubernetes-dashboard
```
For the `type` section: ClusterPort -> NodePort

#### To get token
```
kubectl -n kubernetes-dashboard get secret $(kubectl -n kubernetes-dashboard get sa/admin-user -o jsonpath="{.secrets[0].name}") -o go-template="{{.data.token | base64decode}}"
```

#### configmap to login
```
sudo vim kubernetes-dashboard.configmap
```
```
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: REDACTED
    server: https://kubemaster:6443
  name: kubernetes
contexts:
- context:
    cluster: kubernetes
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
current-context: kubernetes-admin@kubernetes
kind: Config
preferences: {}
users:
- name: kubernetes-admin
  user:
    client-certificate-data: REDACTED
    client-key-data: REDACTED
    token: <TOKEN THAT YOU CHECKED THE ABOVE>
```

#### access
```
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```


