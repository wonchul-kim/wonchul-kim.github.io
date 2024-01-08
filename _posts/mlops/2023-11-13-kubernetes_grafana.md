---
layout: post
title: Install Grafana using Helm
category: MLOps
tag: grafana
---

## Install Grafana using Helm

### 1. Add the Helm chart repository for Prometheus Operator

```
helm repo add grafana https://grafana.github.io/helm-charts
```

### 2. Clone Helm Chart Repository
```
git clone https://github.com/grafana/helm-charts.git
```

### 3. Edit `values.yaml` for grafana

In helm-chart/charts/grafana/values.yaml,

- adminUser & adminPassword
```yaml
# Administrator credentials when not using an existing secret (see below)
adminUser: admin
adminPassword: strongpassword
```

- set service type as `NodePort` (originally, it is `ClusterIP`)
```yaml
service:
  enabled: true
  type: NodePort
  port: 80
  targetPort: 3000
    # targetPort: 4181 To be used with a proxy extraContainer
  ## Service annotations. Can be templated.
  annotations: {}
  labels: {}
  portName: service
  # Adds the appProtocol field to the service. This allows to work with istio protocol selection. Ex: "http" or "tcp"
  appProtocol: ""
```

- set persistence to maintain data
```yaml
persistence:
  type: pvc
  enabled: true  
# storageClassName: default
  accessModes:
    - ReadWriteOnce
  size: 10Gi
```

### 4. Install 
```
helm install gfn grafana/grafana -f values.yaml --namespace <namesapce>                         
```

If you edit values.yaml and then, apply it:
```
helm upgrade gfn grafana/grafana -f values.yaml --namespace <namesapce>
```

### 5. Verify Installation
```
kubectl get svc -o wide -n monitoring
```

Check IP and port, and then access to `localhost:<port>`

