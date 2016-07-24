---
author: Xiaojie Yuan
layout: post
category: leetcode
title: "[LeetCode][322] Coin Change"
date: 2016-07-24
---

```
Formular:
  dp[i] = MIN( dp[i - coins[j]] + 1 ) (j: [0, coinSize - 1])
```

```c
#include <stdio.h>
#include <stdlib.h>
#include <limits.h>

int coinChange(int *coins, int coinSize, int amount)
{
        int i, j;
        int *dp = calloc(amount + 1, sizeof(int));
        int min_so_far;
        int prev;
        int result;

        dp[0] = 0;

        for (i = 1; i <= amount; i++) {
                min_so_far = INT_MAX;
                for (j = 0; j < coinSize; j++) {
                        prev = i - coins[j];
                        if (prev < 0 || dp[prev] == -1)
                                continue;
                        if (dp[prev] + 1 < min_so_far)
                                min_so_far = dp[prev] + 1;
                }
                dp[i] = min_so_far == INT_MAX ? -1 : min_so_far;
        }

        result = dp[amount];
        free(dp);

        return result;
}
```

### References
[1] <https://leetcode.com/problems/coin-change>
