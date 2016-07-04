---
author: Xiaojie Yuan
layout: post
category: kernel
title: "[ARM] CPU registers and modes"
date: 2016-07-04
---

### Registers

1. __r0 ~ r7__: general registers
2. __r8 ~ r12__: banked registers (in fiq mode)
3. __r13 / sp__: stack pointer register
4. __r14 / lr__: link register
5. __r15 / pc__: program counter register

### CPU modes

1. __(usr)__ user mode
2. __(sys)__ system mode
3. __(svc)__ supervisor mode
4. __(abt)__ abort mode
5. __(und)__ undefined mode
6. __(irq)__ interrupt mode
7. __(fiq)__ fast interrupt mode

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
