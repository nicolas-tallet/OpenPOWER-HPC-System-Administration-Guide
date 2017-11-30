---
layout: page
title: xCAT Installation
---
# xCAT Installation

* Extract Installation Files:
```
$ tar -jxvf xcat-core-2.12.4-linux.tar.bz2
$ tar -jxvf xcat-dep-2.12.4-linux.tar.bz2
```
* Create Repositories:
```
$ cd xcat-core
$ ./mklocalrepo.sh
/root/xcat-core
$ cd ../xcat-dep/rh7/ppc64le
$ ./mklocalrepo.sh
/root/xcat-dep/rh7/ppc64le
```
* Install or Upgrade Package:
```
$ yum clean metadata
$ yum update '*xCAT*'
```
* Source Profile:
```
source /etc/profile.d/xcat.sh
```
* Restart Service:
```
$ systemctl restart xcatd.service
```
* Check Version:
```
$ lsxcatd -a
Version 2.12.4 (git commit 64ec2d41285d9a8770d8d9ef909f251ecbb5100b, built Thu Nov 10 23:59:02 EST 2016)
This is a Management Node
dbengine=SQLite
```

## Configuration Backup

The xCat internal configuration can be saved with the `dumpxCATdb` command.
It will create several CSV files with the database content. Please note that xCAT relies on several other files on the system that should be part of the backup.

```
$ /opt/xcat/sbin/dumpxCATdb -p /install/custom/backup/201609
```
