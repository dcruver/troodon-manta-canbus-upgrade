# Phase 1: Manta M8P & CAN Bus Toolhead Upgrade

## Overview
This phase replaces the **Octopus X7** with the **BTT Manta M8P + CB2** and upgrades the toolhead to **CAN Bus (SB2209/SB2240)**.

This process involves:
- **Replacing the mainboard**
- **Rewiring the 40-pin connector**
- **Setting up CAN Bus for the toolhead**
- **Flashing Klipper firmware**

---

## **1️⃣ Backup & Prepare for Disassembly**
✅ **Backup Klipper Configuration** (printer.cfg)  
✅ **Document existing wiring**  
✅ **Ensure a stable work area**  

## **2️⃣ Remove the Existing Octopus X7 Board**
✅ **Disconnect all wiring**  
✅ **Identify the 40-pin connector cables** that need rerouting  

## **3️⃣ Install the Manta M8P & CB2**
✅ **Mount the new Manta M8P board**  
✅ **Adapt the mounting holes if necessary**  
✅ **Power the CB2 module separately**  

## **4️⃣ Rewire Components to the Manta M8P**
✅ **Route the motors, sensors, and fans from the 40-pin connector**  
✅ **Wire the power supply properly**  
✅ **Connect the CAN Bus toolhead**  

## **5️⃣ Set Up CAN Bus**
✅ **Install the SB2209 board on the Stealthburner**  
✅ **Run the CAN Bus umbilical properly**  
✅ **Ensure power is properly routed from the PSU**  

## **6️⃣ Update & Flash Firmware**
✅ **Configure Klipper for the Manta M8P and CAN Bus**  
✅ **Flash firmware to the Manta and CAN board**  
✅ **Test basic functionality**  

---

## Next Steps
After this phase, proceed to **[Phase 2: Smart Filament Sensor Installation](../smart-filament-sensor/sfs-upgrade.md)**.
