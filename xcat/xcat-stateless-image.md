---
layout: page
title: Stateless Image
---
# Stateless Image

## Stateless Image Construction

* Check OS Image definition:
```
$ lsdef -t osimage rhels7.4-ppc64le-netboot-compute
Object name: rhels7.4-ppc64le-netboot-compute
    exlist=/opt/xcat/share/xcat/netboot/rh/compute.rhels7.ppc64le.exlist
    imagetype=linux
    kitcomponents=
    osarch=ppc64le
    osdistroname=rhels7.4-ppc64le
    osname=Linux
    osvers=rhels7.4
    otherpkgdir=/install/post/otherpkgs/rhels7.4/ppc64le
    otherpkglist=/install/custom/install/rh/inter.rhels73.ppc64le.otherpkgs.pkglist,/install/osimages/rhels7.4-ppc64le-netboot-inter/kits/KIT_DEPLOY_PARAMS.otherpkgs.pkglist,/install/osimages/rhels7.4-ppc64le-netboot-inter/kits/KIT_COMPONENTS.otherpkgs.pkglist,/install/osimages/rhels7.4-ppc64le-netboot-inter/kits/KIT_RMPKGS.otherpkgs.pkglist
    partitionfile=/install/custom/install/rh/inter.rhels73.ppc64le.partition
    permission=755
    pkgdir=/install/rhels7.4/ppc64le
    pkglist=/opt/xcat/share/xcat/netboot/rh/compute.rhels7.ppc64le.pkglist,/install/custom/install/rh/inter.rhels73.ppc64le.pkglist
    postinstall=/opt/xcat/share/xcat/netboot/rh/compute.rhels7.ppc64le.postinstall
    profile=inter73
    provmethod=netboot
    rootimgdir=/install/netboot/rhels7.4/ppc64le/compute
    synclists=/install/custom/install/rh/inter.rhels73.ppc64le.synclist
    template=/install/custom/install/rh/inter.rhels73.ppc64le.tmpl
```

> The *pkglist* attribute must contain the default xCAT Package List file. It can be complemented with additional Package Lists but must not be removed as it contains mandatory packages for image construction.

* Launch generation:
```
$ genimage rhels7.4-ppc64le-netboot-inter
Generating image:
cd /opt/xcat/share/xcat/netboot/rh; ./genimage -a ppc64le -o rhels7.4 -p inter73 --permission 755 --srcdir "/install/rhels7.4/ppc64le" --pkglist /opt/xcat/share/xcat/netboot/rh/compute.rhels7.ppc64le.pkglist,/install/custom/install/rh/inter.rhels73.ppc64le.pkglist --otherpkgdir "/install/post/otherpkgs/rhels7.4/ppc64le" --otherpkglist /install/custom/install/rh/inter.rhels73.ppc64le.otherpkgs.pkglist,/install/osimages/rhels7.4-ppc64le-netboot-inter/kits/KIT_DEPLOY_PARAMS.otherpkgs.pkglist,/install/osimages/rhels7.4-ppc64le-netboot-inter/kits/KIT_COMPONENTS.otherpkgs.pkglist,/install/osimages/rhels7.4-ppc64le-netboot-inter/kits/KIT_RMPKGS.otherpkgs.pkglist --postinstall /opt/xcat/share/xcat/netboot/rh/compute.rhels7.ppc64le.postinstall --rootimgdir /install/netboot/rhels7.4/ppc64le/inter --tempfile /tmp/xcat_genimage.89510 rhels7.4-ppc64le-netboot-inter
110 blocks
/opt/xcat/share/xcat/netboot/rh
110 blocks
/opt/xcat/share/xcat/netboot/rh
yum -y -c /tmp/genimage.89517.yum.conf --installroot=/install/netboot/rhels7.4/ppc64le/inter/rootimg/ --disablerepo=* --enablerepo=rhels7.4-ppc64le-0 --enablerepo=rhels7.4-ppc64le-1 --enablerepo=rhels7.4-ppc64le-2  install  bash nfs-utils openssl dhclient kernel openssh-server openssh-clients wget rsyslog vim-minimal ntp rsyslog rpm rsync ppc64-utils iputils dracut dracut-network e2fsprogs bc file lsvpd irqbalance procps-ng parted net-tools gzip tar xz grub2 grub2-tools bzip2 autoconf automake binutils-devel bison cmake ctags cvs dos2unix flex freetype-devel gdb libjpeg-turbo-devel libpng-devel libtool libxml2-devel openssl-devel qt-devel readline-devel patch perf strace tcl-devel valgrind zlib-devel bind bzip2 hwloc hwloc-libs ipmitool lshw net-tools nfs-utils net-snmp ntp numactl openssh-server psacct rsync tcl tcsh tk tree util-linux wget yp-tools ksh m4 gcc-c++ kernel-devel rpm-build openldap-clients sssd git subversion emacs nano vim freeglut mesa-libGLU qt-x11 vim-X11 xorg-x11-xauth xterm rsh rsh-server.ppc64le kernel pciutils lsof gcc-gfortran bind xinetd python-devel python-setuptools unzip "@infiniband" infiniband-diags perftest openmpi openmpi-devel libibverbs-devel
[...]
Enter the dracut mode. Dracut version: 033. Dracut directory: dracut_033.
Try to load drivers: ext3 ext4 to initrd.
chroot /install/netboot/rhels7.4/ppc64le/inter/rootimg dracut  -f /tmp/initrd.7321.gz 3.10.0-693.el7.ppc64le
No '/dev/log' or 'logger' included for syslog logging
Turning off host-only mode: '/sys' is not mounted!
Turning off host-only mode: '/proc' is not mounted!
Turning off host-only mode: '/run' is not mounted!
Turning off host-only mode: '/dev' is not mounted!
the initial ramdisk for stateless is generated successfully.
Try to load drivers: ext3 ext4 to initrd.
chroot /install/netboot/rhels7.4/ppc64le/inter/rootimg dracut  -f /tmp/initrd.7321.gz 3.10.0-693.el7.ppc64le
No '/dev/log' or 'logger' included for syslog logging
Turning off host-only mode: '/sys' is not mounted!
Turning off host-only mode: '/proc' is not mounted!
Turning off host-only mode: '/run' is not mounted!
Turning off host-only mode: '/dev' is not mounted!
the initial ramdisk for statelite is generated successfully.
```
The location of the generation directory is: `/install/netboot/rhels7.4/ppc64le/compute`
The generation directory should have the following content at this stage:
```
$ ls /install/netboot/rhels7.4/ppc64le/compute/
initrd-stateless.gz  initrd-statelite.gz  kernel  rootimg
```

* Pack image into ramdisk file:
```
$ packimage rhels7.4-ppc64le-netboot-inter
Packing contents of /install/netboot/rhels7.4/ppc64le/inter/rootimg
archive method:cpio
compress method:gzip
```
The generated ramdisk file is located in: `/install/netboot/rhels7.4/ppc64le/inter/rootimg.cpio.gz`
