---
layout: page
title: CUDA Toolkit
---
# CUDA Toolkit

## Operations & Use

### *nvidia-smi* Command

Main Options:
```
--display={ COMPUTE | }
--id=<GPU IDs>
--query
--query-gpu
```

Standard Commands:
* List GPUs on Node
```
nvidia-smi --list-gpus
```
* List GPU State and Configuration Information
```
nvidia-smi --query
```
* Show Compute Mode of each GPU
```
nvidia-smi --display=COMPUTE --query
```
* Set GPU \#0 Mode (root Required)
```
nvidia-smi --compute-mode={ DEFAULT | EXCLUSIVE_PROCESS | SHARED_PROCESS } --id=0
```
* Reboot GPU \#0
```
nvidia-smi --gpu-reset --id=0
```

### Clock Settings

* Display Clock Settings:
```
$ nvidia-smi --display=CLOCK --query
.
    Clock Policy
        Auto Boost                  : N/A
        Auto Boost Default          : N/A
```
* Display Supported Clocks:
```
$ nvidia-smi --display=SUPPORTED_CLOCKS --query
```
* Set Application Clocks:
```
$ sudo nvidia-smi --applications-clocks=715,1328
Applications clocks set to "(MEM 715, SM 1328)" for GPU 0002:01:00.0
Applications clocks set to "(MEM 715, SM 1328)" for GPU 0003:01:00.0
Applications clocks set to "(MEM 715, SM 1328)" for GPU 0006:01:00.0
Applications clocks set to "(MEM 715, SM 1328)" for GPU 0007:01:00.0
All done.
```

### GPU Topology

Topology Display
```
$ nvidia-smi topo -m
        GPU0    GPU1    GPU2    GPU3    mlx5_0  mlx5_1  CPU Affinity
GPU0     X      NV2     SOC     SOC     SOC     SOC     0-0,8-8,16-16,24-24,32-32,40-40,48-48,56-56,64-64,72-72
GPU1    NV2      X      SOC     SOC     SOC     SOC     0-0,8-8,16-16,24-24,32-32,40-40,48-48,56-56,64-64,72-72
GPU2    SOC     SOC      X      NV2     SOC     SOC     80-80,88-88,96-96,104-104,112-112,120-120,128-128,136-136,144-144,152-152
GPU3    SOC     SOC     NV2      X      SOC     SOC     80-80,88-88,96-96,104-104,112-112,120-120,128-128,136-136,144-144,152-152
mlx5_0  SOC     SOC     SOC     SOC      X      PIX
mlx5_1  SOC     SOC     SOC     SOC     PIX      X

Legend:

  X   = Self
  SOC  = Connection traversing PCIe as well as the SMP link between CPU sockets(e.g. QPI)
  PHB  = Connection traversing PCIe as well as a PCIe Host Bridge (typically the CPU)
  PXB  = Connection traversing multiple PCIe switches (without traversing the PCIe Host Bridge)
  PIX  = Connection traversing a single PCIe switch
  NV#  = Connection traversing a bonded set of # NVLinks
```

## Installation

### Meta Packages

| Meta Package     | Purpose
|:----------------:|:-------:
| cuda             | Installs all CUDA Toolkit and Driver packages. Handles upgrading to the next version of the cuda package when it's released.
| cuda-8-0         | Installs all CUDA Toolkit and Driver packages. Remains at version 8.0 until an additional version of CUDA is installed.
| cuda-toolkit-8-0 | Installs all CUDA Toolkit packages required to develop CUDA applications. Does not include the driver.
| cuda-runtime-8-0 | Installs all CUDA Toolkit packages required to run CUDA applications, as well as the Driver packages.
| cuda-drivers     | Installs all Driver packages. Handles upgrading to the next version of the Driver packages when they're released.

### Persistence Daemon

Persistence Daemon Control
```
/usr/lib/systemd/system/nvidia-persistenced.service
```

Enable Persistence
```
$ sed -i.bak 's:nvidia-persistenced :nvidia-persistenced --persistence-mode :' /usr/lib/systemd/system/nvidia-persistenced.service
$ systemctl enable nvidia-persistenced
Created symlink from /etc/systemd/system/multi-user.target.wants/nvidia-persistenced.service to /usr/lib/systemd/system/nvidia-persistenced.service.
$ systemctl start nvidia-persistenced
```
