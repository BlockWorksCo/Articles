---
title: "Low power software architecture"
date: 2019-01-21T17:30:54Z
showDate: false
draft: false
tags: ["low power"]
---

Many devices these days are battery powered. Being battery powered requires that we create software to make
the most possible use of the battery.
Software for low-power devices behaves very differently to normal software in a few regards:

* Spend most most of its time in low-power-mode (or even off).
* Startup very quickly.
* Shutdown very quickly.
* Devices must be off by default and only turned on while used.
* Power rails for devices must be under the control of the software.
* Communicate as infrequently as possible.
* Never busy-wait.
* Enter low-power-mode in the idle task.
* Interrupt driven.
* Store as little as possible (storage devices are power hungry).
* Store data in an appropriate manner (i.e. no filesystem).
* Likely super-loop driven rather than being multi-threaded.

# Architecting for low-power operation.
Bearing in mind the above points, what does the typical low-power-software look like?

* 99% of the time is spent in the deepest low-power-mode possible, generally with no RAM retention. This means
you're likely to be booting every time you want to do anything.
* Triggered to wake up by an interrupt, probably from an external device such as an RTC or a button generating an edge
on a GPIO.
* The startup code must first discover *why* it was woken up. From this it can determine the minimum amount of work that
needs to be performed. Note that this is *before* normal initialisation.
* Initialise devices and drivers as needed for the current mode.
* Perform business logic and respond to the event appropriate to the mode.
* Store any required data in the most appropriate place (RAM?, EEPROM?, FLASH?)
* Setup trigger events for the next wake-period.
* Enter low-power-mode or turn off.

# Application-level design for low-power systems

* Asynchronous communication; allow the device to respond in *its* own time, not the servers.
* Infrequent communication; Communication is generally power-hungry (i.e. radio)
* Communication should be driven from device, rather than from server.
* Event driven; software operations can be triggerred from timers, buttons, sensors, etc as well as internal events.
* Minimal storage; there are tradeoffs to discuss here regarding whether to store data or transfer it off-device. There
are tradeoffs aplenty here.
* Deterministic behaviour; Being predictable is important in order to be able to predict battery-life. This requires
a known current-draw from the system. Often, the charactersistics of batteries power-loss are not able to be monitored
so their life has to be predicted. This requires knowing *exactly* how the device behaves.
* Timeouts need to be short and appropriate. Think what condition you're trying to catch. You don't want to stay awake waiting for
an event that will never happen.


# Trade offs
* Storage versus communication.
* Deep-sleep versus startup/shutdown duration.
* Fine-grained vs large-grained wake-cycle.


# Radio communications.
* Appropriate data rate for application.
* Choice of protocol (ZigBee, 6LoWPAN, LoRAWAN, etc).
* Topology; mesh versus star.
* Routing complexity versus efficiency.


# Inappropriate common patterns.
* Filesystems; over-generic and complex.
* Concurrency; overhead of context-switching is wasted energy.
* Abstractions; every wasted instruction is wasted energy. For example, monitor the call-stack depth to identify over-abstracted code.

