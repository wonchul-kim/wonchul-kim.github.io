---
layout: post
title: Model Analyzer
category: Triton
tag: ['triton', 'model analyzer']
---

## Triton Inference Server SDK

Unlike the **Performance Analyzer**, it does not need **Triton Server**. 

> I got ERROR when the **Triton Server Container** is alive.

#### Run the triton sdk container
```
docker run -it --gpus all -v /var/run/docker.sock:/var/run/docker.sock -v <local mount path>:<container mount path> --net=host nvcr.io/nvidia/tritonserver:23.07-py3-sdk
```

The `$workspace` will look like the below:
<img src='/assets/triton/sdk_workspace.png'>

### Commands

```
model-analyzer profile --model-repository <directory for models> --profile-models <folder name of model> --triton-launch-mode=docker --output-model-repository-path <output directory for models> --export-path <output directory for profile results> --override-output-model-repository 
```

For example, 
```
docker run -it --gpus all -v /var/run/docker.sock:/var/run/docker.sock -v $(pwd)/model_analyzer/examples/quick-start:$(pwd)/model_analyzer/examples/quick-start --net=host nvcr.io/nvidia/tritonserver:23.07-py3-sdk

model-analyzer profile --model-repository /HDD/_projects/etc/triton/model_analyzer/examples/quick-start --profile-models detection --triton-launch-mode=docker --output-model-repository-path /HDD/_projects/etc/triton/model_analyzer/examples/quick-start/output_model --export-path /HDD/_projects/etc/triton/model_analyzer/examples/quick-start/profile_results --run-config-search-max-concurrency 2 --run-config-search-max-model-batch-size 2 --run-config-search-max-instance-count 2 --override-output-model-repository
```
```
docker run -it --gpus all -v /var/run/docker.sock:/var/run/docker.sock -v $(pwd)/model_repository:$(pwd)/model_repository --net=host nvcr.io/nvidia/tritonserver:23.07-py3-sdk


model-analyzer profile --model-repository /HDD/_projects/etc/triton/model_repository --profile-models detection --triton-launch-mode=docker --output-model-repository-path /HDD/_projects/etc/triton/model_repository/output_model --export-path /HDD/_projects/etc/triton/profile_results --run-config-search-max-concurrency 2 --run-config-search-max-model-batch-size 2 --run-config-search-max-instance-count 2 --override-output-model-repository
```

- `output-model-repository-path`: This directory shoud locate in the model-repository directory
- All directory path should be absolute.

## References
- [Model Analyzer Quick Start](https://github.com/triton-inference-server/model_analyzer/blob/main/docs/quick_start.md)
- [Install Model Analyzer](https://github.com/triton-inference-server/model_analyzer/blob/main/docs/install.md#recommended-installation-method)
- https://velog.io/@hbjs97/triton-inference-server-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94