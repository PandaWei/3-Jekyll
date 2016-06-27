---
layout: post
title:  "Debug kernel by qemu!"
date:   2016-05-01 12:59:00
categories: Jekell Kernel Virtualization
tags: Kernel Virtualization
---
Steps:
1. creates a configuration based on the defaults for your architecture
# make defconfig

2. define extra configuration if required. 
# vim .config
or
# make menuconfig  -->

3. enable debug info option

Kernel debugging->Compile the kernel with debug info"
    [*] KGDB: kernel debugging with remote gdb
        Method for KGDB communication (KGDB: On generic serial port (8250)) --->
    [*] KGDB: Thread analysis
    [*] KGDB: Console messages through gdb

# make -j<n>

4. confirm the result of compile
[root@localhost linux-debug]# pwd
/mnt/extra_disk/osc/linux-debug
[root@localhost linux-debug]# ll arch/x86/boot/bzImage
-rw-r--r-- 1 root root 6414240 Jun 17 17:10 arch/x86/boot/bzImage

5. start qemu with the compiled kernel
#qemu-system-x86_64 -s -S -kernel
/mnt/extra_disk/osc/linux-debug/arch/x86/boot/bzImage -hda linux-0.2.img
-append "root=/dev/sda console=ttyS0" -m 512M

Note:
'-s' option makes Qemu listen on port tcp::1234
'-S' option makes Qemu stop execution until you give the continue command

6. start gdb in another terminal
# gdb /mnt/extra_disk/osc/linux-debug/arch/x86/boot/vmlinux

(gdb) target remote localhost:1234
Remote debugging using localhost:1234
0x0000000000000000 in irq_stack_union ()
(gdb) b start_kernel
Breakpoint 1 at 0xffffffff81f42c00
(gdb) c
(gdb) list   --> browse the current line's codes

7. Q&A
 Maybe you'll met "gdb - Remote 'g' packet reply is too long",
 Please update gdb from source codes and comment the statments in 'gdb/remote.c' like below,

 //if (buf_len > 2 * rsa->sizeof_g_packet)
 //  error (_("Remote 'g' packet reply is too long: %s"), rs->buf);


Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
