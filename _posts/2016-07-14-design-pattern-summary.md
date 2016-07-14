---
author: Xiaojie Yuan
layout: post
category: kernel
title: "[Design Pattern] Summary"
date: 2016-07-14
---

### Singleton

* private constructor

        class Foo {
        private:
            Foo();
        }

* a get_xxx() method looks like:

        static Foo *g_foo = NULL;
        Foo *get_foo() {
            if (!g_foo)
                g_foo = create_foo();
            return g_foo;
        }


### Factory Method

* also known as 'Virtual Constructor'
* delegate class instantiation to subclass

        +----------------+            +-----------------+
        |    Product     |            |    Creator      |
        +----------------+            +-----------------+
                A                     |                 |
                |                     +-----------------+
                |                     |+factoryMethod() |
                |                     +-----------------+
                |                              A
                |                              |
                |                              |
        +-----------------+           +-----------------+
        | ConcreteProduct |<-------<x>| ConcreteCreator |
        +-----------------+           +-----------------+
                                      |                 |
                                      +-----------------+
                                      |+factoryMethod() |
                                      +-----------------+


### Observer

* also known as 'Publish Subcribe'
* 'MVC' is a variant of Observer pattern?
* when 'publisher' changes state, all registered 'observer's are notified and updated automatically


### Decorator
* also known as 'Wrapper'
* an alternative to subclassing for extending functionality
* an example of Python decorator:

        def bar_decorator():
            def wrapper(func):
                print 'bar'
                func();
                print 'bar'
            return wrapper
        
        @bar_decorator
        def foo():
            print 'foo'

### References
[1] "Design Patterns: Elements of Reusable Object-Oriented Software"
