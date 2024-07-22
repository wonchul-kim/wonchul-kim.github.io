---
layout: post
title: Install Grafana from prometheus-community using helm
category: MLOps
tag: grafana
---


### WHen installing prometheus from `prometheus-community/kube-prometheus-stack`, prometheus-grafana is also installed. Then, you just need to edit some configuration to use it.


#### Make grafana server open to public 

we need to edit grafana service. Need to change type from `ClusterIP` to `NodePort`.
```
KUBE_EDITOR="vim" kubectl edit svc <grafana service name (e.g. prometheus-grafana)> -n monitoring
```    

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
    meta.helm.sh/release-namespace: monitoring
  creationTimestamp: "2024-07-22T03:43:20Z"
  labels:
    app.kubernetes.io/instance: prometheus
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: grafana
    app.kubernetes.io/version: 11.0.0
    helm.sh/chart: grafana-8.0.2
  name: prometheus-grafana
  namespace: monitoring
  resourceVersion: "4789830"
  uid: b32d8a2a-f78f-44ea-a05a-72e2dfb01b1f
spec:
  clusterIP: 10.108.25.56
  clusterIPs:
  - 10.108.25.56
  externalTrafficPolicy: Cluster
  internalTrafficPolicy: Cluster
  ipFamilies:
  - IPv4
  ipFamilyPolicy: SingleStack
  ports:
  - name: http-web
    nodePort: 32312 # TO BE CHANGED TO 30091 (or you want)
    port: 80
    protocol: TCP
    targetPort: 3000
  selector:
    app.kubernetes.io/instance: prometheus
    app.kubernetes.io/name: grafana
  sessionAffinity: None
  type: ClusterIP # TO BE CHANGED TO NodePort
status:
  loadBalancer: {}                
```


#### Admin & Password

The initial `username` and `password` are:

* `username`: admin
* `password`: prom-operator