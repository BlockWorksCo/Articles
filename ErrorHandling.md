

Error Handling and recovery.
============================
This is an important subject for any software system. No one ever intentionally designs their software to be unreliable or insecure but it does happen, despite our best intentions. Therefore we *must* think carefully about how we design in the ability to fail.

First and foremost, we must acknowledge that our one subsystem is not isolated. It exists within an ecosystem. This larger connected system must be taken into account when considering failures.

You cannot discuss error handling without also discussing recovery. Detecting that an error has happened and not attempting recovery does not a fault tolerant system make.

## Definition of an error.
A condition outside of the designed-for program logic.

If your system as a whole (not just the software) can *truly* contain and accept the condition, the let it do so. All other situations are not acceptable and indicate undefined program-state (either malicious or accidental).


### Assertions
An assertion that fails yet does not stop the system is then creating undefined state. Therefore assertions *must* stop the system.
This is the exact same behaviour as errors. therefore assertion === error. In effect an assertion declares that the following logic can handle values in the given range.

Failure to supply that code with values in that range results in undefined behaviour.

See also: Design by contract.


## What is different about embedded systems?
Embedded systems have a few qualities that mean the techniques for handling errors should be different to those on server or desktop systems:
- They don't have users, i.e. they're headless and mostly unmonitored. In contrast to servers which are headless and monitored and also desktop systems which lack both of those features.
- They need to be robust to failure and gracefully handle issues *without* corrupting data which would prevent a correct restart.
- Often, power usage is critical, i.e. in battery powered and mobile systems.
- Determinism is often critical; for time and behaviour. It is not acceptable to be "unsure" about how a system will respond.
- They are often connected to (and reliant on) physical systems which fail in transient and unpredictable ways.
- Error detection on its own is *not* sufficient. Error recovery is necessary.

Driving design factors:
- System (BOM) Cost.
- Development cost.
- Reliability.
- Security.
- Robustness.
- Error handling increases complexity and therefore code size.

Derived from the above:
- Cost drives code-size down
- Cost drives complexity down.
- Reliability drives complexity down
-

Regarding the issue of handling errors in the traditional way (defensively):


- Cost drives code size.
- Cost drives resource sizing.


undefined state

The embedded software is one-part of the larger system. In order to "handle" errors correctly, you *must* do so within the context of the larger system, i.e. not just your device, but anything that is connected to it via wired or wireless links that is assuming some state in your device or subsytem.

## Consider Erlang...
As an example of this mentality is in the telecoms systems written in Erlang. These systems are among the most reliable ever built, yet they embody some aspects that most software engineers would consider anathema.
"Let it crash" mentality.

"Avoid defensive programming".

"Dont keep track of the wreckage".

...yet some of the worlds most reliable systems are build using Erlang.

This goes against the grain for a log of engineers

"If a hardware failure requires any immediate administrative action, the service simply won’t scale cost-effectively and reliably. The entire service must be capable of surviving failure without human administrative interaction. Failure recovery must be a very simple path and that path must be tested frequently. Armando Fox of Stanford has argued that the best way to test the failure path is never to shut the service down normally. Just hard-fail it. This sounds counter-intuitive, but if the failure paths aren’t frequently used, they won’t work when needed."
(James Hamilton https://www.usenix.org/legacy/event/lisa07/tech/full_papers/hamilton/hamilton_html/)

## Strategies

### Best effort

### Integrity preservation


### Time constrained
- Design for failure.
- fail fast
- resetting is your friend.
- when not to use this (short time constraints).
- Supervisor/watchdog.
- Detection & Recovery.
- Corruption prevention
- Corrupted state.
- 2nd, 3rd and 4th order effects.
- Erlang-style.


"You get the paradox of where you are thinking about errors and failures everywhere with the goal of actually handling them in as few places as possible."


## Communicating fault information across a reset boundary.
storage:
- Battery backed SRAM?
- Registers?
- FLASH?
