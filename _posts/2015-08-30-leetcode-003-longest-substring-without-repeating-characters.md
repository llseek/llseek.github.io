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
        int beg, end, candidate;
        int pos = -1;
        char c;
        int i, j;

        if (N < 2)
                return N;

        /* for s[0] */
        beg = 0;
        end = 0;
        candidate = 0;

        /* for s[i] (i >= 1) */
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
