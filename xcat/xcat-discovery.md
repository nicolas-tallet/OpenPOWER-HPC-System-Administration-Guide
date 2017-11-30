---
layout: page
title: Managed Nodes Discovery
---
# Managed Nodes Discovery

## Manual Discovery of BMC

* Launch manual discovery of new nodes BMC:
```
$ bmcdiscover -t -z --range 192.168.1.121-132 > bmcdiscover.nodes
```

* Adjust hostnames inside generated file `bmcdiscover.nodes`

* Create objects for BMC:
```
$ cat bmcdiscover.nodes | mkdef -z
```
makeconservercf
systemctl restart conserver

## Automatic Discovery

* Create discovery network:
```
$ makenetworks
$ tabedit networks
"10_10_10_0-255_255_255_0","10.10.10.0","255.255.255.0","enP3p9s0f0","<xcatmaster>",,"10.10.10.1",,,,"10.10.10.2-10.10.10.100",,,,,,,,
$ chdef -t 10_10_10_0-255_255_255_0 dynamicrange=”10.10.10.2-10.10.10.100”
$ makedhcp -a
$ systemctl restart dhcpd
```

* Create Kernel image for Genesis node analysis agent:
```
$ mknb ppc64
Creating genesis.fs.ppc64.gz in /tftpboot/xcat
```

* Launch Sequential Discovery:
```
$ nodediscoverls -t all
$ nodediscoverstart noderange=”host[01-12]”
$ nodediscoverstatus -V noderange=host[01-12]
Sequential discovery is running.
$ rpower host01-adm on
$ nodediscoverls -t all
$ nodediscover stop
```

* Remove discovery network from xCAT configuration through `tabedit networks`command.

* Remove discovery network on xCAT server:
```
$ ifconfig <>:1 down
$ makenetworks
$ makedhcp -n
$ makedhcp -a
```
