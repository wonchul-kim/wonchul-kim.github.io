---
layout: post
title: Debug Torchrun in VSCode
category: Pytorch
tag: distributed training
---

`vscode`에서 `torchrun`으로 실행한 학습 과정을 debugging하기 위해서는 다음과 같이 `launch.json`을 작성하고 실행해야 한다. 

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File",
            "type": "python",
            "request": "launch",
            // "module": "torch.distributed.launch",
            "program": "/usr/local/lib/python3.9/dist-packages/torch/distributed/run.py",
            "console": "integratedTerminal",
            "justMyCode": true,
            "args": [
                "--nproc_per_node", "2", 
                "dist.py",
                "--world-size", "1",
                "--dist-url", "env://"
            ]
        }
    ]
}
```

* `module`과 `program`을 함께 사용할 수는 없으므로, 둘 중 하나를 사용
    * "module": "torch.distributed.launch"
    * "program": "<path to distributed run.py in torch>"

