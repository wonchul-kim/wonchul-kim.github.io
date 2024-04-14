---
layout: post
title: Use Poetry for docker
category: Python
tag: [poetry]
---

```sh
From python:3.9

WORKDIR /workspace
RUN pip install poetry 

COPY pyproject.toml poetry.lock ./
# with --no-root, "prevent does not contain any element" error
RUN poetry install --no-root
```