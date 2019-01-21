---
title: "Ageing (wear & tear)"
date: 2019-01-20T19:14:13Z
draft: false
---

Embedded systems have a very close relationship with the hardware they're running on.
Often, they're directly controlling or monitoring devices

# Wear and Tear.
- Bad block management. Managing deficiencies in devices.
- Wear levelling needed to preempt issues with particular technologies.
- Fragmentation. Generic data structures not behaving well in bad situations.
- batteries dying without warning, failing to charge, overheating, failing completely.
- Clock drift causing clocks to be less accurate and variable.
- Mechanical issues; connectors bouncing and bad connections causing spikes, noise an intermittent connections

# Data storage related aging issues

The typical type of device on which to store data in an embedded system is NOR FLASH. This type of device
has particular failure modes, the most common of which is failure to erase.
A NOR FLASH whem erased erases all bits to '1'. This results in a read from an erased sector returning all
'0xff' bytes.
Write operations can set the individual bits to a zero (once). In order to return the bit to a '1', the whole
sector must be erased.

This process is not entirely reliable, manufacturers will typically guarantee between 10K and 100K erase 
operations.
The typical failure mode for NOR FLASH to fail in is erase-failure. The erase operation will take longer
and longer to complete until the point at which a threshold is crossed and a failure is flagged.
It is sometimes possible to continue to use the sector but repeated erase operations will now have to 
be performed by the application-level code, i.e. its forced back into our court rather than being the responsibility
of the FLASH device.

Given each sector has a limited number of erase-cycles, it makes sense to spread the wear (erase-cycles) across
the whole population of sectors evenly.

This is 'wear levelling'.

When blocks do go bad, we must make note to ourselves not to attempt to use that particular sector for 
storage and to find another instear.

This is 'bad block management'.

'Fragmentation' is an effect that causes a failure to write to the storage purely because of a suboptimal
pattern of writes to the device. This is really a filesystem problem which can generally be avoided by using
more appropriate (application-specific) storage structures.
Filesystems are too general, complex and inefficient for (small) embedded systems and should not be used.


# Battery life

Battery life is important to predict to allow sufficient time for a replacement to be scheduled and 
organised in order to maintain functionality.

The problem is that the decay-curve of some battery technologies (NiMH?) stays relatively flat before falling
rapidly. This means we will have difficulty measuring the remaining capacity of the battery.
Instead we can in certain conditions accumulate the estimated consumption for the operations we're performing.
This will naturally be more inaccurate but may provide a more grudualand useful estimation as the battery ages.





