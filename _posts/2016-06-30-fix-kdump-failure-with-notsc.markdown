---
layout: post
title:  "Fix kdump failure with notsc!"
date:   2016-06-29 09:00:00
categories: Kernel
tags: Kernel
---
### Kernel Bug:

If specify 'notsc' for capture-kernel, and then trigger crashdown. The capture-kernel will be hang at `Calibrating delay loop...`\. 

serial console log as following,

```sh
............
[    0.000000] Linux version 4.7.0-rc2+
(root@localhost.localdomain)
(gcc version 4.8.2 20140120 (Red Hat 4.8.2-16) (GCC) ) #2 SMP Wed Jun 156
[    0.000000] Kernel command line: BOOT_IMAGE=/vmlinuz-4.7.0-rc2+ root=/dev/mapper/centos-root ro rd.lvm.lv=centos/swap vconsole.font=latarcyrheb-sun16 rd.lvm.lv=centos/root crashkernel=256M vconsole.keymap=us console=tty0 console=ttyS0,115200n8 LANG=en_US.UTF-8 irqpoll nr_cpus=1 reset_devices cgroup_disable=memory mce=off numa=off panic=10 rootflags=nofail acpi_no_memhotplug notsc
............
[    0.000000] tsc: Kernel compiled with CONFIG_X86_TSC, cannot disable TSC completely
............
[    0.000000] clocksource: hpet: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 133484882848 ns
[    0.000000] tsc: Fast TSC calibration using PIT
[    0.000000] tsc: Detected 3192.714 MHz processor
[    0.000000] Calibrating delay loop...
```

Note:

- This bug is found in Kernel Upstream. so we'd better to compile the latest kernel source code. You can refer to my blog [Debug kernel by qemu!](https://pandawei.github.io/kernel/virtualization/2016/05/01/debug-kernel-by-qemu.html) for how to compile.
- Of course, you must enable the configuration about KDUMP before 'make -j<n>'. You can refer to [Documentation/kdump.txt](https://www.kernel.org/doc/Documentation/kdump/kdump.txt)
- specify 'notsc' for capture-kernel in /etc/sysconfig/kdump.

```sh
KDUMP_COMMANDLINE_APPEND="irqpoll nr_cpus=1 reset_devices cgroup_disable=memory mce=off numa=off udev.children-max=2 panic=10 rootflags=nofail"
```

- trigger crashdown as following,

```sh
# echo 1 > /proc/sys/kernel/sysrq
# echo c > /proc/sysrq-trigger
```

### Investigation and Report:

- By browsh the codes, I found that the captrue-kernel is block at calibrate_delay_converge()

```sh
/* wait for "start of" clock tick */
ticks = jiffies;
while (ticks == jiffies)
    ; /* nothing */
```

It seems that the jiffies not being increased.

### Two proposals, You can refer to the [RFC](https://lkml.org/lkml/2016/6/14/178):

- Take a look at it's caller calibrate_delay(). If I specify 'lpj' value, it can skip the bug.

```sh
void calibrate_delay(void)
{
	unsigned long lpj;
	static bool printed;
	int this_cpu = smp_processor_id();

	if (per_cpu(cpu_loops_per_jiffy, this_cpu)) {
		lpj = per_cpu(cpu_loops_per_jiffy, this_cpu);
		if (!printed)
			pr_info("Calibrating delay loop (skipped) "
				"already calibrated this CPU");
	} else if (preset_lpj) {
		lpj = preset_lpj;
		if (!printed)
			pr_info("Calibrating delay loop (skipped) "
				"preset value.. ");
	} else if ((!printed) && lpj_fine) {
		lpj = lpj_fine;
		pr_info("Calibrating delay loop (skipped), "
			"value calculated using timer frequency.. ");
	} else if ((lpj = calibrate_delay_is_known())) {
		;
	} else if ((lpj = calibrate_delay_direct()) != 0) {
		if (!printed)
			pr_info("Calibrating delay using timer "
				"specific routine.. ");
	} else {
		if (!printed)
			pr_info("Calibrating delay loop... ");
		lpj = calibrate_delay_converge();
	}
	per_cpu(cpu_loops_per_jiffy, this_cpu) = lpj;
	if (!printed)
		pr_cont("%lu.%02lu BogoMIPS (lpj=%lu)\n",
			lpj/(500000/HZ),
			(lpj/(5000/HZ)) % 100, lpj);

	loops_per_jiffy = lpj;
	printed = true;

	calibration_delay_done();
}
```

- Revert the 70de9a9. IMO, The flow of getting tsc_khz as following, tsc_init()-> x86_platform.calibrate_tsc()-> native_calibrate_tsc()-> quick_pit_calibrate(). so I think tsc_khz is available to calculate lpj. But it's denied [Denied Message](https://lkml.org/lkml/2016/6/24/237).

```sh
commit 70de9a97049e0ba79dc040868564408d5ce697f9
Author: Alok Kataria <akataria@vmware.com>
Date:   Mon Nov 3 11:18:47 2008 -0800

x86: don't use tsc_khz to calculate lpj if notsc is passed

Impact: fix udelay when "notsc" boot parameter is passed
With notsc passed on commandline, tsc may not be used for
udelays, make sure that we do not use tsc_khz to calculate
the lpj value in such cases.
```

### The Arch-Criminal

- I found the first bad commit <522e66464467> by bisect, which used to fix erratum AVR31 for "Intel Atom Processor C2000 Product Family Specification Update". You can find the [Doc](http://www.intel.com/content/dam/www/public/us/en/documents/specification-updates/atom-c2000-family-spec-update.pdf).

```sh
commit 522e66464467543c0d88d023336eec4df03ad40b
Author: Fenghua Yu <fenghua.yu@intel.com>
Date:   Wed Oct 23 18:30:12 2013 -0700

x86/apic: Disable I/O APIC before shutdown of the local APIC

In reboot and crash path, when we shut down the local APIC, the I/O APIC is
still active. This may cause issues because external interrupts
can still come in and disturb the local APIC during shutdown process.

To quiet external interrupts, disable I/O APIC before shutdown local APIC.
```

- It doesn't make sense to me that change the order of disabling between I/O APIC and local APIC just for a certain model C2000. And I couldn't find any related descriptions for Intel 64 and IA-32 Arch. so, I send a [PATCH](https://lkml.org/lkml/2016/6/29/18) to fix it.


**Nothing seek, nothing find!**
