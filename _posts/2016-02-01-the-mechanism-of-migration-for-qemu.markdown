---
layout: post
title:  "The mechanism of migration in qemu"
date:   2016-02-01 09:00:00
categories: Virtualization
tags: Virtualization
---
### Tech Background

* [Live Migrating QEMU-KVM Virtual Machines] - This discussion will go through the simple design from the early days of live migration in the QEMU/KVM hypervisor, how it has been tweaked and optimized to where it is now, and where we’re going in the future. It will discuss how live migration actually works, the constraints within which it all has to work, and how the design keeps needing new thought to cover the latest requirements.

### Howto

A is the source host, B is the destination host:

1. Start the VM on B with the exact same parameters as the VM on A, in
   migration-listen mode:
```sh
A# x86_64-softmmu/qemu-system-x86_64 -enable-kvm -m 1024 -smp 2 linux-0.2.img -monitor stdio
```

```sh
B# x86_64-softmmu/qemu-system-x86_64 -enable-kvm -m 1024 -smp 2 linux-0.2.img -monitor stdio -incoming tcp:0:4444 (or other PORT))
```

1. Start the migration (always on the source host)

```sh
A# migrate -d tcp:<B's IP>:4444 (or other PORT)
```

1. Check the status (on A only):

```sh
A# (qemu) info migrate
```

1. Check the status (on B only):

```sh
B# (qemu) info status
```

### Learning and Contribution

To be continued ...

Aphorism
----

**I don’t run away from a challenge because I am afraid. Instead, I run towards it because the only way to escape fear is to trample it beneath your foot!**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)


[Live Migrating QEMU-KVM Virtual Machines]: <http://developers.redhat.com/blog/2015/03/24/live-migrating-qemu-kvm-virtual-machines/>

