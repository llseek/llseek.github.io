---
author: Xiaojie Yuan
layout: post
title: "[Pthread] Run Two Threads in Turn"
date: 2015-03-22
---


### Pthread Interfaces

```c
#include <pthread.h>
int pthread_create(pthread_t *tidp,
                   const pthread_attr_t attr,
		   void *(*start_rtn)(void *),
		   void *arg);
int pthread_join(pthread_t *tidp, void **rval_ptr);
int pthread_mutex_lock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);
int pthread_cond_wait(pthread_cond_t *cond, pthread_mutex_t *mutex);
int pthread_cond_signal(pthread_cond_t *cond);
```

### Precautions of pthread\_cond\_wait()

* pthread\_cond\_wait() accepts two arguments: 'cond' and 'mutex'.
* 'cond' is a condition variable.
* a thread should acquire 'mutex' before calling pthread\_cond\_wait().
* pthread\_cond\_wait() ATOMICALLY releases 'mutex' and blocks on 'cond'.
  * ATOMICALLY here means if another thread acquires 'mutex' just after the about-to-block thread has released it, then a subsequent call to pthread\_cond\_signal() in that thread shall behave AS IF it were issued after the about-to-block thread has blocked.
* upon successfully return, 'mutex' shall have been locked and owned by the calling thread.


### Typical Usage of pthread\_cond\_wait/signal()

* When using condition variables, there is always a boolean predicate involving shared variables associated with each condition wait that is true if the thread should proceed.
* Spurious wakeups from condition wait may occur, for example, on multi-processor platform, multiple threads are woken up when the condition variable is signaled simutaneously on different processors.
* Since the return from pthread\_cond\_wait() does not imply anything about the value of the predicate, the thread has to re-evaluate the predicate to determine whether it can safely proceed or should wait again.
* It is thus recommended that a condition wait be enclosed in a while loop that checks the predicate.

```c
thread 1:
	pthread_mutex_lock(&mutex);
	while (!condition)
		pthread_cond_wait(&cond, &mutex);
	/* do something that requires holding the mutex and condition is true */
	pthread_mutex_unlock(&mutex);

thread 2:
	pthread_mutex_lock(&mutex);
	/* do something that might make condition true */
	pthread_cond_signal(&cond);
	pthread_mutex_unlock(&mutex);
```


### Program that Runs Two Threads in Turn

```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>
#include <pthread.h>

pthread_mutex_t mutex_a = PTHREAD_MUTEX_INITIALIZER;
pthread_mutex_t mutex_b = PTHREAD_MUTEX_INITIALIZER;

pthread_cond_t cond_a = PTHREAD_COND_INITIALIZER;
pthread_cond_t cond_b = PTHREAD_COND_INITIALIZER;

bool is_round_of_a = false;
bool is_round_of_b = false;

void *print_a(void *arg)
{
	while (1) {
		pthread_mutex_lock(&mutex_a);
		/* If thread_c has already sent the signal which means
		 * is_round_of_a is also already set to true by thread_c,
		 * thread_a just does not wait for the signal anymore. */
		while (!is_round_of_a)
			pthread_cond_wait(&cond_a, &mutex_a);
		printf("a\n");
		is_round_of_a = false;
		pthread_mutex_unlock(&mutex_a);

		/* send signal to thread_b and set is_round_of_b to true */
		pthread_mutex_lock(&mutex_b);
		pthread_cond_signal(&cond_b);
		is_round_of_b = true;
		pthread_mutex_unlock(&mutex_b);
	}

	return (void *)0;
}

void *print_b(void *arg)
{
	while (1) {
		pthread_mutex_lock(&mutex_b);
		/* If thread_a has already sent the signal which means
		 * is_round_of_b is also already set to true by thread_a,
		 * thread_b just does not wait for the signal anymore. */
		while (!is_round_of_b)
			pthread_cond_wait(&cond_b, &mutex_b);
		printf("b\n");
		is_round_of_b = false;
		pthread_mutex_unlock(&mutex_b);

		/* send signal to thread_a and set is_round_of_a to true */
		pthread_mutex_lock(&mutex_a);
		pthread_cond_signal(&cond_a);
		is_round_of_a = true;
		pthread_mutex_unlock(&mutex_a);
	}

	return (void *)0;
}

int main(int argc, char **argv)
{
	pthread_t tid_a, tid_b;

	pthread_create(&tid_a, NULL, print_a, NULL);
	pthread_create(&tid_b, NULL, print_b, NULL);

	/* At this point, two threads are all waiting to be woken up.
	 * We let main send a signal and wake up thread_a to run first */
	pthread_mutex_lock(&mutex_a);
	pthread_cond_signal(&cond_a);
	is_round_of_a = true;
	pthread_mutex_unlock(&mutex_a);

	/* Waiting for two threads to complete.
	 * Actually we will never reach here since two threads are all
	 * executing in an infinite loop. */
	pthread_join(tid_a, NULL);
	pthread_join(tid_b, NULL);

	return 0;
}
```


### Compile and Run

```
$ gcc ab.c -lpthread
$ ./a.out
a
b
a
b
a
b
a
...
```


### References
[1] <http://stackoverflow.com/questions/16522858/understanding-of-pthread-cond-wait-and-pthread-cond-signal>  
[2] <http://linux.die.net/man/3/pthread_cond_wait>  
[3] <http://www.ibm.com/developerworks/library/l-posix1>  
[4] <http://www.ibm.com/developerworks/library/l-posix2>  
[5] <http://www.ibm.com/developerworks/library/l-posix3>
