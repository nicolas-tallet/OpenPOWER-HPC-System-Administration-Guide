---
layout: page
title: IBM Parallel Environment (PE) Runtime Edition (RTE)
---
# IBM Parallel Environment (PE) Runtime Edition (RTE)

## Environment Variables

License:
```
export IBM_PPE_RTE_LICENSE_ACCEPT=yes
```

Upgrade:
```
export MP_PE_UPGRADE={ no | yes}
```
* No: Keep current version as default version
* Yes: Set new version as default version

## Fortran MPI Modules

Location:
```
GNU:     /opt/ibmhpc/pe2303/mpich/gnu/include64/mpi.mod
IBM XL:  /opt/ibmhpc/pe2303/mpich/xlf/include64/mpi.mod
```

Generation: Fortran MPI Modules for GCC
Fortran MPI Module for GCC automatically generated as part of the installation procedure - requires MP_CONFIG environment variable to be properly set.
Existing script allows manual operation, though:
```
[root@host ~]# export MP_CONFIG=2303
[root@host ~]# /opt/ibmhpc/pe2303/mpich/sbin/f90_gen_mpimod
Generating 64bit mpi_constants.mod.
Generating 64bit mpi_sizeofs.mod.
Generating 64bit mpi_base.mod.
Generating 64bit mpi.mod.
Completed creating MPICH MOD files for gfortran compiler
```

Generation: Fortran MPI Modules for IBM XL Fortran
Fortran MPI Module for XL is NOT automatically generated and compilation must be performed manually as part of the post-install tasks.
```
[root@host ~]# export MP_COMPILER=/opt/ibm/xlf/15.1.4/bin/xlf_r
[root@host ~]# export MP_CONFIG=2303
[root@host ~]# /opt/ibmhpc/pe2303/mpich/sbin/xlf_gen_mpimod
Using IBM XLF compiler version 15.1.4
Generating 64bit mpi_constants.mod.
Generating 64bit mpi_sizeofs.mod.
Generating 64bit mpi_base.mod.
Generating 64bit mpi.mod.
Completed creating MPICH MOD files for IBM XLF compiler
** mpi_constants   === End of Compilation 1 ===
1501-510  Compilation successful for file mpi_constants.f90.
** mpi_sizeofs   === End of Compilation 1 ===
1501-510  Compilation successful for file mpi_sizeofs.f90.
** mpi_base   === End of Compilation 1 ===
1501-510  Compilation successful for file mpi_base.f90.
** mpi   === End of Compilation 1 ===
1501-510  Compilation successful for file mpi.f90.
```

## /etc/ppe.cfg

Location:
```
/etc/ppe.cfg
```

Content:
```
DEFAULT_PE_BASE: /opt/ibmhpc/pe2303
PE_LATEST_LEVEL: /opt/ibmhpc/pe2303
PE_RELEASE_2300: /opt/ibmhpc/pe2300
PE_RELEASE_2302: /opt/ibmhpc/pe2302
PE_RELEASE_2303: /opt/ibmhpc/pe2303
PE_SECURITY_METHOD: SSH poesec_ossh /opt/ibmhpc/pecurrent/base/gnu/lib64/poesec_ossh.so m[t=-1,v=1.0.1e,l=/usr/lib64/libcrypto.so.1.0.1e]
```

Keywords Explanation
* DEFAULT_PE_BASE
The default path is /opt/ibmhpc/pexxxx.
* PE_LATEST_LEVEL
Defines the latest IBM PE Runtime Edition level installed on the system. This value is set during installation and should not be changed by the system administrator. It is used to determine the latest level of IBM PE Runtime Edition software installed, and for starting and stopping key IBM PE daemons and other subsystems.
* PE_RELEASE_xxxx
Defines subsequent release paths, when multiple IBM PE Runtime Edition releases have been installed. When each new release or PTF (program temporary fix) update is installed, an accompanying PE_RELEASE_xxxx stanza is appended to /etc/ppe.cfg. The numeric portion of the stanza forms a possible value, which can be specified as the value of the MP_CONFIG environment variable (or the -config command-line flag) option.

## Protocol Network Services Daemon (PNSD)

Service
```
/etc/init.d/pnsd
```

Installation Directory
```
/opt/ibmhpc/pe2303/ppe.pami/pnsd/
```

## Release Level
After a new IBM PE Runtime Edition level has been installed, the currently installed IBM PE level remains as the default level (unless MP_PE_UPGRADE=yes was set during installation).
To change the default production release level, perform the following steps.
1. On each node, update the /etc/ppe.cfg file to change the DEFAULT_PE_BASE stanza value. For example, if the default level is to be IBM PE Runtime Edition 2.3.0.1 (for PTF1), change the value to /opt/ibmhpc/pe2301.
2. After changing the DEFAULT_PE_BASE stanza in /etc/ppe.cfg, define that level’s installation path as the pecurrent intermediate directory by using the pelinks script by specifying the new level:
```
pelinks 2303
MP_CONFIG=2303 pelinks
```

## IBM Platform LSF Integration

Requires following symbolic link:
```
/usr/lib64/libpermapi.so -> <lsf_prefix>/lsf/9.1/linux3.10-glibc2.17-ppc64le/lib/permapi.so
```

## Usage
