---
author: Xiaojie Yuan
layout:	post
title: "[Performance] Linux Profiling Tools - time"
date: 2015-01-21
---

###Shell built-in time

We often use __time__ command to measure the executing time of a program. It will print program's __real__, __user__ and __sys__ time respectively.

However, our commonly used 'time' is just a shell (bash, zsh) built-in command (same kind as 'cd', 'pwd' and etc). In some use cases, information it delivers is too few to satisfy our needs.

```
$ time tar zcf linux-stable.tar.gz linux-stable
 
real	2m39.398s
user	2m32.908s
sys	0m11.556s
```

###GNU time

Here, we will turn to another rarely used, but quite useful __time__ command - __GNU time__. This command is usually located under /usr/bin in most Linux distributions. Other than what shell built-in 'time' can show, GNU time prints more verbose system information like __CPU usage__, __page fault__, __context switch__ and etc.

```
#=> show GNU time version
$ /usr/bin/time --version
GNU time 1.7

#=> print in shell built-in time sytle
$ /usr/bin/time -p tar zcf linux-stable.tar.gz linux-stable
real 169.08
user 158.96
sys 12.19

#=> print verbosely
$ /usr/bin/time -v tar zcf linux-stable.tar.gz linux-stable
	Command being timed: "tar zcf linux-stable.tar.gz linux-stable"
	User time (seconds): 155.18
	System time (seconds): 11.18
	Percent of CPU this job got: 103%
	Elapsed (wall clock) time (h:mm:ss or m:ss): 2:41.20
	Average shared text size (kbytes): 0
	Average unshared data size (kbytes): 0
	Average stack size (kbytes): 0
	Average total size (kbytes): 0
	Maximum resident set size (kbytes): 5088
	Average resident set size (kbytes): 0
	Major (requiring I/O) page faults: 0
	Minor (reclaiming a frame) page faults: 790
	Voluntary context switches: 109556
	Involuntary context switches: 26354
	Swaps: 0
	File system inputs: 3932480
	File system outputs: 2333728
	Socket messages sent: 0
	Socket messages received: 0
	Signals delivered: 0
	Page size (bytes): 4096
	Exit status: 0
```

###References

[1] [wikipedia:Time_Unix](http://en.wikipedia.org/wiki/Time_%28Unix%29)
