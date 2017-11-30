---
layout: page
title: Open MPI
---
# Open MPI

## Usage

### *mpirun* Submission Command

* Without Processor Affinity Management
```
mpirun --hostfile ${LSB_DJOB_HOSTFILE} -n ${LSB_DJOB_NUMPROC}
```

* With Processor Affinity Management
```
mpirun --rankfile ${LSB_RANK_HOSTFILE}
```
Requires submission through 'ompi' application:
```
bsub -app ompi < job.sh
```

## Configuration

### Integration w/IBM Spectrum LSF

Enable Spectrum LSF Support if not Built-In:
```
export OMPI_MCA_mpi_warn_on_fork=0
export OMPI_MCA_plm_rsh_agent="blaunch"
```

### MPI Wrappers Configuration Files

Location:
```
<Open MPI Prefix>/share/openmpi/mpicc-wrapper-data.txt
<Open MPI Prefix>/share/openmpi/mpic++-wrapper-data.txt
<Open MPI Prefix>/share/openmpi/mpiCC-wrapper-data.txt
<Open MPI Prefix>/share/openmpi/mpicxx-wrapper-data.txt
<Open MPI Prefix>/share/openmpi/mpifort-wrapper-data.txt
<Open MPI Prefix>/share/openmpi/mpif77-wrapper-data.txt
<Open MPI Prefix>/share/openmpi/mpif90-wrapper-data.txt
```

Content
```
project=Open MPI
project_short=OMPI
version=1.10.2
language=C
compiler_env=CC
compiler_flags_env=CFLAGS
compiler=/usr/bin/gcc
extra_includes=
preprocessor_flags=
compiler_flags_prefix=
compiler_flags=-pthread
linker_flags=-L/usr/share/lsf/9.1/linux3.13-glibc2.19-ppc64le/lib    -Wl,-rpath -Wl,/usr/share/lsf/9.1/linux3.13-glibc2.19-ppc64le/lib -Wl,-rpath -Wl,@{libdir} -Wl,--enable-new-dtags
# Note that per https://svn.open-mpi.org/trac/ompi/ticket/3422, we
# intentionally only link in the MPI libraries (ORTE, OPAL, etc. are
# pulled in implicitly) because we intend MPI applications to only use
# the MPI API.
libs=-lmpi
libs_static=-lmpi -lopen-rte -lopen-pal -ldl -lrt -lbat -llsf -lnsl -losmcomp -libverbs -lrdmacm -lutil -lm
dyn_lib_file=libmpi.so
static_lib_file=libmpi.a
required_file=
includedir=${includedir}
libdir=${libdir}
```
