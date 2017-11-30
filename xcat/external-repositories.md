---
layout: page
title: IBM Power Tools
---
# IBM Power Tools

## Reference

https://www-304.ibm.com/webapp/set2/sas/f/lopdiags/LicenseInformation.html

## Install

* Download Configuration RPM File:
```
$ wget http://public.dhe.ibm.com/software/server/POWER/Linux/yum/download/ibm-power-repo-latest.noarch.rpm
```
* Install Configuration RPM File:
```
$ rpm -ivh ibm-power-repo-latest.noarch.rpm
```
* Configure YUM Repository:
```
/opt/ibm/lop/configure
```
* YUM Repository Configuration File Created:
```
/etc/yum.repos.d/ibm-power-repo.repo
```
* Configuration File Content:
```
[IBM_Power_Tools]
name=IBM Power Tools
baseurl=http://public.dhe.ibm.com/software/server/POWER/Linux/yum/OSS/RHEL/7/ppc64le
enabled=1
gpgcheck=1
[IBM_Power_SDK_Tools]
name=IBM Power SDK Tools
baseurl=http://public.dhe.ibm.com/software/server/POWER/Linux/yum/SDK/RHEL/7/ppc64le
enabled=1
gpgcheck=1
[Advance_Toolchain]
name=Advance Toolchain
baseurl=http://ftp.unicamp.br/pub/linuxpatch/toolchain/at/redhat/RHEL7
enabled=1
gpgcheck=1
```
