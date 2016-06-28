---
layout: post
title:  "Introduce avocado and avocado-vt"
date:   2016-01-01 22:30:00
categories: Automatic-Test
tags: Automatic-Test
---
### Preface

Autotest is a framework for fully automated testing, It is designed primarily to test the Linux kernel, though it is useful for many other
functions such as qualifying new hardware. It's an open-source project under the GPL and is used and developed by a number of organizations,
including Google, IBM, Red Hat, Fujitsu, and many others.

and it's also very popular for virtualization test, recommended by Qemu, KVM and Libvirt's official.

- [Recommended-by-QEMU](http://wiki.qemu.org/Contribute/KVMAutotestInstallfest)
- [Recommended-by-KVM](http://www.linux-kvm.org/page/KVM-Autotest)

This is an introducation for it's successor - Avocado.

### Avocado Test Framework

Avocado is a test framework that is built on the experience accumulated with autotest, while improving on its weaknesses and shortcomings.

The main goal of the Avocado project is to provide a set of smart tools for automated testing and continuous integration. Among them, we can highlight:

- A powerful test runner;
- A multiplexer that allows tests to be run with different sets of variables;
- Test APIs for test writers;
- A database for results, with a web interface;
- A scheduler for setting up a test grid.

Avocado itself is open source with a [public repository][repo1] on GitHub.

Avocado comes with in tree documentation about the most advanced features and its API. It can be built with sphinx, but a publicly available build of the latest master branch documentation and releases can be seen on read the [docs](http://avocado-framework.github.io/).

### Avocado VT Plugins

Avocado-VT is a compatibility plugin that lets you execute virtualization related tests (then known as virt-test), with all conveniences provided by Avocado.

Avocado-VT itself is open source with a [public repository][repo2] on GitHub.

User's Guide and More details about it can be found here [docs](http://avocado-vt.readthedocs.io/):

### Development

Want to contribute? Great and Welcome! Let's meet each other on Github.

### Todos

 - Write More TestCases
 - Fix Any Bug
 - Add New Function
 - More Other Not Mentioned Here

**The best preparation for tomorrow is doing your best today!**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

   [repo1]: <https://github.com/avocado-framework/avocado>
   [repo2]: <https://github.com/avocado-framework/avocado-vt>
