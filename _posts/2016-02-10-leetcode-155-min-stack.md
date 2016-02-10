---
author: Xiaojie Yuan
layout: post
category: leetcode
title: "[LeetCode][155] Min Stack"
date: 2016-02-10
---

```python
class MinStack(object):
    def __init__(self):
        # stack with entries of (value, cur_min)
        self.l = []

    def push(self, x):
        """
        :type x: int
        :rtype: nothing
        """
        prev_min = self.getMin()
        if prev_min == None or prev_min > x:
            cur_min = x
        else:
            cur_min = prev_min
        self.l.append((x, cur_min))

    def pop(self):
        """
        :rtype: nothing
        """
        if self.l:
            self.l.pop()

    def top(self):
        """
        :rtype: int
        """
        if not self.l:
            return None
        return self.l[-1][0]

    def getMin(self):
        """
        :rtype: int
        """
        if not self.l:
            return None
        return self.l[-1][1]
```

### References
[1] <https://leetcode.com/problems/min-stack>
