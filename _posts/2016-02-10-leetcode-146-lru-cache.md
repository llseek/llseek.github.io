---
author: Xiaojie Yuan
layout: post
category: leetcode
title: "[LeetCode][146] LRU Cache"
date: 2016-02-10
---

```python
class LRUCache(object):
    def __init__(self, capacity):
        self.__capacity = capacity
        self.__dict = {}
        # oldest to youngest
        self.__list = []

    def get(self, key):
        if not self.__dict.has_key(key):
            return -1

        # move key to the tail of list
        if key != self.__list[-1]:
            self.__list.remove(key)
            self.__list.append(key)

        return self.__dict[key]

    def set(self, key, value):
        if self.__dict.has_key(key):
            self.__dict[key] = value
            if key != self.__list[-1]:
                self.__list.remove(key)
                self.__list.append(key)
        else:
            if len(self.__dict) == self.__capacity:
                self.__dict.pop(self.__list[0])
                self.__list.pop(0)
            self.__dict[key] = value
            self.__list.append(key)
```

### References
[1] <https://leetcode.com/problems/lru-cache>
