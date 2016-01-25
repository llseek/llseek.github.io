---
author: Xiaojie Yuan
layout: post
title: "[LeetCode][199] Binary Tree Right Side View"
date: 2016-01-25
---

```c
struct QueueNode {
        struct TreeNode *val;
        struct QueueNode *next;
};

struct Queue {
        struct QueueNode *front;
        struct QueueNode *rear;
};

int is_empty(struct Queue *q)
{
        return !q->front && !q->rear;
}

struct Queue *queue_create(void)
{
        struct Queue *q = malloc(sizeof(struct Queue));

        q->front = NULL;
        q->rear = NULL;

        return q;
}

void queue_free(struct Queue *q)
{
        free(q);
}

int queue_is_empty(struct Queue *q)
{
        return is_empty(q);
}

void enqueue(struct Queue *q, struct TreeNode *val)
{
        struct QueueNode *n = malloc(sizeof(struct QueueNode));

        n->val = val;
        n->next = NULL;

        if (is_empty(q)) {
                q->front = n;
                q->rear = n;
                return;
        }

        q->rear->next = n;
        q->rear = n;
}

struct TreeNode *dequeue(struct Queue *q)
{
        struct TreeNode *val;
        struct QueueNode *victim;

        if (is_empty(q)) {
                printf("queue is empty\n");
                return NULL;
        }

        victim = q->front;
        val = q->front->val;

        if (q->front == q->rear)
                q->rear = NULL;
        q->front = q->front->next;

        free(victim);

        return val;
}

int *
rightSideView(struct TreeNode *root, int *returnSize)
{
        int *result;
        int *result_old;
        int result_len;
        int result_cnt;

        struct TreeNode *n;
        struct Queue *q;
        int current_depth_node_count;
        int next_depth_node_count;

        if (!root)
                return NULL;

        result_len = 1;
        result_cnt = 0;
        result = malloc(result_len * sizeof(int));
        if (!result) {
                printf("malloc failed\n");
                return NULL;
        }

        q = queue_create();
        if (!q) {
                printf("creating queue failed\n");
                return NULL;
        }
        enqueue(q, root);
        current_depth_node_count = 1;
        next_depth_node_count = 0;

        /* Breadth-first Traversal for Binary Tree */
        while (!queue_is_empty(q)) {
                n = dequeue(q);
                current_depth_node_count--;

                if (n->left) {
                        enqueue(q, n->left);
                        next_depth_node_count++;
                }
                if (n->right) {
                        enqueue(q, n->right);
                        next_depth_node_count++;
                }

                if (current_depth_node_count == 0) {
                        current_depth_node_count = next_depth_node_count;
                        next_depth_node_count = 0;

                        if (result_cnt >= result_len) {
                                result_old = result;
                                result_len *= 2;
                                result = realloc(result_old, result_len * sizeof(int));
                                if (!result) {
                                        printf("realloc failed\n");
                                        free(result_old);
                                        return NULL;
                                }
                        }
                        result[result_cnt] = n->val;
                        result_cnt++;
                }
        }

        queue_free(q);

        *returnSize = result_cnt;

        return result;
}

```

### References
[1] <https://leetcode.com/problems/binary-tree-right-side-view>
