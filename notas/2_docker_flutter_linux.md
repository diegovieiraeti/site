---
title: Docker Flutter Linux 
layout: simple_page 
tags: [docker, vscode]
date: 2021-11-29
---


Arquivo Dockerfile, contendo:

```Dockerfile
FROM ubuntu:20.04
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update \
    && apt-get --yes install --reinstall ca-certificates \
    && apt-get --yes install --no-install-recommends \
        xdg-user-dirs xdg-utils \
        clang \
        cmake \
        curl \
        file \
        git \
        libblkid-dev \
        libglu1-mesa \
        libgtk-3-dev \
        liblzma-dev \
        libxinerama1 \
        libxcursor1 \
        libxrandr2 \
        pkg-config \
        ninja-build \
        unzip \
        xz-utils \
    && apt-get --purge autoremove \
    && apt-get autoclean \
    && rm -rf /var/lib/apt/lists/*

RUN useradd -ms /bin/bash developer
USER developer
WORKDIR /home/developer

RUN git clone https://github.com/flutter/flutter.git -b stable /home/developer/flutter/
ENV PATH "$PATH:/home/developer/flutter/bin"

RUN flutter upgrade \
    && flutter config --enable-linux-desktop \
    && flutter precache --linux --no-web --no-android --no-ios \
    && flutter doctor -v
```

e um arquivo .devcontainer\devcontainer.json contendo:

```json
{
    "name": "flutter",
    "context": "..",
    "dockerFile": "../Dockerfile",
    "remoteUser": "developer",
    "runArgs": [
        "--privileged",
        "--net", "host", 
    //linux
    //  "-e", "DISPLAY=${env:DISPLAY}",
    // windows
    //  "-e", "DISPLAY=IP_WINDOWS:0",
        "-e", "LIBGL_ALWAYS_INDIRECT=0",
        "-v", "/tmp/.X11-unix:/tmp/.X11-unix",
      //  "-e", " xhost local:root",
    ],
    "extensions": ["dart-code.flutter"],
    "workspaceMount": "source=${localWorkspaceFolder}/workspace,target=/home/developer/workspace,type=bind,consistency=delegated",
    "workspaceFolder": "/home/developer/workspace"
  }
```

É necessário considerar o sistema operacional nativo, (não) comentando a váriavel **DISPLAY** de acordo.

Caso o sistema operacional hospedeiro seja o Windows:

1. É necessário substituir *IP_WINDOWS* pelo endereço de rede local do Windows.
2. Instalar o aplicativo **Xming X Server for Windows** [https://sourceforge.net/projects/xming/](https://sourceforge.net/projects/xming/)
3. Executar o aplicativo **Xlaunch** com as seguintes configurações selecionadas e permitir a aplicação no Firewall do windows:
    - Display settings: *Multiple windows*, Display number: *-1*.
    - Client startup: *Start no client*.
    - Extra settings: *Clipboard*, *Primary Selection*, *Disable access control*

