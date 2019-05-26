---
author: Xiaojie Yuan
layout: post
category: leetcode
title: "[LeetCode][008] String to Integer (atoi)"
date: 2019-05-26
---

```c
#include <limits.h>

#define INT_MAX_DIV_BY_10 (INT_MAX / 10)
#define INT_MAX_REM_BY_10 (INT_MAX % 10)
#define INT_MIN_DIV_BY_10 (INT_MIN / 10)
#define INT_MIN_REM_BY_10 (INT_MIN % 10)

int myAtoi(char * str){
	char *p = str, c;
	int s = 1;
	int d;
	int v = 0;

	while (c = *p++) {
		if (c == ' ')
			continue;

		if ((c == '+' || c == '-') && *p >= '0' && *p <= '9') {
			if (c == '-')
				s = -1;
			break;
		} else if (c >= '0' && c <= '9') {
			p--; // point back to currect digit
			break;
		} else {
			return 0; // invalid
		}
	}

	// handle empty string or it contains only whitespaces
	if (!c)
		return 0;

	// at this time p must point to a digit
	while (c = *p++) {
		if (c < '0' || c > '9')
			return v;

		d = c - '0';

		if (s == 1) { // exceeds INT_MAX?
			if (v < INT_MAX_DIV_BY_10 || (v == INT_MAX_DIV_BY_10 && d < INT_MAX_REM_BY_10))
				v = 10 * v + d;
			else
				return INT_MAX;
		} else { // exceeds INT_MIN?
			if (v > INT_MIN_DIV_BY_10 || (v == INT_MIN_DIV_BY_10 && -d > INT_MIN_REM_BY_10))
				v = 10 * v - d;
			else
				return INT_MIN;
		}
	}

	return v;
}
```

### References
[1] <https://leetcode.com/problems/string-to-integer-atoi>
