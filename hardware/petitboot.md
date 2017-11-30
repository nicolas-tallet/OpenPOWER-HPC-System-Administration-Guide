---
layout: page
title: Petitboot
---
# Petitboot

## Collect SOS Report
* Set Debug Configuration:
```
nvram --update-config "petitboot,debug?=true"
```
* Reboot System
* Launch SOS Report Data Collection:
```
pb-sos [-v] [-f file] [-d user@host:/path]                               
```
