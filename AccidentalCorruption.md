


Accidental corruption.
======================

Corruption can take many forms in a typical C/microcontroller software system, namely:

- stack overflow
    incorrect array bounds.
    incorrect sizing.
    stack pointer corruption.

- stack underflow
    incorrect array bounds.
    stack pointer corruption.

- stack corruption
    difficult to detect.
    canaries, -fstack-check

- heap corruption
    use-after-free
    dangling pointers from e.g. linked lists & trees.

- heap metadata corruption.
    use-after-free.

- Heap leaking.
    misplaced free.
    missing free.

- static buffer corruption.
    debugger & watchpoints
    CRC of buffer can provide detection.

- Pointer arithmetic.
    pointer checks.
    address range is well-defined.
    range is further narrowed by Domain Protection.

- Attempt to trap fault as close as possible to the original fault.
    instrumentation.
    dedicated and specific checking code.
    instruction tracing.
    write-buffering causing loss of precision in fault catching.

- Concurrency as a source of corruption.
    shared data.
    atomic access.
    multiprocessors & caching (invalidation, flushing and non-cached areas).
    interrupts.
    DMA transferring data into incorrect location.
    per-task heap? reduce contention & synchronisation overhead.

