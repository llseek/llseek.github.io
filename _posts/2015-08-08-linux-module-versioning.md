---
author: Xiaojie Yuan
layout: post
category: kernel
title: "[Linux] Module.symvers"
date: 2015-08-08
---

### What is Module.symvers

* Module.symvers is a plain text file which is generated after kernel building.
* It records CRC values of all the exported symbols in both vmlinux and modules.

### What Module.symvers is used for

* Module.symvers is used as a simple ABI consistency check.
* Before a module is loaded, its CRC value is compared with which in Module.symvers, if unequal, kernel will refuse to load the module.
* Module versioning is enabled by CONFIG_MODVERSIONS, if not enabled, all CRC values in Module.symvers will be 0x00000000.

### Syntax of Module.symvers

```
<CRC>           <symbol>                <module>
0xda13ea39      kmem_cache_alloc        vmlinux                 EXPORT_SYMBOL_GPL
0xc796a6f2      ieee80211_wake_queues   net/mac80211/mac80211   EXPORT_SYMBOL
```

### Symbols from External Modules

* When building an external module, the build system will access the symbols from the kernel to check if all external symbols are defined.
* MODPOST step obtains the symbols by reading Module.symvers from kernel source tree.
* If another Module.symvers file exists in current building directory, this file will also be read.
* During MODPOST step, a new Module.symvers file will be generated in current building directory, and it will contain all exported symbols which are not provided by kernel.

### Symbols from Another External Module

* Sometimes, an external module uses exported symbols from another external module, we have three solutions for this situation.
* For details, please refer section 6 of [Documentations/kbuild/modules.txt](https://www.kernel.org/doc/Documentation/kbuild/modules.txt)

### References
[1] <https://www.kernel.org/doc/Documentation/kbuild/modules.txt>
