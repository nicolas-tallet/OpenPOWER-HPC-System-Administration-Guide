---
title: Power LC Data Collection Script
layout: page
---
# Power LC Data Collection Script

## Links



## Command Usage
```
Usage: plc.pl [-OPTIONS [-MORE_OPTIONS]] [--] [PROGRAM_ARG1 ...]
.
The following single-character options are accepted:
        With arguments: -a -b -s -h -u -p
        Boolean (without arguments): -v -i -f

Options may be merged together.  -- stops processing of options.
Space is not required between options and their arguments.
  [Now continuing due to backward compatibility and excessive paranoia.
   See 'perldoc Getopt::Std' about $Getopt::Std::STANDARD_HELP_VERSION.]
ERROR: no BMC host specified with -b flag
POWER LC BMC Data Collection version 3.0
.
usage: /pwrlocal/ibm/diags/plc.pl { -b bmc_address | -i } [-a admin_pw] [-s sysadmin_pw] [-h host -u user -p password]
Flags:
   -b BMC hostname or IP address
   -a BMC ADMIN password if changed from default (admin)
   -s BMC sysadmin password if changed from default (superuser)
   -f Collect PNOR Firmware Image
   -h Linux host address
   -u Linux host user ID
   -p Linux host password
   -i Interactive mode
```

## Example
```
[root@xcat ~]# plc.pl -b <host>
Power LC Data Collection Script Version 3.0

Output File: <host>-2016-11-28.1727.powerlc.tar.gz

Getting BMC data
..........................
Getting IPMI Data
........

Getting IPMI Data
........
<host>-2016-11-28.1727.bmc.txt
<host>-2016-11-28.1727.files.txt
<host>-2016-11-28.1727.led.txt
<host>-2016-11-28.1727.status.txt

Complete
```
