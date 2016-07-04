---
author: Xiaojie Yuan
layout: post
category: kernel
title: "[ARM] CPU registers and modes"
date: 2016-07-04
---

### Registers

1. r0 ~ r7: general registers
2. r8 ~ r12: banked registers (in fiq mode)
3. r13: sp, stack pointer register
4. r14: lr, link register
5. r15: pc, program counter register

### CPU modes

1. (usr) user mode
2. (sys) system mode
3. (svc) supervisor mode
4. (abt) abort mode
5. (und) undefined mode
6. (irq) interrupt mode
7. (fiq) fast interrupt mode

### Registers across CPU modes

```
+-----------------------------------------+
|                  Modes                  |
|     |<------ previleged modes --------->|
|           |<---- exception modes ------>|
+-----------------------------------------+
| usr | sys | svc | abt | und | irq | fiq |
+-----------------------------------------+
|                 r0                      |
|                 r1                      |
|                 r2                      |
|                 r3                      |
|                 r4                      |
|                 r5                      |
|                 r6                      |
|                 r7                      |
|               r8                  |*r8  |
|               r9                  |*r9  |
|               r10                 |*r10 |
|               r11                 |*r11 |
|               r12                 |*r12 |
| r13 |*r13 |*r13 |*r13 |*r13 |*r13 |*r13 |
| r14 |*r14 |*r14 |*r14 |*r14 |*r14 |*r14 |
|                 r15                     |
|                cpsr                     |
|           |*spsr|*spsr|*spsr|*spsr|*spsr|
+-----------------------------------------+

*: indicates that the normal register used by User or system modes has
   been replaced by an alternative register specific to the exception mode
```

### References
[1] <http://stenlyho.blogspot.jp/2008/08/arm-register.html>  
[2] <https://en.wikipedia.org/wiki/ARM_architecture#Registers>
