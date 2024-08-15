---
layout: post
title: Upgrade Training Performance
category: Deep Learning
tag: [overfitting]
---

#### Mixed Precision


#### torch.compile()


#### Fully Shared Data Parallel


#### Pytorch Profiler 

For example, 
```python
with profile(
    activities=[ProfilerActivity.CPU, ProfilerActivity.CUDA],
    record_shapes=True
) as prof:

    # where to profile
    optimizer.step()

prof.export_chrome_trace('optimizer_trace.json')
```


#### Nsight Compute


#### EMA


