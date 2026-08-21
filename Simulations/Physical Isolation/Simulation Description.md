###  Firmware & Hardware Integration
This branch contains the core component development for the **Li-Fi Cybersecurity Gateway**, focusing specifically on physical tamper detection and microcontroller logic.

---

###  What's Included in This Branch?
* **Hardware Interrupt Script (`Firmware/`):** Implements an interrupt service routine (`attachInterrupt`) connected to a chassis microswitch to instantly capture physical breaches.
* **Zeroization Mechanism:** C++ logic designed to immediately wipe sensitive database encryption keys from volatile memory (`RAM`) the moment an intrusion is detected.
* **Simulation & Testing Support:** Ready for validation via browser-based simulators (like Wokwi) or physical microcontrollers (ESP32/Arduino).

---

###  How to Test
1. Load the script from the `Firmware/` directory into your Arduino IDE or Wokwi workspace.
2. Wire a digital input pin (`Pin 2`) to a push-button simulating the chassis door.
3. Trigger the button/switch to verify that the system locks down and clears all key arrays.
4. Online Simulation: Click here to view the live circuit schematic and observe component behavior in real time.
  
https://wokwi.com/projects/472855077629837313


<img width="801" height="729" alt="WhatsApp Image 2026-08-21 at 2 01 10 PM" src="https://github.com/user-attachments/assets/4018623d-3d68-4efc-b1dc-1c53e11839d2" />

