---
author: Xiaojie Yuan
layout: post
category: perf
title: "[Performance] Linux Profiling Tools - vmstat"
date: 2015-01-20
---

### Run vmstat

```
#=> update every 1 second
$ vmstat 1
procs -----------memory---------- ---swap-- -----io---- -system-- ----cpu----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa
 15  0   2852 46686812 279456 1401196    0    0     0     0    0    0  0  0 100  0
 16  0   2852 46685192 279456 1401196    0    0     0     0 2136 36607 56 33 11  0
 15  0   2852 46685952 279456 1401196    0    0     0    56 2150 36905 54 35 11  0
 15  0   2852 46685960 279456 1401196    0    0     0     0 2173 36645 54 33 13  0
<snip>
```

### Meaning of each field

#### __procs__
* _r_ - processes on run queue (running processes + runnable processes)
* _b_ - processes in uninterruptible sleep (TASK_UNINTERRUPTIBLE, marked as D in ps
command)

#### __memory__
* _swpd_ - virtual memory used (KB) (to be confirmed)
* _free_ - idle memory (KB)
* _buff_ - memory used as buffers (KB)
* _cache_ - memory used as cache (KB)

#### __swap__
* _si_ - memory swapped in from disk (KB/s)
* _so_ - memory swapped out to disk (KB/s)

#### __io__
* _bi_ - blocks received from a block device (blocks/s)
* _bo_ - blocks sent to a block device (blocks/s)

#### __system__
* _in_ - interrupts, including the clock (/s)
* _cs_ - context switches (/s)

#### __cpu__
* _us_ - time spent running non-kernel code (%)
* _sy_ - time spent running kernel code (%)
* _id_ - time spent idle (%)
* _wa_ - time spent waiting for IO (%)
