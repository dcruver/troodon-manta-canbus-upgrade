# Troodon 2.0 Pro Upgrade

## Overview
The wire management of the Stealthburner on this printer makes removing the fan shroud, whether for clearing extruder 
jams or maintenance, a nerve-wracking process. Seeing the simplified wiring of the BTT SB2209/SB2240 CANBus system has 
motivated me to undertake a major upgrade to my Troodon 2.0 Pro.

Additionally, I want to improve filament sensing and bed mesh mapping capabilities to further refine print quality and 
reliability.

Goals of This Upgrade
1. Replace the Troodon 2.0 Pro's Octopus X7 with a BTT Manta M8P
2. Upgrade the Raspberry Pi clone with a BTT CB2
3. Switch from traditional wiring to CANBus, using a BTT SB2209/SB2240 toolhead board
4. Implement a CANBus umbilical from the M8P to the Stealthburner
5. Improve filament detection with a BTT Smart Filament Sensor
6. Upgrade bed mesh probing by replacing the Voron Tap with a BTT Eddy Duo

## Reference Documentation
This type of mainboard replacement has been done before, notably by arj43090, who documented their work on 
[Reddit](https://www.reddit.com/r/Troodon/comments/11u8lno/troodon_20_documentation/). I'm relying on those 40-pin 
connector notes heavily for re-wiring the Manta M8P.

### Upgrade Plan: Phased Approach
This upgrade is divided into three distinct phases to ensure that each component is installed, configured, and tested 
before moving on.

### Phase 1: Mainboard Replacement & CANBus Toolhead

Goal: Replace the Octopus X7 with the Manta M8P, migrate the Raspberry Pi clone to CB2, and switch the toolhead to 
CANBus. This phase is the most complex, as it requires rewiring connections from the 40-pin connector and ensuring 
CANBus communication works reliably.

Steps:
+ Print & install mounting adapters for the Manta M8P
+ Rewire all necessary connections from the 40-pin connector to the Manta
+ Wire the CANBus connection for the SB2209/SB2240 toolhead board
+ Run a new CANBus umbilical instead of using the existing cable chains
+ Power the CANBus toolhead directly from the PSU
+ Update Klipper configuration for the new hardware
+ Perform test prints & verify stability

✅ Completion Unlocks: A fully functional Manta M8P & CANBus toolhead with improved wire management.

### Phase 2: BTT Smart Filament Sensor
Goal: Replace the basic runout sensor with the BTT Smart Filament Sensor (SFS) to detect filament presence & extrusion 
issues. This phase is less complex than Phase 1 but still requires additional wiring.

Steps:
+ Print & install a mount for the Smart Filament Sensor
+ Decide placement (back of the printer or toolhead)
+ Connect the sensor to the Manta M8P (ADC IN Fil-DET)
+ Update Klipper configuration to enable filament presence & extrusion detection
+ Test sensor behavior with various filament types
+ Verify that it pauses prints correctly on detection

✅ Completion Unlocks: Filament presence & extrusion failure detection for improved print reliability.

### Phase 3: Adding BTT Eddy Duo (Alongside Voron Tap)

Goal: Add the BTT Eddy Duo probe while keeping Voron Tap, allowing both probes to be used for different tasks.

Voron Tap will continue to handle Z homing
Eddy Duo will be used for bed mesh leveling
This setup allows redundancy and improved accuracy, leveraging Eddy Duo’s inductive sensing for better bed mapping 
while keeping Tap’s mechanical precision for Z-homing.

Steps:
+ Print & install a mount for the Eddy Duo
+ Wire it to the SB2209/SB2240 toolhead MCU
+ Update Klipper configuration to integrate both probes**
+ Run Quad Gantry Leveling & Bed Mesh Calibration
+ Validate probe accuracy with multiple test prints

✅ Completion Unlocks: Hybrid Z-homing and bed leveling using both probes for maximum accuracy.

### Wiring & Installation
The wiring process involves:

* Migrating all necessary connections from the Octopus X7 to the Manta M8P
* Rewiring the 40-pin connector to accommodate new mainboard connections
* Creating a CANBus umbilical for a cleaner toolhead wiring setup
* Powering the CANBus toolhead directly from the PSU

## Related Links
+ [BTT Manta M8P Documentation](https://github.com/bigtreetech/Manta-M8P)
+ [BTT SB2209/SB2240 Toolhead](https://github.com/bigtreetech/EBB)
+ BTT Smart Filament Sensor
+ [BTT Eddy Duo Probe](https://github.com/bigtreetech/Eddy)