---
author: Xiaojie Yuan
layout: post
category: kernel
title: "[MPEG] Miscellaneous concepts"
date: 2016-09-03
---

### Terminologies

* GOP
  * Group Of Pictures
  * starts with an I-frame
* I-frame
  * intra-coded-frame
  * key-frame
  * doesn't rely on other frames
  * the 1st frame of a GOP
  * is stored just like a static image
  * can serve as the random access point
* P-frmame
  * predictive-frame
  * only relies on preceeding I-frame
* B-frame
  * bidirectional-predictive-frame
  * relies on both preceeding I/P-frame and following I/P-frame
* Delta-frame
  * refers to P-frame and B-frame
* Macro Block
* PTS
  * presentation timestamp
  * the time when a frame is displayed
* DTS
  * decode timestamp
  * the time when a frame is decoded

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
[1] <http://www.cnblogs.com/qingquan/archive/2011/07/27/2118967.html>  
[2] <http://google.com>
