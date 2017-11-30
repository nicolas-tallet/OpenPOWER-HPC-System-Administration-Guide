---
layout: page
title: Power Management
---

# Power Management

## Pre-Requisites

### Inband IPMI
IPMI inband connection allows a system to access directly to its IPMI without going through the network.

example :
```
root@cap:~# ipmitool power status
Chassis Power is on
```
This is possible, because in that case, ipmitool will connect not to an host or IP, but to a internal device /dev/ipmi0

Inband IPMI connection advantages:
* Firmware upgrade (especially on LC models which requires ipmitool > 1.8.15)
* Faster access to ipmi sensors (no network latency)

To enable "inband IPMI" and activate /dev/ipmi0 device, you need to have the following kernel modules :
```
root@cap:~# lsmod | grep ipmi
ipmi_devintf           13691  0
ipmi_powernv            6489  0
ipmi_msghandler        51075  2 ipmi_powernv,ipmi_devintf
```
If some of them are missing, you can load them with modprobe <module> command.

* Ubuntu 16.04
To activate the modules automatically on system boot, add the modules in /etc/modules list.
```
root@cap:~# cat /etc/modules
# /etc/modules: kernel modules to load at boot time.
#
# This file contains the names of kernel modules that should be loaded
# at boot time, one per line. Lines beginning with "#" are ignored.
ibmpowernv
ipmi_devintf
```
* RHEL 7
```
To activate the modules automatically on system boot, add an ipmi-deintf.conf in /etc/modules-load.d  directory with the following inside:
[ibmadmin@magellan ~]$ cat /etc/modules-load.d/ipmi-devintf.conf  
# Load ipmi-devintf at boot
ipmi-devintf
```

## Power Data Collection

### Amester

https://github.com/open-power/amester
https://github.com/open-power/docs/blob/master/occ/OCC_ipmitool_sensors.pdf

### IPMI
* *ipmitool* Command + *sensor* Option:
```
ipmitool> sensor
Fan Power A      | 52.000     | Watts      | ok    | 0.000     | 0.000     | 0.000     | 255.000   | 255.000   | 255.000
Mem Proc0 Pwr    | 39.000     | Watts      | ok    | 0.000     | 0.000     | 0.000     | 255.000   | 255.000   | 255.000
Mem Proc1 Pwr    | 38.000     | Watts      | ok    | 0.000     | 0.000     | 0.000     | 255.000   | 255.000   | 255.000
PCIE Proc0 Pwr   | 67.000     | Watts      | ok    | 0.000     | 0.000     | 0.000     | 255.000   | 255.000   | 255.000
Mem Cache Power  | 226.000    | Watts      | ok    | 0.000     | 0.000     | 0.000     | 255.000   | 255.000   | 255.000
Proc0 Power      | 113.000    | Watts      | ok    | 0.000     | 0.000     | 0.000     | 255.000   | 255.000   | 255.000
CPU VDD Volt     | 1.030      | Volts      | ok    | 0.000     | 0.000     | 0.000     | 2.550     | 2.550     | 2.550
CPU VDD Curr     | 90.000     | Amps       | ok    | 0.000     | 0.000     | 0.000     | 255.000   | 255.000   | 255.000
Proc1 Power      | 97.000     | Watts      | ok    | 0.000     | 0.000     | 0.000     | 255.000   | 255.000   | 255.000
PCIE Proc1 Power | 65.000     | Watts      | ok    | 0.000     | 0.000     | 0.000     | 255.000   | 255.000   | 255.000
GPU Sense        | 231.000    | Watts      | ok    | 0.000     | 0.000     | 0.000     | 255.000   | 255.000   | 255.000
```
* *ipmitool* Command + *dcmi* Option:
```
ipmitool> dcmi power reading

    Instantaneous power reading:                   883 Watts
    Minimum during sampling period:                930 Watts
    Maximum during sampling period:                998 Watts
    Average power reading over sample period:      952 Watts
    IPMI timestamp:                           Thu May 11 14:37:28 2017
    Sampling period:                          00010000 Milliseconds
    Power reading state is:                   activated
```

> Open Question: How to modify the 'Sampling period'?
