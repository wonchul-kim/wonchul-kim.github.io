---
layout: post
title: Mount Settings
category: Ubuntu
tag: ubuntu mounts
---

## To mount external storages (less than 2TB)

1. Check external disks
```
sudo fdisk -l
```

> 이미 사용 중이던 외장하드를 그대로 마운트하는 것이라면, 파티션을 생성하고 포맷을 하지 않고, 4번부터 실행한다.


2. Generate partition
```
sudo fdisk /dev/sdb
```

<img src='/assets/ubuntu_setting_mounts/partition.png'>

3. Partition format
```
sudo mkfs.ext4 /dev/sdb1
```

4. Check UUID 
```
sudo blkid
```

5. Mount 

#### Generate folder to be used for mount
```
sudo mkdir -p /data
```

#### Mount that folder with external disk using UUID
```
sudo vim /etc/fstab
```

* Insert the below content:
    ```
    UUID=2080bf0c-3a97-4d3e-869c-f4b11d18fbcf /data ext4 defaults 0 0
    ```

* and then, apply changes:
    ```
    sudo mount -a
    ```

### Check
```
df -h
```


----------------------------------------------------------------

## To mount external storages (more than 2TB)

https://minimin2.tistory.com/158

----------------------------------------------------------------

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

## To mount external storages (lower than 2TB)


https://minimin2.tistory.com/158


----------------------------------------------------------------


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



  
