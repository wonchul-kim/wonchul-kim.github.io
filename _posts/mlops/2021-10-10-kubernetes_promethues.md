---
layout: post
title: Install Prometheus using Helm
category: MLOps
tag: prometheus
---

## Install Prometheus using Helm

### 1. Add the Helm chart repository for Prometheus Operator

The Prometheus Operator chart is hosted on the prometheus-community Helm repository. 
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

### 2. Update Helm Chart Repository
```
helm repo update
```

### 3. Install Prometheus Operator

- To install it with the release name prometheus and in the default namespace, you can run:.
```
helm install prometheus prometheus-community/kube-prometheus-stack
```

- values.yaml to specify the worker/master node 
```yaml
prometheus:
  prometheusSpec:
    nodeSelector:
      kubernetes.io/hostname: <node name>
```

- Specify a namespace with -n flag
```
kubectl create namespace monitoring
helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring -f values.yaml
```

### 4. Verify Installation
```
kubectl get pods -n monitoring
```

### 5. Make prometheus server open to public 

we need to edit prometgeus service. Need to change type from `ClusterIP` to `NodePort`.
```
KUBE_EDITOR="vim" kubectl edit svc prometheus-kube-prometheus-prometheus -n monitoring
```    
Originally, it seems like the below:
```yaml
# Please edit the object below. Lines beginning with a '#' will be ignored,
# and an empty file will abort the edit. If an error occurs while saving this file will be
# reopened with the relevant failures.
#
apiVersion: v1
kind: Service
metadata:
  annotations:
    meta.helm.sh/release-name: prometheus
    meta.helm.sh/release-namespace: aivdl
  creationTimestamp: "2024-06-26T00:37:39Z"
  labels:
    app: kube-prometheus-stack-prometheus
    app.kubernetes.io/instance: prometheus
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/part-of: kube-prometheus-stack
    app.kubernetes.io/version: 60.4.0
    chart: kube-prometheus-stack-60.4.0
    heritage: Helm
    release: prometheus
    self-monitor: "true"
  name: prometheus-kube-prometheus-prometheus
  namespace: aivdl
  resourceVersion: "150695"
  uid: 943a75b7-41b0-48db-bda3-fa12d9af70d3
spec:
  clusterIP: 10.106.251.187
  clusterIPs:
  - 10.106.251.187
  internalTrafficPolicy: Cluster
  ipFamilies:
  - IPv4
  ipFamilyPolicy: SingleStack
  ports:
  - name: http-web
    nodePort: 30090
    port: 9090
    protocol: TCP
    targetPort: 9090
  - appProtocol: http
    name: reloader-web
    port: 8080
    protocol: TCP
    targetPort: reloader-web
  selector:
    app.kubernetes.io/name: prometheus
    operator.prometheus.io/name: prometheus-kube-prometheus-prometheus
  sessionAffinity: None
  type: ClusterIP
status:
  loadBalancer: {}
```



- Ports should be like this:

```yaml
  ports:
  - name: http-web
      nodePort: 30090 # to be added
      port: 9090
      protocol: TCP
      targetPort: 9090
  - name: reloader-web
      nodePort: 30237 # to be added
      port: 8080
      protocol: TCP
      targetPort: reloader-web
  selector:
      app.kubernetes.io/name: prometheus
      prometheus: prometheus-kube-prometheus-prometheus
  sessionAffinity: None
  type: NodePort # to be changed

```

---------------------
## Install dcgm-exporter(Nvidia-gpu exporter) using Helm
```
helm repo add gpu-helm-charts https://nvidia.github.io/dcgm-exporter/helm-charts
helm repo update
helm install dcgm-exporter gpu-helm-charts/dcgm-exporter -n monitoring
```

The label of the exporter should be `prometheus`. 
If not, prometheus cannot get metric info from dcgm-exporter.
```
kubectl label servicemonitors.monitoring.coreos.com dcgm-exporter release=prometheus -n monitoring
```
