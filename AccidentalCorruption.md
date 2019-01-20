---
title: "Accidental Corruption"
date: 2019-01-20T19:14:13Z
draft: false
---



# Accidental corruption.

Considering your typical C and microcontroller based embedded application, corruption can be caused in many ways.

Regardless of the root-cause, if corruption is detected, the only course of action is an immediate PANIC situation. Why do we PANIC? because we cannot now trust the machine state. At the point of detection we cannot know what else has been affected and for how long, therfore the only safe thing to do is PANIC our way back to safety.

Above all, we *must* avoid persisting any data to FLASH or EEPROM at this point as we cannot determine if it is still valid. If we have the facility to do so, we must mark any recently persisted data as untrustworthy to cause a rollback to a last-known-good state.

Without further ado, lets have a look at the common sources of RAM corruption.

## Stack overflow
This condition occurs when too much data is pushed onto the stack. Without an MPU/MMU to detect this siutation, the processor will not immediately raise an exception but instead will continue on blindly overwriting whatever data is immediately adjacent to the stack.
### Detection
Detecting a stack overflow situation is relatively easy as there are telltale signs, namely:
- The stack pointer is past the end of stack.
- The contents of the stack area are relatively random, indicating the stack has actually been used. Beware though that if the cause of the corruption is a large array or structure on the stack then this may appear to be empty space (filled with the stack-fill value).
This can be caught during runtime by instrumentation of the code or via checks at context-switch time. Relying on the OS to detect this at context switch time is less preferable to instrumented checking as it only checks periodically. Instrumented checks can catch the root-cause more easily though with more performance overhead.
### Causes & Resolutions
- The obvious cause here is incorrect sizing of the stack. Although it is tempting to simply increase the size of the stack, RAM is typically a limited resource in embedded systems and as such should be conserved. Instead, try to identify the reason that a large amount of stack space is being allocated on the stack and ask whether it really needs to be.
- Recursion; rarely is this intended and is unwise in an embedded system with limited stack space. Refactor the code to avoid recursion.
- Incorrect array indexing; if a local array is indexed by for example, a negative value then this will not be trapped. Instead the data that lies adjacent to the array will be corrupted. This can cause secondary effect such as stack pointer corruption, data corruption and return-address corruption.
- Always remember that all these effects can be secondary effects of other types of corruption, for example stack pointer corruption would potentially also place the stack pointer outside of the stack making it (incorrectly) appear as though there is too much data on the stack.

## Stack underflow
Similar to the stack overflow situation, but this is in the other direction, i.e. it appears that too much data has been taken *off* the stack. This results in the stack pointer ending up *below* the start of the stack.
The lack of an MPU/MMU means that the accesses *below* the stack are not trapped but instead are allowed to corrupt the data.
### Detection
This can be caught with at runtime with the same techniques as the overflow situation, namely instrumented code on entry/exit to functions and OS checking at context switch time.
### Causes & Resolutions
In contrast to the overflow situation, underflow cannot occur in the normal running of the application. This is because "pop" operations are always matched to "push" operations.
For this reason, it should be considered a secondary effect, i.e. the root cause will always be another issue such as incorrect indexed local arrays corrupting a link-register.


##  Stack data corruption
Although the underflow and overflow situations can be considered sstack corruption, here we are considering the data itself that resides on the stack rather than the stack metadata (SP and LR).

### Detection
The difficulty here is that the data on the stack can be modified in a perfectly legal and normal manner when passing data into a function by reference.
Assuming that the corruption is not malicious (and hence targetted), then implementing canary values in local data does have some chance of catching this situtation. Unfortunately, I know of no current compiler that will place canaries around local data automatically, leaving it up to the programmer to implement this manually.

This issue is inherently difficult (almost impossible) to detect, even with an MPU/MMU in larger systems.

### Causes & Resolutions
Limiting the use of pass-by-reference, possibly by coding standards and code review will go someway to making this more detectable in an automated way. An extended version of the Domain Protection system can enforce no passing-by-reference when crossing domains by using the MPU to make only the stack area for the current domain accessible. This is not a complete solution though.

Further complicating the matter is that this may also be a secondary level effect of another type of corruption.

## Heap data corruption
Data can also be corrupted while in the heap, but this is typically a secondary effect, i.e. the root cause is another problem such as incorrect pointer arithmetic or bad array indexing.

Since buffers allocated from the heap are not protected from each other in anyway whatsoever, they are susceptible to incorrect dereferencing in neighbouring buffers.

### Detection
There is no real telltale sign that heap data has been corrupted other than what you yourself can detect manually.

### Causes and resolutions
One mechanism to prevent heap issues is to prevent use of  pointers into the heap directly.

use-after-free can appear to corrupt data whereas the real cause is misuse of the heap.
Accessing the heap through functions rather than directly gives you a single point to verify accesses.
Language design can help here, for example C++ vectors are an improvement here due to bounds checking.

dangling pointers from e.g. linked lists & trees.

## heap metadata corruption.
    use-after-free.

## Heap leaking.
    misplaced free.
    missing free.

## static buffer corruption.
    debugger & watchpoints
    CRC of buffer can provide detection.

## Pointer arithmetic.
    pointer checks.
    address range is well-defined.
    range is further narrowed by Domain Protection.

## Attempt to trap fault as close as possible to the original fault.
    instrumentation.
    dedicated and specific checking code.
    instruction tracing.
    write-buffering causing loss of precision in fault catching.

## Concurrency as a source of corruption.
    shared data.
    atomic access.
    multiprocessors & caching (invalidation, flushing and non-cached areas).
    interrupts.
    DMA transferring data into incorrect location.
    per-task heap? reduce contention & synchronisation overhead.



# Detection mechanisms in detail.
Here, we will discuss the detail of the corruption detection mechanisms:

## Stack filling

## Code instrumentation for stack check.

## Manual canary values for local data.
