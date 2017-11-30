---
layout: page
title: IBM Spectrum MPI
---
# IBM Spectrum MPI

## Usage

### Interconnect / Protocol Supported

InfiniBand (POWER): PAMI with MOFED 3.1, 3.2, and 3.3

### *mpirun* Command Options

* Only Use PAMI Shared Memory Mechanism & Avoid Opening IB Device:
```
-pami_noib
```
* Display Protocols to be Used inside and between Hosts:
```
-prot
```
* Bypass PAMI Verbs (Requires IB Verbs 3.1 / 3.2 / 3.3):
```
-verbsbypass {3.1|3.2|3.3}
```

## Configuration

### MCA Parameters

Configuration Files:
```
~/.openmpi/mca_params
```

## Installation

### eFix

Prerequisites:
* Repository: RHELS 7.3
```
perl-Digest-MD5
```
* Repository: RHELS 7.3 Optional
```
perl-Switch
```
