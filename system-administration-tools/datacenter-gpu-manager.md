---
title: Datacenter GPU Manager (DCGM)
layout: page
---
# Datacenter GPU Manager (DCGM)

## Operation & Use

Main Commands:

* Discover GPU Devices:
```
$ dcgmi discovery -l
4 GPUs found.
+--------+-------------------------------------------------------------------+
| GPU ID | Device Information                                                |
+========+===================================================================+
| 0      |  Name: Tesla P100-SXM2-16GB                                       |
|        |  PCI Bus ID: 0002:01:00.0                                         |
|        |  Device UUID: GPU-06162e6e-80e9-16e9-357e-ac30a929731c            |
+--------+-------------------------------------------------------------------+
| 1      |  Name: Tesla P100-SXM2-16GB                                       |
|        |  PCI Bus ID: 0003:01:00.0                                         |
|        |  Device UUID: GPU-88098753-f09f-2dd2-c9c7-b6f95e8c99e3            |
+--------+-------------------------------------------------------------------+
| 2      |  Name: Tesla P100-SXM2-16GB                                       |
|        |  PCI Bus ID: 0006:01:00.0                                         |
|        |  Device UUID: GPU-4711407e-2948-ea85-3365-11046a8c79ea            |
+--------+-------------------------------------------------------------------+
| 3      |  Name: Tesla P100-SXM2-16GB                                       |
|        |  PCI Bus ID: 0007:01:00.0                                         |
|        |  Device UUID: GPU-a00c0a41-564a-5905-eee3-fde37134a138            |
+--------+-------------------------------------------------------------------+
```

* Define Group:
  * Create Group:
  ```
  $ dcgmi group -c default
  Successfully created group "default" with a group ID of 1
  ```
  * Add GPU Devices To Group:
  ```
  $ dcgmi group -g 1 -a 0,1,2,3
  Add to group operation successful.
  ```

* Run Diagnostics:
  * Level 1
  ```
  $ dcgmi diag --group 1 --run 1
  +---------------------------+------------------------------------------------+
  | Diagnostic                | Result                                         |
  +===========================+================================================+
  |-----  Deployment  --------+------------------------------------------------|
  | Blacklist                 | Pass                                           |
  | NVML Library              | Pass                                           |
  | CUDA Main Library         | Pass                                           |
  | CUDA Toolkit Libraries    | Skip                                           |
  | Permissions and OS Blocks | Pass                                           |
  | Persistence Mode          | Pass                                           |
  | Environment Variables     | Pass                                           |
  | Page Retirement           | Pass                                           |
  | Graphics Processes        | Pass                                           |
  +---------------------------+------------------------------------------------+
  ```
  * Level 2:
  ```
  $ dcgmi diag --group 1 --run 2
  +---------------------------+------------------------------------------------+
  | Diagnostic                | Result                                         |
  +===========================+================================================+
  |-----  Deployment  --------+------------------------------------------------|
  | Blacklist                 | Pass                                           |
  | NVML Library              | Pass                                           |
  | CUDA Main Library         | Pass                                           |
  | CUDA Toolkit Libraries    | Skip                                           |
  | Permissions and OS Blocks | Pass                                           |
  | Persistence Mode          | Pass                                           |
  | Environment Variables     | Pass                                           |
  | Page Retirement           | Pass                                           |
  | Graphics Processes        | Pass                                           |
  +-----  Hardware  ----------+------------------------------------------------+
  | GPU Memory                | Pass - All                                     |
  | Diagnostic                | Skip - All                                     |
  +-----  Integration  -------+------------------------------------------------+
  | PCIe                      | Pass - All                                     |
  +-----  Performance  -------+------------------------------------------------+
  | SM Performance            | Skip - All                                     |
  | Targeted Performance      | Skip - All                                     |
  | Targeted Power            | Skip - All                                     |
  +---------------------------+------------------------------------------------+
  ```

## Installation

* Install Software Package:
```
datacenter-gpu-manager-<version>-<release>.ppc64le.rpm
```
* Create NVIDIA Host Engine Service:
```
$ cat /usr/lib/systemd/system/nv-hostengine.service
# NVIDIA Host Engine Service
.
[Unit]
Description=NVIDIA Host Engine Daemon
Wants=
.
[Service]
Type=forking
ExecStart=/usr/bin/nv-hostengine
ExecStop=/usr/bin/nv-hostengine --term
.
[Install]
WantedBy=multi-user.target
```
* Enable NVIDIA Host Engine Service:
```
$ systemctl enable nv-hostengine
```
* Start NVIDIA Host Engine Service:
```
$ systemctl start nv-hostengine
```
