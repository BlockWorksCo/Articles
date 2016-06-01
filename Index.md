

Articles Index


Robustness in embedded software
===============================

Embedded software is defined by:
- lack of human interaction.
- long, unattended lifetime.
- many units, built-to-a-cost, therefore resource constrained in RAM, storage and performance.
- often low power requirements.
- Often interacting with remote systems via messaging interfaces.
Implemented correctly, these techniques can provide significant improvements in robustness over unprotected code.


- Accidental corruption.
    stack overflow
    stack underlow
    stack corruption
    heap corruption
    heap metadata corruption

- Malicious corruption.
    stack smashing
    code corruption
    vector table

- Flooding attacks.
    interrupts.
    messaging interfaces.

- Wear and Tear.
    bad-block management
    wear levelling.
    fragmentation.

- Propagation prevention
    Even if one device is compromised, limit the damage so the exploit is widely useable.

- Defense in depth    
    layers
    admit that no one mechanism will catch everything.
    reduce chance of widespread exploit.

- Recovery
    Plan for reset.
    do it regularly, architect your system around it.
    test it.

- Error Handling
    fail fast
    resetting is your friend.
    when not to use this (short time constraints).
    Supervisor/watchdog.

- Architecture
    static.
    reduce decisions.
    code-driven, not data-driven.
    determinism (response, error recovery, resource usage).
    reduce concurrrency.

- Protection mechanisms
    address sanitiser
    stack checker
    strong pointer checking.
    Domain Protection
    random stack offsetting.
    stack return opcode check
    stack return address check
    vector table checking.
    heap bounds checking
    heap metadata checking.
    stack bounds checking with OS.
    code integrity & continuous partial checks.
    per-device RNG, unique ID for propagation prevention.
    Keep it static, XIP.
    Simulation, portability & unit and host-based testing.
    Build-time protection, static analysis.
    Message field validation, input sanitisation.
    Stack, heap and reset sanitisation.






