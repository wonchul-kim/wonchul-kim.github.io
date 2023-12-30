---
layout: post
title: Load Jupyter Server by Docker
category: Jupyter
tag: jupyter
---

Considering you already have docker image such as my docker image, [python3.9.18-jupyter](https://hub.docker.com/repository/docker/onedang2/python3.9.18-jupyter/general) ...

### 0. Run docker image and attach it by bash
```cmd
docker run --name jupyter -it --p -d 8888:8888 -v /mnt/HDD:/HDD <docker image name>
docker exec -it jupyter bash
```

### 1. Install jupyter
```cmd
pip install jupyter
```

### 2. Generate jupyter server password
```cmd
jupyter server password
Enter password:  ****
Verify password: ****
```

Then, jupyter_server_config.json is generated in `~/.jupyter/jupyter_server_config.json`

Copy it.

### 3. Set jupyter notebook config with the copied password
```cmd
vim ~/.jupyter/jupyter_notebook_config.py
```

Then, copy and paste the below:
```python
c = get_config()

c.NotebookApp.ip='localhost'
c.NotebookApp.open_browser=False
c.NotebookApp.password='argon2:$argon2......' # paste the generated password
c.NotebookApp.password_required=True
c.NotebookApp.port=8888
c.NotebookApp.iopub_data_rate_limit=1.0e10  
c.NotebookApp.terminado_settings={'shell_command': ['/bin/bash']}
```

### 4. Run jupyter notebook server
```cmd
jupyter notebook --ip 0.0.0.0 --allow-root
```

### 5. Access by `localhost:8888`

### 6. etc...

You can add alias to execute the jupyter server easily.
```cmd
vim ~/.barch
```

Add the below:
```
alias jupyter='cd /HDD && jupyter notebook --ip 0.0.0.0 --allow-root'
```