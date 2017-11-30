---
layout: page
title: CUDA Drivers
---
# CUDA Drivers

## Troubleshooting
* Check GPU Presence:
```
[root@host: ~]# lspci | grep -i nvidia

0002:01:00.0 3D controller: NVIDIA Corporation Device 15fe (rev a1)
0003:01:00.0 3D controller: NVIDIA Corporation Device 15fe (rev a1)
0006:01:00.0 3D controller: NVIDIA Corporation Device 15fe (rev a1)
0007:01:00.0 3D controller: NVIDIA Corporation Device 15fe (rev a1)
```
* Check Driver Presence:
  * Case #1: Driver NOT Present
```
[ root@host: ~]$ nvidia-smi
modprobe: FATAL: Module nvidia not found.
NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. Make sure that the latest NVIDIA driver is installed and running.
```
or
```
[root@host: ~]# cat /proc/driver/nvidia/version
cat: /proc/driver/nvidia/version: No such file or directory
```
  * Case #2: Driver Present
```
[root@host: ~]# cat /proc/driver/nvidia/version
NVRM version: NVIDIA UNIX ppc64le Kernel Module  352.59  Sat Oct 17 20:31:58 PDT 2015
GCC version:  gcc version 4.8.5 20150623 (Red Hat 4.8.5-4) (GCC)
```
* Check Driver Modules Presence:
  * Case #1: Modules NOT Present
```
[root@host: ~]# modinfo nvidia
modinfo: ERROR: Module nvidia-current not found.
[root@host: ~]# modinfo nvidia-uvm
modinfo: ERROR: Module nvidia-uvm not found.
```
  * Case #2: Modules Present
```
[root@host: ~]# modinfo nvidia
filename:       /lib/modules/3.10.0-327.10.1.el7.ppc64le/extra/nvidia.ko
alias:          char-major-195-*
version:        352.59
supported:      external
license:        NVIDIA
rhelversion:    7.2
alias:          pci:v000010DEd00000E00sv*sd*bc04sc80i00*
alias:          pci:v000010DEd*sv*sd*bc03sc02i00*
alias:          pci:v000010DEd*sv*sd*bc03sc00i00*
depends:        drm,i2c-core
vermagic:       3.10.0-327.10.1.el7.ppc64le SMP mod_unload modversions
parm:           NVreg_Mobile:int
parm:           NVreg_ResmanDebugLevel:int
parm:           NVreg_RmLogonRC:int
parm:           NVreg_ModifyDeviceFiles:int
parm:           NVreg_DeviceFileUID:int
parm:           NVreg_DeviceFileGID:int
parm:           NVreg_DeviceFileMode:int
parm:           NVreg_UpdateMemoryTypes:int
parm:           NVreg_InitializeSystemMemoryAllocations:int
parm:           NVreg_UsePageAttributeTable:int
parm:           NVreg_MapRegistersEarly:int
parm:           NVreg_RegisterForACPIEvents:int
parm:           NVreg_CheckPCIConfigSpace:int
parm:           NVreg_EnablePCIeGen3:int
parm:           NVreg_EnableMSI:int
parm:           NVreg_MemoryPoolSize:int
parm:           NVreg_RegistryDwords:charp
parm:           NVreg_RmMsg:charp
parm:           NVreg_AssignGpus:charp
.
[root@host: ~]# modinfo nvidia-uvm
filename:       /lib/modules/3.10.0-327.10.1.el7.ppc64le/extra/nvidia-uvm.ko
supported:      external
license:        MIT
rhelversion:    7.2
srcversion:     A347F556C35EE8E88DF9DEB
depends:        nvidia
vermagic:       3.10.0-327.10.1.el7.ppc64le SMP mod_unload modversions
parm:           NVuvm_prefetch_stats:int
parm:           NVuvm_prefetch_threshold:int
parm:           NVuvm_prefetch_adaptive:int
parm:           NVuvm_prefetch_epoch:int
parm:           NVuvm_prefetch_sparsity_inc:int
parm:           NVuvm_prefetch_sparsity_dec:int
parm:           NVuvm_prefetch:int
```
If Necessary: Reinstall Driver Modules
```
[root@host: ~]# yum reinstall nvidia-kmod
[...]
[root@host: ~]# yum reinstall nvidia-uvm-kmod
[...]
```

## Operations & user

### Rebuild NVIDIA Driver

* Compile Modules *nvidia* & *nvidia-uvm*:
```
[root@host ~]# cd /var/lib/dkms/nvidia/361.93.02/source
[root@host ~]# make
make "CC=cc" KBUILD_OUTPUT=/lib/modules/3.10.0-327.36.3.el7.ppc64le/build KBUILD_VERBOSE= -C /lib/modules/3.10.0-327.36.3.el7.ppc64le/source M=/var/lib/dkms/nvidia/361.93.02/source ARCH=powerpc NV_KERNEL_SOURCES=/lib/modules/3.10.0-327.36.3.el7.ppc64le/source NV_KERNEL_OUTPUT=/lib/modules/3.10.0-327.36.3.el7.ppc64le/build NV_KERNEL_MODULES="nvidia nvidia-uvm" INSTALL_MOD_DIR=kernel/drivers/video modules
make[1]: Entering directory `/usr/src/kernels/3.10.0-327.36.3.el7.ppc64le'
make -C /lib/modules/3.10.0-327.36.3.el7.ppc64le/build \
KBUILD_SRC=/usr/src/kernels/3.10.0-327.36.3.el7.ppc64le \
KBUILD_EXTMOD="/var/lib/dkms/nvidia/361.93.02/source" -f /usr/src/kernels/3.10.0-327.36.3.el7.ppc64le/Makefile \
modules
 CONFTEST: INIT_WORK
 CONFTEST: remap_pfn_range
[...]
LD [M]  /var/lib/dkms/nvidia/361.93.02/source/nvidia-uvm.o
ld -EL -m elf64lppc -r -o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-interface.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-frontend.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-instance.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-acpi.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-chrdev.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-cray.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-dma.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-drm.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-gvi.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-i2c.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-mempool.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-mmap.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-p2p.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-pat.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-procfs.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-usermap.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-vm.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-vtophys.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/os-interface.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/os-mlock.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/os-pci.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/os-registry.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/os-usermap.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv-modeset-interface.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nv_uvm_interface.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nvlink_linux.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/nvlink_pci.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/ebridge_linux.o /var/lib/dkms/nvidia/361.93.02/source/nvidia/ibmnpu_linux.o
Building modules, stage 2.
MODPOST 2 modules
CC      /var/lib/dkms/nvidia/361.93.02/source/nvidia-uvm.mod.o
LD [M]  /var/lib/dkms/nvidia/361.93.02/source/nvidia-uvm.ko
CC      /var/lib/dkms/nvidia/361.93.02/source/nvidia.mod.o
LD [M]  /var/lib/dkms/nvidia/361.93.02/source/nvidia.ko
make[1]: Leaving directory `/usr/src/kernels/3.10.0-327.36.3.el7.ppc64le'
```
* Check Module Files:
```
[root@host: ~]# modinfo nvidia | awk '/filename/'
filename:       /lib/modules/3.10.0-327.36.3.el7.ppc64le/kernel/drivers/video/nvidia.ko
[root@host: ~]# modinfo nvidia_uvm | awk '/filename/'
filename:       /lib/modules/3.10.0-327.36.3.el7.ppc64le/kernel/drivers/video/nvidia-uvm.ko
```
