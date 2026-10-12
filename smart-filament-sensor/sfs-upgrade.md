# Smart Filament Sensor (SFS 2.0) Upgrade

## Overview
This phase of the upgrade adds the **BTT Smart Filament Sensor (SFS 2.0)** to the **Troodon 2.0 Pro**. The **SFS 2.0** improves filament detection by monitoring both **filament presence** and **extrusion movement**, helping to prevent failed prints.

## Why Upgrade?
The **original filament sensor** on the **Troodon 2.0 Pro** was a simple switch-based sensor that only detected whether filament was present. The **BTT SFS 2.0**, however, offers:
- **Runout detection** (detects when filament runs out)
- **Extrusion detection** (detects if filament is moving properly)
- **Higher reliability** compared to basic sensors

## Wiring

The **SFS 2.0** has two leads, one per sensor. On the **Manta M8P V2** they plug straight into the motor endstop (STOP) row — no 40-pin harness, no Fil-DET port.

| SFS 2.0 Lead | Manta M8P V2 Header | Pin | Plug |
|-------------|-------------|-----|------|
| **Switch Sensor** (filament present) | **M5-STOP** | PF0 | JST-XH 3-pin |
| **Motion Sensor** (filament moving, needs 5V) | **M6-STOP** | PC15 | JST-XH 3-pin |

Match the plug orientation to the neighbouring STOP plugs. Both inputs use Klipper's internal pull-up (`^PF0`, `^PC15`).

> **Retired stock sensor:** the original Troodon filament sensor line (40-pin connector PC2) is no longer used and its 3-wire extension was pulled out of the harness on 2026-10-12. It is not in the config.

## Mounting
The **Smart Filament Sensor** will be mounted near the filament path, ideally at the **rear of the printer** in place of the existing runout sensor.

## Firmware Configuration
Live config lives in `octopus-x7-to-manta/klipper_config/printer/fans.cfg`:

```ini
[filament_switch_sensor sfs_switch]
switch_pin: ^PF0  # M5-STOP on Manta M8P V2
pause_on_runout: False
runout_gcode:
    PAUSE
    M117 Filament runout detected!
insert_gcode:
    M117 Filament inserted.

[filament_motion_sensor sfs_motion]
switch_pin: ^PC15  # M6-STOP on Manta M8P V2
detection_length: 25
extruder: extruder
pause_on_runout: True
runout_gcode:
    PAUSE
    M117 Filament motion stopped!
insert_gcode:
    M117 Filament encoder inserted.
```

Both sensors are disabled at startup and enabled by `SFS_ENABLE` in `PRINT_START`; `PRINT_END` runs `SFS_DISABLE`.

To verify wiring: `QUERY_FILAMENT_SENSOR SENSOR=sfs_switch` should flip between `True` and `False` as filament is inserted/removed. A disconnected lead floats high and reads as filament present.

## Summary
- The SFS 2.0 plugs into the Manta M8P V2 endstop row: **M5-STOP (PF0)** for runout, **M6-STOP (PC15)** for motion.
- The 40-pin harness and the Fil-DET port are not involved.
- The stock Troodon filament sensor line is retired and has been removed from the harness.
