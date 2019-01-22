---
title: "Characteristics of embedded systems"
date: 2019-01-22T22:28:11Z
draft: false
---

What is an embedded system? Lets have some examples:

* Electricity meter
* Watch
* TV
* Fridge
* Broadband router
* Washing machine
* Hearing aid
* Radar system
* Mobile base staotion
* HFT (High Frequency Trading) system.

# Possible characteristics
Different people have different definitions of what constitutes an embedded system.
They tend to imply one of the following aspects.

* Resource constrained?
* Low power?
* Cheap?
* mass produced?
* single purpose?
* No user interface (headless)?
* Real-time?

There is a difference here in the technical definition and the common use.
I would argue that for a strict technical definition, a sensible definition is that embedded systems are:

**_"An integral part of another system, single purpose and not exposed for general use."_**

This allows for example, a high throughput radar processing system to be classified as an embedded system
(which it is) or even the HFT example.
Where then is the distinction between those and a typical server which are generally headless, single-purpose
and quite often part of a larger system (of servers or networks)? These type of systems certainly aren't
resource-constrained (generally), cheap, real-time or low-power.

I would argue that the technically correct definition is far too broad to be useful

On the other hand, the common use definition of embedded systems is something like the following:

**_"Embedded systems are resource-constrained microcontroller based systems"_**

I think more useful phrases are:

* Resource-constrained systems.
* Cost-limited systems
* Headless systems.
* Single-purpose.

These could be combined, so you could for example classify a hearing aid as a "headless, resource-constrained 
system" whereas a radar system would be a "headless, single-purpose system".

These definitions get to the core of what is different about these types of system, they
imply different behaviours to "typical" software.

## Resource-constrained systems
Resources in this sense can be any (or all) of:

* Energy.
* RAM.
* Storage.
* Processing power.
* Communications throughput.

The implication is that a particular characteristic has been limited (intentionally or not).

Development challenges here are with regard to minimising the use of these resources which will
likely involve trade-offs with other desirable aspects.

## Cost-limited systems
Mass produced system are always built down to a cost. 
There is a direct correlation between the cost of a product and the profit made by the company.
This implies that software for these types of devices needs to make the most of all the available
resources (and no more) in order for the hardware cost to be minimmal.

The key here is (cost) optimisation for the system (not just software). Software performance 
optimisation will also likely be a factor.


## Headless systems
These are intended to run continuously without human (or non-human?) intervention.
There is no user-interface, all recovery *must* be performed autommatically or the system fails.

The emphasis in developing these type of systems is on robustness, recovery and the ability to run
continuously.


## Single-purpose systems
This implies that there need be no ability to run arbitrary (user-specified) software on the system.
This restriction allows greater optimisations to be performed such as:

* Reduction (or removal) of a general purpose Operating System.
* Specialised storage schemes instead of general purpose filesystems.
* Freedom to remove abstractions and allow the software to become more
tailored to the application.

---------------------------------

Of course, all the above can quite commonly be combined into a single system. these limitations
(and freedoms) are what make embedded systems interesting.
There are large opportunities for creative solutions to have a real impact.




