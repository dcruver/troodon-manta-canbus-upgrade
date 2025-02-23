# Phase 3: Eddy Duo Probe Installation

## Overview
This phase integrates the BTT Eddy Duo Probe alongside the Voron Tap to improve bed mesh leveling.

This process involves:
- Installing Eddy Duo on the toolhead
- Wiring it to the CAN Bus toolhead
- Configuring Klipper for dual-probing

## Why Upgrade?
The Voron Tap provides reliable Z-homing and gantry leveling, but it does not account for bed inconsistencies in real 
time. The BTT Eddy Duo Probe improves bed-leveling accuracy by:

+ Combining an inductive probe with Voron Tap, allowing dynamic compensation for bed warping.
+ Providing faster and more precise Z-offset calibration. 
+ Eliminating potential inconsistencies caused by mechanical wear on the Tap mechanism. 
+ Improving first-layer reliability, reducing the need for manual adjustments. 
+ By adding the Eddy Duo, we maintain the strengths of Voron Tap while improving bed mesh accuracy, leading to more 
consistent and high-quality prints.

## Upgrade Steps
### Install Eddy Duo on the Toolhead
+ Mount the Eddy Probe next to Voron Tap  
+ Run wires through the toolhead harness  

### Connect to CAN Bus
+ Connect Eddy Duo probe to the SB2209 CAN Bus toolhead  

### Update Firmware & Tune Mesh Leveling
+ Modify Klipper configuration  
+ Calibrate bed mesh with both Tap & Eddy Duo  
+ Test prints with new probing system

## Final Steps
With the Eddy Duo Probe installed, the upgrade is complete! 🎉  
For troubleshooting and additional tuning, refer to the [40-Pin Rewiring Guide](40-pin-rewiring.md).
