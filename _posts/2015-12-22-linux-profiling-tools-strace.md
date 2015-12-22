---
author: Xiaojie Yuan
layout: post
title: "[Performance] Linux Profiling Tools - strace"
date: 2015-12-22
---

### Normal usage

```
$ strace gcc hello_world.c
execve("/usr/bin/gcc", ["gcc", "hello_world.c"], [/* 50 vars */]) = 0
brk(0)                                  = 0x1c35000
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f8c4a4d2000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
<snip>
vfork()                                 = 31922
wait4(31922, [{WIFEXITED(s) && WEXITSTATUS(s) == 0}], 0, NULL) = 31922
--- SIGCHLD (Child exited) @ 0 (0) ---
stat("/tmp/ccjgkBiS.o", {st_mode=S_IFREG|0600, st_size=1512, ...}) = 0
unlink("/tmp/ccjgkBiS.o")               = 0
stat("/tmp/cc48vBzx.s", {st_mode=S_IFREG|0600, st_size=506, ...}) = 0
unlink("/tmp/cc48vBzx.s")               = 0
exit_group(0)                           = ?
```

### Trace child processes

> -f

> Trace child processes as they are created by currently traced processes as a result of the fork(2) system call.

```
$ strace -f gcc hello_world.c
execve("/usr/bin/gcc", ["gcc", "hello_world.c"], [/* 50 vars */]) = 0
brk(0)                                  = 0x729000
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f40cc537000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
<snip>
vfork(Process 31990 attached (waiting for parent)
Process 31990 resumed (parent 31989 ready)
)                                 = 31990
[pid 31989] wait4(31990, Process 31989 suspended
 <unfinished ...>
 [pid 31990] execve("/usr/lib/gcc/x86_64-linux-gnu/4.6/cc1", ["/usr/lib/gcc/x86_64-linux-gnu/4."..., "-quiet", "-imultilib", ".", "-imultiarch", "x86_64-linux-gnu", "hello_world.c", "-quiet", "-dumpbase", "hello_world.c", "-mtune=generic", "-march=x86-64", "-auxbase", "hello_world", "-fstack-protector", "-o", ...], [/* 53 vars */]) = 0
<snip>
[pid 31990] exit_group(0)               = ?
Process 31989 resumed
Process 31990 detached
<... wait4 resumed> [{WIFEXITED(s) && WEXITSTATUS(s) == 0}], 0, NULL) = 31990
--- SIGCHLD (Child exited) @ 0 (0) ---
<snip>
vfork(Process 31992 attached
)                                 = 31992
[pid 31989] wait4(31992, Process 31989 suspended
 <unfinished ...>
 [pid 31992] execve("/usr/lib/gcc/x86_64-linux-gnu/4.6/collect2", ["/usr/lib/gcc/x86_64-linux-gnu/4."..., "--sysroot=/", "--build-id", "--no-add-needed", "--as-needed", "--eh-frame-hdr", "-m", "elf_x86_64", "--hash-style=gnu", "-dynamic-linker", "/lib64/ld-linux-x86-64.so.2", "-z", "relro", "/usr/lib/gcc/x86_64-linux-gnu/4."..., "/usr/lib/gcc/x86_64-linux-gnu/4."..., "/usr/lib/gcc/x86_64-linux-gnu/4."..., ...], [/* 55 vars */]) = 0
<snip>
[pid 31992] exit_group(0)               = ?
Process 31989 resumed
Process 31992 detached
<... wait4 resumed> [{WIFEXITED(s) && WEXITSTATUS(s) == 0}], 0, NULL) = 31992
--- SIGCHLD (Child exited) @ 0 (0) ---
stat("/tmp/ccl0l91K.o", {st_mode=S_IFREG|0600, st_size=1512, ...}) = 0
unlink("/tmp/ccl0l91K.o")               = 0
stat("/tmp/ccjF661G.s", {st_mode=S_IFREG|0600, st_size=506, ...}) = 0
unlink("/tmp/ccjF661G.s")               = 0
exit_group(0)                           = ?

```

### Record each syscall's execution time

> -r
>
> Print a relative timestamp upon entry to each system call. This records the time difference between the beginning of successive system calls.

```
$ strace -r gcc hello_world.c
     0.000000 execve("/usr/bin/gcc", ["gcc", "hello_world.c"], [/* 50 vars */]) = 0
     0.000697 brk(0)                    = 0x97f000
     0.000159 access("/etc/ld.so.nohwcap", F_OK) = -1 ENOENT (No such file or directory)
     0.000283 mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fd44bba3000
     0.000247 access("/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
     0.000230 open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
<snip>
     0.000021 vfork()                   = 2017
     0.000116 wait4(2017, [{WIFEXITED(s) && WEXITSTATUS(s) == 0}], 0, NULL) = 2017
     0.009011 --- SIGCHLD (Child exited) @ 0 (0) ---
     0.000017 stat("/tmp/ccZqTCaG.o", {st_mode=S_IFREG|0600, st_size=1512, ...}) = 0
     0.000028 unlink("/tmp/ccZqTCaG.o") = 0
     0.000037 stat("/tmp/ccUdyB8W.s", {st_mode=S_IFREG|0600, st_size=506, ...}) = 0
     0.000033 unlink("/tmp/ccUdyB8W.s") = 0
     0.000030 exit_group(0)             = ?
```

### Summary of each syscall's execution time

> -c

> Count time, calls, and errors for each system call and report a summary on program exit. On Linux, this attempts to show system time (CPU time spent running in the kernel) independent of wall clock time. If -c is used with -f or -F (below), only aggregate totals for all traced processes are kept.

From the log shown below, we can see that _wait4_ accounts for almost all the execution time, which means the bottleneck exists in child processes.  
Thus we can inspect the log of 'strace -f -r' to further analyze which syscall consumes the most execution time.

```
$ strace -c gcc hello_world.c
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
100.00    0.004000        1333         3           wait4
  0.00    0.000000           0         3           read
  0.00    0.000000           0        10         4 open
  0.00    0.000000           0         6           close
  0.00    0.000000           0        56        25 stat
  0.00    0.000000           0         4           fstat
  0.00    0.000000           0         8           lstat
  0.00    0.000000           0        10           mmap
  0.00    0.000000           0         4           mprotect
  0.00    0.000000           0         2           munmap
  0.00    0.000000           0         3           brk
  0.00    0.000000           0         9           rt_sigaction
  0.00    0.000000           0        59        42 access
  0.00    0.000000           0         1           getpid
  0.00    0.000000           0         3           vfork
  0.00    0.000000           0         1           execve
  0.00    0.000000           0         2           unlink
  0.00    0.000000           0         2           readlink
  0.00    0.000000           0         1           getrlimit
  0.00    0.000000           0         1           arch_prctl
  0.00    0.000000           0         1           setrlimit
------ ----------- ----------- --------- --------- ----------------
100.00    0.004000                   189        71 total
```

### References
[1] <http://linux.die.net/man/1/strace>
