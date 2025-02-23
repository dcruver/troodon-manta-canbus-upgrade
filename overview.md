# Troodon 2.0 Pro Upgrade Overview

This project documents the process of upgrading a **Formbot Troodon 2.0 Pro** with **BTT Manta M8P**, 
**CANBus Toolhead**, and additional enhancements such as the **Smart Filament Sensor (SFS 2.0)** and **Eddy Duo Probe**.

## Project Phases
1. [Phase 1: Mainboard Replacement & CANBus Toolhead](./octopus-x7-to-manta/manta-upgrade.md)
   - Replace **Octopus X7** with **Manta M8P**.
   - Install **BTT CM2** for Raspberry Pi functionality.
   - Rewire 40-pin connector to Manta.
   - Install **BTT SB2209/SB2240 CANBus Toolhead**.


2. [Phase 2: Smart Filament Sensor](./smart-filament-sensor/sfs-upgrade.md)
   - Mount **BTT SFS 2.0**.
   - Connect switch and motion sensor to **PF0/PC15**.


3. [Phase 3: Eddy Duo Probe](./eddy-probe/eddy-probe-install.md)
   - Add **Eddy Duo Probe** alongside Voron Tap.
   - Adjust Z-probe settings.

For detailed wiring, see [wiring](octopus-x7-to-manta/wiring).
