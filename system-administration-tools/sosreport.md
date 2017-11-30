---
layout: page
title: SOSReport
---
# SOSReport

## Links
- http://www.ibm.com/support/knowledgecenter/P8DEA/p8ei3/p8ei3_collect_diagnostic_data.htm
- https://linux.die.net/man/1/sosreport

## Command Usage

## Example
```
[root@host ~]# sosreport
.
sosreport (version 3.2)
.
This command will collect diagnostic and configuration information from
this Red Hat Enterprise Linux system and installed applications.
.
An archive containing the collected information will be generated in
/var/tmp and may be provided to a Red Hat support representative.
.
Any information provided to Red Hat will be treated in accordance with
the published support policies at:
.
  https://access.redhat.com/support/
.
The generated archive may contain data considered sensitive and its
content should be reviewed by the originating organization before being
passed to any third party.
.
No changes will be made to system configuration.
.
Press ENTER to continue, or CTRL-C to quit.
.
Please enter your first initial and last name [B. Gates]:
Please enter the case id that you are generating this report for []:
.
 Setting up archive ...
 Setting up plugins ...
 Running plugins. Please wait ...
.
  Running 86/86: yum...
Creating compressed archive...
.
Your sosreport has been generated and saved in:
  /var/tmp/sosreport-bgates-20161128165938.tar.xz
.
The checksum is: 6cc5328365493beae430640df2369e0a
.
Please send this file to your support representative.
.
```
