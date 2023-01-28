---
layout: post
title: kubernetes 따라하기 1
category: MLOps
tag: kubernetes
---

## [따배쿠] 3-1. kubectl 실습 / 실습환경 구성하기

### kubectl 이란

사용자가 kubernetes에 원하는 명령을 전달할 떄 사용하는 API

```
kubectl [command] [type] [name] [flag]
```
* command: 자원(object)에 실행할 명령어 (create, get, delete, edit, ...)
* type: 자원의 종류 (node, pod, service, ...)
* name: 자원의 이름
* flag: 옵션

e.g.)
```
kubectl get pod webservice -o wide
```

## [따배쿠] 3-2. kubectl command / pod 생성하기

#### kubernetes에 존재하는 모든 object에 대한 약어 정보 보기
```
kubectl api-resources
```

#### help를 많이 이용하자
```
kubectl --help
```

#### node 정보 보기
```
kubectl get nodes
kubectl get nodes -o wide
kubectl describe node [node name]
```

#### pod(container)

* 한 개만 생성
  ```
  kubectl run webserver --image=nginx:1.14 --port 80
  ```
  
  > conatainer를 하나만 실행시키고자 할 때는 `run` command 사용

* 띄워지는 과정이나 pod에 대한 자세한 정보를 보고 싶다면,
  ```
  kubectl get nodes
  kubectl get nodes -o wide
  kubectl get nodes -o yaml
  kubectl describe pod webserver
  ```

* 생성한 곳에 접속해서 직접 확인해보자
  ```
  curl <IP>
  ```
  
  ```
  sudo apt install elinks
  elinks <IP>
  ```

* 여러 개를 생성하고자 할때,
  ```
  kubectl create depolyment <name> <image> <number of pods>
  ```
  
  e.g.)
  ```
  kubectl create depolyment mainui --image=httpd --replicas=3
  ```
  
  확인을 할 때는 
  ```
  kubectl get pods
  ```
  도 가능 하지만, `deployment`를 사용했으므로
  ```
  kubectl get deployment
  ```
  도 가능하다.
  
* pod(container) 내부 접속하기
  ```
  kubectl exec [name] -it -- /bin/bash
  ```
  
  e.g.)
  ```
  kubectl exec webserver -it -- /bin/bash
  ```

* log를 보자
  ```
  kubectl logs [name]
  ```
  
  e.g.)
  ```
  kubectl logs webserver
  ```
  
* port-forward
  ```
  kubectl port-forward webserver 8080:80
  ```
  
* 실행한 `pod` 내용 바꾸기
  ```
  kubectl edit [type] [name]
  ```
  
  e.g.)
  ```
  kubectl edit deployment mainui
  ```
  내부 정보를 보면서 recplicas를 통해서 pod의 갯수를 조정하는 등의 해당 pod에 대한 정보를 수정할 수 있다.
  
* kubernetes에서 container를 띄우기 위한 yaml temaplate를 만들어보자
  ```
  kubectl run webserver --iamge=nginx:1.14 --port 80 --dry-run=client -o yaml > webserver.yaml
  ```
  
  만든 `yaml`을 통해서 container를 띄워보자
  ```
  kubectl create -f webserver.yml
  ```
  
  





