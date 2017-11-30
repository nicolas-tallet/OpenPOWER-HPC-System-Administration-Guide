---
layout: page
title: PGI Accelerator
---
# PGI Accelerator

## Usage

### Compiler Options

* Specify Compute Capability of the Target GPU:
```
–ta=tesla:{cc37|cc60}
```
* Specify CUDA Version:
```
–ta=tesla:cuda8.0
```
* Keep CUDA-C Compiler-Generated Files:
```
–ta=tesla:keepgpu,nollvm
```

### Accelerator Information
Command *pgaccelinfo*:
```
[user@host ~]$ pgaccelinfo

CUDA Driver Version:           8000
NVRM version:                  NVIDIA UNIX ppc64le Kernel Module  361.93.02  Wed Sep 21 15:31:40 PDT 2016

Device Number:                 0
Device Name:                   Tesla P100-SXM2-16GB
Device Revision Number:        6.0
Global Memory Size:            17071669248
Number of Multiprocessors:     56
Concurrent Copy and Execution: Yes
Total Constant Memory:         65536
Total Shared Memory per Block: 49152
Registers per Block:           65536
Warp Size:                     32
Maximum Threads per Block:     1024
Maximum Block Dimensions:      1024, 1024, 64
Maximum Grid Dimensions:       2147483647 x 65535 x 65535
Maximum Memory Pitch:          2147483647B
Texture Alignment:             512B
Clock Rate:                    1303 MHz
Execution Timeout:             No
Integrated Device:             No
Can Map Host Memory:           Yes
Compute Mode:                  default
Concurrent Kernels:            Yes
ECC Enabled:                   Yes
Memory Clock Rate:             715 MHz
Memory Bus Width:              4096 bits
L2 Cache Size:                 4194304 bytes
Max Threads Per SMP:           2048
Async Engines:                 3
Unified Addressing:            Yes
Managed Memory:                Yes
PGI Compiler Option:           -ta=tesla:cc60
```

### Current Limitations & Know Issues

* Unified Memory Profiling Not Yet Supported on Pascal - Disable Profiling w/Option:
```
--unified-memory-profiling off
```

## Configuration

### Default Configuration

Configuration File:
```
/opt/pgi/linuxpower/2016/bin/siterc
```

Content:
* Default Compute Capability:
```
set COMPUTECAP=35 60;
```
* Default CUDA Version:
```
set DEFCUDAVERSION=8.0;
```
