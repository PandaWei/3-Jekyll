---
: post
title:  "Remove the old kernel compiled by yourself!"
date:   2016-04-01 10:30:00
categories: Kernel
tags: Kernel
---
### Background

Tell you how to permanently delete older kernels On RedHat, CentOs and Fedora.

### HowTo:

- remove the old and compiled kernel

```sh
# sudo rm -rf /lib/modules/<kernel_version>
# sudo rm -f /boot/vmlinuz-<kernel_version>*
# sudo rm -f /boot/initrd.img-<kernel_version>*
# sudo rm -f /boot/config-<kernel_version>*
# sudo rm -f /boot/System.map-<kernel_version>*
```

- remove the menuentry for old kernel in /boot/grub2/grub.cfg

```sh
# vim /boot/grub2/grub.cfg
menuentry 'Fedora, with Linux 3.11.10-301.fc20.x86_64' --class fedora ... {
    <...skip..>
    linux   /vmlinuz-3.11.10-301.fc20.x86_64 root=/dev/mapper/fedora-root ro rd.lvm.lv=fedora/swap vconsole.font=latarcyrheb-sun16 rd.lvm.lv=fedora/root rhgb quiet
    initrd  /initramfs-3.11.10-301.fc20.x86_64.img
}
```

**If you fail, don't forget to learn your lesson!**
