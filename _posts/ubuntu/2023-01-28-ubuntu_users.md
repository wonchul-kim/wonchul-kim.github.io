---
layout: post
title: Setting Users
category: Ubuntu
tag: users
---

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



  
