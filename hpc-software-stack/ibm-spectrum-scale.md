---
layout: page
title: IBM Spectrum Scale
---
# IBM Spectrum Scale

## Configuration

### Performance Settings
```
[root@host: ~]# mmfsadm dump config | grep failureDetectionTime
   failureDetectionTime -1
[root@host: ~]# mmfsadm dump config | grep minMissedPingTimeout
      minMissedPingTimeout 3
```
