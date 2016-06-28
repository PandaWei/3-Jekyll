---
layout: post
title:  "Enable serial console!"
date:   2016-06-01 12:59:00
categories: Kernel
tags: Kernel
---

In order to enable the serial console on a RHEL7/CentOS7 KVM guest you just need to add the following lines to /etc/default/grub:

```sh
# vim /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="console=tty0 console=ttyS0,115200n8"
GRUB_TERMINAL=serial
GRUB_SERIAL_COMMAND="serial --speed=115200 --unit=0 --word=8 --parity=no --stop=1"
```

Comments:
GRUB_CMDLINE_LINUX_DEFAULT applies this configuration only to the default menu entry, 
use GRUB_CMDLINE_LINUX to apply it to all the menu entries.

Now rebuild the grub.cfg file:
```sh
# grub2-mkconfig -o /boot/grub2/grub.cfg
```

After rebooting the guest it is possible to access the serial console using the virsh console command.
If you want to debug host kernel by serial, the setting is the same as above.

**Do what you say,say what you do**
