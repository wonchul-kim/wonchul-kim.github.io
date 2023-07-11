---
layout: post
title: Nexus3
category: File repository
tag: nexus3 file repository
---

Using Nexus3, we can store some files, and download and use them by command line. 

For me, I am using neuxs3 to store weights for models.

### Create blob and repository

1. create blob for `file`

2. create respository based on the `file` blob

3. command line to upload file
```
curl -v -u {username}:{password} --upload-file <path including filename> <repository url including nexu3 url>/<directory to save a file (optional)>/<filename to save>
```

Adding `<directory to save a file (optional)>`, we can manage the files in clean.


4. command line to download file
```
curl -u {username}:{password} -O <repository url including nexu3 url>/<directory to download a file (optional)>/<filename to download>
```
