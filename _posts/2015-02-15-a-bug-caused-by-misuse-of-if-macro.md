---
author: Xiaojie Yuan
layout: post
title: "A bug caused by misuse of #if macro"
date: 2015-02-15
---

### Bug description

To fully simplify my situation but also clearly show the key points, I use some dummy code to describe this bug.

Considering that we want to let our code execute in following logic:

* If FOO is defined, execute in path A
* Else, execute in path B

But to our surprise, when 'FOO' is 'not defined', program still executes in path A.

### Original code

FOO is defined:

```c
#include <stdio.h>
#include <stdlib.h>

#define FOO	true

int main(int argc, char **argv)
{
#if FOO == true
	printf("I'm foo.\n");
#else
	printf("I'm not foo.\n");
#endif

	return 0;
}
```

Compile and Run:

```
$ gcc misuse_of_if_macro.c ; ./a.out
I'm foo.
#=> this is what we expect
```

FOO is not defined:

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char **argv)
{
#if FOO == true
	printf("I'm foo.\n");
#else
	printf("I'm not foo.\n");
#endif

	return 0;
}
```

Compile and Run:

```
$ gcc misuse_of_if_macro.c ; ./a.out
I'm foo.
#=> this is not what we expect
```

### Replace 'true' with '1'

#### FOO is defined

```c
#include <stdio.h>
#include <stdlib.h>

#define FOO	1

int main(int argc, char **argv)
{
#if FOO == 1
	printf("I'm foo.\n");
#else
	printf("I'm not foo.\n");
#endif

	return 0;
}
```

```
$ gcc misuse_of_if_macro.c ; ./a.out
I'm foo.
#=> this is what we expect
```

#### FOO is not defined

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char **argv)
{
#if FOO == 1
	printf("I'm foo.\n");
#else
	printf("I'm not foo.\n");
#endif

	return 0;
}
```

```
$ gcc misuse_of_if_macro.c ; ./a.out
I'm not foo.
#=> this is what we expect
```

### Cause of bug

According to [GCC document](https://gcc.gnu.org/onlinedocs/cpp/If.html) which describes #if macro:

* The ‘#if’ directive allows you to test the value of an __arithmetic__ expression, rather than the mere existence of one macro. Its syntax is:

```c
#if expression

controlled text

#endif /* expression */
```

* _expression_ is a C expression of integer type, subject to stringent restrictions. It may contain:
  * Integer constants.
  * Character constants.
  * Arithmetic operators.
  * Macros.
  * Uses of the _defined_ operator.
  * __Identifiers that are not macros, which are all considered to be the number ZERO__.

In our situation, when 'FOO' is not defined, 'FOO' in '#if FOO == true' is treated as __0__. On the other hand, based on the last rule that the GCC document describes, 'true' is also treated by GCC preprocessor as __0__. Therefore, when 'FOO' is not defined, the program goes to the wrong branch.

When we replace 'true' with '1', #if is now evaluating a arithmetic expression. So we can get what we want.

__In summary, #if macro expects an arithmetic expression, in which a string like 'true' is not allowed.__

### Solution

In this case, we just need to use __#ifdef__ instead of __#if__ since the value inside 'FOO' is not concerned at all.

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char **argv)
{
#ifdef FOO
	printf("I'm foo.\n");
#else
	printf("I'm not foo.\n");
#endif

	return 0;
}
```

### References
[1] <https://gcc.gnu.org/onlinedocs/cpp/If.html>
