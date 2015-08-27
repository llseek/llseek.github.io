---
author: Xiaojie Yuan
layout: post
title: "[Kernel] Kernel and module debugging with gdb"
date: 2015-08-26
---

### What is a loadable module

* Module entry routine is tagged with __init attribute.
* __init is to identify code that can be discarded after the the module is loaded so as to not waste any kernel space.
* Module exit routine is tagged with __exit attribute.
* __exit is to identify code that should be kept around for unloading, unless there's no possibility that this module will ever be unloaded.
* several related definitions:

```c
#define __section(S)    __attribute__ ((__section__(#S)))
#define __init          __section(.init.text) __cold notrace
#define __initdata      __section(.init.data)
#define __exit          __section(.exit.text) __exitused __cold notrace
#define __exitdata      __section(.exit.data)
```

### How to debug with gdb

### References
[1] <https://www.linux.com/learn/linux-training/33991-the-kernel-newbie-corner-kernel-and-module-debugging-with-gdb>  
[2] <http://www.linux.com/learn/linux-training/32867-the-kernel-newbie-corner-whats-in-that-loadable-module-anyway>
