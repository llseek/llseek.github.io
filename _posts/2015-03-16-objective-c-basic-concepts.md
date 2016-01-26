---
author: Xiaojie Yuan
layout: post
category: obj-c
title: "[Objective-C] Basic Concepts"
date: 2015-03-16
---


Property
--------

### Use weak property to avoid strong reference cycle

* Typical senario: delegate

```
+---------------------------------------------------------------------+
| NSTableView:                       Delegate Object:                 |
|                          strong                                     |
|                        --------->                                   |
| @property id delegate;            @property NSTableView *tableview; |
|                        <---------                                   |
|                          strong                                     |
+---------------------------------------------------------------------+
```

* Solution: declare 'delegate' as weak property

```
+----------------------------------------------------------------------------+
| NSTableView:                              Delegate Object:                 |
|                                  weak                                      |
|                               --------->                                   |
| @property (weak) id delegate;            @property NSTableView *tableview; |
|                               <---------                                   |
|                                 strong                                     |
+----------------------------------------------------------------------------+
```

### Use object cache to keep weak object alive

* In order to keep a weak object alive, assign it to a strong object (object cache).
* Then use the object cache to access the content that the weak object points to.
* Assign nil to the object cache when things are done.

### Use copy property

* If an object wishes to keep its own copy of its property objects, declare these objects with copy property


Category
--------


Protocol
--------


Delegate
--------
