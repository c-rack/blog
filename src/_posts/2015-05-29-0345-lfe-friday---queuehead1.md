---
layout: post
title: "LFE Friday - queue:head/1"
description: ""
category: tutorials
tags: [lfe friday,lfe,erlang]
author: Robert Virding
---
{% include JB/setup %}
{% include LFEFriday/setup %}

Today’s LFE Friday continues to dig into the Okasaki API of Erlang's ``queue`` module, and take a look at [queue:head/1](http://erlang.org/doc/man/queue.html#head-1).

``queue:head/1`` takes a queue as it's first argument, and returns the first item in the queue.

```lisp
> (set queue (queue:from_list '(1 2 3 4 5)))
#((5 4) (1 2 3))
> (queue:head queue)
1
> queue
#((5 4) (1 2 3))
```

As we can see in the example above, the ``queue:head/1`` function does not modify the original queue at all, but just returns the first item.

Because ``queue:head/1`` only returns the value found at the head of the queue, and not a tagged tuple, it raises an error if we try to get the head item from an empty queue.

```lisp
> (set empty-queue (queue:new))
#(() ())
> (queue:head empty-queue)
exception error: empty
  in (: queue head #(() ()))
```

To be safe, and not get the error raised on an empty queue, the ``queue`` module also defines a function [queue:is_empty/1](http://erlang.org/doc/man/queue.html#is_empty-1) that you can use to check if a queue is empty.

```lisp
> (queue:is_empty empty-queue)
true
> (queue:is_empty queue)      
false
```

Like ``queue:cons/2``, and other functions of the Okasaki API, there is also a function [queue:daeh/1](http://erlang.org/doc/man/queue.html#daeh-1) (head backwards), to get the last item from the queue, as well as an alias for ``queue:daeh/1`` of [queue:last/1](http://erlang.org/doc/man/queue.html#last-1).

```lisp
> (queue:daeh queue)
5
> (queue:last queue)
5
```

Both ``queue:daeh/1`` and ``queue:last/1`` also raise an error of ``empty`` if you call them with an empty queue as the argument.

```lisp
> (queue:daeh empty-queue)
exception error: empty
  in (: queue get_r #(() ()))

> (queue:last empty-queue)
exception error: empty
  in (: queue get_r #(() ()))

```

And if we look at the error that is raised on ``queue:daeh/1`` and ``queue:last/1``, we see that the error is coming from [queue:get_r/1](http://erlang.org/doc/man/queue.html#get_r-1) from the Extended API.  If we look at the behavior of ``queue:get_r/1`` it looks like ``queue:last/1`` and ``queue:daeh/1`` are indeed just aliases for ``queue:get_r/1``.

```lisp
> (queue:get_r queue)     
5
> (queue:get_r empty-queue)
exception error: empty
  in (: queue get_r #(() ()))

> (queue:get_r queue)      
5
> queue
#((5 4) (1 2 3))

```

-Proctor, Robert
