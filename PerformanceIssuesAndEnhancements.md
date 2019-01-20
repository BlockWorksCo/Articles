---
title: "Performance Issues and Enhancements"
date: 2019-01-20T19:14:13Z
draft: false
---


Performance issues and enhancements.
====================================
- overhead of instrumentation.
- MPU enhancements
- Periodic checking.
- Cant check everything on every function entry/exit but can *randomly* choose which checks to perform. This 
decreases the load at the expense of detection-latency.
- Dont store anything persistently until a full check has been performed.

