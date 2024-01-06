---
layout: post
title: Distributed Programming 
category: Large Scale
tag: large-scale, distributed programming
---

## Multi-processing w/ Pytorch

- Node: 컴퓨터를 의미
- Global Rank: 모든 node를 통틀어서 프로세스의 우선순위를 의미하지만, ML에서는 GPU의 id(number)를 의미
- Local Rank: 한 node에서의 프로세스의 우선순위를 의미하지만, ML에서는 node에서의 GPU id(number)를 의미
- World Size: 모든 node에서의 프로세스의 개수

<img src='/assets/large_scale/node.png'>

-------------------------------------
### Pytorch에서 multi-process application을 실행하는 방법

#### 1. User code가 main process로 하여 특정 함수를 subprocess로 분기

<img src='/assets/large_scale/multiprocess.png'>

subprocess를 분기하는 방법으로는 

- 'Spawn'
    - main process의 자원을 공유하지 않고, 필요한 만큼의 자원만 subprocess에게 새로 할당
    - 속도가 느리지만 안정적

- 'Fork'
    - main process의 모든 자원을 subprocess와 공유
    - 속도가 빠르지만 불안정

- 'Forkserver'

#### 2. Pytorch launcher가 main-process가 되어 사용자의 코드 전체를 sub-process로 분기

<img src='/assets/large_scale/multiprocess_2.png'>

`torch`에 내장된 launcher가 전체 프로세스를 관장하는 방식으로 다음과 같이 python의 옵션을 활용해서 실행해야 한다. 

`torch`의 예전 버전에서는 `python -m torch.distributed.launch --nproc_per_node=x xxx.py`와 같이 실행해야 하고, 최신 버전에서는 `torchrun --nproc_per_node=x xxx.py`을 사용한다.


### Message Passing

각각의 process들이 서로 메세지를 보낼 때, 주소공간을 공유하지 않고 Message에 Tag를 부여함으로서 sender와 receiver가 정해지도록 하는 방식이다. 

이런 개념으로 여러 process들은 메모리를 공유하지 않고 소통한다. 

#### MPI (Message Passing Interface)

Message Passing에 사용되는 여러 연산 (broadcast, reduce, scatter, gather, ...)이 정의되어 있다.

- `OpenMPI`
- `NCCL` (NVIDIA Collective Communication Library)
    - GPU에 특화된 Message Passing library
    - NVIDIA GPU 사용시 성능이 더 좋음
- `GLOO` (Facebook's Collective Communication Library)
    - torch에서는 주로 CPU 분산처리에 사용하라고 추천

이러한 backend library는 대표적으로 `OpenMPI`라는 오픈소스가 있으나 특별한 경우가 아니라면, GPU를 사용할 때에는 `NCCL`, CPU를 사용할 때에는 `GLOO`를 사용하는 것이 일반적(?)인거 같으며, `torch.distributed`는 `gloo`, `nccl`, `openmpi`를 wrapping하고 있다.

[DISTRIBUTED COMMUNICATION PACKAGE - TORCH.DISTRIBUTED](https://pytorch.org/docs/stable/distributed.html)에서 더 자세한 정보를 참고할 수 있으며, 다음과 같이 정리된다.

<img src='/assets/large_scale/mpi.png'>


#### Process Group

여러 프로세스를 관리하기 위해서는 grouping을 하는 것이 편하기 때문에 `init_process_group`를 통해서 default_pg가 만들어서 관리를 할 수 있도록 지원한다. 

<u>이 때, process group에 대한 정의 및 관리는 subprocess에서 해야한다.</u>




### P2P Communication (Point to Point)

<img src='/assets/large_scale/p2p.png'>

`torch.distributed`의 













#### References:
- [What’s the Diff: Programs, Processes, and Threads](https://www.backblaze.com/blog/whats-the-diff-programs-processes-and-threads/)

- [DISTRIBUTED COMMUNICATION PACKAGE - TORCH.DISTRIBUTED](https://pytorch.org/docs/stable/distributed.html)