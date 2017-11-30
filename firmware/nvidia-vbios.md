---
layout: page
title: NVIDIA VBIOS
---
# NVIDIA VBIOS

## Install / Update

* Disable Persistence Daemon:
```
[root@host ~]# systemctl disable nvidia-persistenced
```
* Blacklist NVIDIA Modules:
```
[root@host ~]# cat /etc/modprobe.d/blacklist-nvidia.conf
# Unload NVIDIA Driver For GPU Firmware Upgrade
#
blacklist nvidia
blacklist nvidia_uvm
```
* Reboot System
* Wait For Reboot
* Apply Update Using NVIDIA Firmware Update Utility:
```
[root@host ~]# ./auto_confirm_self_extract_h403_0201_890_0__8600260002
.
NVIDIA Firmware Update Utility
Version 5.323.0
.
                              *** IMPORTANT ***
Do not turn off the computer or attempt to reboot your computer while the
NVIDIA firmware is being updated.  If the computer is turned off, or
power is lost, you may be unable to restart your computer.
.
Searching for display adapters to update...
Adapter: Tesla P100-SXM2-16GB Device Path: S:06,B:01,D:00,F:00
.
Firmware Image Version: 86.00.26.00.02
Found display adapter suitable for this update:
Tesla P100-SXM2-16GB Device Path: S:06,B:01,D:00,F:00
                     Current Firmware Version: 86.00.1C.00.01
UPDATE WILL BEGIN IN ABOUT 3 SECONDS.
.
....................................
Update successful.
.
Firmware image has been updated from version 86.00.1C.00.01 to 86.00.26.00.02.
[...]
.
No more matches found.
```
* Remove NVIDIA Modules Blacklisting:
```
[root@host ~]# rm -fv /etc/modprobe.d/blacklist-nvidia.conf
```
* Re-Enable Persistence Daemon:
```
[root@host ~]# systemctl enable nvidia-persistenced
```
* Reboot System

## Generate VBIOS Update Binary

* Retrieve Latest NVFlash Package From NVOnline Web Site
* Retrieve Latest VBIOS Package From NVOnline Web Site
* Extract Both Archives
* Generate VBIOS Update Binary Using *setrom* Utility:
```
[root@host ~]# ./setrom h403_0201_890_0__8600410001.rom nvuflash nvuflash-h403_0201-8600410001

NVIDIA Firmware Update Utility Customize Tool
Version 5.396.0
Update Utility Customization Complete.
```
