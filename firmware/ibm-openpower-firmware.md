---
layout: page
title: OpenPOWER Firmware
---

# OpenPOWER Firmware

## Firmware Update

### xCAT 'rflash' Command
```
[root@xcat]# rflash <host> <firmware>.hpm
<host>: rflash started, please wait.......
<host>: rflash completed.
```

### IPMI Toolkit
- Power Off the System:
```
[root@xcat ~]# ipmitool -H <BMC IP> -I lanplus -U ADMIN -P admin chassis power off
```
- Reset BMC (Establish Stable Starting Point)
```
[root@xcat ~]# ipmitool -H <BMC IP> -I lanplus -U ADMIN -P admin mc reset cold
```
- Wait 2-3 Min. for BMC To Be Ready (C0h: Boot In Progress | 00h: Boot Complete):
```
[root@xcat ~]# ipmitool -H <BMC IP> -I lanplus -U ADMIN -P admin raw 0x3a 0x0a
```
- Preserve IPMI and Network Settings:
```
[root@xcat ~]# ipmitool -H <BMC IP> -I lanplus -U ADMIN -P admin raw 0x32 0xba 0x18 0x00
```
- Flash BMC and pnor Firmware:
```
[root@xcat ~]# ipmitool -H <BMC IP> -U ADMIN -I lanplus -P admin -z 15000 hpm upgrade <firmware.hpm> force
Setting large buffer to 15000
.
PICMG HPM.1 Upgrade Agent 1.0.9:
.
Validating firmware image integrity...OK
Performing preparation stage...
Services may be affected during upgrade. Do you wish to continue? (y/n): y
OK
.
Performing upgrade stage:
.
-------------------------------------------------------------------------------
|ID  | Name        |                     Versions                        | %  |
|    |             |      Active     |      Backup     |      File       |    |
|----|-------------|-----------------|-----------------|-----------------|----|
|*  2|BIOS         |   1.11 00000001 | ---.-- -------- |   1.11 19000000 |100%|
|    |Upload Time: 00:55             | Image Size: 33554584 bytes             |
|*  0|BOOT         |   2.13 35000000 | ---.-- -------- |   2.13 3A000000 |100%|
|    |Upload Time: 00:00             | Image Size:  262296 bytes              |
|*  1|APP          |   2.13 35000000 | ---.-- -------- |   2.13 3A000000 |100%|
|    |Upload Time: 01:06             | Image Size: 33292440 bytes             |
-------------------------------------------------------------------------------
(*) Component requires Payload Cold Reset
.
Firmware upgrade procedure successful
.
```
- Wait for BMC to come back online

## Firmware Version Identification

### xCAT 'rflash' Command
```
[root@xcat]# rflash <host> -c
<host>: Node firmware version for BOOT component:   2.13 35000000
<host>: Node firmware version for APP component:   2.13 35000000
<host>: Node firmware version for BIOS component:   1.11 00000001
```

### IPMI Tool
```
[root@xcat]# ipmitool -H <host bmc> -I lan -U ADMIN -P admin fru print 47
Product Name          : OpenPOWER Firmware
Product Version       : IBM-garrison-ibm-OP8_v1.11_2.19
Product Extra         :        op-build-6ce5903
Product Extra         :        buildroot-81b8d98
Product Extra         :        skiboot-5.3.7
Product Extra         :        hostboot-1f6784d-02b09df
Product Extra         :        linux-4.4.24-openpower1-5d537af
Product Extra         :        petitboot-v1.2.6-8fa93f2
Product Extra         :        garrison-xml-3db7b6e
Product Extra         :        occ-69fb587
Product Extra         :        hostboot-bina
```

### Virtual Filesystem: /proc/ractrends
```
[root@xcat ~]# ssh -x ADMIN@<host bmc> "cat /proc/ractrends/Helper/FwInfo"
ADMIN@<host>-ipmi's password:
FW_VERSION=2.13.00053
FW_DATE=Sep 2 2016
FW_BUILDTIME=11:14:57 CDT
FW_DESC=8335-GTB de281ec gar.820.160902a
FW_PRODUCTID=1
FW_RELEASEID=RR9
FW_CODEBASEVERSION=2.X
```
