---
layout: page
title: IPMI
---
# IPMI

## Inband IPMI

### Principle

Inband IPMI allows a system to access its own IPMI subsystem directly without going through the network.
With Inband IPMI, the `ipmitool` command connects to the `/dev/ipmi0` internal device instead of the host network interface.

Inband IPMI connection advantages:
* Faster access to IPMI sensors (no network latency)
* Firmware upgrade (especially on LC models which require ipmitool > 1.8.15)

### Implementation

* Install required software packages:
```
$ yum install OpenIPMI OpenIPMI-tools
```

* Manually load modules:
```
$ modprobe ipmi_msghandler
$ modprobe ipmi_watchdog
$ modprobe ipmi_poweroff
$ modprobe ipmi_devintf
$ modprobe ipmi_powernv
```

* Check that following kernel modules are properly loaded:
```
$ lsmod | grep ipmi
ipmi_devintf           13691  0
ipmi_powernv            6489  0
ipmi_msghandler        51075  2 ipmi_powernv,ipmi_devintf
```

* Automate loading of these modules during system boot:

  * RHEL 7
Specify module list:
```
# Load ipmi_devintf Module
ipmi_devintf
```
inside the following configuration file: `/etc/modules-load.d/ipmi-devintf.conf`

  * Ubuntu 16.04
Specify module list:
```
# /etc/modules: kernel modules to load at boot time.
#
# This file contains the names of kernel modules that should be loaded
# at boot time, one per line. Lines beginning with "#" are ignored.
ibmpowernv
ipmi_devintf
```
inside the following configuration file: `/etc/modules`

* Check that the Inbound IPMI device is available:
```
$ ls -l /dev/ipmi0
crw-------. 1 root root 244, 0 Jan 27 15:06 /dev/ipmi0
```

* Test that Inbound IPMI works properly:
```
$ ipmitool sdr type list
Sensor Types:
 Temperature (0x01) Voltage (0x02)
 Current (0x03) Fan (0x04)
```

## Outbound IPMI

```
$ ipmitool -H <IPMI Interface Hostname> -I lanplus -P admin -U ADMIN <IPMI Tool Command>`
```

## Miscellaneous IPMI Commands

* *sensor*:
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
