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

* 현재 사용중인 그래픽카드 확인
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

* pytorch for rtx3XXX
    ```
    pip install torch==1.7.1+cu110 torchvision==0.8.2+cu110 torchaudio===0.7.2 -f https://download.pytorch.org/whl/torch_stable.html
    ```
    
* tensorflow
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


## Issues

#### apt-install is not available

#### pip commands are not working at all

    https://richwind.co.kr/172