---
layout: page
title: IBM Spectrum LSF Configuration
---

# IBM Spectrum LSF Configuration

## Configure GPU Resources

### *elim* Binaries

* Rebuild *elim* Binaries
  * Navigate to Directory:
```
$ cd ${LSF_TOP}/10.1/misc/examples/elim.gpu.ext
```
  * Edit Makefile to Specify Paths to Following Libraries:
```
HWLOC_PATH := /products/spack/opt/spack/linux-redhat7-ppc64le/gcc-4.8/hwloc-1.11.3-ezgyqiynseqqa5i7gdooc52cgepg6big
HWDYLIBS := /products/spack/opt/spack/linux-redhat7-ppc64le/gcc-4.8/libtool-2.4.6-ifajfg5sc4m4ebo3g4koekfifm7kawgl/lib/libltdl.a /products/spack/opt/spack/linux-redhat7-ppc64le/gcc-4.8/libpciaccess-0.13.4-b2t45f5vhekrd4pmgpmvyqfviack7f5k/lib/libpciaccess.a -lnuma -lxml2
```
  * Build *elim* Binaries:
```
$ make
gcc -I/usr/local/cuda/include/ -o elim.gpu.ext.o -c elim.gpu.ext.c
gcc -fPIC -Wl,--no-as-needed -ldl -L/usr/lib -lxml2 -o elim.gpu.ext elim.gpu.ext.o
gcc -I/products/spack/opt/spack/linux-redhat7-ppc64le/gcc-4.8/hwloc-1.11.3-ezgyqiynseqqa5i7gdooc52cgepg6big/include -o elim.gpu.topology.o -c elim.gpu.topology.c
gcc -fPIC -rdynamic elim.gpu.topology.o /products/spack/opt/spack/linux-redhat7-ppc64le/gcc-4.8/hwloc-1.11.3-ezgyqiynseqqa5i7gdooc52cgepg6big/lib/libhwloc.a /products/spack/opt/spack/linux-redhat7-ppc64le/gcc-4.8/libtool-2.4.6-ifajfg5sc4m4ebo3g4koekfifm7kawgl/lib/libltdl.a /products/spack/opt/spack/linux-redhat7-ppc64le/gcc-4.8/libpciaccess-0.13.4-b2t45f5vhekrd4pmgpmvyqfviack7f5k/lib/libpciaccess.a -lnuma -lxml2  -o elim.gpu.topology
echo "Done."
Done.
```
  * *elim* Binaries Dynamic Library Links:
```
$ ldd elim.gpu.ext elim.gpu.topology
elim.gpu.ext:
        linux-vdso64.so.1 =>  (0x0000100000000000)
        libdl.so.2 => /lib64/libdl.so.2 (0x0000100000040000)
        libxml2.so.2 => /lib64/libxml2.so.2 (0x0000100000070000)
        libc.so.6 => /lib64/power8/libc.so.6 (0x00001000002b0000)
        /lib64/ld64.so.2 (0x000000005e9c0000)
        libz.so.1 => /lib64/libz.so.1 (0x0000100000490000)
        liblzma.so.5 => /lib64/liblzma.so.5 (0x00001000004d0000)
        libm.so.6 => /lib64/power8/libm.so.6 (0x0000100000530000)
        libpthread.so.0 => /lib64/power8/libpthread.so.0 (0x0000100000620000)
elim.gpu.topology:
        linux-vdso64.so.1 =>  (0x0000100000000000)
        libnuma.so.1 => /lib64/libnuma.so.1 (0x0000100000040000)
        libxml2.so.2 => /lib64/libxml2.so.2 (0x0000100000070000)
        libc.so.6 => /lib64/power8/libc.so.6 (0x00001000002b0000)
        libpthread.so.0 => /lib64/power8/libpthread.so.0 (0x0000100000490000)
        libgcc_s.so.1 => /lib64/libgcc_s.so.1 (0x00001000004d0000)
        /lib64/ld64.so.2 (0x000000002c400000)
        libdl.so.2 => /lib64/libdl.so.2 (0x0000100000510000)
        libz.so.1 => /lib64/libz.so.1 (0x0000100000540000)
        liblzma.so.5 => /lib64/liblzma.so.5 (0x0000100000580000)
        libm.so.6 => /lib64/power8/libm.so.6 (0x00001000005e0000)
```
  * Copy *elim* Binaries into Platorm-Specific Binary Directory

### GPU Resource Declaration

