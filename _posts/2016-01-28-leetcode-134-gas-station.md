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
    def __canCompleteCircuit(self, diff, starting_idx):
        length = len(diff)
        wrap_around_list = range(starting_idx, len(diff)) + range(0, starting_idx)

        total = 0
        for i in wrap_around_list:
            total += diff[i]
            if total < 0:
                return False
        return True

    def canCompleteCircuit(self, gas, cost):
        """
        :type gas: List[int]
        :type cost: List[int]
        :rtype: int
        """
        diff = map(lambda a, b : a - b, gas, cost)

        # sort by diff value
        check_list = map(lambda x : x[0],
                sorted(enumerate(diff), key=lambda x : x[1], reverse=True))

        # try each starting point (from the largest to the smallest)
        for i in check_list:
            if self.__canCompleteCircuit(diff, i):
                return i

        return -1
```

__C__

```
TBD
```

### References
[1] <https://leetcode.com/problems/gas-station>
