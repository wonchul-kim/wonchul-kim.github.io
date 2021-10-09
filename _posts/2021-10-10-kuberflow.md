---
layout: post
title: kubeflow
category: mlops
tag: kubeflow
---


# To run kubeflow 

## on kubernetes (k8s)

```
wget [해당 버전의 kfctl_darwin.tar.gz from https://github.com/kubeflow/kfctl/releases]
```

```
tar -xvf ....tar.gz
```

```
export PATH=$PATH:${pwd}
export KF_NAME='kubeflow'
export BASE_DIR='kubeflow'
export KF_DIR=${BASE_DIR}/${KF_NAME}
export CONFIG_FILE=${KF_DIR}/kfctl_k8s_istio.v1.2.0.yaml
export CONFIG_URI='https://raw.githubusercontent.com/kubeflow/manifests/v1.2-branch/kfdef/kfctl_k8s_istio.v1.2.0.yaml'

mkdir -p ${KF_DIR}
cd ${KF_DIR}

mv kfctl ./# 처음에 다운받고 풀었던 압축 파일을 현재의 폴더에 이동
kfctl build -V -f ${CONFIG_URI}

kfctl apply -V -f ${CONFIG_URI}
```





## microk8s

### To install & execute 
```
sudo snap intall microk8s --classic
```

```
microk8s enable kubeflow
```

### To uninstall
```
sudo snap remove microk8s --purge
```
-----------------------------------------------------------------------------------
#### Issues

* Problem
    ```
    Couldn't contact api.jujucharms.com
    Please check your network connectivity before enabling Kubeflow.
    Failed to enable kubeflow
    ```

* solution
    ```
    curl -v https://api.jujucharms.com/charmstore/v5/~kubeflow-charmers/ambassador-88/icon.svg
    ``` 

-----------------------------------------------------------------------------------

## minikf

### To install
* https://www.kubeflow.org/docs/distributions/minikf/minikf-vagrant/

### examples to use it 
* https://medium.com/kubeflow/an-end-to-end-ml-pipeline-on-prem-notebooks-kubeflow-pipelines-on-the-new-minikf-33b7d8e9a836

* https://jarikki.tistory.com/10?category=809726


https://lsjsj92.tistory.com/580



https://towardsdatascience.com/kubeflow-how-to-install-and-launch-kubeflow-on-your-local-machine-e0d7b4f7508f


https://github.com/kubeflow/manifests

https://github.com/kubeflow/pipelines/tree/master/manifests/kustomize



https://sidepun.ch/entry/Kubeflow-%EC%84%A4%EC%B9%98-Ubuntu-2104-minikube

## on minikube

```
# minikube 설치
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube

# kubectl 설치
curl -LO "https://dl.k8s.io/release/v1.19.3/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# kfctl 설치
wget https://github.com/kubeflow/kfctl/releases/download/v1.2.0/kfctl_v1.2.0-0-gbc038f9_linux.tar.gz
tar -xvf kfctl_v1.2.0-0-gbc038f9_linux.tar.gz
export PATH=$PATH:$PWD
kfctl version
```

```
minikube start \
    --kubernetes-version v1.19.3 \
    --cpus 6 \
    --memory 10240 \
    --driver docker \
    --extra-config=apiserver.service-account-signing-key-file=/var/lib/minikube/certs/sa.key \
    --extra-config=apiserver.service-account-issuer=kubernetes.default.svc \
    --extra-config=apiserver.feature-gates=TokenRequest=true \
    --extra-config=apiserver.feature-gates=TokenRequestProjection=true
```

```
# kubeflow 설치
wget -O kfctl_istio_dex.yaml https://raw.githubusercontent.com/kubeflow/manifests/v1.2-branch/kfdef/kfctl_istio_dex.v1.2.0.yaml

kfctl apply -V -f kfctl_istio_dex.yaml
```

```
# port-forward 를 통해 centraldashboard 에 접속해보자
kubectl port-forward svc/istio-ingressgateway -n istio-system 8080:80      
```

* email address: admin@kubeflow.org 
* password: 12341234 

