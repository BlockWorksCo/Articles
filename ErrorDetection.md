---
title: "Error Detection"
date: 2019-01-24T22:27:15Z
draft: false
---

How can we classify the various types of error that occur in embedded systems?

* Errors in externally-sourced data (e.g. corrupt input)
* Errors in internally-generated data, i.e internal state (e.g. stack/RAM corruption, RAM starvation).
* Errors in behaviour (e.g. incomplete state machine, spurious interrupts)
* Errors in hardware (e.g. clock running too fast, GPIO stuck).

Why classify them?
Each different type can be handled in a manner thats more appropriate.


# Errors in externally-sourced data.
Can be caught by validation checks
Need to be complete. *must* strictly control our input.
One of two recovery options, "silent drop" or "log and continue".

* Out of range dates in input fields.
* CRC errors.
* Unhandled message type.
* Text exceeds maximum length.
* Wrong password or data decrypt failed.

# Errors in internally-generated data
This is the internal state of the application, and tends to be contained on the stack or in global variables.
The implication here is that the software itself has corrupted its internal state and hence cannot
recover.
Unsafe to continue, must reset to restore a valid internal state.

* State machine variable of an unhandled type (i.e. default case in a switch).
* Bad pointers (strict pointer checks).
* Stack-check failure.
* Failure to 'give' a semaphore.


# Errors in behaviour
These tend to be the incorrect behaviour of the application, 'designed-in'.
Unrecoverable, must log and reset.

* Memory allocation failure.
* Hard-fault (or double-fault, memmanage fault, etc).
* Interrupt rate too-high (CPU starvation).
* Deadlock between tasks.


# Errors in hardware
Requires application-level recovery.
Application can invoke particular behaviours to attempt recovery.
Likely go into a 'safe mode'.

* FLASH sector erases can be recovered with more erase cycles.
* RTC failure can be set repeatedly by communication with time server.
* Modem goes mmute, maybe attempt to use reset line to recover it before retry.


# Chains of errors
One type of error can trigger another.
Should ideally be detected at first error.

For example, a


