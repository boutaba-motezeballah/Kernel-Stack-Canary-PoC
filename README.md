# Boutaba-Kernel-Stack-Canary-PoC v1.0

## Overview
**Boutaba-Kernel-Stack-Canary-PoC** is a low-level microarchitectural mitigation utility engineered in **Pure x86_64 Assembly** for Linux systems. The project serves as a safe Proof-of-Concept (PoC) illustrating custom Stack Canary injection and verification algorithms to proactively defend software runtimes against buffer overflow and stack-smashing exploits without relying on compiler-generated extensions.

---

## Architecture & Structural Execution Flow

The framework allocates a cryptographic sentinel token (Canary) directly on the local stack frame during initialization. Before returning from the active routine, the architecture re-evaluates the integrity of the token via hardware-level registers.

```mermaid
graph TD
    A[Function Prologue] --> B[Allocate Local Stack Frame]
    B --> C[Inject SECRET_CANARY at rbp-8]
    C --> D[Execute Local Payload Execution]
    D --> E[Function Epilogue Auditing]
    E --> F{Verify: Current Stack value == SECRET_CANARY?}
    F -->|True: Integrity Verified| G[Syscall 1: Print Safe Status]
    F -->|False: Attack Detected| H[Trigger Stack Smashing Defensive Routine]
    G --> I[Syscall 60: Exit Code 0]
    H --> J[Syscall 60: Core Dump Exit Code 139]

    style A fill:#1f1f1f,stroke:#333,stroke-width:2px,color:#fff
    style C fill:#0052cc,stroke:#333,stroke-width:2px,color:#fff
    style F fill:#ff5555,stroke:#333,stroke-width:2px,color:#fff
    style G fill:#00875a,stroke:#333,stroke-width:2px,color:#fff
    style H fill:#de350b,stroke:#333,stroke-width:2px,color:#fff
```

## Specifications
- **Language:** Pure Assembly (x86_64 ASM 100.0%)
- **Target Subsystem:** Ring 3 Process Space interacting with Linux Kernel ABI.
- **Security Scope:** Anti-Exploitation, Memory Integrity Protection.

---
*Developed and maintained by **Boutaba Motezeballah** — Systems Architect & Reverse Engineer.*
