---
title: "Design for test"
date: 2019-01-21T22:24:08Z
draft: false
tags: ["testing", "ci"]
---

Embedded systems are by definition one part of a larger device. The purpose of the software
is not to stand alone but function within another device.
When testing, this cannot be ignored.

# Differences from hardware
The phrase "design for test" is typically applied to hardware, and to some extent this topic is a logical
extension from the hardware to the software.

When looking at hardware from a software (testing) point-of-view, there are some crticial differences:

* Hardware is not clean.
* Hardware is not reliable.
* Hardware is messy.
* Digital systems fail in analog ways.
* Zeros and ones can have other values.
* Hardware is expensive.
* RF is special kind of magic.

# So how do we test embedded systems?

Despite the above points, we still want and need to do the following:

* Run system-tests (end-to-end)
* Run module/unit tests

So, we can:

* Make the software portable (i.e. to build for the host as well as the target).
* Make the system resettable so we can clear out system state between tests.
* Don't depend on the speed of a particular processor (logging and debug overhead will hit you badly).
* Be flexible with communications options, try not to be proprietary.

# Outside-in testing

Testing is expensive.
Hardware *must* pass acceptance tests.
Therefore start off having automated acceptance tests. These can evolve into more general end-to-end
system tests.
Only when you have exhausted your suppply of end-to-end tests should you move inwards to do more 
focussed module and unit tests.

The thinking here is that the end-to-end tests will test everything else by implication, not with the
same degree of repeatability or control, but the functionality will be tested.

At the end of the day, the product is what matters.

