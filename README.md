# QuadShield: Hardware-Enforced 4-Layer Cyber Security System

**Team ID:** SIH26-T13  
**Team Name:** Electronauts  
**Hackathon:** Smart India Hackathon (SIH)  
**Theme:** Blockchain & Cybersecurity  

---

##  Project Overview

**QuadShield** is a 4-layer hardware-backed zero-trust security architecture designed to prevent unauthorized physical access, fault injection attacks, remote command execution, and data breaches. By leveraging the ultra-fast switching properties, low forward voltage drop ($V_F \approx 0.3\text{V}$), and near-zero reverse recovery times of Schottky diodes, QuadShield enforces physical security barriers that cannot be bypassed via software exploits.

---

##  Architecture & 4-Layer Defense

* **Layer 1: Physical Isolation (Optical Li-Fi Link)** — Replaces vulnerable RF/Wi-Fi signals with localized, line-of-sight optical data communication. Uses high-speed Schottky photodiodes to prevent wireless eavesdropping outside secure room walls.
* **Layer 2: Port Transient Clamping (Anti-Glitch Shield)** — Protects server physical data lines (USB/Serial). Dual Schottky clamping diode arrays instantly shunt malicious high-voltage spikes ($20\text{V}+$ fault injection) safely to Ground and $V_{CC}$ within nanoseconds.
* **Layer 3: Unidirectional Data Gateway (Hardware Data Diode)** — Enforces strict physical one-way data flow using diode-level logic gates. Transaction logs flow out to public ledgers, while inbound execution commands are physically blocked.
* **Layer 4: Chassis Defense (Tamper-Evident Zeroization)** — Hardware-interrupt logic linked to a physical chassis switch. Triggers immediate memory key-wiping (zeroization) upon physical box intrusion.

---

##  Repository Structure & Branches

The repository is structured across multiple dedicated branches to streamline development, documentation, and evaluation:

```text
├── main                            # Master branch containing final code and project overview
├── simulation/                     # Primary simulation directory containing folder modules
│   ├── chassis-defense/            # Layer 4: Microcontroller interrupt & zeroization logic
│   ├── physical-isolation/         # Layer 1: High-speed Li-Fi optical receiver models
│   ├── port-transient-clamping/    # Layer 2: LTspice schematic, wave plots, & clamping demo
│   └── unidirectional-data-gateway/ # Layer 3: One-way hardware data diode implementation
├── UI/UX                           # User Interface & monitoring dashboard assets
├── Port-Transient-Clamping         # Dedicated branch for Layer 2 development & testing
├── Unidirectional-data-gateway     # Dedicated branch for Layer 3 development & testing
├── chassis-defense                 # Dedicated branch for Layer 4 firmware & sensor development
├── presentation                    # PPT slides, architecture diagrams, and hackathon media
└── doc                             # Project documentation, datasheets, & math models
