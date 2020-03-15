---
author: Xiaojie Yuan
layout: post
category: kernel
title: "[Linux] Generate kernel crash dump"
date: 2020-03-16
---

### Steps

1. Make sure following configs are on/off for current kernel
```none
# CONFIG_KASAN is not set
CONFIG_RELOCATABLE=y
CONFIG_KEXEC=y
CONFIG_CRASH_DUMP=y
CONFIG_DEBUG_INFO=y
CONFIG_MAGIC_SYSRQ=y
CONFIG_PROC_VMCORE=y
```

2. Install kexec and crash tool
```none
$ sudo apt install kexec-tools makedumpfile crash
```

3. Add 'crashkernel' parameter to grub
```none
$ sudo vi /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="crashkernel=512M"
$ sudo update-grub
```

4. Load new kernel to be rebooted to into memory
```none
$ sudo kexec -p -l /boot/vmlinuz-$(KDUMP_KVER) \
       --append="$(cat /proc/cmdline | sed 's/crashkernel=[^\s]*//') nr_cpus=1 irqpoll" \
       --initrd=/boot/initrd.img-$(KDUMP_KVER)
```

5. Trigger kernel panic and new kernel will be automatically rebooted to
```none
$ echo c | sudo tee /proc/sysrq-trigger
```

6. Generate compressed kdump file from /proc/vmcore
```none
(In new kernel)
$ sudo makedumpfile -c -d 31
       -x /usr/lib/debug/boot/vmlinux-$(PANIC_KVER) /proc/vmcore dumpfile
```

7. Use crash tool to inspect generated kdump file
```none
$ sudo crash /usr/lib/debug/boot/vmlinux-$(PANIC_KVER) dumpfile
```

### Note

* Compile crash from source
```none
$ sudo apt install bison
$ git clone https://github.com/crash-utility/crash.git
$ cd crash
$ aria2c -s 16 -x 16 http://mirrors.nju.edu.cn/gnu/gdb/gdb-7.6.tar.gz
$ tar xvf gdb-7.6.tar.gz
$ make -j16
$ sudo make install
```

* Compile makedumpfile from source
```none
$ aria2c -s 16 -x16 https://sourceforge.net/projects/makedumpfile/files/makedumpfile/1.6.7/makedumpfile-1.6.7.tar.gz/download
$ tar xvf makedumpfile-1.6.7.tar.gz
$ cd makedumpfile-1.6.7
(refer README for build and intall steps)
```

* Tested workable combination
```none
kernel 5.5 + makedumpfile 1.6.7 + crash 7.2.8
```


### References
[1] <https://github.com/crash-utility/crash>  
[2] <https://sourceforge.net/projects/makedumpfile>  
[3] <https://help.ubuntu.com/lts/serverguide/kernel-crash-dump.html>
