---
layout: page
title: Mellanox OFED
---
# Mellanox OFED

## Build Driver

- Install Prerequisites
 - mkisofs


- Mount Original ISO File
```
[root@host ~] mount -o loop MLNX_OFED_LINUX-3.4-1.0.0.4-rhel7.3-ppc64le.iso /media
```
- Use Provided Script To Generate New ISO File
```
[root@host ~] /media/mlnx_add_kernel_support.sh --make-iso --mlnx_ofed /media --skip-repo --tmpdir /root
Note: This program will create MLNX_OFED_LINUX ISO for rhel7.3 under /root directory.
Do you want to continue?[y/N]:y
See log file /root/mlnx_ofed_iso.75195.log
.
Checking if all needed packages are installed...
Building MLNX_OFED_LINUX RPMS . Please wait...
Running mkisofs...
Created /root/MLNX_OFED_LINUX-3.4-1.0.0.4-rhel7.3-ppc64le-ext.iso
```

## Install Driver
```
mlnxofed_ib_install -p /install/custom/repositories/mlnx_ofed-rhels-7.3-ppc64le/MLNX_OFED_LINUX-3.4-1.0.0.4-rhel7.3-ppc64le-ext-3.10.0-514.iso -m --force --without-32bit --without-fw-update -end-
```

## Fabric Collective Accelerator (FCA)

- Package Name:
```
fca-2.5.2431-1.34100.ppc64le
```
- *ld.so* Configuration File:
```
/etc/ld.so.conf.d/fca.conf
```
- Add Following Line To *fca.conf*:
```
/opt/mellanox/hcoll/lib
```
