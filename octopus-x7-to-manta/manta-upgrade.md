# Phase 1: Manta M8P & CAN Bus Toolhead Upgrade

## Overview
This phase replaces the Octopus X7 with the BTT Manta M8P + CB2 and upgrades the toolhead to CAN Bus (SB2209/SB2240).

This process involves:
- Replacing the mainboard
- Rewiring the 40-pin connector
- Setting up CAN Bus for the toolhead
- Flashing Klipper firmware

## Why Upgrade?
The Troodon 2.0 Pro comes stock with a BTT Octopus X7 mainboard, which lacks native CAN support and requires a 
complex wiring harness for the toolhead. Additionally, the provided Raspberry Pi alternative is outdated and struggles 
with resource-intensive tasks.

Upgrading to the BTT Manta M8P + CB2 offers several benefits:

+ Native CANBus support, simplifying toolhead wiring.
+ Integrated CB2 Raspberry Pi alternative, providing better processing power.
+ Improved expandability for additional features like Smart Filament Sensor (SFS 2.0) and Eddy Duo Probe.
+ Easier maintenance, as the rewiring process reduces the number of connections running through the printer’s frame.
+ This upgrade provides future-proofing for additional enhancements while making the printer easier to maintain.

## Upgrade Steps

### Printing Parts
While waiting for the mainboard and other electronics to be delivered, I started printing the parts that I'll need and
designing those that don't already have publicly available solutions. Those are listed in here:
[Printed Parts](./printed-parts-list.csv).

### Install Rocker Feet
This isn't **absolutely** necessary, but it really makes accessing the bottom of the printer much easier. The rocker
feet I used are on [Printables](https://www.printables.com/model/629765-troodon-20-rocker-feet), along with instructions
for a ssembly.

### Update & Flash Firmware (off-printer)
+ Use a 24v power source to power the Manta. Don't attach the CAN Bus cable yet.
+ This process has been extensively documented elsewhere, so I won't go into depth here. I found 2 excellent sources for
instructions on this:
  + [Esoterical CAN Bus Guide](https://canbus.esoterical.online/)
  + [LukasOtis](https://www.youtube.com/watch?v=bmgJjOtXhtQ)
+ Note that, assuming these are the only 2 devices on your CAN Bus, they'll both need jumpers for the resistors. The 
Esoterical link above shows exactly where they need to go. 
+ Be sure to remove the 5v power jumper from the SB2209 *before* you supply it with 24v power. This jumper is located
right next to the USB-C port on the 2209.
+ Before moving on, you should have your CM2 and CAN Bus up and running. You can verify that with:
```bash
$ ~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0
```

You should see output something like:
```
Found canbus_uuid=486555826296, Application: Klipper
Found canbus_uuid=1667e2a61a60, Application: Klipper
Total 2 uuids found
+ ```

### Remove the Existing Octopus X7 Board
+ Disconnect all wiring  
+ Identify the 40-pin connector cables that need rerouting  

### Install the Manta M8P & CB2
+ + Use the Octopus X7 to Manta M8P mounting adapter to mount the M8P on the printer.

### Rewire Components to the Manta M8P
| **Purpose**          | **Label**     | **Handled by CAN Bus** |
|----------------------|--------------|------------------------|
| **Stepper Motor A (X-Axis1)** | A14 (Red), A13 (Ylw), B13 (Grn), B11 (Blk) | ❌ (Stays direct) |
| **Stepper Motor B (X-Y Axis2)** | A12 (Red), B12 (Ylw), A11 (Grn), B11 (Blk) | ❌ (Stays direct) |
| **Extruder Motor**  | B18 (Red), A18 (A2), B19 (M2-A1), A19 (M2-A2) | ✅ |
| **Hotend Heater**   | A17 (DCIN), B17 (DCIN) | ✅ |
| **Thermistor**      | A3 (GND), A4 (PF4) | ✅ |
| **Hotend Fan**      | A16 (DCIN), B16 (PB8) | ✅ |
| **Blower Fan**      | A15 (DCIN), B15 (PB8) | ✅ |
| **X Stop**          | B8 (GND), B7 (PC13) | ✅ |
| **Y Stop**          | B8 (GND), A6 (PC15) | ❌ (Stays direct – not managed by CAN Bus) |
| **Auto Z** (Z Probe) | B7 (GND), A9 (PC13) | ✅ (Replaced by CAN) |
| **Filament Sensor** | A8 (Red), B8 (B), A7 (Ylw), PE14 (GND) | ❌ (Stays direct) |
| **Bed-Level Sensor** | B20 (M2-A1), A20 (M2-A2) | ❌ (Not in use) |
| **LED (Inside Light)** | B1 (PE6 DCIN), A1 (DCIN) | ❌ (Stays direct) |
| **Exhaust Fan** | B10 (PE5), A10 (PE5) | ❌ (Stays direct) |

**Summary of What to Disconnect**
1. Auto Z (Z Probe)
2. Extruder Motor
3. Hotend Heater
4. Thermistor
5. Hotend Fan
6. Blower Fan
7. X Stop
8. Bed-Level Sensor (not in use)

Everything else stays connected to the Manta M8P.

### Set Up CAN Bus
+ Install the SB2209 board on the Stealthburner  
+ Run the CAN Bus umbilical properly  
+ Ensure power is properly routed from the PSU  
+ Verify CAN Bus connection using `ls /sys/class/net` or `ip link` to ensure the CAN device is recognized.  

---

## Next Steps
Once all wiring is complete and Klipper firmware is validated, we can move on to additional enhancements.
After this phase, proceed to [Phase 2: Smart Filament Sensor Installation](../smart-filament-sensor/sfs-upgrade.md).

