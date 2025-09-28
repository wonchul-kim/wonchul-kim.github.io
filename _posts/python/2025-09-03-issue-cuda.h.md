---
layout: post
title: "cuda.h: No such file or directory"
category: Python
tag: [importerror]
---


### Error message:

```sh
      In file included from src/cpp/cuda.cpp:4:

      src/cpp/cuda.hpp:14:10: fatal error: cuda.h: No such file or directory

         14 | #include <cuda.h>

            |          ^~~~~~~~

      compilation terminated.

      error: command '/usr/bin/aarch64-linux-gnu-gcc' failed with exit code 1

      [end of output]

  

  note: This error originates from a subprocess, and is likely not a problem with pip.

  ERROR: Failed building wheel for pycuda

Failed to build pycuda

ERROR: Could not build wheels for pycuda, which is required to install pyproject.toml-based projects```


### Solution: 
```sh
apt install nvidia-cuda-dev
'''