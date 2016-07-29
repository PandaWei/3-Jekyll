---
layout: post
title:  "Passthrough/SR-IOV knowledge and usage!"
date:   2016-07-01 09:00:00
categories: Virtualization
tags: Virtualization
---
### Background

* [Intel VT-d] - Intel® Virtualization Technology for Directed I/O (VT-d): Enhancing Intel platforms for efficient virtualization of I/O devices

### Tech Article&Web

* [PCI Passthrough -cn] - introduece device emulation and hardware(PCI) I/O virtualization, which based on Intel® (VT-d) or AMD's IOMMU
* [PCI Passthrough -en] - introduece device emulation and hardware(PCI) I/O virtualization, which based on Intel® (VT-d) or AMD's IOMMU

* [SR-IOV Hardware] - Oracle about Single Root I/O Virtualization (SR-IOV)
* [SR-IOV Feature] - about Single Root I/O Virtualization (SR-IOV)

* [Redhat SR-IOV Primer] - Redhat docs
* [Understanding the Basics] - Redhat docs
* [Walking Through the Implementation] - Redhat docs

* [PCI Assignment for KVM] - assign PCI devices from your KVM host machine to guest virtual machines
* [Using PCI passthrough with KVM (1)] - how to use PCI passthrough with KVM
* [Using PCI passthrough with KVM (2)] - implementing PCI Device Passthrough (IOMMU) with Intel VT-d, KVM, QEMU, and libvirtd on Fedora 21
* [Using VFIO PCI passthrough for KVM] - create a gaming virtual machine using VFIO PCI passthrough for KVM

* [vfio vs virtio] - comparing between VFIO passthrough and virtio approaches, and tell us how to check if your NIC supports SR-IOV

### Tech Manual

* [PCI Spec] - The PCI-sig's specifications library


Aphorism
----

**Do one thing at a time, and do well!**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

[Intel VT-d]: <https://software.intel.com/en-us/articles/intel-virtualization-technology-for-directed-io-vt-d-enhancing-intel-platforms-for-efficient-virtualization-of-io-devices>

[PCI Passthrough -cn]: <https://www.ibm.com/developerworks/cn/linux/l-pci-passthrough/>
[PCI Passthrough -en]: <http://www.ibm.com/developerworks/library/l-pci-passthrough/>
[SR-IOV Hardware]: <http://docs.oracle.com/cd/E38902_01/html/E38873/glbzi.html>
[SR-IOV Feature]: <http://fedoraproject.org/wiki/Features/SR-IOV>

[Redhat SR-IOV Primer]: <http://rhelblog.redhat.com/2016/05/23/sr-iov/>
[Understanding the Basics]: <http://redhatstackblog.redhat.com/2015/03/05/red-hat-enterprise-linux-openstack-platform-6-sr-iov-networking-part-i-understanding-the-basics/>
[Walking Through the Implementation]: <http://redhatstackblog.redhat.com/2015/04/29/red-hat-enterprise-linux-openstack-platform-6-sr-iov-networking-part-ii-walking-through-the-implementation/>

[PCI Assignment for KVM]: <http://fedoraproject.org/wiki/Features/KVM_PCI_Device_Assignment>
[Using PCI passthrough with KVM (1)]: <https://docs.fedoraproject.org/en-US/Fedora/13/html/Virtualization_Guide/chap-Virtualization-PCI_passthrough.html>
[Using PCI passthrough with KVM (2)]: <https://bluehatrecord.wordpress.com/2015/07/26/implementing-pci-device-passthrough-iommu-with-intel-vt-d-kvm-qemu-and-libvirtd-on-fedora-21/>
[Using VFIO PCI passthrough for KVM]: <http://www.firewing1.com/howtos/fedora-20/create-gaming-virtual-machine-using-vfio-pci-passthrough-kvm>

[vfio vs virtio]: <http://www.linux-kvm.org/page/10G_NIC_performance:_VFIO_vs_virtio>

[PCI SPEC]: <http://pcisig.com/specifications>
