---
title: "Error recovery in embedded systems"
date: 2019-01-22T22:26:21Z
draft: false
---

Considering error conditions in a single-purpose, embedded system:

- No user interaction is allowed, therefore recovery needs to be automatic.
- Worst case: software is not running, so hardware watchdog fallback is necessary.
- Hardware watchdog will reset processor, therefore the reset cycle needs to be factored in as a normal
recovery mechanism.
- During development, we can turn off watchdog and busy-wait allowing an opportunity for a debugger to attach. This
preserves processor state (stacks & RAM contents) for examination.
- Boot time must be minimised as this is now a normal recovery mechanism.
- Low-power wake-cycle is also a natural reset-cycle and also requires boot time to be minimised.


# Live recovery
Recovery using normal program control flow (i.e. without resets).


# Reset recovery.
Recovery using the reset mechanism to clear bad state. This is performed by the PANIC call.
Even though we are resetting, we *must not* lose track of the fact that we had an unhandled 
condition.
This could be logged relatively easily by 

Why is this a better option?

* Coming to a correct application-state from a reset is a necessity as we have to startup at some point.
* If we can recover from one state (the reset-state), then why should we attempt live-recovery? staying live
does not gain us anything.
* There is effectively an infinity of possible bad internal states we have to recover from. It is not
feasible to attempt to craft a recovery path from all of them; so don't. Just call PANIC.

What does our PANIC handler look like?

~~~~~~~~~~~~~~
void PANIC( const char* message, 
            uint32_t messageLength)
{
    // Don't let anything else run.
    disableInterrupts();

    // Log message for startup routine to output
    memcpy(&noInitMessage, message, messageLength);

    // Don't allow the compiler to optimise out 
    // the return code.
    volatile bool   resumeFlag  = false;

    // Sit here waiting for user attention.
    // - In release mode, the watchdog will kick 
    // in here.
    // - In debug mode, we can attach with the 
    // debugger.
    while( resumeFlag == false )
    {
        flashLED();
    }
}
~~~~~~~~~~~~~~

The intention is that the above code will be called from any assert failures or exception handlers
where live-recovery is not an option.

Normalisation of the reset path seems antithetical to good software development practice, but in fact it is not.
There are numerous benefits:

* Single recovery path. Greatly reduces the amount of code related to recovering from awkward situations.
* That single recovery path is the existing one that is *always* required anyway(start-up)
* Low-power systems *should be* normalising this path anyway as a result of their normal wake-cycle.


* Sometimes, resets are out of our control (i.e. cosmic rays corrupting RAM, power-failures, etc).
* resets may be due to bad internal-state. We cannot recover from this, neitherwe cannot allow program-execution to continue
log resets


What we're saying is that we treat reset as a normal part of program control flow. We can perform resets
as a result of:

* Power cycles.
* Low-power wake-cycles.
* Exceptions.
* Bad internal-state.

# How do we normalise the reset-path?
Treating reset as a normal code-path means making everything robust. This particularly means:

- Peripheral state, i.e. floating GPIOs. Most processors will "float" their GPIOs during their reset period. This may have unintended
consequences on the devices attached to them. This needs to be taken into account by the hardware design.
- Storage-state i.e. FLASH storage schemes. This is persistent state that crosses the reset-boundary.
- RAM sanitisation. Rubbish will be left in RAM, make sure we clear it out.

This is *not* extra work that you have to do. This work was *always* required but was ignored due to the reset-path
being unused (most of the time).


# Debugging a fault

* Call PANIC.
* Attach debugger.
* Pause processor.
* Examine stacktrace(s) for both supervisor and user mode as appropriate.
* Examine key variables by obtaining addresses from ".elf" file.
* Potentially return fromm the PANIC by setting the "resumeFlag" and stepping
back into the faulting-code.

# Effect on the codebase

## Live recovery
~~~~~~~~
bool myFoo(int input)
{
    if(input > 0)
    {
        if(input < 100)
        {
            if(theData != NULL)
            {
                if(theCallback != NULL)
                {
                    theCallback( theData );
                    return true;
                }
                else
                {
                    maybeCallADefault();
                    return false;
                }
            }
            else
            {
                tryAndRecover();
                return false;
            }
        }
        else
        {
            someRecoveryAction();
            return false;
        }
    }
    else
    {
        anotherRecoveryAction();
        return false;
    }
}
~~~~~~~~

## Reset-recovery
~~~~~~~~
void myFoo(int input)
{
    PANIC_IF( input<0, "input too small!" );
    PANIC_IF( input>=100, "input too great!" );
    PANIC_IF( theData == NULL, "data not set!" );
    PANIC_IF( theCallback == NULL, "fn not set");

    theCallback( theData );
}
~~~~~~~~



  _*Reset is your friend!*_
