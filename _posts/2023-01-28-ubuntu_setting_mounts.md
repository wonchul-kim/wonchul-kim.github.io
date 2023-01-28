---
layout: post
title: Ubuntu Mount Settings
category: ubuntu
tag: ubuntu mounts
---

## To mount external storages

1. 하드디스크 확인
```
sudo fdisk -l
```
2. 파티션 생성
```
sudo fdisk /dev/sdb
```


https://psychoria.tistory.com/521

 



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



  
