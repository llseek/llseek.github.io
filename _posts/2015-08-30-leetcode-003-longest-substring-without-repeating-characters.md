---
author: Xiaojie Yuan
layout: post
title: "[LeetCode][003] Longest Substring Without Repeating Characters"
date: 2015-08-30
---

```c
int lengthOfLongestSubstring(char* s)
{
        int N = strlen(s);
        int beg = 0;
        int end = 0;
        int candidate = 0;
        char c;
        int i, j;

        if (N == 0)
                return 0;

        for (i = 1; i < N; i++) {
                c = s[i];
                for (j = candidate; j <= i - 1; j++) {
                        if (s[j] == c) {
                                candidate = j + 1;
                                break;
                        }
                }
                if (i - candidate > end - beg) {
                        beg = candidate;
                        end = i;
                }
        }

        return end - beg + 1;
}
```

### References
[1] <https://leetcode.com/problems/longest-substring-without-repeating-characters>
