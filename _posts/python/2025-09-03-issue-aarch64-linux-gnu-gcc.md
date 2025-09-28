---
layout: post
title: 'aarch64-linux-gnu-gcc' failed: No such file or directory
category: Python
tag: [importerror]
---


### Error message:

```sh
      aarch64-linux-gnu-gcc -Wno-unused-result -Wsign-compare -fwrapv -Wall -O3 -DNDEBUG -fPIC -DBOOST_ALL_NO_LIB=1 -DBOOST_THREAD_BUILD_DLL=1 -DBOOST_MULTI_INDEX_DISABLE_SERIALIZATION=1 -DBOOST_PYTHON_SOURCE=1 -Dboost=pycudaboost -DBOOST_THREAD_DONT_USE_CHRONO=1 -DPYGPU_PACKAGE=pycuda -DPYGPU_PYCUDA=1 -DHAVE_CURAND=1 -Isrc/cpp -Ibpl-subset/bpl_subset -I/usr/lib/python3/dist-packages/numpy/core/include -I/usr/include/python3.10 -c bpl-subset/bpl_subset/libs/python/src/converter/arg_to_python_base.cpp -o build/temp.linux-aarch64-3.10/bpl-subset/bpl_subset/libs/python/src/converter/arg_to_python_base.o

      error: command 'aarch64-linux-gnu-gcc' failed: No such file or directory

      [end of output]

  

  note: This error originates from a subprocess, and is likely not a problem with pip.

  ERROR: Failed building wheel for pycuda

Failed to build pycuda

ERROR: Could not build wheels for pycuda, which is required to install pyproject.toml-based projects
```


### Solution: 
```sh
apt-get install -y build-essential python3-dev
'''