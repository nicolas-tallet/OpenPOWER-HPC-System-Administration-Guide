---
layout: page
title: IBM XL Fortran
---
# IBM XL Fortran

## Installation Packages

Package List for Compiler:
```
xlf-license-beta.15.1.5-15.1.5.0
xlf.15.1.5-15.1.5.0-160617c
libxlf-15.1.5.0-160617c
libxlf-devel.15.1.5-15.1.5.0-160617c
libxlmass-devel.8.1.5-8.1.5.0-160617c
libxlsmp-4.1.5.0-160617
libxlsmp-devel.4.1.5-4.1.5.0-160617c
```

Package List for Runtime:
```
libxlsmp-4.1.5.0-160617
libxlf-15.1.5.0-160617c
```

## XLF Configure

Post-Install Script to be executed (as root).

Interactively:
```
/opt/ibm/xlf/15.1.5/bin/xlf_configure
```
Unattended:
```
echo 1 | /opt/ibm/xlf/15.1.5/bin/xlf_configure
```
Options:
* -at : Support for Advance Toolchain libraries
* -nosymlink : No Symbolic Links Generation

Output:
```
    #
If your language of preference is not available,
exit the application and view the PDF version of the
license (/opt/ibm/xlf/15.1.5/license.pdf) before proceeding.

International License Agreement for Early Release of Programs

[...]

Press Enter to continue viewing the license agreement, or, Enter "1" to accept the agreement, "2" to decline it or "99" to go back to the previous screen, "3" Print.
1
###########################################################
    ############################################################
INFORMATIONAL: GCC version used in "/opt/ibm/xlf/15.1.5/etc/xlf.cfg.rhel.7.2.gcc.4.8.5" -- "4.8.5"
INFORMATIONAL: /opt/ibm/xlf/15.1.5/bin/xlf_configure completed successfully
```

## Fortran Libraries

Usual Fortran Libraries to add at Link Time:
```
-L/opt/ibm/xlf/15.1.4/lib64 -lxl
-L/opt/ibm/xlf/15.1.4/lib64 -lxlf90_r
-L/opt/ibm/xlf/15.1.4/lib64 -lxlomp_ser
```
