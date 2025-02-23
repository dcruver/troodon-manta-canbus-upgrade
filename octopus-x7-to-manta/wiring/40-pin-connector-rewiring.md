# Troodon 2.0 Pro 40-Pin Connector Rewiring Guide

## Overview
This document explains the **40-pin connector rewiring** necessary for upgrading the Troodon 2.0 Pro with the **BTT Manta M8P** and **CAN Bus toolhead**. 

The **stock Octopus X7 mainboard** uses a 40-pin connector to route signals between the mainboard and various components of the printer, including motors, fans, sensors, and the heated bed. However, with the **Manta M8P upgrade**, some of these connections will **no longer use the 40-pin connector** and will instead be wired directly to the new mainboard or the CAN bus system.

This chart **only includes the wires that need to be rerouted** from the **40-pin connector to the Manta M8P.**  
Anything handled **via CAN Bus** (such as the toolhead stepper motor, X-endstop, and hotend-related components) is **not included** in this chart.

---

## Understanding the Chart Notation

- **Pin Number (40-Pin Connector)**  
  This column represents the **original pin assignment** on the 40-pin connector. Each pin corresponds to a specific signal or power line.  
  Example: `M0-1` refers to **Motor 0, Coil A+**.

- **Function**  
  Describes the **purpose of the signal or wire** that was originally connected through the 40-pin connector.  
  Example: `X Motor (Coil A+)` controls part of the X-axis stepper motor.

- **New Connection (Manta M8P)**  
  This is the **updated wiring destination** on the Manta M8P. Some components connect directly to the board, while 
others may be rerouted to different connectors.  
  Example: The **bed thermistor**, originally wired through the 40-pin connector, is now connected to `TH0` on the Manta.

- **Connector Type**  
  Identifies the type of physical connector used to wire the component into the new mainboard.  
  - `JST-XH 4-Pin` → Used for **stepper motors**  
  - `JST-XH 2-Pin` → Used for **thermistors and sensors**  
  - `Screw Terminals` → Used for **bed heater and SSR control**  
  - `PWM Fan Connector` → Used for **chamber and exhaust fans**  
  - `2-Pin Endstop` → Used for **Y-axis endstop**  

---

## Notable Changes in the Wiring  
### 🔄 **Wires No Longer Routed Through the 40-Pin Connector**
The following components **used to be connected through the 40-pin** but will now be handled differently:
- **X-Axis Endstop** → Now connected to the **SB2209 CAN Bus toolhead** (not wired through the 40-pin).  
- **Z-Axis Endstop** → Now wired **directly to the Manta M8P** instead of going through the 40-pin.  
- **Extruder Motor** → Now controlled **via the CAN Bus**, so the extruder stepper motor wiring is removed from the 
40-pin connector.  

### ✅ **Wires That Still Need to Be Rewired**
The following **connections remain**, but must be manually moved from the 40-pin connector to the correct ports on the 
**Manta M8P**:
- **X and Y stepper motors**  
- **Bed thermistor & heater SSR control**  
- **Chamber & exhaust fans**  
- **Y-axis endstop** (since the X-endstop is now handled by CAN)  
- **Filament sensor** (now connecting to `ADC IN (Fil-DET)`)  

---

## Conclusion
The **40-pin connector is being simplified** as part of this upgrade, with **most toolhead functions moving to CAN 
Bus**. The new wiring setup reduces clutter and improves maintainability while allowing the printer to take advantage 
of advanced features like **CAN Bus communications and a Smart Filament Sensor**.

For a full pin-by-pin breakdown of the changes, see the [Updated Wiring Chart](./40pin-to-manta.csv).
