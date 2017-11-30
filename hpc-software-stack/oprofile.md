---
layout: page
title: OProfile
---
# OProfile

## Configuration


Unable to obtain cpu_type
Verify that a pre-1.0 version of OProfile is not in use.
If the /dev/oprofile/cpu_type file exists, locate the pre-1.0 OProfile
installation, and use its 'opcontrol' command, passing the --deinit option.
Unable to ascertain cpu type.  Exiting.

## Installation

* Install Prerequisites:
```
yum install git ibpfm-devel popt-devel binutils-devel
yum groups install Development\ Tools
```
* Build OProfile:
```
# git clone git://git.code.sf.net/p/oprofile/oprofile
# cd oprofile
# git tag
# git checkout <version>
# ./autogen.sh
# export CFLAGS="-m64" CXXFLAGS="-m64"
# ./configure --with-java=<java_sdk_dir> --libdir=/usr/local/lib64
( ./configure --with-java=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.91-1.b14.el7_2.x86_64 --libdir=/usr/local/lib64 )
# make
# make install
```
