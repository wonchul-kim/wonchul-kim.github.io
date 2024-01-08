---
layout: post
title: Swap memory settings.
category: Ubuntu
tag: ubuntu swap
---

https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04

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
