---
title: "The right amount of abstraction"
date: 2019-01-27T08:19:18Z
draft: false
---


Abstractions should provide value for the business, because they typically harm the readbility of the codebase.

* Will it make future code easier to write?
* Does it provide flexibility  in a place where flexibility is needed (i.e. *no one* ever replaces the database).


# What is commonly understood?

* Sequences
* Grouping


"All problems in computer science can be solved by another level of indirection, except for the problem of too many layers of indirection." – David J. Wheeler

Frameworks vs libraries
Look behind the API, understand how its working.


When making a suit, a good Tailor will always leave a small amount of fabric at strategic locations within the garment to allow the garment to be taken in, or out, without changing its overall shape or structure.
Good Tailor's don't leave reams of fabric at each seam just incase you happen to grow a third arm, or become pregnant.

# Summary:

* Since all abstractions are leaky, you have to choose to put a leak in your code.
* "Low-level programming is good for the soul" John Carmack
* “If we wish to count lines of code, we should not regard them as ‘lines produced’ but as ‘lines spent.'”— Edsger Dijkstra17.
* “Programming can be fun, so can cryptography; however they should not be combined.”— Kreitzberg and Shneiderman 
* All abstraction are leaky.
* Overuse of DRY.
* A good abstraction will reduce LOC, not increase them.
* Read code! Code is written once, but read *many* times.
* Callbacks hinder readbility Shouldn't need a debugger to follow code or discover code paths.
* Read code! Code is written once, but read *many* times.
* Naming is important.
* Read code! Code is written once, but read *many* times.
* Duplication is better than the wrong abstraction.
* JIT abstraction. Wait until you need it.
* YAGNI
* "Hell is someone else's abstraction." -Anonymous
* Sequences are the easiest code to follow.


# Required reading:

* [Architecture of open source applications](http://aosabook.org/en/index.html)
* [The law of leaky abstractions](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/)

