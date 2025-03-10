# Troodon 2.0 Pro Upgrade Guide

This document provides a comprehensive guide for upgrading a **Formbot Troodon 2.0 Pro** with a **BTT Manta M8P**,
**CANBus Toolhead**, and additional enhancements such as the **Smart Filament Sensor (SFS 2.0)** and **Eddy Duo Probe**.

## Why Choose the Manta M8P Over the Octopus Pro?

The **BTT Manta M8P** was selected over the **BTT Octopus Pro** for several reasons:

- **Native CB1/CB2 Support (Integrated SBC)**  
  - The Manta M8P supports **BTT’s CB1/CB2 Raspberry Pi alternatives**, reducing the need for an external SBC.  
  - This simplifies power and wiring management.


- **Built-in CAN Bus Support**  
  - The Manta M8P has **dedicated CAN Bus support**, making it easier to integrate a **CAN toolhead (SB2209/SB2240)**.  
  - The **Octopus Pro requires additional adapters** (like a USB-to-CAN module) to use CAN Bus.


- **Simplified Wiring**  
  - CAN Bus allows **a single connection to handle multiple toolhead functions**, reducing the number of wires running through the printer frame.
  - This makes maintenance easier and improves reliability.


- **Future-Proofing & Expandability**  
  - Easier to add upgrades like **Smart Filament Sensor (SFS 2.0), Eddy Duo Probe**, and other enhancements.
  - Built-in support for **SPI, UART, and additional expansion ports**.


- **Direct Octopus X7 Replacement**  
  - The Manta M8P has a **similar footprint and mounting points** as the Octopus X7, making installation seamless.
  - The **Octopus Pro may require modifications** for mounting and wiring compatibility.


- **Simpler Power Management**  
  - The **CB1/CB2 module powers directly from the M8P**, while using a Raspberry Pi with the **Octopus Pro requires additional power wiring**.

## Prerequisites
Before starting, ensure you have:
- A **working Klipper setup** on your existing Octopus X7.
- Basic knowledge of **CAN Bus** and **Klipper firmware flashing**.
- Necessary **hardware**:
  - **BTT Manta M8P**
  - **BTT CB2 (or CB1)**
  - **SB2209/SB2240 CAN Toolhead**
  - Required **connectors, wiring, and mounting hardware**.
- Essential **software/tools**:
  - Klipper, Moonraker, Fluidd/Mainsail
  - CAN Bridge setup (if applicable)
  - Basic **soldering or crimping tools** if rewiring is needed.

## Project Phases

1. **Mainboard Replacement & CANBus Toolhead**  
   - Replace **Octopus X7** with **Manta M8P**.
   - Install **BTT CM2** for Raspberry Pi functionality.
   - Rewire 40-pin connector to Manta.
   - Install **BTT SB2209/SB2240 CANBus Toolhead**.
   - [Detailed Steps](./octopus-x7-to-manta/manta-upgrade.md)

2. **Smart Filament Sensor**  
   - Mount **BTT SFS 2.0**.
   - Connect switch and motion sensor to **PF0/PC15**.
   - [Installation Guide](./smart-filament-sensor/sfs-upgrade.md)

3. **Eddy Duo Probe**  
   - Add **Eddy Duo Probe** alongside Voron Tap.
   - Adjust Z-probe settings.
   - [Installation Guide](./eddy-probe/eddy-probe-install.md)

## Before You Start
✔ Backup your existing Klipper configuration (`printer.cfg`, `moonraker.conf`, etc.).  
✔ Print necessary mounting brackets or adapters (see [Printed Parts](./printed-parts-list.csv)).  
✔ Identify all connections on the **40-pin connector** that need rerouting (see [Wiring Guide](octopus-x7-to-manta/wiring)).  
✔ Verify that the **CB2 module is flashed with the correct OS** before installing.  

## Troubleshooting & Notes
- If CAN Bus is not detected, check with `ls /sys/class/net` or `ip link`.
- The **40-pin connector rewiring is detailed in the wiring guide**.
- If you need to revert to the Octopus X7, ensure you have a backup of the original Klipper firmware.

For additional setup details, refer to the **installation guides** linked above.

