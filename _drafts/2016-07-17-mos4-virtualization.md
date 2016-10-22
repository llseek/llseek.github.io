---
author: Xiaojie Yuan
layout: post
category: kernel
title: "[Topic] This is a title"
date: 2016-07-17
---

### One sentence

* hypervisor tricks guest OS into believing that it has the entire system

### Terminology

* sandbox
* VMM (Virtual Machine Monitor) == Hypervisor
* VMWare, Xen, KVM, VirtualBox, Hyper-V, Parallel
* sensitive instruction
* previledged instruction
* trap-and-emulate
* binary-translation
* paravirtualization/full virtualization
* type 1/2 hypervisor

  +-------------------+
  |    Gueset OSes    |
  +-------------------+
  | Type 1 hypervisor |
  +-------------------+
  |       H/W         |
  +-------------------+

  +-------------------+
  |    Gueset OSes    |
  +-------------------+
  | Type 1 hypervisor |
  +-------------------+
  |     Host OS       |
  +-------------------+
  |       H/W         |
  +-------------------+

### Section 2

```c
#include <stdio.h>

int main(int argc, char **argv)
{
	printf("Hello World!\n");
	return 0;
}
```

### Section 3

### References
[1] Modern Operating Systems 4th Edition - Andrew S.Tanenbaum
