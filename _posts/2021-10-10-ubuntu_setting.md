---
layout: post
title: Ubuntu settings
category: etc.
tag: ubuntu settings
---

## To install nvidia-driver for GPUs

0. check the available nvidia-drivers

```
ubuntu-drivers devices
```

- 현재 사용중인 그래픽카드 확인
  ```
  lshw -numeric -C display
  lspci | grep -i nvidia
  ```

1. automatically install

```
sudo ubuntu-drivers autointall
```

or install specific version of driver

```
sudo apt install nvidia-driver-xxx
```

2. install cuda

3. install cudnn

4. install libraries

- pytorch for rtx3XXX
  ```
  pip install torch==1.7.1+cu110 torchvision==0.8.2+cu110 torchaudio===0.7.2 -f https://download.pytorch.org/whl/torch_stable.html
  ```
- tensorflow
  ```
  pip install tensorflow-gpu==2.4 -i http://pypi.douban.com/simple --trusted-host pypi.douban.com
  ```
  ```
  tf.test.is_gpu_available()
  tf.config.list_physical_devices()
  ```

## To mount ubuntu with NAS

1. install cifs-utils

```
sudo apt-get install cifs-utils
```

2. make folder to mount with NAS

```
mkdir /mnt
```

3. mount

```
sudo mount -t cifs //192.168.0.15/01.Data /home/wonchul/mnt/NAS/Data/ -o user=user,pass=pass,rw
```

4. make it standard

```
sudo vim /etc/fstab
```

Add the below line at the bottom

```
//192.168.0.15/01.Data /home/wonchul/mnt/NAS/Data/ cifs user=abc,pass=123,rw   0   0
```

## To mount with samba

Ubuntu에서 smb로 공유한 폴더를 다른 네트워크의 컴퓨터에 공유하여 mount시킬 때

1. install smb

```
sudo apt-get install smbclient cifs-utils
```

2. mount

```
sudo mount -t cifs //컴퓨터이름(혹은 주소)/공유이름 /공유할/디렉토리
```

- 해당 ubuntu에 아이디와 암호가 있는 경우

```
-o username=계정이름,password=암호
```

- 공유한 폴더를 mount하였는데, 해당 폴더를 사용(폴더나 파일 생성)할 때 `permission denied`가 발생하는 경우

```
-o uid=uid,gid=gid
```

- 서버쪽이 Utf-8이 아니라서 한글이 깨지는 경우

```
-o iocharset=utf8,codepage=cp949
```

3. 참조

https://ddbobd.tistory.com/3

## To fix ubuntu computer IP address

1. ethernet 이름 확인

```
ifconfig -a
```

2. network configuration 수정

```
sudo vim /etc/netplan/01-network-manager-all.yaml
```

```
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    <ethernet 이름>:
      dhcp6: no
      addresses: [192.168.59.<원하는 IP>/24]
      gateway4: 192.168.59.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```

3. 적용

```
sudo netplan apply
```

## To change user

1. 임시 유저를 만들어 로그인
```
sudo adduser <tmp user name>
sudo adduser <tmp user name> sudo 
```

2. 기존 유저에서 로그인하고 <tmp user>로 로그인
  
3. 바꾸고자 하는 유저 변경하기 (이름과 디렉토리 모두)
```
sudo usermod -l <new user name> <current user name>
sudo suermod -m -d /home/<new user name> <new user name>
```
  
4. 임시 유저 삭제 
```
sudo deluser <tmp user name>
sudo rm -r /home/<tmp user name>
```

### To change hostname
```
hostnamectl set-hostname abc
```


## To increase swap

학습을 하다보면, 아무런 error 없이 강제종료(killed)가 되는 경우가 있는데 이런 경우 swap이 모자라기 때문이다.

1. check swap

```

free

```

2. swap 비활성

```

sudo swapoff -v /swapfile

```

3. swap 을 8GB 로 조정한 경우

```

sudo fallocate -l 8G /swapfile

```

4. 권한 설정

```

sudo chmod 600 /swapfile

```

5. swap file 만들기

```

sudo mkswap /swapfile

```

6. swap file 활성화

```

sudo swapon /swapfile

```

7. 컴퓨터를 껐다가 켜도 수정이 자동으로 되도록 /etc/fstab에서 아래의 내용으로 수정 또는 추가한다.

```

swapfile none swap sw 0 0

```
  

## To turn on syslog message 
  
```
sudo vim /etc/rsyslog.d/50-default.conf
```
  
uncomment the below content
```
*.=info;*.=notice;*.=warn;\
       auth,authpriv.none;\
       cron,daemon.none;\
       mail,news.none          -/var/log/messages
```

Then, restart the rsyslog 
```
sudo service rsyslog restart
ll /var/log/messages  # to check the file is generated
```
    
  

## Issues

#### apt-install is not available

#### pip commands are not working at all

    https://richwind.co.kr/172


  