* Add Following Section to Global LSF Configuration File `conf/lsf.shared`:
```
Begin Resource
RESOURCENAME         TYPE      INTERVAL  INCREASING  CONSUMABLE  DESCRIPTION
gpu_ecc0             Numeric   60        N           ()          (ECC errors on 1st GPU)
gpu_ecc1             Numeric   60        N           ()          (ECC errors on 2nd GPU)
gpu_ecc2             Numeric   60        N           ()          (ECC errors on 3rd GPU)
gpu_ecc3             Numeric   60        N           ()          (ECC errors on 3rd GPU)
gpu_driver           String    60        ()          ()          (GPU driver version)
gpu_mode0            String    60        ()          ()          (Mode of 1st GPU)
gpu_mode1            String    60        ()          ()          (Mode of 2nd GPU)
gpu_mode2            String    60        ()          ()          (Mode of 3rd GPU)
gpu_mode3            String    60        ()          ()          (Mode of 3rd GPU)
gpu_model0           String    60        ()          ()          (Model name of 1st GPU)
gpu_model1           String    60        ()          ()          (Model name of 2nd GPU)
gpu_model2           String    60        ()          ()          (Model name of 3rd GPU)
gpu_model3           String    60        ()          ()          (Model name of 3rd GPU)
gpu_shared_avg_ut    Numeric   60        Y           ()          (Average of all shared mode GPUs utilization)
gpu_temp0            Numeric   60        Y           ()          (Temperature of 1st GPU)
gpu_temp1            Numeric   60        Y           ()          (Temperature of 2nd GPU)
gpu_temp2            Numeric   60        Y           ()          (Temperature of 3rd GPU)
gpu_temp3            Numeric   60        Y           ()          (Temperature of 3rd GPU)
gpu_topology         String    60        ()          ()          (GPU topology on host)
gpu_ut0              Numeric   60        Y           ()          (GPU utilization of 1st GPU)
gpu_ut1              Numeric   60        Y           ()          (GPU utilization of 2nd GPU)
gpu_ut2              Numeric   60        Y           ()          (GPU utilization of 3rd GPU)
gpu_ut3              Numeric   60        Y           ()          (GPU utilization of 3rd GPU)
ngpus                Numeric   60        N           N           (Number of GPUs)
ngpus_excl_p         Numeric   60        N           Y           (Number of GPUs in Exclusive Process Mode)
ngpus_excl_t         Numeric   60        N           Y           (Number of GPUs in Exclusive Thread Mode)
ngpus_physical       Numeric   60        N           Y           (Total GPU)
ngpus_prohibited     Numeric   60        N           N           (Number of GPUs in Prohibited Mode)
ngpus_shared         Numeric   60        N           Y           (Number of GPUs in Shared Mode)
End Resource
```

* Add Following Section to LSF Cluster Configuration File `conf/lsf.cluster.cluster1`:
```
Begin ResourceMap
# RESOURCENAME         LOCATION
gpu_ecc0            ([default])
gpu_ecc1            ([default])
gpu_ecc2            ([default])
gpu_ecc3            ([default])
gpu_mode0           ([default])
gpu_mode1           ([default])
gpu_mode2           ([default])
gpu_mode3           ([default])
gpu_model0          ([default])
gpu_model1          ([default])
gpu_model2          ([default])
gpu_model3          ([default])
gpu_shared_avg_ut   ([default])
gpu_temp0           ([default])
gpu_temp1           ([default])
gpu_temp2           ([default])
gpu_temp3           ([default])
gpu_topology        ([default])
gpu_ut0             ([default])
gpu_ut1             ([default])
gpu_ut2             ([default])
gpu_ut3             ([default])
ngpus               ([default])
ngpus_excl_t        ([default])
ngpus_excl_p        ([default])
ngpus_physical      ([default])
ngpus_prohibited    ([default])
ngpus_shared        ([default])
gpu_driver          ([default])
End ResourceMap
```

* Edit LSF Resources configuration file `conf/lsbatch/cluster1/configdir/lsb.resources`:
```
Begin ReservationUsage
RESOURCE        METHOD        RESERVE
ngpus_excl_p    PER_HOST      N
ngpus_excl_t    PER_HOST      N
ngpus_shared    PER_HOST      N
End ReservationUsage
```

* Restart LSF daemons:
```
$ lsadmin reconfig; badmin mbdrestart; badmin hrestart
```

* Check detected GPU resources:
```
$ lsload -I ngpus:ngpus_physical:ngpus_shared:ngpus_excl_p
HOST_NAME       status  ngpus ngpus_shared ngpus_excl_t ngpus_excl_p
host01              ok    2.0         20.0          0.0          0.0
host02              ok    4.0         20.0          0.0          0.0
```

### GPU Resource Requirement

* Turn new GPU requirement syntax in configuration file `conf/lsf.conf`:
```
LSB_GPU_NEW_SYNTAX=Y
```

* Define default requirement in configuration file `conf/lsf.conf`:
```
LSB_GPU_REQ="num=4:mode=exclusive_process:mps=yes:j_exclusive=yes"
```

