---
title: Amester
layout: page
---
# Amester

## Principles

https://github.com/open-power/amester

## Prerequisites
* CVS

* Check OpenPOWER POWER8 On-Chip Controller is Enabled:
```
$ ipmitool -H hostname-ipmi -I lanplus -U ADMIN -P admin sdr elist | grep OCC
OCC 1 Active     | 08h | ok  | 210.0 | Device Enabled
OCC 2 Active     | 09h | ok  | 210.1 | Device Enabled
```

## Build

- Edit Configuration Files:
```
package/kbs/kbs-amester.tcl
package/kbs/kbs-kbs.tcl
```
 - Set *staticstdcpp* to 0
 - Modify Download URL for *tcllib1.8*
    http://downloads.sourceforge.net/project/tcllib/tcllib/1.18/tcllib-1.18.tar.gz
 - Add ppc64le Target to expect-4.5 Configuration:
 ```
 --build=ppc64le-redhat-linux --host=ppc64le-redhat-linux
 ```
- Allow ppc64le Target Detection for expect-5.41 Configuration:
 - Option #1: Add Target to Configuration File:
 ```
 package/kbs/sources/expect5.45/tclconfig/config.sub
 ```
 - Option #2: Update File with Latest Release:
 ```
 package/kbs/sources/expect5.45/tclconfig/config.guess
 ```
- Create Symbolic Link:
```
ln -sf libstdc++.so.6 listdc++.so
```
