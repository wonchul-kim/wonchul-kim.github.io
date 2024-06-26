---
layout: post
title: sourcelist
category: Ubuntu
tag: sleep
---


When doing s.t. with `apt-add-repository` command and `apt-get update`, there could be some issue b/c the source is corrupted or something as the below:

```cmd
Err:13 https://packages.cloud.google.com/apt kubernetes-xenial Release                                                                   
  404  Not Found [IP: 142.250.206.206 443]
Get:14 http://us.archive.ubuntu.com/ubuntu focal-updates InRelease [128 kB]                          
Get:15 https://repo.download.nvidia.com/baseos/ubuntu/focal/x86_64 focal-updates InRelease [18.4 kB]
Err:12 http://deb.anydesk.com all InRelease                                                             
  The following signatures were invalid: EXPKEYSIG 18DF3741CDFFDE29 philandro Software GmbH <info@philandro.com>
```

Then, you should delete/clean that repository from `sourcelist`.

### Remove at `/etc/apt/sourcelist.d`

Find the releated list and then, delete it


### By the command
In the above, to solve `Err:13 https://packages.cloud.google.com/apt kubernetes-xenial`

```cmd
sudo apt-add-repository --remove "deb http://apt.kubernetes.io/ kubernetes-xenial main"
```
