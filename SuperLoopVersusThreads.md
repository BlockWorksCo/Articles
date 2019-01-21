---
title: "Super-loop vs threads"
date: 2019-01-21T19:07:00Z
draft: false
tags: ["superloop","threads","tasks","concurrency"]
---

A pattern that has mostly fallen out of fashion these days is the "super loop".


Consider a system that has numerous operations to perform. These operations are notionally
decoupled and so can be run concurrently.

The two patterns we are considering here are:

# Super loop

~~~~~~~~
int main()
{
  while(true)
  {
      enterLowPowerMode();
      checkConditionOne();
      checkConditionTwo();
  }
}
~~~~~~~~


# Threads

~~~~~~~~
int threadOne()
{
    while(true)
    {
        checkConditionOne();
    }
}

int threadTwo()
{
    while(true)
    {
        checkConditionTwo();
    }
}

int main()
{
    startThread(threadOne);
    startThread(threadTwo);
    while(true)
    {
        enterLowPowerMode();
    }
}
~~~~~~~~

Both the above snippets logically do the same thing with different runtime behaviours.
The threaded one will attempt to perform both actions (notionally) simultaneously.
The superloop version attempts to perform the actions sequentially and repeatedly.

The threaded one (to be correct) would require:

* Extra RAM for the stacks.
* Extra CPU overhead for synchronisation and context-switching.
* Extra RAM and initialisation overhead for synchronisation primitives (mutexes, queues).
* Extra RAM and code for OS overhead.
* Overhead of managing the above in the codebase.

Remember that the typical embedded processor this code will be running on will be a single-core with
limited RAM.

Presuming that both the above snippets will meet the timing requirements of the system, the 
superloop version:
- Has less RAM usage.
- Is more energy efficient.
- Deterministic behaviour.
- No synchronisation overhead (due to deterministic behaviour).
- Simpler.



