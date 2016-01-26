---
author: Xiaojie Yuan
layout: post
category: leetcode
title: "[LeetCode][125] Valid Palindrome"
date: 2016-01-25
---

```c
bool isUpperCase(char c)
{
        return c >= 'A' && c <= 'Z';
}

bool isLowerCase(char c)
{
        return c >= 'a' && c <= 'z';
}

bool isAlpha(char c)
{
        return isUpperCase(c) || isLowerCase(c);
}

bool isDigit(char c)
{
        return c >= '0' && c <= '9';
}

bool isAlphanumeric(char c)
{
        return isAlpha(c) || isDigit(c);
}

bool isCaseInsensitiveEqual(char c1, char c2)
{
        if (isUpperCase(c1) && isLowerCase(c2))
                return c1 - 'A' == c2 - 'a';

        if (isLowerCase(c1) && isUpperCase(c2))
                return c1 - 'a' == c2 - 'A';

        return c1 == c2;
}

bool isPalindrome(char *s)
{
        int len = strlen(s);
        int i = 0;
        int j = len - 1;

        /* empty string is palindrome */
        if (len == 0)
                return true;

        while (i <= j) {
                while (!isAlphanumeric(s[i])) {
                        i++;
                        if (i >= j)
                                return true;
                }

                while (!isAlphanumeric(s[j])) {
                        j--;
                        if (i >= j)
                                return true;
                }

                if (!isCaseInsensitiveEqual(s[i++], s[j--]))
                        return false;
        }

        return true;
}

```

### References
[1] <https://leetcode.com/problems/valid-palindrome>
