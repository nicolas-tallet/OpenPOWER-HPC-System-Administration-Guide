---
layout: page
title: Mellanox ConnectX4 Firmware
---

# Mellanox ConnectX4 Firmware

## Links
* Mellanox Driver & Firmware: http://www.mellanox.com/page/firmware_table_IBM_SystemP

## Packages
- IBM P/N 00WT010: 1-Port CX4 EDR Adapter
- IBM P/N 00WT039: 2-Port CX4 EDR Adapter

## Prerequisites
- Packages: Mellanox MFT Tools / Mellanox OFED (Includes MFT tools)
- Start MST Service (Prerequisite for Mellanox Firmware Management):
```
[root@host ~]# mst start
Starting MST (Mellanox Software Tools) driver set
Loading MST PCI module - Success
Loading MST PCI configuration module - Success
Create devices
Unloading MST PCI module (unused) - Success
```

## Command
```
flint --device <MST Device> --image <Image File> { burn | query
```

## Firmware Upgrade
- Identify Existing MST Devices (/dev/mst/<id>):

```
[root@host ~]# mst status -v
MST modules:
------------
    MST PCI module is not loaded
    MST PCI configuration module loaded
PCI devices:
------------
DEVICE_TYPE             MST                           PCI             RDMA            NET                       NUMA
ConnectX4(rev:0)        /dev/mst/mt4115_pciconf0.1    0004:01:00.1    mlx5_1          net-ib1                   1
.
ConnectX4(rev:0)        /dev/mst/mt4115_pciconf0      0004:01:00.0    mlx5_0          net-ib0                   1
.
```
- Launch Firmware Upgrade:
```
[root@host ~]# flint --device /dev/mst/mt4115_pciconf0 --image fw-ConnectX4-rel-12_17_2010-00WT039_Ax.bin burn
    Current FW version on flash:  12.16.1006
    New FW version:               12.17.2010
-E- PSID mismatch. The PSID on flash (MT_2150110033) differs from the PSID in the given image (IBM2150110033).
Note if you get PSID mismatch error, please contact IODev
    Current FW version on flash:  12.16.1006
    New FW version:               12.17.2010
.
Do you want to continue ? (y/n) [n] : y
Burning FS3 FW image without signatures - OK
Restoring signature                     - OK
-I- To load new FW run mlxfwreset or reboot machine.
```
- Manually Reboot System
Note: *mlxfwreset* does not currently work on Power systems

## Firmware Query
```
[root@host: ~]# flint --device /dev/mst/mt4115_pciconf0 query
Image type:          FS3
FW Version:          12.17.2010
FW Release Date:     2.11.2016
Product Version:     rel-12_17_2010
Description:         UID                GuidsNumber
Base GUID:           7cfe900300814cf0        4
Base MAC:            00007cfe90814cf0        4
Image VSD:
Device VSD:
PSID:                IBM2190110032
```
