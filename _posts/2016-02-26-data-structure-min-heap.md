---
author: Xiaojie Yuan
layout: post
category: data structure
title: "[Data Structure] Min Heap"
date: 2016-02-26
---

### Two Charactics of Min Heap

A _Min Heap_ is represented as a binary tree, this binary tree has two characteristics:  

1. value of a parent node are smaller than which of its child nodes
2. the binary tree is a _complete binary tree_
   - a _complete binary tree_ is defined as:
     - each level of _complete binary tree_ is completely filled, except possibly the last level
     - nodes of the last level are as far left as possible
     - a _complete binary tree_ can be efficiently implemented using an array

### Implementation in C

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include <assert.h>
#include <time.h>

#ifdef DEBUG
#define DBG(fmt, args...)	printf(fmt, ##args)
#else
#define DBG(fmt, args...)
#endif

#define SWAP(a, b)	{ \
	int tmp = a; \
	a = b; \
	b = tmp; \
}

struct min_heap {
	size_t capacity;
	int *a;
	size_t size;
};

struct min_heap *min_heap_initialize(size_t capacity)
{
	struct min_heap *mh;

	mh = calloc(1, sizeof(struct min_heap));
	assert(mh);

	mh->capacity = capacity;
	mh->a = calloc(capacity + 1, sizeof(int));
	assert(mh->a);
	mh->size = 0;

	return mh;
}

void min_heap_finalize(struct min_heap *mh)
{
	free(mh->a);
	free(mh);
}

#ifdef DEBUG
void min_heap_show(struct min_heap *mh)
{
	size_t i;

	printf("[ ");
	for (i = 1; i <= mh->size; i++)
		printf("%d ", mh->a[i]);
	printf("]\n");
}
#else
void min_heap_show(struct min_heap *mh) { }
#endif

void min_heap_siftup(struct min_heap *mh)
{
	size_t n = mh->size;
	int *a = mh->a;

	while (n > 1) {
		if (mh->a[n] >= mh->a[n/2])
			break;
		SWAP(a[n], a[n/2]);
		n = n/2;
	}
}

void min_heap_insert(struct min_heap *mh, int val)
{
	assert(mh->size < mh->capacity);

	mh->a[++mh->size] = val;
	min_heap_siftup(mh);
}

void min_heap_siftdown(struct min_heap *mh)
{
	size_t n = 1;
	int *a = mh->a;
	/* smallest index among [ parent, left child, right child ] */
	size_t k;

	while (n <= mh->size / 2) {
		k = n;

		if (a[2*n] < a[n])
			k = 2*n;

		if (2*n + 1 <= mh->size && a[2*n+1] < a[k])
			k = 2*n + 1;

		if (k == n)
			break;

		SWAP(a[n], a[k]);
		n = k;
	}
}

int min_heap_pop(struct min_heap *mh)
{
	int top = mh->a[1];

	mh->a[1] = mh->a[mh->size--];
	min_heap_siftdown(mh);

	return top;
}

bool is_sorted(int a[], size_t size)
{
	size_t i;

	for (i = 0; i < size - 1; i++) {
		if (a[i] > a[i + 1])
			return false;
	}

	return true;
}

int main(int argc, const char *argv[])
{
	int a[65536];
	size_t size = sizeof(a) / sizeof(a[0]);
	struct min_heap *mh;
	size_t i;

	srandom(time(NULL));
	for (i = 0; i < size; i++) {
		a[i] = random();
	}

	mh = min_heap_initialize(size);

	for (i = 0; i < size; i++) {
		DBG("insert %d\n", a[i]);
		min_heap_insert(mh, a[i]);
		min_heap_show(mh);
	}

	for (i = 0; i < size; i++) {
		a[i] = min_heap_pop(mh);
		DBG("pop %d\n", a[i]);
		min_heap_show(mh);
	}

	min_heap_finalize(mh);

	for (i = 0; i < size; i++) {
		DBG("%d ", a[i]);
	}
	DBG("\n");

	if (is_sorted(a, size)) {
		printf("TEST PASS\n");
		return 0;
	} else {
		printf("TEST FAIL\n");
		return -1;
	}

	return 0;
}
```

### References
[1] <https://en.wikipedia.org/wiki/Binary_tree>
