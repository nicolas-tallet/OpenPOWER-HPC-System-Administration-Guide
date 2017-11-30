---
layout: page
title: IBM XL C/C++
---
# IBM XL C/C++

## Installation Packages

For Compiler:
```
xlc-license-beta.13.1.5-13.1.5.0
libxlsmp-4.1.5.0-160617c
libxlsmp-devel.4.1.5-4.1.5.0-1606
libxlmass-devel.8.1.5-8.1.5.0-160
libxlc-devel.13.1.5-13.1.5.0-160617c
libxlc-13.1.5.0-160617c
xlc.13.1.5-13.1.5.0-160617c
```

For Runtime Environment:
```
libxlsmp-4.1.5.0-160617
libxlc-13.1.5.0-160617c
```

## XLC Configure

Post-Install Script to be executed (as root).

Interactively:
```
/opt/ibm/xlC/13.1.5/bin/xlc_configure
```
Unattended:
```
echo 1 | /opt/ibm/xlC/13.1.5/bin/xlc_configure
```
Options:
* -at : Support for Advance Toolchain libraries
* -nosymlink : No Symbolic Links Generation

Output:
```
    #
If your language of preference is not available,
exit the application and view the PDF version of the
license (/opt/ibm/xlC/13.1.5/license.pdf) before proceeding.

International License Agreement for Early Release of Programs

[...]

Press Enter to continue viewing the license agreement, or, Enter "1" to accept the agreement, "2" to decline it or "99" to go back to the previous screen, "3" Print.
1
###########################################################
    ############################################################
INFORMATIONAL: GCC version used in "/opt/ibm/xlC/13.1.5/etc/xlc.cfg.rhel.7.2.gcc.4.8.5" -- "4.8.5"
INFORMATIONAL: /opt/ibm/xlC/13.1.5/bin/xlc_configure completed successfully
```

## [Temporary] Issue with Configuration File

Error Message:
```
[user@host ~] xlc -qversion
Compilation cannot proceed because one or more IBM compiler product files are missing or corrupted.  Reinstall the complete IBM XL C/C++ for Linux, V13.1.5 compiler.
```

Root Cause is Blank Line in Configuration File:
```
cuda_version = 7.5

defaultmsg = /opt/ibm/xlC/13.1.5/msg/en_US
```

Development Team Feedback:
The incorrect code that injected this blank line has been fixed in our latest dev builds, and we are also working on improving the compiler's error message to make it clear the root cause of the failure, instead of the 'corrupted' message you received.
Until we re-issue a new beta compiler with the fix(es), you can manually work around this by overriding the cuda version detection code after installation, by re-running the configuration tool with either:

Solution:
* Fix Configuration File Manually
* Regenerate Configuration File with Explicit CUDA Version
```
/opt/ibm/xlC/13.1.5/bin/xlc_configure -cudaVersion 8.0
```
