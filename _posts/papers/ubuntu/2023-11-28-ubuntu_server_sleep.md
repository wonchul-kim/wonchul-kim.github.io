---
layout: post
title: Disable sleep mode
category: Ubuntu
tag: sleep
---

## For Ubuntu Server,

Need to add the following paraphrase to prevent the server from sleeping.

```
echo "setterm -blank 0" >> ~/.bashrc
source ~/.bashrc
```