* Restart LSF daemons:
```
$ lsadmin reconfig; badmin mbdrestart; badmin hrestart
```

## GPFS Resources

* Place the GPFS *elim* binaries into the Master server directory for both architectures (ppc64le and x86-64):
```
$ cp -pv /share/lsf/10.1/util/gpfs/elim.gpfs* /share/lsf/10.1/linux3.10-glibc2.17-ppc64le/etc/
'/share/lsf/10.1/util/gpfs/elim.gpfsglobal' -> '/share/lsf/10.1/linux3.10-glibc2.17-ppc64le/etc/elim.gpfsglobal'
'/share/lsf/10.1/util/gpfs/elim.gpfshost' -> '/share/lsf/10.1/linux3.10-glibc2.17-ppc64le/etc/elim.gpfshost'
$ cp -pv /share/lsf/10.1/util/gpfs/elim.gpfs* /share/lsf/10.1/linux3.10-glibc2.17-x86_64/etc/
'/share/lsf/10.1/util/gpfs/elim.gpfsglobal' -> '/share/lsf/10.1/linux3.10-glibc2.17-x86_64/etc/elim.gpfsglobal'
'/share/lsf/10.1/util/gpfs/elim.gpfshost' -> '/share/lsf/10.1/linux3.10-glibc2.17-x86_64/etc/elim.gpfshost'
```

* Restart LSF services:
```
$ lsfadmin reconfig
$ badmin mbdrestart
```

## Configure Max. Number of Tasks Per Node

* Redefine CPUs Autodetection Performed by LSF
Configuration File:
```
${LSF_ROOTDIR}/lsf/conf/lsf.conf
```
Add Following Option:
```
# Dynamically Define Slots as Number of CPUs
EGO_DEFINE_NCPUS=threads
```

* Reconfigure LSF Daemon
```
$ lsadmin reconfig
Checking configuration files ...
No errors found.
Restart only the master candidate hosts? [y/n] y
Restart LIM on <master01> ...... done
Restart LIM on <master02> ...... done
```

* Check Auto-Detected Maximum CPUs Values
```
$ bhosts
HOST_NAME          STATUS       JL/U    MAX  NJOBS    RUN  SSUSP  USUSP    RSV
host01             ok              -    160      0      0      0      0      0
host02             ok              -    160      0      0      0      0      0
```

* Default limit can be overriden for specific queues inside by setting the following option inside configuration file `${LSF_TOP}/conf/lsbatch/<cluster name>/configdir/lsb.queues`:
```
TASKLIMIT    = 160
```

* Reconfigure LSF Batch Daemon:
```
$ badmin reconfig
Checking configuration files ...
No errors found.
Reconfiguration initiated
```

## Management & Login Nodes

Management nodes and login nodes should be permanently closed to job submission.
This can be achieved by explicitely declaring these hosts in configuration file and specifying 0 available slot (MXJ field):
```
Begin Host
HOST_NAME    MXJ   r1m     pg    ls     tmp    DISPATCH_WINDOW  AFFINITY
default      !     ()      ()    ()     ()     ()               (Y)
# Close Login + Master Nodes by Setting MXJ To 0
frontend01   0     ()      ()    ()     ()     ()               (Y)
frontend02   0     ()      ()    ()     ()     ()               (Y)
master01     0     ()      ()    ()     ()     ()               (Y)
End Host
```

## User Commands Output Format

### `bjobs`

* Set Short Hostlist for Allocated Nodes:
```
LSB_SHORT_HOSTLIST=1
```

* Define list of fields and associated format inside configuration file `${LSF_ROOTDIR}/lsf/conf/lsf.conf`:
```
LSB_BJOBS_FORMAT="jobid:8 job_name:14 user:8 stat:6 start_time:14 run_time:16 slots:6 exec_host"
```

### `lsload`

* Define list of fields and associated format inside configuration file `${LSF_TOP}/lsf/conf/lsf.conf`:
```
LSF_LSLOAD_FORMAT="HOST_NAME:20 status:15 r15m:6 ut:6 mem:10"
```

Note:
> All indices provided by elim can also be specified.

## Processor Affinity with IBM Spectrum LSF and Open MPI

* Check Existence of LSF Support inside Open MPI Library:
```
ompi_info | grep lsf
     MCA ess: lsf (MCA v2.0.0, API v3.0.0, Component v1.10.0)
     MCA plm: lsf (MCA v2.0.0, API v2.0.0, Component v1.10.0)
     MCA ras: lsf (MCA v2.0.0, API v2.0.0, Component v1.10.0)
```

* Create LSF Application Profile for Open MPI:
```
Begin Application
NAME = ompi
DESCRIPTION = Open MPI
DJOB_ENV_SCRIPT = openmpi_rankfile.sh
End Application
```
