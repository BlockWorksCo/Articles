---
title: "Malicious Corruption"
date: 2019-01-20T19:14:13Z
draft: false
---


Malicious corruption.
=====================
- different from accidental corruption because of intent. Assume intelligence behind attacks.
- stack smashing. Code appears within a payload. negative indexing causes corruption of return value to cause a jump to code in payload.
- code corruption; dont store code in RAM if at all possible. code should be immutable if possible. XIP.
- vector table corruption. This is a potentially easy attack vector (overwrite a vector with own code, trigger the interrupt).

Mitigations.
============
- CRC the vector table, periodically check it. if paranoid, check it on entry to the ISR (in dev mode, this is bad for response/performance).
- Periodic (not onetime) code integrity check. Dont check it on startup then run forever.
