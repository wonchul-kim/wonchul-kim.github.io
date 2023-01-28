---
layout: post
title: Fix IP Address
category: ubuntu
tag: ubuntu fix ip
---

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


  
