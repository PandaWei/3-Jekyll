---
layout: post
title:  "live migration supports passthrough device!"
date:   2016-07-02 09:00:00
categories: Virtualization Kernel
tags: Virtualization Kernel
---
### SR-IOV device's live migration

This solution is used to support for SRIOV NIC in live migration, proposed by Lan Tianyu (Intel). And the solution needs to modify both Qemu and Kernel .

* [Qemu Part v1] - [RFC PATCH 0/3] Qemu/IXGBE: Add live migration support for SRIOV NIC
* [Qemu Part v2] - [RFC PATCH V2 00/10] Qemu: Add live migration support for SRIOV NIC

* [Kernel Part v1] - [RFC Patch 00/12] IXGBE: Add live migration support for SRIOV NIC
* [Kernel Part v2] - [RFC PATCH V2 0/3] IXGBE/VFIO: Add live migration support for SRIOV NIC

### Latest solution

To be continued ...

Aphorism
----

**Do one thing at a time, and do well!**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

[Qemu Part v1]: <http://lists.nongnu.org/archive/html/qemu-devel/2015-10/msg04979.html>
[Qemu Part v2]: <https://lists.gnu.org/archive/html/qemu-devel/2015-11/msg05254.html>

[Kernel Part v1]: <https://lkml.org/lkml/2015/10/21/629>
[Kernel Part v2]: <https://lkml.org/lkml/2015/11/24/434>
