---
author: Xiaojie Yuan
layout: post
category: leetcode
title: "[LeetCode][001] Two Sum"
date: 2015-08-29
---

```c
#include <stdio.h>
#include <stdlib.h>

#define MIN(a, b) ((a) < (b) ? (a) : (b))
#define MAX(a, b) ((a) > (b) ? (a) : (b))

struct node {
	int index;
	int value;
};

void list_qsort(struct node **list, int size)
{
	struct node *anchor = list[0];
	int i = 1;
	int j = size - 1;
	struct node *tmp = NULL;

	if (size < 2)
		return;

	while (1) {
		while (i < size && list[i]->value <= anchor->value) i++;
		while (j > 0 && list[j]->value > anchor->value) j--;
		
		if (i > j)
			break;

		tmp = list[i];
		list[i] = list[j];
		list[j] = tmp;
	}

	list[0] = list[j];
	list[j] = anchor;

	list_qsort(list, j);
	list_qsort(list + i, size - i);
}

/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int *twoSum(int *nums, int numsSize, int target)
{
	int i, j;
	int *ret = (int *)malloc(2 * sizeof(int));
	struct node **list = (struct node **)malloc(numsSize * sizeof(struct node *));

	if (!ret || !list) {
		printf("malloc failed\n");
		return ret;
	}

	/* initialize node list */
	for (i = 0; i < numsSize; i++) {
		list[i] = (struct node *)malloc(sizeof(struct node));
		if (!list[i]) {
			printf("malloc node failed\n");
			return NULL;
		}
		list[i]->index = i;
		list[i]->value = nums[i];
	}

	/* sort node list */
	list_qsort(list, numsSize);

	/* find target */
	for (i = 0; i < numsSize - 1; i++) {
		for (j = numsSize - 1; j > i; j--) {
			if (list[i]->value + list[j]->value == target) {
				ret[0] = MIN(list[i]->index + 1, list[j]->index + 1);
				ret[1] = MAX(list[i]->index + 1, list[j]->index + 1);

				return ret;
			} else if (list[i]->value + list[j]->value < target) {
				/* stop searching in sorted array */
				break;
			}
		}
	}

	return NULL;
}
```

### References
[1] <https://leetcode.com/problems/two-sum>
