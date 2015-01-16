---
layout: post
title: "LFE Friday - erlang:apply/3"
description: ""
category: tutorials
tags: [lfe friday,lfe,erlang]
author: Robert Virding
---
{% include JB/setup %}
{% include LFEFriday/setup %}

Today's LFE Friday is on [erlang:apply/3](http://www.erlang.org/doc/man/erlang.html#apply-3).

With functional languages we love to pass functions around as the first class citizens that they are. But sometimes we don't know which function it is that we will need to invoke, causing us to be unsure of the arguments the function takes up front. If we knew, we could just invoke it as ``(funcall fun arg1 arg2 ... argn)``, but that doesn’t work if we could get different functions of varying arities. Enter ``erlang:apply/3``.

``erlang:apply/3`` takes the module name, the function name, and a list of the arguments to be passed to the function. The function passed to ``erlang:apply/3`` must also have been exported, otherwise an error will be raised.

```cl
> (apply 'lists 'max '((7 3 5 11 1)))
11
> (apply 'lists 'merge '((1 2 3) (a b c)))
(1 2 3 a b c)
```

The Erlang documentation points out that this should be used *only* when the number of arguments is not known at compile time. Otherwise we could just do the a normal function invocation, even if passed an anonymous function.

```cl
> (funcall #'lists:max/1 '(1 2 3 4))
4
```

The erlang module also includes a version ``erlang:apply/2`` that takes a function as it’s first argument, and a list of the arguments to be passed to the function as it’s second argument.

```cl
> (apply #'lists:merge/2 '((1 2 3) (a b c)))
(1 2 3 a b c)
```

While ``erlang:apply/2`` and ``erlang:apply/3`` will not be part of your common usage, there are cases where it is needed, like last weeks [timer:tc](http://blog.lfe.io/tutorials/2015/01/10/2201-lfe-friday---timertc3/). And though your usage of it will likely be rare, it is still good to know that you have it handy.

–Proctor, Robert
