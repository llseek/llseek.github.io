---
author: Xiaojie Yuan
layout: post
title: "[LeetCode][006] Zigzag Conversion"
date: 2015-09-17
---

```c
char *convert(char *s, int numRows)
{
	/* string length */
	int L = strlen(s);
	/* row numbers */
	int R = numRows;

	/* parameter check */
	if (L == 0 || R == 1)
		return s;

	/* full block numbers */
	/*
	 * one 'block' is defined as:
	 *     0     
	 *     1   5
	 *     2 4
	 *     3
	 */
	int B = L / (2 * R - 2);
	/* remain characters */
	int M = L % (2 * R - 2);
	/* column numbers */
	int C = B * (R - 1) + (M <= R ? 1 : M - R + 1);

	/* allocate R*C 2-dimensional array */
	char **arr = NULL;
	int r, c, b, i;

	arr = calloc(R, sizeof(char *));
	if (!arr)
		return NULL;
	for (r = 0; r < R; r++) {
		arr[r] = calloc(C, sizeof(char));
		if (!arr[r])
			return NULL;
	}

	/* put characters to array one by one */
	i = 0;
	for (b = 0; b < B; b++) {
		/* vertical direction */
		for (r = 0; r < R; r++) {
			arr[r][b * (R - 1)] = s[i++];
		}
		/* diagonal direction */
		for (r = R - 2; r > 0; r--) {
			arr[r][(b + 1) * (R - 1) - r] = s[i++];
		}
	}

	/* remaining characters */
	/* vertical directi n */
	for (r = 0; i < L && r < R; r++) {
		arr[r][B * (R - 1)] = s[i++];
	}
	/* diagonal direction */
	for (r = R - 2; i < L; r--) {
		arr[r][(B + 1) * (R - 1) - r] = s[i++];
	}

	/* reuse input 's' as output */
	i = 0;
	for (r = 0; r < R; r++) {
		for (c = 0; c < C; c++) {
			if (arr[r][c] != '\0')
				s[i++] = arr[r][c];
		}
	}

	/* free 2-dimensional array */
	for (r = 0; r < R; r++)
		free(arr[r]);
	free(arr);

	return s;
}
```

### References
[1] <https://leetcode.com/problems/zigzag-conversion>
