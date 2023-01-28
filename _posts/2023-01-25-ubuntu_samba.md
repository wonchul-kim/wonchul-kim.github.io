---
layout: post
title: Samba Settings
category: ubuntu
tag: samba
---

### Install and Set Samba on Ubuntu

1. Install 
```
sudo apt-get install samba
```

2. Make group (Optional)
```
sudo groupadd <group name>
```

3. Add user (Optional)
```
sudo adduser <user name>
```

4. Add user into group (Optional)
```
sudo gpasswd -a <user name> <group name>
```

5. Generate `samba` account
The `user` should alreadly exists in the system user list
```
sudo smbpasswd -a <user name>
```

6. Edit samba configuration to add directory to be shared and users to use it
```
sudo vim /etc/samba/smb.conf
```
Add the below contents:
```
[storage]
   comment = For Deep Learning # description for the directory to be shared
   path = /mnt/storage # path to folder to be shared
   browsable = yes # yes or no
   read only = no # yes or no
   writable = # yes or no
   geust ok = # yes or no
   create mask = 0664 # authority to generate
   directory mask = 0775 # authority for directory
   public = no # yes or no
   force group = <group name> 
   valid users = aivdl, <user name1>, <user name2>  # user to use this shared folder
```

If you want to search this shared folder in Window with hostname, 
```
sudo vim /etc/hostname
```
Change the hostname and Open the file explore in window, and type `\\<hostname>` or `<ip>` to access this shared folder.


### Samba commends

* samba status
```
sudo smbstatus
```

* samba accounts
```
sudo pdbedit -L -v
```

* delete samba account
```
sudo smbpasswd -x <user name>
```
* monitor shared folder
```
watch -n 1 'sudo smbstatus; df -h; du -h'
```






