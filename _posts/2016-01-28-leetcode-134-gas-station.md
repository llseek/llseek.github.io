---
author: Xiaojie Yuan
layout: post
category: leetcode
title: "[LeetCode][134] Gas Station"
date: 2016-01-28
---

__Python__

```python
class Solution(object):
    def __canCompleteCircuit(self, remains, start):
        l = remains[start:] + remains[:start]
        tank = 0

        for val in l:
            tank += val
            if tank < 0:
                return False
        return True

    def canCompleteCircuit(self, gas, cost):
        """
        :type gas: List[int]
        :type cost: List[int]
        :rtype: int
        """
        remains = map(lambda a, b : a - b, gas, cost)

        # sort by remain value in descending order
        start_points = map(lambda x : x[0],
                sorted(enumerate(remains), key=lambda x : x[1], reverse=True))

        # try each start point
        for i in start_points:
            if self.__canCompleteCircuit(remains, i):
                return i

        return -1
```

__C__

```
TBD
```

### References
[1] <https://leetcode.com/problems/gas-station>
