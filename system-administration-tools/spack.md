---
title: Spack
layout: page
---
# Spack

## Configuration

### Compilation Environment

* File `etc/spack/compilers.yaml`:
```
compilers:
  linux-ppc64le:
    gcc@4.8:
      cc: /usr/bin/gcc
      cxx: /usr/bin/g++
      f77: /usr/bin/gfortran
      fc: /usr/bin/gfortran
    gcc@5.3:
      cc: /opt/at9.0/bin/gcc
      cxx: /opt/at9.0/bin/g++
      f77: /opt/at9.0/bin/gfortran
      fc: /opt/at9.0/bin/gfortran
    gcc@6.3:
      cc: /opt/at10.0/bin/gcc
      cxx: /opt/at10.0/bin/g++
      f77: /opt/at10.0/bin/gfortran
      fc: /opt/at10.0/bin/gfortran
    llvm@latest:
      cc: /gpfs16l/pwrlocal/llvm/clang/latest/bin/clang
      cxx: /gpfs16l/pwrlocal/llvm/clang/latest/bin/clang
      f77: /gpfs16l/pwrlocal/llvm/xlflang/latest/bin/xlflang
      fc: /gpfs16l/pwrlocal/llvm/xlflang/latest/bin/xlflang
    xl@13.1:
      cc: /gpfs16l/pwrlocal/ibm/xlC/13.1.3/bin/xlc_r
      cxx: /gpfs16l/pwrlocal/ibm/xlC/13.1.3/bin/xlC_r
      f77: /gpfs16l/pwrlocal/ibm/xlf/15.1.3/bin/xlf_r
      fc: /gpfs16l/pwrlocal/ibm/xlf/15.1.3/bin/xlf90_r
```

### `config.guess`

* Make Recent `config.guess` File Available:
```
/usr/share/automake/config.guess
```
Note: Give appropriate permissions to directory (rwxr-xr-x) and file (rwxr-xr-x)

* Paths:
**
