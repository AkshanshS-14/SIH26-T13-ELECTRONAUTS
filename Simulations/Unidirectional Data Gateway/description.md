# Layer 3: Physical Data Diode & Protocol Isolation

Layer 3 provides a hardware-enforced, unidirectional communication boundary. By utilizing a physical Schottky diode circuit instead of a software firewall, it guarantees that server logs egress safely while completely preventing reverse signal ingress.

---

## Circuit Schematic & Waveform Analysis

<p align="center">
  <img width="1919" height="958" alt="Screenshot 2026-08-20 114536" src="https://github.com/user-attachments/assets/449064d2-938d-458e-b448-a37d227b9f29" />
</p>

> **Simulation Analysis:**  
> * **Green Trace (`V(server_tx)`):** Represents the 5V pulse signal from the internal server TX line. Output is physically isolated on ingress.  
> * **Magenta Trace (`V(logger_rx)`):** Signal received at the logger node. The slight drop (~0.3V) reflects the low forward voltage drop of the BAT54 Schottky diode while completely blocking reverse path signals.

---

## Circuit Components

| Designator | Component Type | Value / Model | Function |
| :--- | :--- | :--- | :--- |
| **V1** | Pulse Voltage Source | `PULSE(0 5 0 1n 1n 0.5u 1u)` | Generates 5V digital transmit signal (1 MHz). |
| **D1** | Schottky Diode | **BAT54** | Enforces unidirectional current flow with sub-nanosecond switching speed. |
| **R1** | Pull-down Resistor | **10kΩ** | Holds line at 0V logic LOW during idle states and drains residual charge. |

---

##  Operational Mechanics

* **Forward Egress (Logic HIGH - 5V):** When `SERVER_TX` drives 5V, diode **D1 (BAT54)** turns ON (forward-biased). Current flows through D1 and across the **10kΩ R1** resistor to Ground, producing a clean **~4.7V logic HIGH** at `LOGGER_RX`.
* **Idle State (Logic LOW - 0V):** When `SERVER_TX` drops to 0V, D1 turns OFF. **R1** drains any line capacitance, pulling `LOGGER_RX` locked down to 0V.
* **Reverse Attack Defense:** Any attempt to inject an external command signal from `LOGGER_RX` back toward the server places **D1** in reverse bias. The diode acts as an open circuit, physically blocking ingress and routing external current through **R1** safely to Ground.

---

##  Electrical Specifications

* **Forward Voltage Drop ($V_F$):** ~0.3V (Schottky technology minimizes power loss and signal degradation).
* **Reverse Recovery Time ($t_{rr}$):** < 5 ns (Ideal for high-speed UART / SPI data protocols).
* **Transient Analysis Setup:** `.tran 5u`.
