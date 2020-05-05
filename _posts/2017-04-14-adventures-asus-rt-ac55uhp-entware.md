---
title: "Adventures Asus RT AC55UHP Entware"
description: "How to install entware on your router"
category: 
tags: [ASUS, RT-AC55UHP, ENTWARE]
---
{% include JB/setup %}

For people that have bought RT-AC55UHP and want to install entware on your router.

If you want to build the opensource GPL version asuswrt just download the GPL source from asus, follow the README.md, use ubuntu 14.04 with 30G of drive space.

```
wget -O - http://pkg.entware.net/binaries/mipsel/installer/installer.sh | sh
```

Initially want to install entware from original website, found it did not work because not compatiable with RT-AC55UHP different kernel.

```
/tmp/mnt/sdb1/asusware.mipsbig/root # uname -a
Linux (none) 3.3.8 #1 Tue Apr 14 15:08:50 CST 2015 mips GNU/Linux

admin@RT-AC55UHP:/tmp/home/root# cat /proc/cpuinfo
system type             : Qualcomm Atheros QCA9558 rev 0
machine                 : Atheros AP135 reference board
processor               : 0
cpu model               : MIPS 74Kc V5.0
BogoMIPS                : 358.80
wait instruction        : yes
microsecond timers      : yes
tlb_entries             : 32
extra interrupt vector  : yes
hardware watchpoint     : yes, count: 4, address/irw mask: [0x0000, 0x0ff8, 0x0f
f8, 0x0ff8]
ASEs implemented        : mips16 dsp
shadow register sets    : 1
kscratch registers      : 0
core                    : 0
VCED exceptions         : not available
VCEI exceptions         : not available

```

Next tried to install entware for 3.x kernel

```
wget -O - http://entware-3x.zyxmon.org/binaries/mips/installer/install_std.sh | sh
```

Make sure you start with clean usb drive. You can insert into router telnet and use fdisk and mke2fs to format.  
```
fdisk -l
fdisk /dev/sdb
```

Create new Linux partition.
```
mke2fs /dev/sdb1
```

Create the directory /mnt/sdb1/asuswrt.mipsbig so that /opt is linked to /tmp/opt.

Create the usbmount script so that /opt can be linked on boot.

```
cat << EOF > /tmp/script_usbmount.tmp
if [ \$1 = "/tmp/mnt/sdb1" ]
then
ln -sf \$1 /tmp/opt
/opt/etc/init.d/rc.unslung start
fi
EOF
nvram set script_usbmount="`cat /tmp/script_usbmount.tmp`"

cat << EOF > /tmp/script_usbumount.tmp
if [ \$1 = "/tmp/mnt/sdb1" ]
then
/opt/etc/init.d/rc.unslung stop
fi
EOF
nvram set script_usbumount="`cat /tmp/script_usbumount.tmp`"

nvram commit 
reboot
```

Had problems with adduser which is needed for openssh-server.  
Tried putting adduser in /jffs/scripts/services-start  
Tried putting adduser in /mnt/sdb1/asuswrt.mipsbig/.asusrouter  
Tried putting adduser in /mnt/sdb1/asuswrt.mipsbig/S40sshd.1  

reboot everytime then telnet and check /etc/passwd, new user does not appear.

Found out eventually you have to install the alternative version so that entware user does not mix with asuswrt.

```
wget -O - http://entware-3x.zyxmon.org/binaries/mips/installer/install_alt.sh | sh
```

For openssh-server
```
ssh-keygen -A
adduser -h /opt/tmp -s /opt/bin/false -D -H sshd 
```
edit /opt/etc/ssh/sshd_config file and change the lines 
#Port 22 to Port 2222 
and 
#PermitRootLogin prohibit-password to PermitRootLogin yes 
to allow root logins.


