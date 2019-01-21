---
title: "LowPower"
date: 2019-01-21T17:30:54Z
showDate: true
draft: false
tags: ["low power"]
---

# Characteristics of low-power systems
- Periodic operation.


# Key points from a software point-of-view
- Mostly off
- Appropriate application model that allows a low-power mode of operation.
- Asunchronous APIs that allow slow response.
- Spend the most time possible in the deepest possible sleep mode (preferably powered-off).
- Start-up quickly
- Shutdown quickly
- Turn on as few devices as possible for the job at hand.
- Turn off the devices (or place them into low-power mode) as soon as the need for them is complete.
- Don’t transmit or receive if possible.
- Transmit in an efficient way avoiding collisions and retries.
- Don’t store data on FLASH is possible, FLASH erases are expensive in both power and time.
- Be deterministic in time and performance, to allow for battery-life to be estimated accurately.
- When communicating, communicate over as few hops as possible.
- Use appropriate data storage mechanisms (i.e. filesystems are generic, but far too complex and non-deterministic for a logging application).
- Software control over power-rails to devices (and appropriate segmentation of power-rails) is important to allow devices to be truly ‘off’.



# Examples
- Doorbell
- Watch
- Gas meter

