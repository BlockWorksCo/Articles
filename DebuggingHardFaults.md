---
title: "Debugging Hard Faults on the Cortex-M"
date: 2019-03-01T22:05:50Z
draft: false
---

A lot of developers are afraid of hard-faults or exceptions happening in
their software, but they don't need to be. Quite often, (though not always)
they can be relatively easy problems to solve precisely because the hardware
has frozen the machine state at the exact point the problem occurred.


What do we mean by machine-state?

The Cortex-M architecture has two stacks, one for normal user-mode execution
and the other for supervisor & exception mode.
When an exception handler executes, for example during a hard-fault condtion
then what you see in the debuggers call stack does not seem to make sense.

What you need to do in order to fix the fault is to obtain the call stack
for the user-mode stack.
Here, I'm going to demonstrate an easy method for doing exactly that.

_Note that although we're looking specifically at a Cortex-M here, the technique
is broadly applicable to other architectures._

{{< highlight cpp "" >}}

void HardFault_Handler()
{
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
}

{{< / highlight >}}

With a hard-fault handler looking like the above, what we're going to do is to
place a breakpoint on the 'while' loop line.

When the debugger stops at this breakpoint we can now see that the stack trace
looks nothing like what we expect.

{{< highlight cpp "" >}}

stack trace from hard-fault handler.

{{< / highlight >}}

Now, let the debugger go a few times around this loop by letting it continue
for a few cycles. This is done to ensure that the line above (the variable initialisation)
has actually completed.

We're nearly there... now, set the loop variable "returnFromHere" to true. and continue to
step using the debugger.

The aim now is to step just enough so the processor returns from the handler and switches 
the stack back to the user mode.
When this happens, look at the call stack again and voila... the processor has stopped
at exactly the point the crash occured.

{{< highlight cpp "" >}}

stack trace from application.

{{< / highlight >}}

From here, the entire machine-state is open to our view. We can walk up and down the stack
examining local and global variables, look at the state of semaphores, mutexes, queues 
and threads, etc. 

In short, it should now be possible to track your bug down successfully.






