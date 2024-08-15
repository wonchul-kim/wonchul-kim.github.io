---
layout: post
title: Monitor GPUs
category: Ubuntu
tag: gpus
---

#### nvidia-smi topo -m 

```r
	   GPU0	GPU1	GPU2	GPU3	CPU Affinity	NUMA Affinity	GPU NUMA ID
GPU0	 X 	SYS	  SYS	  SYS	    0-31	0		             N/A
GPU1	SYS	 X 	  SYS	  SYS	    0-31	0		             N/A
GPU2	SYS	SYS	   X 	  SYS	    0-31	0		             N/A
GPU3	SYS	SYS	  SYS	   X 	    0-31	0		             N/A

Legend:

  X    = Self
  SYS  = Connection traversing PCIe as well as the SMP interconnect between NUMA nodes (e.g., QPI/UPI)
  NODE = Connection traversing PCIe as well as the interconnect between PCIe Host Bridges within a NUMA node
  PHB  = Connection traversing PCIe as well as a PCIe Host Bridge (typically the CPU)
  PXB  = Connection traversing multiple PCIe bridges (without traversing the PCIe Host Bridge)
  PIX  = Connection traversing at most a single PCIe bridge
  NV#  = Connection traversing a bonded set of # NVLinks
```

#### [nvbandwdith](https://github.com/NVIDIA/nvbandwidth)

```cmd
nvbandwidth -t device_to_device_memcpy_read_ce
```


