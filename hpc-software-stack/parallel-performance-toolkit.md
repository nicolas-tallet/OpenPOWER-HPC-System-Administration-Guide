---
layout: page
title: Parallel Performance Toolkit For POWER
---
# Parallel Performance Toolkit For POWER

## Prerequisites
* 64-Bit Java Runtime Environment for HPC Toolkit Viewer

## Installation


## Usage

* Source Parallel Performance Toolkit Environment
```
[user@host ~]# source /opt/ibmhpc/ppedev.hpct/env_sh
```
* Build Application Binary Using Following Compilation Flags:
```
-emit-stub-syms -g -Wl,--hash-style=sysv
```
* Instrument Application Binary:
```
[user@host ~]# hpctInst -dmpi <binary>
```
