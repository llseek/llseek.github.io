---
author: Xiaojie Yuan
layout: post
category: leetcode
title: "[LeetCode][002] Add Two Numbers"
date: 2015-08-29
---

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "list.h"

struct ListNode {

	int val;
	struct ListNode *next;
};

/*
 * [in]
 * @nd1: linked list of nodes
 * @nd2: linked list of nodes
 * 
 * [out]
 * none
 *
 * [in/out]
 * @carry(in): carry value of last sum
 * @carry(out): carry value of current sum
 */
static struct ListNode *addTwoNodes(struct ListNode *nd1,
				    struct ListNode *nd2,
				    int *carry)
{
	struct ListNode *nd_sum = malloc(sizeof(struct ListNode));

	if (!nd_sum) {
		printf("malloc node failed\n");
		return NULL;
	}
	nd_sum->val = 0;
	nd_sum->next = NULL;

	nd_sum->val = (nd1->val + nd2->val + *carry) % 10;
	*carry = (nd1->val + nd2->val + *carry) / 10;

	return nd_sum;
}

struct ListNode *addTwoNumbers(struct ListNode *l1, struct ListNode *l2)
{
	struct ListNode *nd1 = l1;
	struct ListNode *nd2 = l2;
	struct ListNode *nd_cont = NULL;
	struct ListNode nd_zero = { 0, NULL };

	struct ListNode *l_head = NULL;
	struct ListNode *l_tail = l_head;
	struct ListNode *nd_sum = NULL;
	int carry = 0;

	while (nd1 && nd2) {
		nd_sum = addTwoNodes(nd1, nd2, &carry);
		if (!nd_sum)
			return NULL;
		
		if (!l_head) { /* empty list */
			l_tail = l_head = nd_sum;
		} else { /* l_tail keeps track of the last node of list */
			l_tail->next = nd_sum;
			l_tail = nd_sum;
		}

		nd1 = nd1->next;
		nd2 = nd2->next;
	}

	if (!nd1 && !nd2) {
		goto carry;
	} else if (nd1 && !nd2) {
		nd_cont = nd1;
	} else { /* !nd1 && nd2 */
		nd_cont = nd2;
	}

	while (nd_cont) {
		nd_sum = addTwoNodes(nd_cont, &nd_zero, &carry);
		if (!nd_sum)
			return NULL;

		if (!l_head) { /* in case one of the two lists is initially empty */
			l_tail = l_head = nd_sum;
		} else {
			l_tail->next = nd_sum;
			l_tail = nd_sum;
		}

		nd_cont = nd_cont->next;
	}

carry:
	if (carry) {
		nd_sum = malloc(sizeof(struct ListNode));
		if (!nd_sum)
			return NULL;
		nd_sum->val = carry;
		nd_sum->next = NULL;

		l_tail->next = nd_sum;
		l_tail = nd_sum;
	}

	return l_head;
}
```

### References
[1] <https://leetcode.com/problems/add-two-numbers>
