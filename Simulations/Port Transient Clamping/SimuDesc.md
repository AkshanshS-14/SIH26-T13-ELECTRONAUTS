# Layer 2: Port Transient Clamping (Hardware Anti-Glitch Protection)

##  Overview
This directory contains the LTspice transient simulation for **Layer 2: Hardware Anti-Glitch Port Transient Clamping**. 

Physical data interfaces (USB, Serial, Ethernet) exposed on servers are vulnerable to **voltage glitching** and **fault injection attacks**. Attackers inject high-voltage, rapid-transient electrical spikes into data lines to corrupt memory states, induce instruction-skipping in processors, or physically degrade silicon. 

This circuit utilizes a dual-diode steering array featuring **Schottky diodes (BAT54)** to clamp high-voltage transients in nanoseconds, shunting overvoltage safely to $V_{CC}$ and negative surges to Ground before they reach sensitive downstream server microcontrollers.

---

##  Circuit Schematic & Topology

The clamping network is configured in parallel with the data bus line (`DATA_BUS`):

Data Input Line -[R1]-+---+---------------------> To Server MCU Pin
(V_pulse)      50Ω  |   |
[R_load] --- Schottky Diode 2 (U2)
10kΩ   / \  [Cathode to DATA_BUS, Anode to GND]
|    -----
GND     |
GND

### Component Roles:
* **`V_pulse`**: Voltage pulse generator modeling a high-voltage fault injection attack ($20\text{V}$ spike, $1\text{ns}$ rise time).
* **`R1` ($50\,\Omega$)**: Represents physical trace impedance and current-limiting resistance.
* **`R_load` ($10\,\text{k}\Omega$)**: Represents the high-input impedance of the server's input pin.
* **`S1` (BAT54 Schottky)**: Upper clamping diode. Forwards high-voltage spikes above $V_{CC} + V_F$ to the power rail.
* **`U2` (BAT54 Schottky)**: Lower clamping diode. Forwards negative transient spikes below $0\text{V} - V_F$ to Ground.

---

## Physics & Working Principle

Standard silicon diodes (e.g., 1N4007) rely on minority carriers, resulting in slow reverse recovery times ($t_{rr}$) that allow high-frequency voltage spikes to pass through untouched. 

Schottky diodes operate purely on **majority carriers** across a metal-semiconductor barrier, yielding:
1. **Near-Zero Reverse Recovery Time ($t_{rr} \approx \text{picoseconds}$)**: Turns on instantly when an overvoltage transient hits the bus.
2. **Low Forward Voltage Drop ($V_F \approx 0.3\text{V} - 0.5\text{V}$)**: Clamps voltage tightly to safe logic levels:
   $$\text{Max Upper Clamp} = V_{CC} + V_F \approx 3.3\text{V} + 0.5\text{V} = 3.8\text{V}$$
   $$\text{Min Lower Clamp} = \text{GND} - V_F \approx 0\text{V} - 0.5\text{V} = -0.5\text{V}$$

---

## Simulation Results

Running the transient analysis (`.tran 3u`) demonstrates complete threat neutralization:

| Signal Node | Waveform Description | Peak Voltage Measured | Clamping Status |
| :--- | :--- | :--- | :--- |
| **`V(n001)`** | Unprotected Raw Attack Spike | **$20.0\text{V}$** | Unclamped Threat |
| **`V(ata_bus)`** | Protected Output to Server Pin | **$\approx 3.8\text{V}$** | **Successfully Clamped** |

### Key Observation:
When the $20\text{V}$ pulse fires at $1.0\,\mu\text{s}$, `S1` forward-biases instantly. The surge current is diverted into the $V_{CC}$ rail, capping the node voltage safely at $\sim 3.8\text{V}$, well within the maximum absolute ratings of standard $3.3\text{V}$ / $5\text{V}$ microcontroller CMOS input buffers.

---

## How to Run the Simulation in LTspice

1. Download and install [LTspice](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html).
2. Clone this repository.

The circuit and Graph in LTSpice Software:

<img width="1361" height="567" alt="lt png" src="https://github.com/user-attachments/assets/a9011edb-c327-4f50-9b26-ee3473200db1" />
