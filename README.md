# Stack Canary Mitigations (PoC)

## Overview
This repository contains a low-level Proof-of-Concept (PoC) implemented in **pure x86_64 Assembly** to demonstrate the mechanics of **Stack Canary** protection. It simulates how modern compilers and operating systems inject guard tokens into the stack frame to detect and mitigate buffer overflow vulnerabilities before execution flow subversion can occur.

> **Note:** This project serves as a localized microarchitectural simulation to study memory mitigation strategies and structured execution safety.

## How it Works
The program establishes a protected stack frame by executing a simple validation sequence:
1. **Token Injection:** During function initialization, a specific hexadecimal token (the "Canary") is placed right before the return address on the stack.
2. **Buffer Simulation:** The program executes memory operations that safely simulate a standard buffer input layer.
3. **Integrity Validation:** Before the function epilogue pops the instruction pointer, the local token is compared against the master reference.
4. **Forensic Interception:** If the values mismatch (indicating memory corruption or an ongoing overflow attempt), the application aborts immediately with an error log to prevent execution redirection.

## Compilation and Assembly
To test and analyze this assembly implementation natively on Linux:

```bash
# Assemble the source layout
nasm -f elf64 clock_verifier.asm -o stack_canary.o

# Link the object module into a static binary
ld stack_canary.o -o canary_verifier

# Run the mitigation binary
./canary_verifier
```

## Security Scope
- Buffer Overflow Analysis & Mitigation Mechanics.
- Stack Frame Layout Auditing (x86_64 Calling Conventions).
- Memory Smashing Protections (SSP).
