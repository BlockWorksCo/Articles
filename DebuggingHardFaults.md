---
title: "Debugging Hard Faults"
date: 2019-03-01T22:05:50Z
draft: false
---

A lot of developers are afraid of hard-faults or exceptions happening in
their software, but they don't need to be. Quite often, (though not always)
they can be relatively easy problems to solve precisely because the hardware
has frozen the machine state at the exact point the problem occurred.


What do we mean by machine-state?

two stacks

Cortex-M example bug other architectures are similar.

{{< highlight cpp "" >}}

void HardFault_Handler()
{
#ifdef DEBUG

   // In debug mode, put a breakpoint on here, 
   // disable the watchdog allowing you opportunity
   // to debug te problem.
   volatile bool   returnFromHere  = false;
   while( returnFromHere == false );

   // return from here by setting the flag to 
   // escape the loop
   // under the control of the debugger.
   // Returning will switch stacks back to the 
   // user-mode at which point you can examine the 
   // entire machine-state such as the call stack.

#else
   // Capture machine state

   // Store machine state in a 'noinit' area of RAM.

   // Sit in a tight loop, waiting for the watchdog 
   // to kick in.

   // On startup, check the 'noinit' area for stored
   // crash data and
   // send it as an event.
#endif
}

{{< / highlight >}}



