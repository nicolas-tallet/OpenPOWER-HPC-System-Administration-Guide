---
layout: page
title: Automated Job Monitoring Through LSF
---
# Automated Job Monitoring Through IBM Spectrum LSF

## Principle

The solution described below allows a System Administrator to implement a comprehensive monitoring of the Compute Nodes during a user job with a high-level of automation:
* Automated startup & shutdown of the monitoring services.
* Automated post-processing of the monitoring data into a user-readable format.

The solution involves the following monitoring services:
* Global compute node monitoring, including:
  * CPU utilization
  * CPU frequency
  * Memory utilization
  * GPU utilization
  * GPU frequency
* GPU-specific monitoring
* Energy consumption

## Technical Components

The solution is based on the following software components:

* [nmon](http://nmon.sourceforge.net/) / Version: 16g
  * Comprehensive system monitoring

* [Datacenter GPU Manager (DCGM)](https://developer.nvidia.com/data-center-gpu-manager-dcgm) / Version 1.1
  * GPU-specific monitoring

* [nmonchart](https://github.com/aguther/nmonchart)
  * Graph generation

* [nmon External Data Collector (EDC)](https://www.ibm.com/developerworks/community/blogs/aixpert/entry/nmon_and_External_Data_Collectors?lang=en)
  * Extend standard monitoring scope of *nmon* by:
    * Performing time-based data collection through an external mechanism
    * Merging extra data into standard *nmon* output

* IBM Spectrum LSF / Version: 10.1.0.3
  * Automation layer

* Inband IPMI Through `ipmitool`
  * Direct access to energy measurement data

### Energy Measurement Mechanism

Energy measurement data can be retrieved through different mechanisms:

* Option #1: Data Center Manageability Interface - `dcmi power reading` Command
```
$ sudo ipmitool dcmi power reading
.
    Instantaneous power reading:                   883 Watts
    Minimum during sampling period:                930 Watts
    Maximum during sampling period:                998 Watts
    Average power reading over sample period:      952 Watts
    IPMI timestamp:                                Thu May 11 14:37:28 2017
    Sampling period:                               00010000 Milliseconds
    Power reading state is:                        activated
```
> The main drawback of this method is that the sampling period does not seem to be adjustable.

* Option #2: Sensor Data Record - `sdr` Command
```
$ sudo ipmitool sdr entity 215 -S /var/tmp/sdr
Fan Power        | B0h | ok  | 215.15 | 44 Watts
Mem Proc0 Pwr    | ACh | ok  | 215.11 | 38 Watts
Mem Proc1 Pwr    | ADh | ok  | 215.12 | 38 Watts
PCIE Proc0 Pwr   | A6h | ok  | 215.5  | 95 Watts
Mem Cache Power  | ABh | ok  | 215.10 | 72 Watts
Proc0 Power      | A2h | ok  | 215.1  | 144 Watts
APSS Fault       | B2h | ok  | 215.0  |
Proc1 Power      | A3h | ok  | 215.2  | 128 Watts
PCIE Proc1 Power | A7h | ok  | 215.6  | 77 Watts
System Power     | A1h | ok  | 215.0  | 840 Watts
GPU Power        | AAh | ok  | 215.9  | 115 Watts
```
> Note #1: The advantage of this method is that the global system energy consumption is further decomposed into the main system components.
>
> Note #2: This mechanism relies on the existence of a SDR dump file that must be generated through the following command:
```
$ sudo /usr/bin/ipmitool sdr dump /var/tmp/sdr >/dev/null
```

## Technical Implementation

### Setup Inbound IPMI

* Allow invocation of the `ipmitool` command via `sudo` on the Compute Nodes:
```
# sudoers.d/users
Defaults !requiretty
Cmnd_Alias STANDARD_USER_COMMANDS = /usr/bin/ipmitool sdr*
ALL ALL = NOPASSWD: STANDARD_USER_COMMANDS
```

### Setup LSF Host Pre-/Post-Exec Environment

* Create a Host Pre-/Post-Exec configuration file which defines:
  * The commands to be used, including their list of options
  * The names of output files
{% gist nicolas-tallet/b9e4558cc932bd5ccd6149fa0c645ac8 %}

* Create a Host Pre-Exec script which will perform the following actions on each execution host:
  * Start nmon monitoring process in background
  * Start DCGM
{% gist nicolas-tallet/8b681ac8644933027d7b0b34552f9d4a %}

* Create a Host Post-Exec script which will perform the following actions on each execution host:
  * nmon
    * Stop the background monitoring process
    * Post-process the monitoring file to generate a per-host HTML analysis
  * DCGM
    * Stop the job monitoring process
    * Export the monitoring data into an output file
{% gist nicolas-tallet/5f96abb5cca07d8290726564b8cc1c19 %}

### Setup nmon External Data Collector (EDC)

The process to setup an energy consumption EDC is the following:

* Create EDC configuration file:
{% gist nicolas-tallet/29e6f05df440f8b840c3307304c997b2 %}

* Create EDC startup script:
{% gist nicolas-tallet/dfc3b07605c0858689307266a4fe8c3b %}

* Create EDC snapshot script:
{% gist nicolas-tallet/d0151a1bfa53c92d09b5f374e30b5348 %}

* Create EDC termination script:
{% gist nicolas-tallet/1b8360bf2471ff157e5c882d6f0f8e49 %}

### Extend *nmonchart* Graph Generator Script

In order for the `nmonchart` script to be able to process Power Usage data, the following patch must be applied to the standard script (version 31):
{% gist nicolas-tallet/f3440ef41fc20a46febb94b011593e84 %}

The patch can be applied through the following command:
```
$ patch < nmonchart.patch
```

This patch performs the following additions:
* Power Usage Data Records Parsing
* Power Usage Graph Declaration
* Power Usage Graph Generation
* Average Power Usage Value Computation

### Create LSF Application

* Define an LSF application with the following configuration:
```
# Supervision
Begin Application
DESCRIPTION = Supervision - Automated Host Monitoring
HOST_POST_EXEC = /shared/lsf/conf/prepostexec/supervision-hostpostexec.sh
HOST_PRE_EXEC = /shared/lsf/conf/prepostexec/supervision-hostpreexec.sh
NAME = supervision
End Application
```
inside the following LSF configuration file: `conf/lsbatch/<cluster>/configdir/lsb.applications`

* Reconfigure LSF Master Batch Daemon:
```
$ badmin reconfig
```

### Configure Specific *cgroup* for Monitoring Process

In the Spectrum LSF standard configuration, a Post-Exec stage does not start while a process which was started during the Pre-Exec stage is still active.
Thus, in our current implementation, the background monitoring process prevents the Post-Exec stage to start once the execution is done.

In order for the Post-Exec stage to start, it is required to make sure that the background monitoring process is detached from the set of processes that are under the Spectrum LSF control.
This is achieved through the following *cgroup* configuration:

* Configure a permanent group named `cgnmon` in configuration file `/etc/cgconfig.conf`:
```
group cgnmon {
  cpuacct {}
  freezer {}
  memory {}
}
```

* Configure `cgred` daemon to place all *nmon* related processes to the newly-created `cgnmon` group in configuration file `/etc/cgrules.conf`:
```
*:nmon cpuacct,freezer,memory cgnmon/
```
* Restart the following two daemons:
```
$ systemctl restart cgconfig
$ systemctl restart cgred
```

## Usage

* Specify LSF application as part of the user job submission:
  * Submission command argument:
```
$ bsub -app "supervision" <...>
```
  * Job submission file directive:
```
#BSUB -app "supervision"
```

* At the end of the execution, collect the monitoring output files residing inside the execution directory:
  * `dcgm.<hostname>.<timestamp>.html`
    * Human-readable text file
  * `nmon.<hostname>.<timestamp>.html`
    * HTML file including dynamic controls, to be displayed with a Web browser

* The following Power Usage graph is an example of what can be obtained:
![nmon Power Usage Graph](https://pages.github.ibm.com/IBM-Client-Center-Montpellier/openpower-high-performance-computing/system-administration-guide/11-advanced-topics/nmon-graph-powerusage.png)
