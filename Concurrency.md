---
title: "Concurrency and threads"
date: 2019-01-21T20:18:15Z
showDate: false
draft: false
tags: ["threads","tasks","concurrency"]
---

Threads are not 'free'.
Threads come with significant cognitive overhead and management complexity.


# Overheads of threading
* RAM usage.
* Context switching.
* Concurrency management (semaphore, mutexes, queues, etc).
* Startup and shutdown overhead (synchronisation).

# Good reasons to use threads
* When dealing with a truly asynchronous source of events (i.e. from an external device).
* When the workload is such that a single CPU cannot complete the operation by the deadline.
* To perform the oeprations of a lower-half of an interrupt handler.


# Bad reasons to use threads
* To make a module decoupled and standalone.
* For "securiity", i.e. to provide a supposed element of separation.
* Every module should have a thread (active objects).
* I like threads (seriously).


In general, many embedded systems are overly-threaded. This leads to inefficient, bloated and
overly-dynamic complex code.
Concurrency is quite often a form of "accidental complexity", i.e. complexity that is not inherent
to the problem we're solving, but rather complexity we have added ourselves due to our
design.

There are valid reasons for using threads, for example when interfacing a software-system to
the outside world. Interactions of this form are truly asynchronous and should have threads 
associate with them.
This leads to a common pattern where the input/sensor devices have threads driving the input through
the system to some output.

