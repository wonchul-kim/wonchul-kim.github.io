---
layout: post
title: Cmake
category: Ubuntu
tag: cmake
---

### DO NOT install with `apt`

```
sudo apt install cmake
```

This will install the old version of `cmake`...


### How to install cmake

#### 1. Before installing cmake

```cmd
sudo apt install libssl-dev
```

Or Need to set `OpenSSL` option as False


If cmake is already installed with old version, need to remove it
```
sudo apt remove cmake 
```


#### 2. Download the install files from the [official site](https://cmake.org/download/)

```cmd
tar -xvzf <filename>.tar.gz
cd <filename>
./boostrap --prefix=/usr/local
make
make install
```

```cmd
cmake --version
```

#### 3. When you cnanot find `cmake`

```
vim ~/.bashrc
```

and then, add the below
```
PATH=/usr/local/bin:$PATH:$HOME/bin
```





