---
title: Docker - Urubu
layout: simple_page 
date: 2021-11-25
---


Arquivo Dockerfile, contendo:

```Dockerfile
FROM ubuntu:20.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update \
    && apt-get --yes install --reinstall ca-certificates \
    && apt-get --yes install --no-install-recommends \
        build-essential \ cd
        curl \
        git \
        python \
    && apt-get --purge autoremove \
    && apt-get autoclean \
    && rm -rf /var/lib/apt/lists/*

RUN curl https://bootstrap.pypa.io/pip/2.7/get-pip.py --output get-pip.py
RUN  python2 get-pip.py

RUN useradd -ms /bin/bash developer
USER developer
WORKDIR /home/developer/workdir

RUN pip install urubu
RUN pip install 'Markdown<3.0'
```

e um arquivo .devcontainer\devcontainer.json contendo:
```json
{
    "name": "urubu",
    "context": "..",
    "dockerFile": "../Dockerfile",
    "remoteUser": "developer",
    "runArgs": [
        "--privileged",
        "--net", "host", 
    ],
    "workspaceMount": "source=${localWorkspaceFolder}/workdir,target=/home/developer/workdir,type=bind,consistency=delegated",
    "workspaceFolder": "/home/developer/workdir"
  }
```

no Terminal:
```
make
```

em um segundo terminal, para rodar um servidor:
```
make serve
```

Para atualizar a página é necessário reexecutar o comando **make** no primeiro terminal.
