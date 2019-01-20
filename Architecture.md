---
title: "Architecture"
date: 2019-01-20T19:14:13Z
draft: false
---


Architecture
============
- Assumptions for embedded-code. one-use, running forever (barring upgrades and resets), limited resources (RAM, performance, power, space).
- static. Take advantage of the embedded-assumptions and make everything you can, static. the compiler will be your friend here.
- reduce decisions. Anything decided at runtime that will never change is wasted energy and has potential for exploitation.
- code-driven, not data-driven. Data changes, code is static. therefore data is more vulnerable.
- determinism (response, error recovery, resource usage). Being able to predict patterns in the software means mitigations are much more successful. Code is also clarified.
- reduce concurrency. Use concurrency where needed, i.e. at asychronous interface points. This doesn't necessarily mean tasks; interrupts are better.
- Plan for resets.
- Decouple, simplify, separation-of-concerns, encapsulation; all these help clarify the software and allow a better mental-model to be kept in the programmers head.

