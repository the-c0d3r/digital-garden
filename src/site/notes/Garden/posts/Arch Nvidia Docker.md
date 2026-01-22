---
{"dg-publish":true,"permalink":"/garden/posts/arch-nvidia-docker/","created":"2024-05-11 14:54","updated":"2026-01-22 13:43"}
---


# Arch Nvidia Docker

topics: [[02-Area/programming/Linux/Arch Linux\|Arch Linux]]
tags: #commands #linux 

Refer to [[02-Area/programming/Linux/Arch Manual Package Installation\|Arch Manual Package Installation]].

I was trying to run some LLM docker stuff on my Arch Linux and I encountered the following error message.
```
docker: Error response from daemon: could not select device driver "" with capabilities: [[gpu]].
```

I looked around and I noticed that the packages `nvidia-container-toolkit` and `libnvidia-container` are required to make this work, which seems to provide the GPU functionality to the docker containers for Nvidia cards. 

I tried to install them using the `pacman` and `yay`, but both says package not found. When I google it, I found it on the following links, which is in the extra repo of Arch.

- [ArchLinux extra nvidia-container-toolkit](https://archlinux.org/packages/extra/x86_64/nvidia-container-toolkit/)
- [ArchLinux extra libnvidia-container](https://archlinux.org/packages/extra/x86_64/libnvidia-container/)

I went through like changing the pacman config, pacman mirrors and none of them seem to help me find the packages I need. 

Finally I found a post on [reddit](https://www.reddit.com/r/archlinux/comments/18xtglk/nvidia_container_toolkit/) and that leads me to try downloading the packages, building it and installing myself. 

```bash
git clone https://gitlab.archlinux.org/archlinux/packaging/packages/libnvidia-container
cd libnvidia-container
makepkg -si
```

```bash
git clone https://gitlab.archlinux.org/archlinux/packaging/packages/nvidia-container-toolkit
cd nvidia-container-toolkit
makepkg -si
```

When installing the `nvidia-container-toolkit`, it might fail with the following. 

```bash
time="2024-05-11T14:30:59+08:00" level=info msg="Installed '/tmp/output-2742741441/input.real'"
time="2024-05-11T14:30:59+08:00" level=info msg="Installed wrapper '/tmp/output-2742741441/input'"
--- PASS: TestInstallExecutable (0.00s)
=== RUN   TestNvidiaContainerRuntimeInstallerWrapper
--- PASS: TestNvidiaContainerRuntimeInstallerWrapper (0.00s)
PASS
ok      github.com/NVIDIA/nvidia-container-toolkit/tools/container/toolkit      (cached)
FAIL
==> ERROR: A failure occurred in check().
    Aborting...
```


I just `vi PKGBUILD` and removed the `check()` function. Then it was able to install, and docker was able to use GPU. The following is how I test if the GPU is detected in the docker container. 

Remember to restart the docker service after installing the above packages. 

```bash
docker run --gpus all nvidia/cuda:12.1.1-runtime-ubuntu22.04 nvidia-smi

==========
== CUDA ==
==========

CUDA Version 12.1.1

Container image Copyright (c) 2016-2023, NVIDIA CORPORATION & AFFILIATES. All rights reserved.

This container image and its contents are governed by the NVIDIA Deep Learning Container License.
By pulling and using the container, you accept the terms and conditions of this license:
https://developer.nvidia.com/ngc/nvidia-deep-learning-container-license

A copy of this license is made available in this container at /NGC-DL-CONTAINER-LICENSE for your convenience.

Sat May 11 06:45:03 2024
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 550.67                 Driver Version: 550.67         CUDA Version: 12.4     |
|-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce RTX 4080 ...    Off |   00000000:01:00.0  On |                  N/A |
|  0%   44C    P8             21W /  320W |     622MiB /  16376MiB |     19%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI        PID   Type   Process name                              GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
+-----------------------------------------------------------------------------------------+
```

---

For Ubuntu

1. Configure the repository:

```bash
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey |sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
&& curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list \
&& sudo apt-get update
```

2. Install the NVIDIA Container Toolkit packages:

```bash
sudo apt-get install -y nvidia-container-toolkit
```

3. Configure the container runtime by using the nvidia-ctk command:

```bash
sudo nvidia-ctk runtime configure --runtime=docker
```

4. Restart the Docker daemon:

```bash
sudo systemctl restart docker
```

5. Restart the system as well