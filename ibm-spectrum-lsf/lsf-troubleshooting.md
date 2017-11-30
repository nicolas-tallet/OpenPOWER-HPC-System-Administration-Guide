---
layout: page
title: IBM Spectrum LSF Troubleshooting
---

# IBM Spectrum LSF Troubleshooting

## Trace Slave Batch Daemon

* Find Slave Batch Daemon PID:
```
$ ps -ef | grep sbatchd
```
* Attach Stack Trace to Process:
```
$ strace -f -tt -o /tmp/strace.sbd.out -p <pid>
```
