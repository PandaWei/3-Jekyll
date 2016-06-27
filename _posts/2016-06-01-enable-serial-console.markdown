---
layout: post
title:  "Enable serial console!"
date:   2016-06-01 12:59:00
categories: Jekyll Update Kernel
tags: Kernel
---

In order to enable the serial console on a RHEL7/CentOS7 KVM guest you just need to add the following lines to /etc/default/grub:

GRUB_CMDLINE_LINUX_DEFAULT="console=tty0 console=ttyS0,115200n8"
GRUB_TERMINAL=serial
GRUB_SERIAL_COMMAND="serial --speed=115200 --unit=0 --word=8 --parity=no --stop=1"

Comments:
GRUB_CMDLINE_LINUX_DEFAULT applies this configuration only to the default menu entry, 
use GRUB_CMDLINE_LINUX to apply it to all the menu entries.

Now rebuild the grub.cfg file:
# grub2-mkconfig -o /boot/grub2/grub.cfg

After rebooting the guest it is possible to access the serial console using the virsh console command.
If you want to debug host kernel by serial, the setting is the same as above.

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
