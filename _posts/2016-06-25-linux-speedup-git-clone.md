---
author: Xiaojie Yuan
layout: post
category: kernel
title: "[Linux] Speedup Linux clone"
date: 2016-06-25
---

### Background

In Nov. 2015, [kernel.org](https://www.kernel.org) announced that [Fastly](https://www.fastly.com)(a CDN provider) offered CDN service to provide fast download for Linux kernel releases.


### How to use this feature

1. Download Linux bundle

        $ wget -c --no-check-certificate https://cdn.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/clone.bundle
In Shanghai, it only takes about __20min__ to download the bundle.

2. Verify downloaded bundle

        $ git bundle verify clone.bundle

3. Clone from the bundle

        $ git clone clone.bundle linux

4. Change 'origin' point to the live git repo

        $ cd linux
        $ git remote remove origin
        $ git remote add origin https://git.kernel.org/pub/scm/linux/kernel/git/torvalds
        $ git pull origin master

Now, everything is done.

### What is git bundle?

refer [this link](https://git-scm.com/docs/git-bundle)

### References
[1] <https://www.kernel.org/category/site-news.html>  
[2] <https://git-scm.com/docs/git-bundle>
