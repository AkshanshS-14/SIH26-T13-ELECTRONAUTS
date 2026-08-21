# Hardware Chassis Zeroization Module (Layer 4 Defense)

A low-latency, hardware-driven zeroization routine designed to prevent cold-boot, bus-snooping, and physical extraction attacks on cryptographic servers.



## Overview

**Layer 4: Tamper-Evident Zeroization (Chassis Defense)** acts as an autonomous hardware kill-switch. When a physical chassis breach is detected via mechanical switches, optical sensors, or tamper meshes, the microcontroller executes an instant non-maskable interrupt (NMI) routine to destroy ephemeral master keys and session secrets stored in volatile memory.

```
+------------------+      Chassis Breach      +------------------------+
| Physical Switch  | -----------------------> | Non-Maskable Interrupt |
+------------------+                          +------------------------+
                                                          |
                                                          v
                                              +------------------------+
                                              | Volatile Key Zeroize   |
                                              | Bus Line Isolation     |
                                              | Microcontroller Lockout|
                                              +------------------------+
```




## Features

- **Microsecond Response:** Implements a naked ISR to wipe keys without function prologue delays.
- **Optimization-Proof Erasure:** Uses volatile memory barriers to prevent compiler dead-store elimination (`-O2`/`-O3` safe).
- **Fail-Secure Architecture:** Defaults to total key destruction to guarantee confidentiality over availability.
- **Hardware Isolation:** Asserts bus isolation signals to prevent logic analyzer inspection during breach events.



## Architecture & Operation

1. **Detection:** Chassis micro-switch triggers an active-low/high signal on a dedicated tamper GPIO pin.
2. **Interrupt:** The secure microcontroller catches the signal via a high-priority hardware interrupt.
3. **Crypto-Shredding:** Key structures in battery-backed SRAM are overwritten with zeros.
4. **Halt/Drain:** Triggers power-cut to volatile domains and halts CPU execution.

5. <img width="656" height="537" alt="image" src="https://github.com/user-attachments/assets/c0df59a1-4a96-4ccc-9f68-a2e1828e78bb" />




## Code Structure

```text
├── include/
│   └── zeroize.h          # Hardware pin mappings and vault definitions
├── src/
│   ├── main.c             # System initialization and loop
│   └── tamper_isr.c       # Low-latency zeroization ISR
├── docs/
│   └── threat_model.md    # Physical attack vectors and mitigation details
└── README.md
```



## Quick Start

### Prerequisites
- GCC cross-compiler for ARM Cortex-M / RISC-V (`arm-none-eabi-gcc`)
- Target hardware with battery-backed SRAM and dedicated tamper pins

### Build
```bash
make TARGET=ARM_CORTEX_M4
```



## Security Considerations

- **Power Independence:** Ensure the microcontroller is backed by a supercapacitor or coin cell to detect intrusions during unpowered transit.
- **Physical Meshes:** Best paired with continuous serpentine PCB traces on enclosure walls for drilling protection.




