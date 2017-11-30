---
layout: page
title: IBM Spectrum LSF Installation
---

# IBM Spectrum LSF Installation

## Installation

The following installation packages are required:

* Installer package
```
lsf10.1_lsfinstall_linux_x86_64.tar.Z
lsf10.1_no_jre_lsfinstall.tar.Z
```
*	Distribution files for ppc64le and x86-64 architectures:
```
lsf10.1_lnx310-lib217-ppc64le.tar.Z
lsf10.1_lnx310-lib217-x86_64.tar.Z
```

The installation is performed through the following steps on the Master node:

*	Uncompress installation package:
```
$ tar -zxvf /share/lsf/lsf-10.1.0.0/lsf10.1_lsfinstall_linux_x86_64.tar.Z
```
*	Edit `install.config` installation configuration file by setting the following parameters:
  * Install directory:
```
LSF_TOP="/share/lsf"
```
  * List of LSF administrators:
    ```
LSF_ADMINS="lsfadmin"
```
  * LSF cluster name:
```
LSF_CLUSTER_NAME="mycluster"
```
  * Entitlement file location:
```
LSF_ENTITLEMENT_FILE="/share/lsf/lsf-10.1.0.0/license/lsf_std_entitlement.dat"
```
  * Installation packages location:
```
LSF_TARDIR="/share/lsf/lsf-10.1.0.0"
```
  * Launch installation:
```
$ ./lsfinstall -f install.config
```

The following installation log is automatically created:
```
/share/lsf/lsf-10.1.0.0/install/lsf10.1.0_lsfinstall/Install.log
```

On each host belonging to the cluster, the LSF daemons must be configure through the following command:
```
$ /share/lsf/10.1/install/hostsetup --top=/share/lsf --boot=y
Setting up LSF server host "compute01-adm" ...
Checking LSF installation for host "compute01-adm" ... Done
Installing LSF RC scripts on host "compute01-adm" ... Done
LSF service ports are defined in /share/lsf/conf/lsf.conf.
Checking LSF service ports definition on host "compute01-adm" ... Done
You are installing IBM Platform LSF -  Standard Edition.
... Setting up LSF server host "compute01-adm" is done
... LSF host setup is done.
```

## Update

The update procedure is the following:

* Open session as root on the Spectrum LSF master host

* Load the Spectrum LSF environment:
```
$ source /share/lsf/conf/profile.lsf
```

* Move to the install directory:
```
cd ${LSF_TOP}/10.1/install/
```

* Copy update file to the install directory

* Close Spectrum LSF cluster hosts:
```
$ badmin hclose all
$ badmin qinact all
```

* Apply update:
```
$ ./patchinstall lsf10.1_lnx310-lib217-ppc64le-427336.tar.Z
```

* Shut Spectrum LSF daemons down:
```
$ badmin hshutdown all
$ lsadmin resshutdown all
$ lsadmin limshutdown all
```

* Start Spectrum LSF daemons up:
```
$ lsadmin limstartup all
$ lsadmin resstartup all
$ badmin hstartup all
```

* Open Spectrum LSF hosts:
```
$ badmin hopen all
$ badmin qact all
```

## Version Check

The installed versions of the product can be identified through the following command:
```
$ /share/lsf/10.1/install/pversions -p LSF

IBM Spectrum LSF 10.1
---------------------
  binary type: linux3.10-glibc2.17-ppc64le, Jun 03 2016, Build 403338
  installed: Dec 13 2016
  patched:  Fix , build 427336, installed Dec 13 2016

IBM Spectrum LSF 10.1
---------------------
  binary type: linux3.10-glibc2.17-x86_64, Jun 03 2016, Build 403338
  installed: Dec 13 2016
  patched:  Fix , build 427336, installed Dec 13 2016
```
