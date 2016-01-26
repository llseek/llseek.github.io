---
author: Xiaojie Yuan
layout: post
category: leetcode
title: "[LeetCode][007] Reverse Integer"
date: 2015-09-18
---

```c
#define MAX_NUM_OF_DIGITS	10
#define MAX_VALUE_OF_INT	0x7fffffff
#define MIN_VALUE_OF_INT	-0x80000000

int reverse(int x)
{
	int digits[MAX_NUM_OF_DIGITS] = { 0 };
	int sign = x > 0 ? 1 : 0;
	unsigned abs = sign ? x : -x;
	unsigned max = sign ? MAX_VALUE_OF_INT : -MIN_VALUE_OF_INT;
	unsigned result = 0;
	int i = 0;
	int num_digits = 0;

	while (abs) {
		digits[i++] = abs % 10;
		abs /= 10;
	}
	num_digits = i;

	for (i = 0; i < num_digits && i < 9; i++)
		result = 10 * result + digits[i];

	if (num_digits == MAX_NUM_OF_DIGITS) {
		/* overflow for 32-bit signed int */
		if (result > max / 10 ||
		    (result == max / 10 && digits[9] > max % 10))
			return 0;
		else
			result = 10 * result + digits[i];
	}

	return sign ? result : -result;
}
```

### References
[1] <https://leetcode.com/problems/reverse-integer>
