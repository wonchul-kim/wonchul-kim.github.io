---
layout: post
title: Install Prometheus using Helm
category: Kubernetes
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

- Specify a namespace with -n flag
```
kubectl create namespace monitoring
helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring
```

### 4. Verify Installation
```
kubectl get pods -n monitoring
```

### 5. Make prometheus server open to public 

we need to edit prometgeus service. Need to change type from `ClusterIP` to `NodePort`.
```
'KUBE_EDITOR="vim" kubectl edit svc prometheus-kube-prometheus-prometheus -n monitoring'
```    

- Ports should be like this:
```python
  'ports:
  - name: http-web
      nodePort: 30090
      port: 9090
      protocol: TCP
      targetPort: 9090
  - name: reloader-web
      nodePort: 30237
      port: 8080
      protocol: TCP
      targetPort: reloader-web
  selector:
      app.kubernetes.io/name: prometheus
      prometheus: prometheus-kube-prometheus-prometheus
  sessionAffinity: None
  type: NodePort
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
