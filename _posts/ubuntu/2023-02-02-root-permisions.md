---
layout: post
title: Empower root permissions
category: Ubuntu
tag: root permissions
---

1. 사용자 계정 추가
```
sudo adduser <user name>
```

2. 관리자에 추가
```
sudo vim /etc/sudoers
```

```python
root ALL=(ALL:ALL) ALL
```
위와 같이 추가하기
```python
<user name> ALL=(ALL:ALL) ALL
```

3. 사용자 리스트에서 권한 변경
```
sudo vim /etc/passwd
```

`<user name>:x:____:____: .... `에서 `____`을 `0`으로 바꾼다

4. `<user name>`을 `root` 그룹에 추가
```
sudo vim /etc/group
```

에서 `root:x:0:<user name>`을 추가한다.

5. 확인
``` 
su - <user name>
```



